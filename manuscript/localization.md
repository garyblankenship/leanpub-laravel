# Localization {#localization}

- [Introduction](#localization-introduction)
- [Language Files](#language-files)
- [Basic Usage](#localization-basic-usage)
- [Pluralization](#pluralization)

## Introduction {#localization-introduction}

The Laravel `Lang` class provides a convenient way of retrieving strings in various languages, allowing you to easily support multiple languages within your application.

## Language Files {#language-files}

Language strings are stored in files within the `app/lang` directory. Within this directory there should be a subdirectory for each language supported by the application.

	/app
		/lang
			/en
				messages.php
			/es
				messages.php

Language files simply return an array of keyed strings. For example:

**Example Language File**

	<?php

	return array(
		'welcome' => 'Welcome to our application'
	);

The default language for your application is stored in the `app/config/app.php` configuration file. You may change the active language at any time using the `App::setLocale` method:

**Changing The Default Language At Runtime**

	App::setLocale('es');

## Basic Usage {#localization-basic-usage}

**Retrieving Lines From A Language File**

	echo Lang::get('messages.welcome');

The first segment of the string passed to the `get` method is the name of the language file, and the second is the name of the line that should be retrieved.

> **Note**: If a language line does not exist, the key will be returned by the `get` method.

**Making Replacements In Lines**

You may also define place-holders in your language lines:

	'welcome' => 'Welcome, :name',

Then, pass a second argument of replacements to the `Lang::get` method:

	echo Lang::get('messages.welcome', array('name' => 'Dayle'));

**Determine If A Language File Contains A Line**

	if (Lang::has('messages.welcome'))
	{
		//
	}

## Pluralization {#pluralization}

Pluralization is a complex problem, as different languages have a variety of complex rules for pluralization. You may easily manage this in your language files. By using a "pipe" character, you may separate the singular and plural forms of a string:

	'apples' => 'There is one apple|There are many apples',

You may then use the `Lang::choice` method to retrieve the line:

	echo Lang::choice('messages.apples', 10);

Since the Laravel translator is powered by the Symfony Translation component, you may also create more explicit pluralization rules easily:

	'apples' => '{0} There are none|[1,19] There are some|[20,Inf] There are many',
