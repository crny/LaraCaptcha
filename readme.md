# LaraCaptcha Bundle

## Installation

	php artisan bundle:install laracaptcha

Note: **LaraCaptcha requires to have set a Session Driver**. Check /application/config/session.php. I recommend you to set **'driver' => 'file'**, especially for development. Setting "'driver' => 'cookie'" on localhost may cause domain-related problems.

## Bundle Registration

Add the following to your **application/bundles.php** file:

	'laracaptcha' => array('auto' => true, 'handles' => 'captcha'),

## Usage

In **application\routes.php** place something like:

	// on "get" we display /views/layouts/register.php, which contains our registration form
	Route::get('register', function()
	{
		return View::make('layouts.register');
	});

	// on "post" we validate the input
	Route::post('register', function()
	{
		$rules = array(
			'captcha' => 'laracaptcha|required'
		);
		$messages = array(
			'laracaptcha' => 'Invalid captcha',
		);

		$validation = Validator::make(Input::all(), $rules, $messages);

		if ($validation->valid())
		{
			// valid captcha
		} else
		{
			return Redirect::to('register')->with_errors($validation);
		}
	});

Feel free to add to the above code other validation rules according to your application.

Next, in your view (say, **/views/layouts/register.php**), place something like:

	echo Form::open('register', 'POST', array('class' => 'register_form'));
	... [other fields] ...
	<?php echo Form::text('captcha', '', array('class' => 'captcha', 'placeholder' => 'Insert captcha...')); ?>
	**<img src="<?php echo LaraCaptcha\Captcha::img(); ?>" alt="" />**
	... [other fields] ...
	echo Form::close();

## Customisation

You can configure the font, size, alphabet & more in **config/config.php**. Each line of the config is thoroughly documented.

## Further information
This bundle is maintained by Vlad Rusu (vlad.rusu@gmail.com). If you have any questions, email me. You can always grab the latest version from http://github.com/vladrusu/laracaptcha