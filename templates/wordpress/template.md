# WordPress Theme and Plugin Template

Use this when the repository is a theme, a plugin, a mu-plugin, or a site checkout that contains them. Pair it with `php`. A checkout of only `wp-content/themes` or `wp-content/plugins` is still WordPress. Do not assume a full core tree is present.

## Detected Technologies

Record **Detected / Not detected / Unknown** for:

- Theme, plugin, or both. A theme has `Theme Name:` in `style.css`. A plugin has `Plugin Name:` in its main PHP file.
- Classic theme (`index.php` plus PHP templates) or block theme (`templates/index.html` and `theme.json`).
- `Requires at least` and `Requires PHP` from those headers. Those values beat any default in this template.
- Composer, PHPUnit, PHP_CodeSniffer / WPCS, `@wordpress/scripts`.
- WooCommerce only when `woocommerce` is a dependency or the code calls `WC()` / `wc_get_product`.

WordPress 7.0 ("Armstrong", May 2026) requires PHP 7.4 and recommends PHP 8.3. Real-time collaboration did not ship in 7.0. Do not call collaboration APIs, `WP_COLLABORATION_MAX_USERS`, or a `sync.providers` filter. Read the headers and `wp-includes/version.php` when core is in the tree before using 7.0-only functions.

## Stack Detection Signals

```bash
rg --files -g 'style.css' -g 'theme.json' -g 'functions.php' -g 'uninstall.php' -g 'composer.json' -g '!vendor' -g '!node_modules' -g '!wp-includes' -g '!wp-admin'
rg -n -g '*.css' -g '*.php' -g '!vendor' -g '!node_modules' '^(Theme Name|Plugin Name|Requires at least|Requires PHP|Text Domain|Version):'
rg -n -g '*.php' -g '!vendor' '\$wpdb|WP_Query|query_posts|register_rest_route|wp_enqueue_|dbDelta|register_post_type|add_theme_support' | head -40
```

## Where code belongs

- Presentation, templates, `theme.json`, patterns, and theme supports belong in the theme.
- Custom post types, taxonomies, custom tables, REST routes, cron, and options that must survive a theme switch belong in a plugin.
- Do not edit WordPress core. Do not copy a parent theme into a child and then diverge file by file.

## Themes

- A classic theme needs a `style.css` header and `index.php`. A block theme needs `style.css`, `theme.json`, and `templates/index.html`.
- `theme.json` version is 3 for current block themes. Change settings the theme already structures. Do not invent a parallel styling system in `functions.php` when `theme.json` already owns that token.
- On `after_setup_theme`: `add_theme_support` for `title-tag`, `post-thumbnails`, `html5`, and `wp-block-styles` / `editor-styles` only when the theme is a block theme or already opts into them. Register nav menus with `register_nav_menus`.
- Call `wp_head()` in `header.php` and `wp_footer()` in `footer.php`. Use `language_attributes()` and `body_class()`.
- Enqueue CSS and JS with `wp_enqueue_style` and `wp_enqueue_scripts`. Do not print raw `<script>` or `<link>` tags. Declare dependencies and a version. The stylesheet for a classic theme is `get_stylesheet_uri()`.
- Child themes set `Template:` to the parent slug. Use `get_stylesheet_directory()` for child files and `get_template_directory()` for parent files.
- Text domain matches the theme slug. Translate with `esc_html_e`, `esc_attr_e`, or `esc_html__`. Escape translated HTML with `wp_kses_post`.
- Do not call `query_posts`. Use `WP_Query` and `wp_reset_postdata()`, or hook `pre_get_posts` and return immediately unless `$query->is_main_query()` is true and the request is not admin.
- Patterns go through `register_block_pattern`. Block styles go through `register_block_style`.
- Custom post types registered in a theme disappear when the theme is switched. Move them to a plugin when the content must remain.

## Plugins

- The main file starts with the plugin header and `defined( 'ABSPATH' ) || exit;`.
- Prefix functions, classes, hooks, option names, script handles, and custom table suffixes with the plugin slug. WordPress core and other plugins share one global namespace.
- Register hooks in a class the plugin boots once. Do not re-register the same action on every `init` without a guard.
- `register_activation_hook` creates tables and options. Store a schema version in an option. On later loads, compare that version and run `dbDelta` only when it changed. Do not run `dbDelta` on every front-end request.
- `uninstall.php` begins with `defined( 'WP_UNINSTALL_PLUGIN' ) || exit;` and deletes options, transients, and custom tables the plugin owns. Deactivation is not uninstall. Do not drop tables on deactivate.
- Settings use `register_setting` with a `sanitize_callback`. The form prints `settings_fields`, `wp_nonce_field`, and checks `current_user_can` before saving.
- Admin actions verify a nonce with `check_admin_referer` or `wp_verify_nonce`, then check a specific capability such as `manage_options` or `edit_posts`. A logged-in check is not authorization.
- REST routes use `register_rest_route` with a real `permission_callback`. `__return_true` is only acceptable for public read endpoints that expose public content. Mutations require a capability check. Sanitize arguments in `args` or the callback with `sanitize_text_field`, `absint`, or `wp_kses_post` as the field type requires. Return `WP_Error` or `WP_REST_Response`, not a bare `wp_die`.
- Post meta that the block editor or REST must see uses `register_post_meta` with `show_in_rest`, `single`, `type`, `sanitize_callback`, and `auth_callback`. Registering meta with `show_in_rest` keeps it available for future collaboration. Do not write collaboration client code.
- Cron: `wp_next_scheduled` before `wp_schedule_event`. Clear the hook on uninstall. The callback must be idempotent.
- Call `wp_ai_client_prompt()` only inside `function_exists( 'wp_ai_client_prompt' )`. Credentials belong in Settings → Connectors, not in plugin source. Check `is_wp_error` on the result.
- Register Abilities only inside `wp_abilities_api_init` when `function_exists( 'wp_register_ability' )`. Every ability has a `permission_callback`.
- A PHP-only block uses `register_block_type` with a `render_callback` that escapes every attribute before printing it. Keep existing `block.json` builds when the plugin already has them.

## Database

WordPress tables use `$wpdb`. The database is usually MySQL or MariaDB. Playground and some tests use SQLite. Read `DB_HOST` and the drop-in before assuming MySQL features.

```php
global $wpdb;
$ids = array_map( 'absint', $ids );
$placeholders = implode( ',', array_fill( 0, count( $ids ), '%d' ) );
$sql = "SELECT id, title FROM {$wpdb->prefix}items WHERE status = %s AND id IN ($placeholders)";
$prepared = $wpdb->prepare( $sql, array_merge( array( 'publish' ), $ids ) );
$rows = $wpdb->get_results( $prepared );
```

- `$wpdb->prepare` placeholders are `%d`, `%f`, `%s`, and, on WordPress 6.2+, `%i` for identifiers. `%s` is already quoted by prepare. Do not wrap `%s` in extra quotes.
- A `LIKE` pattern is `esc_like()` first, then `%` wildcards, then `prepare`.
- An `IN ()` list is one placeholder per value. Never concatenate raw IDs into the SQL string.
- Table names use `$wpdb->prefix`. Create tables with `dbDelta` and `$wpdb->get_charset_collate()`. `dbDelta` requires two spaces before the primary-key column list: `PRIMARY KEY  (id)`.
- Index columns used in `WHERE`, `JOIN`, and `ORDER BY`. A custom table that looks up by `post_id` needs a key on `post_id`.
- Check `$wpdb->last_error` after a write. Do not hide failures with `$wpdb->suppress_errors( true )` and then ignore the result.
- `WP_Query` and `get_posts`: set `no_found_rows` when you do not need found-post counts, `update_post_meta_cache` and `update_post_term_cache` to false when you will not read meta or terms, and `fields` to `ids` when you only need IDs. `posts_per_page` of `-1` loads the whole table. Paginate.
- Inside the loop, a fresh `$wpdb` query or `get_posts` per row is an N+1. Prime caches with one query, or use `update_meta_cache`.
- `get_post_meta` is not itself N+1 when the meta cache was primed. Do not "fix" it by issuing a query per post.
- Options that must autoload stay small. A large array in `wp_options` with `autoload` yes is loaded on every request. Use a custom table or a non-autoloaded option.
- WooCommerce orders: when High-Performance Order Storage is active, orders are not `shop_order` rows you should query from `wp_posts`. Use `wc_get_orders` and the CRUD objects the store already uses.

## Security and output

- Unslash input, then sanitize: `sanitize_text_field( wp_unslash( $_POST['title'] ?? '' ) )`. Use `absint` for IDs.
- Escape at echo time. `esc_html` for text, `esc_attr` for attributes, `esc_url` for URLs, `wp_kses_post` for limited HTML, `wp_json_encode` for data placed in a script via `wp_add_inline_script`.
- Nonces are CSRF tokens, not authorization. Check both the nonce and `current_user_can`.
- AJAX handlers (`wp_ajax_` and `wp_ajax_nopriv_`) check a nonce and a capability. A nopriv handler is public. Do not put privileged writes on the nopriv hook.
- Do not log secrets, nonces, or password fields. Do not commit `wp-config.php` credentials.

## Performance

- Remote HTTP on a page view (`wp_remote_get` during `template_redirect` or `the_content`) belongs behind a transient or cron unless the user is waiting on that exact response.
- Cache keys are prefixed and grouped: `wp_cache_get( $key, 'plugin_slug' )`. Delete or replace the key when the underlying row changes.
- Transients expire. Do not use them as a permanent store.
- Autoloaded options, `posts_per_page => -1`, and uncached remote calls are the first places to look.

## Agent Mapping

| Owner | Focus |
|---|---|
| `database` | `$wpdb` prepare, `dbDelta`, indexes, `WP_Query` flags, WooCommerce HPOS |
| `sentinel` | Nonces, capabilities, escaping, REST `permission_callback`, file includes |
| `bolt` | N+1 queries, `posts_per_page => -1`, autoloaded options, remote calls on render |
| `picasso` | Templates, theme.json, navigation, focus, empty states in theme output |
| `testing` | PHPUnit or Pest already in the repo, Brain Monkey or `WP_UnitTestCase` when that harness exists |
| `api` | `register_rest_route`, argument schemas, permission callbacks |
| `shtef` | `Requires PHP`, Composer platform, tested plugin/theme header bumps |
| `docs` | Readme headers and install steps that match the headers in code |
| `custodian` | Unused templates, dead shortcodes, options no code reads |

## Verification Commands

Run what the repository already has. A missing tool is **Unknown**.

```bash
find . -name '*.php' -not -path '*/vendor/*' -not -path '*/node_modules/*' -print0 | xargs -0 -n1 php -l
composer test
vendor/bin/phpcs
```

`php -l` checks syntax only. It does not prove a nonce, a query, or a template works. Use PHPUnit when `phpunit.xml` or `phpunit.xml.dist` exists. Use WordPress Coding Standards only when `phpcs.xml` or `phpcs.xml.dist` already configures them.
