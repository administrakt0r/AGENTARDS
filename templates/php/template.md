# PHP Stack Template

## Detected Technologies
- **Framework:** [Laravel/Symfony/CodeIgniter/Yii/etc.]
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
- Type declarations
- PHPStan/Psalm static analysis
- Composer autoload optimization
- OPcache configuration

### Security
- CSRF protection
- XSS prevention (Blade escaping)
- SQL injection (Eloquent parameterization)
- Mass assignment protection
