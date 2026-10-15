# PHP Stack Template


## Stack Detection Signals

```bash
rg --files -g 'composer.json' -g 'artisan' -g '**/*.php' -g '!vendor'
rg -n 'laravel/framework|symfony/framework-bundle|Illuminate\\|Plugin Name:|Theme Name:' composer.json -g '*.php' -g '*.css' -g '!vendor'
```

## Detected Technologies
- **Framework:** [Laravel/Symfony/WordPress/CodeIgniter/Yii/etc.]
- **PHP version:** from `composer.json` `require.php` or `config.platform.php`, or a WordPress `Requires PHP` header. Do not use syntax newer than that version.
- **Package Manager:** [Composer]
- **Test Runner:** [PHPUnit/Pest/etc.]
- **Linter:** [PHP_CodeSniffer/Pint/etc.]
- **Build Tool:** [Webpack/Vite/etc.]

## Common Stack Patterns

### Laravel
- MVC: Controllers, Models, Views
- Eloquent ORM: relationships, scopes, accessors
- Routes: `routes/web.php`, `routes/api.php`
- Migrations: `database/migrations/`
- Seeders: `database/seeders/`
- Artisan commands
- Blade templates: `resources/views/`
- Service providers: `app/Providers/`

### WordPress
A `Theme Name:` or `Plugin Name:` header means this is WordPress. Load `templates/wordpress/template.md` as well. The rules below still apply to any PHP in that repository.

- Guard plugin files with `defined( 'ABSPATH' ) || exit;`.
- `$wpdb` queries go through `$wpdb->prepare`. Output goes through `esc_html`, `esc_attr`, `esc_url`, or `wp_kses_post`.
- Nonces and `current_user_can` are both required for privileged writes.
- Do not call `query_posts`. Paginate `WP_Query`. `posts_per_page => -1` is a full table read.

### Symfony
- Bundle structure
- Doctrine ORM
- YAML/XML/PHP config
- Twig templates
- Console commands

### Common File Locations
```
├── app/                  # Application code
│   ├── Http/Controllers/
│   ├── Models/
│   ├── Services/
│   └── Providers/
├── routes/               # Route definitions
├── database/
│   ├── migrations/
│   ├── seeders/
│   └── factories/
├── resources/views/      # Blade templates
├── config/               # Configuration
├── public/               # Web root
└── tests/                # Tests
```

## Project Type Detection Signals
- `composer.json`
- `artisan` (Laravel)
- `symfony` in composer.json
- `public/index.php`
- `.env` with APP_KEY, DB_*

## Agent Customizations

### Laravel-Specific
- Eloquent relationships
- Form Requests for validation
- Policies and Gates for authorization
- Events and Listeners
- Job queues: Horizon, Redis

### PHP Quality
- Type declarations on new code when the project's PHP version allows them. WordPress 7.0 supports PHP 7.4, so do not require union types, enums, or `readonly` unless `Requires PHP` or `composer.json` says 8.0, 8.1, or newer.
- Nullable parameters and return types that match the version. Do not add `strict_types=1` to a WordPress file that is loaded by core, because core is not strict and the mismatch surfaces as type errors at the boundary.
- PHPStan or Psalm only when the repository already runs them. WPCS / PHPCS only when `phpcs.xml` exists.
- Composer autoload. Do not `require` random files when the project has a PSR-4 map.
- `declare` and short closures (`fn()`) only on PHP 7.4 or newer. Named arguments only on PHP 8.0 or newer.

### Security
- CSRF protection
- XSS prevention (Blade escaping)
- SQL injection (Eloquent parameterization)
- Mass assignment protection

## Common Issues
- PHP version drift vs `composer.json` platform config or a WordPress `Requires PHP` header
- Debug/error display enabled in production (`WP_DEBUG_DISPLAY`, `display_errors`)
- Raw SQL string interpolation (SQL injection), including `$wpdb` queries built with `.` or double-quoted variables
- Unescaped `echo` of `$_GET`, `$_POST`, post titles, or meta
- Privileged actions with no nonce or no capability check
- `posts_per_page => -1` and queries inside loops
- OPcache / autoload misconfiguration
- Low `memory_limit` / `max_execution_time`
- Missing session/cookie security flags
- `eval`, `create_function`, `extract` on request input

## Testing Patterns
- `composer test` / `phpunit` / `pest`
- `php -l` syntax lint (or `composer lint`)
- Laravel: `php artisan test`, Dusk for browser
- Static analysis: PHPStan / Psalm
- Coverage: `phpunit --coverage-text`

## Industry Standards (verified 2026-10 via Context7)

Sources: `www.php.net/manual/en/`, `laravel.com/docs/`, `symfony.com/doc/current/index.html`, `www.php-fig.org/psr/psr-12/`, `developer.wordpress.org/apis/`

- Follow the framework version in `composer.lock` when it exists. Do not upgrade the framework as a side effect of a bugfix.
- Match language syntax to the declared PHP version. WordPress 7.0 requires PHP 7.4 and recommends 8.3. A plugin or theme header wins over that default.
- Validate request input with the validator the app already uses. Eloquent, Doctrine, PDO, and `$wpdb` queries stay parameterized.
- Run PHPUnit or Pest the way `composer.json` scripts define them.
- Do not commit `.env` or `wp-config.php` secrets.
