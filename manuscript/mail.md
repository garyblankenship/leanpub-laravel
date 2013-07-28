# Mail {#mail}

- [Configuration](#mail-configuration)
- [Basic Usage](#mail-basic-usage)
- [Embedding Inline Attachments](#embedding-inline-attachments)
- [Queueing Mail](#queueing-mail)
- [Mail & Local Development](#mail-and-local-development)

## Configuration {#mail-configuration}

Laravel provides a clean, simple API over the popular [SwiftMailer](http://swiftmailer.org) library. The mail configuration file is `app/config/mail.php`, and contains options allowing you to change your SMTP host, port, and credentials, as well as set a global `from` address for all messages delivered by the library. You may use any SMTP server you wish. If you wish to use the PHP `mail` function to send mail, you may change the `driver` to `mail` in the configuration file. A `sendmail` driver is also available.

## Basic Usage {#mail-basic-usage}

The `Mail::send` method may be used to send an e-mail message:

	Mail::send('emails.welcome', $data, function($message)
	{
		$message->to('foo@example.com', 'John Smith')->subject('Welcome!');
	});

The first argument passed to the `send` method is the name of the view that should be used as the e-mail body. The second is the `$data` that should be passed to the view, and the third is a Closure allowing you to specify various options on the e-mail message.

> **Note:** A `$message` variable is always passed to e-mail views, and allows the inline embedding of attachments. So, it is best to avoid passing a `message` variable in your view payload.

You may also specify a plain text view to use in addition to an HTML view:

	Mail::send(array('html.view', 'text.view'), $data, $callback);

Or, you may specify only one type of view using the `html` or `text` keys:

	Mail::send(array('text' => 'view'), $data, $callback);

You may specify other options on the e-mail message such as any carbon copies or attachments as well:

	Mail::send('emails.welcome', $data, function($message)
	{
		$message->from('us@example.com', 'Laravel');

		$message->to('foo@example.com')->cc('bar@example.com');

		$message->attach($pathToFile);
	});

When attaching files to a message, you may also specify a MIME type and / or a display name:

	$message->attach($pathToFile, array('as' => $display, 'mime' => $mime));

> **Note:** The message instance passed to a `Mail::send` Closure extends the SwiftMailer message class, allowing you to call any method on that class to build your e-mail messages.

## Embedding Inline Attachments {#embedding-inline-attachments}

Embedding inline images into your e-mails is typically cumbersome; however, Laravel provides a convenient way to attach images to your e-mails and retrieving the appropriate CID.

**Embedding An Image In An E-Mail View**

	<body>
		Here is an image:

		<img src="<?php echo $message->embed($pathToFile); ?>">
	</body>

**Embedding Raw Data In An E-Mail View**

	<body>
		Here is an image from raw data:

		<img src="<?php echo $message->embedData($data, $name); ?>">
	</body>

Note that the `$message` variable is always passed to e-mail views by the `Mail` class.

## Queueing Mail {#queueing-mail}

Since sending e-mail messages can drastically lengthen the response time of your application, many developers choose to queue e-mail messages for background sending. Laravel makes this easy using its built-in [unified queue API](#queues). To queue a mail message, simply use the `queue` method on the `Mail` class:

**Queueing A Mail Message**

	Mail::queue('emails.welcome', $data, function($message)
	{
		$message->to('foo@example.com', 'John Smith')->subject('Welcome!');
	});

You may also specify the number of seconds you wish to delay the sending of the mail message using the `later` method:

	Mail::later(5, 'emails.welcome', $data, function($message)
	{
		$message->to('foo@example.com', 'John Smith')->subject('Welcome!');
	});

If you wish to specify a specific queue or "tube" on which to push the message, you may do so using the `queueOn` and `laterOn` methods:

	Mail::queueOn('queue-name', 'emails.welcome', $data, function($message)
	{
		$message->to('foo@example.com', 'John Smith')->subject('Welcome!');
	});

## Mail & Local Development {#mail-and-local-development}

When developing an application that sends e-mail, it's usually desirable to disable the sending of messages from your local or development environment. To do so, you may either call the `Mail::pretend` method, or set the `pretend` option in the `app/config/mail.php` configuration file to `true`. When the mailer is in `pretend` mode, messages will be written to your application's log files instead of being sent to the recipient.

**Enabling Pretend Mail Mode**

  Mail::pretend();