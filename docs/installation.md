# Installation

Require the package using composer.

```bash
composer require buildcode/laravel-database-emails
```

If you're running Laravel 5.5 or later you may skip this step. Add the service provider to your application.

```
Buildcode\LaravelDatabaseEmails\LaravelDatabaseEmailsServiceProvider::class,
```

Publish the configuration files.

```bash
php artisan vendor:publish --provider=Buildcode\\LaravelDatabaseEmails\\LaravelDatabaseEmailsServiceProvider
```

Create the database table required for this package.

```bash
php artisan migrate
```

Add the e-mail cronjob to your scheduler

```php
<?php

/**
 * Define the application's command schedule.
 *
 * @param  \Illuminate\Console\Scheduling\Schedule  $schedule
 * @return void
 */
protected function schedule(Schedule $schedule)
{
     $schedule->command('email:send', ['--timeout' => 300])->everyMinute()->withoutOverlapping(5);
}
```

Using the above configuration, the `email:send` process will exit after 5 minutes (`--timeout`) and won't overlap if the process still runs after 5 minutes (`withoutOverlapping`)