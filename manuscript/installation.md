# Installation {#installation}

- [Install Composer](#install-composer)
- [Install Laravel](#install-laravel)
- [Server Requirements](#server-requirements)
- [Configuration](#configuration)
- [Pretty URLs](#pretty-urls)

<a name="install-composer"></a>
## Install Composer

Laravel utilizes [Composer](http://getcomposer.org) to manage its dependencies. First, download a copy of the `composer.phar`. Once you have the PHAR archive, you can either keep it in your local project directory or move to `usr/local/bin` to use it globally on your system. On Windows, you can use the Composer [Windows installer](https://getcomposer.org/Composer-Setup.exe).

<a name="install-laravel"></a>
## Install Laravel

Once Composer is installed, download the [latest version](https://github.com/laravel/laravel/archive/master.zip) of the Laravel framework and extract its contents into a directory on your server. Next, in the root of your Laravel application, run the `php composer.phar install` (or `composer install`) command to install all of the framework's dependencies. This process requires Git to be installed on the server to successfully complete the installation.

Once Laravel is installed, you may update the framework using the `php composer.phar update` command.

<a name="server-requirements"></a>
## Server Requirements

The Laravel framework has a few system requirements:

- PHP >= 5.3.7
- MCrypt PHP Extension

<a name="configuration"></a>
## Configuration

Laravel needs almost no configuration out of the box. You are free to get started developing! However, you may wish to review the `app/config/app.php` file and its documentation. It contains several options such as `timezone` and `locale` that you may wish to change according to your application.

> **Note:** One configuration option you should be sure to set is the `key` option within `app/config/app.php`. This value should be set to a 32 character, random string. This key is used when encrypting values, and encrypted values will not be safe until it is properly set. You can set this value quickly by using the following artisan command `php artisan key:generate`.

<a name="permissions"></a>
### Permissions
Laravel requires one set of permissions to be configured - folders within app/storage require write access by the web server.

<a name="paths"></a>
### Paths

Several of the framework directory paths are configurable. To change the location of these directories, check out the `bootstrap/paths.php` file.

> **Note:** Laravel is designed to protect your application code, and local storage by placing only files that are necessarily public in the public folder.  It is recommended that you either set the public folder as your site's documentRoot (also known as a web root) or to place the contents of public into your site's root directory and place all of Laravel's other files outside the web root.

<a name="pretty-urls"></a>
## Pretty URLs

The framework ships with a `public/.htaccess` file that is used to allow URLs without `index.php`. If you use Apache to serve your Laravel application, be sure to enable the `mod_rewrite` module.

If the `.htaccess` file that ships with Laravel does not work with your Apache installation, try this one:

	Options +FollowSymLinks
	RewriteEngine On

	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteRule ^(.+)/$ http://%{HTTP_HOST}/$1 [R=301,L]

	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteRule ^ index.php [L]
