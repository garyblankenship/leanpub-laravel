# Forms & HTML {#html}

- [Opening A Form](#opening-a-form)
- [CSRF Protection](#csrf-protection)
- [Form Model Binding](#form-model-binding)
- [Labels](#labels)
- [Text, Text Area, Password & Hidden Fields](#text)
- [Checkboxes and Radio Buttons](#checkboxes-and-radio-buttons)
- [File Input](#file-input)
- [Drop-Down Lists](#drop-down-lists)
- [Buttons](#buttons)
- [Custom Macros](#custom-macros)

## Opening A Form {#opening-a-form}

**Opening A Form**

	{{ Form::open(array('url' => 'foo/bar')) }}
		//
	{{ Form::close() }}

By default, a `POST` method will be assumed; however, you are free to specify another method:

	echo Form::open(array('url' => 'foo/bar', 'method' => 'put'))

> **Note:** Since HTML forms only support `POST` and `GET`, `PUT` and `DELETE` methods will be spoofed by automatically adding a `_method` hidden field to your form.

You may also open forms that point to named routes or controller actions:

	echo Form::open(array('route' => 'route.name'))

	echo Form::open(array('action' => 'Controller@method'))

You may pass in route parameters as well:

  echo Form::open(array('route' => array('route.name', $user->id)))

  echo Form::open(array('action' => array('Controller@method', $user->id)))

If your form is going to accept file uploads, add a `files` option to your array:

	echo Form::open(array('url' => 'foo/bar', 'files' => true))

## CSRF Protection {#csrf-protection}

Laravel provides an easy method of protecting your application from cross-site request forgeries. First, a random token is placed in your user's session. Don't sweat it, this is done automatically. The CSRF token will be added to your forms as a hidden field automatically. However, if you wish to generate the HTML for the hidden field, you may use the `token` method:

**Adding The CSRF Token To A Form**

	echo Form::token();

**Attaching The CSRF Filter To A Route**

	Route::post('profile', array('before' => 'csrf', function()
	{
		//
	}));

## Form Model Binding {#form-model-binding}

Often, you will want to populate a form based on the contents of a model. To do so, use the `Form::model` method:

**Opening A Model Form**

	echo Form::model($user, array('route' => array('user.update', $user->id)))

Now, when you generate a form element, like a text input, the model's value matching the field's name will automatically be set as the field value. So, for example, for a text input named `email`, the user model's `email` attribute would be set as the value. However, there's more! If there is an item in the Session flash data matching the input name, that will take precedence over the model's value. So, the priority looks like this:

1. Session Flash Data (Old Input)
2. Explicitly Passed Value
3. Model Attribute Data

This allows you to quickly build forms that not only bind to model values, but easily re-populate if there is a validation error on the server!

> **Note:** When using `Form::model`, be sure to close your form with `Form::close`!

## Labels {#labels}

**Generating A Label Element**

	echo Form::label('email', 'E-Mail Address');

**Specifying Extra HTML Attributes**

	echo Form::label('email', 'E-Mail Address', array('class' => 'awesome'));

> **Note:** After creating a label, any form element you create with a name matching the label name will automatically receive an ID matching the label name as well.

## Text, Text Area, Password & Hidden Fields {#text}

**Generating A Text Input**

	echo Form::text('username');

**Specifying A Default Value**

	echo Form::text('email', 'example@gmail.com');

> **Note:** The *hidden* and *textarea* methods have the same signature as the *text* method.

**Generating A Password Input**

	echo Form::password('password');

**Generating Other Inputs**

  echo Form::email($name, $value = null, $attributes = array());
  echo Form::file($name, $attributes = array());

## Checkboxes and Radio Buttons {#checkboxes-and-radio-buttons}

**Generating A Checkbox Or Radio Input**

	echo Form::checkbox('name', 'value');

	echo Form::radio('name', 'value');

**Generating A Checkbox Or Radio Input That Is Checked**

	echo Form::checkbox('name', 'value', true);

	echo Form::radio('name', 'value', true);

## File Input {#file-input}

**Generating A File Input**

	echo Form::file('image');

## Drop-Down Lists {#drop-down-lists}

**Generating A Drop-Down List**

	echo Form::select('size', array('L' => 'Large', 'S' => 'Small'));

**Generating A Drop-Down List With Selected Default**

	echo Form::select('size', array('L' => 'Large', 'S' => 'Small'), 'S');

**Generating A Grouped List**

	echo Form::select('animal', array(
		'Cats' => array('leopard' => 'Leopard'),
		'Dogs' => array('spaniel' => 'Spaniel'),
	));

## Buttons {#buttons}

**Generating A Submit Button**

	echo Form::submit('Click Me!');

> **Note:** Need to create a button element? Try the *button* method. It has the same signature as *submit*.

## Custom Macros {#custom-macros}

It's easy to define your own custom Form class helpers called "macros". Here's how it works. First, simply register the macro with a given name and a Closure:

**Registering A Form Macro**

	Form::macro('myField', function()
	{
		return '<input type="awesome">';
	});

Now you can call your macro using its name:

**Calling A Custom Form Macro**

	echo Form::myField();
