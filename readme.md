# Laravel Email Domain Blacklist

Validate email input that it ins not blacklisted for a specific domain name.

# Installation

Require this package with composer:

```
composer require alariva/laravel-email-domain-blacklist
```

This package uses *AutoDiscovery*.

If you are using Laravel <= 5.4 manually add the Service Provider to the providers array in `config/app.php`

```php
Alariva\EmailDomainBlacklist\EmailDomainBlacklistServiceProvider::class,
```

# Documentation

Laravel Email Domain Blacklist is a lightweight package that extends your validation rules with `blacklist`.

You may pass a local or remote JSON file containing all the blacklisted email domains, usually those that are disposable email services.

In case you are using a third-party remote list you may also append your custom email domains.

You may update the cached list with the console command (manually or scheduled).

An auto-update option is available in case you don't want to run the command and prefer to auto-update on the first validation taking place.

The validation message is translated into english and spanish, feel free to PR your language.

## Configuration

### source: string|null

You may specify the preferred URL or file path to update the blacklist.

Keep `null` if you don't want to use a remote source.

Default: `https://raw.githubusercontent.com/ivolo/disposable-email-domains/master/index.json`

### cache-key: string|null

You may change the cache key for the sourced blacklist.

Keep `null` if you want to use the default value.

### auto-update: true|false

Specify if should automatically get source when cache is empty.

**ADVICE:** This may slow down the first request upon validation.

Default: `false`

### append: string|null

You may a string of pipe `|` separated domains list.

Keep `null` if you don't want to append custom domains.

**Example:** `example.com|example.net|foobar.com`.

## Laravel validator

```php
public function store(Request $request) {
    $this->validate($request,
    	['email' => 'required|email|blacklist']
    );
}
```

## Console Updater

Manually updating the cached blacklist:

```
php artisan blacklist:update-email-domains
```

Scheduling the cached blacklist update (example):

```php

	// app/Console/Kernel.php @schedule

    // ...
    $schedule->command('blacklist:update-email-domains')
             ->monthly()
             ->sundays()
             ->at('05:00')
             ->withoutOverlapping()
             ->sendOutputTo(storage_path('logs/email-domains-blacklist.txt'));
    // ...
```

# Testing

```
vendor/bin/phpunit
```

# Contributing

Please try to follow the psr-2 coding style guide. http://www.php-fig.org/psr/psr-2/

# Credits

  * [Ariel Vallese](https://www.linkedin.com/in/alariva/)
  * This package was inspired [on this great post by Matt Kingshott](https://medium.com/@mattkingshott/laravel-validation-rule-block-disposable-email-blacklisted-domains-949cab9c59fe)

# Package alternatives

  * [Laravel-Email-Domain-Validation](https://github.com/madeITBelgium/Laravel-Email-Domain-Validation)

# License

MIT