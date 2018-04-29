# Usage

### Send an email

```php
<?php

use Buildcode\LaravelDatabaseEmails\Email;

Email::compose()
    ->label('welcome')
    ->recipient('john@doe.com')
    ->subject('This is a test')
    ->view('emails.welcome')
    ->variables([
        'name' => 'John Doe',
    ])
    ->send();
```

### Specify multiple recipients

```php
<?php

use Buildcode\LaravelDatabaseEmails\Email;

Buildcode\LaravelDatabaseEmails\Email::compose()
    ->recipient([
        'john@doe.com',
        'jane@doe.com'
    ]);
```

### CC and BCC

```php
<?php

use Buildcode\LaravelDatabaseEmails\Email;

Email::compose()
    ->cc('john@doe.com')
    ->cc(['john@doe.com', 'jane@doe.com'])
    ->bcc('john@doe.com')
    ->bcc(['john@doe.com', 'jane@doe.com']);
```

### Using mailables

You may also pass a mailable to the e-mail composer.

```php
<?php

use Buildcode\LaravelDatabaseEmails\Email;

Email::compose()
    ->mailable(new OrderShipped())
    ->send();
```

### Attachments

```php
<?php

use Buildcode\LaravelDatabaseEmails\Email;

Email::compose()
    ->attach('/path/to/file');
```

Or for in-memory attachments:

```php
<?php

use Buildcode\LaravelDatabaseEmails\Email;

Email::compose()
    ->attachData('<p>Your order has shipped!</p>', 'order.html');
```

### Custom Sender

```php
<?php

use Buildcode\LaravelDatabaseEmails\Email;

Email::compose()
    ->from('john@doe.com', 'John Doe');
```

### Scheduling

You may schedule an e-mail by calling `later` instead of `send`. You must provide a Carbon instance or a strtotime valid date.

```php
<?php

use Buildcode\LaravelDatabaseEmails\Email;

Email::compose()
    ->later('+2 hours');
```

### Resend failed e-mails

#### Resend all failed e-mails

```bash
php artisan email:resend
```

#### Resend a specific failed e-mail

```bash
php artisan email:resend 1
```

### Encryption (Optional)

If you wish to encrypt your e-mails, please enable the `encrypt` option in the configuration file. This is disabled by default. Encryption and decryption will be handled by Laravel's built-in encryption mechanism. Please note that by encrypting the e-mail it takes more disk space.

```text
Without encryption

7    bytes (label)
16   bytes (recipient)
20   bytes (subject)
48   bytes (view name)
116  bytes (variables)
1874 bytes (e-mail content)
4    bytes (attempts, sending, failed, encrypted)
57   bytes (created_at, updated_at, deleted_at)
... x 10.000 rows = ± 21.55 MB

With encryption the table size is ± 50.58 MB.
```

### Test mode (Optional)

When enabled, all newly created e-mails will be sent to the specified test e-mail address. This is turned off by default.

### E-mails to send per minute

To configure how many e-mails should be sent each command, please check the `limit` option. The default is `20` e-mails every command.