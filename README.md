# BadRouter

A minimalist router inspired by Ruby's Sinatra router.

Visit the official documentation page here: https://badrouter.badluck.io

# Install
**1.** Run `composer require badluck/badrouter` in your project directory to install the package and its dependencies.

**2.** Require the autoloader. At the beginning of your PHP file, require the Composer autoloader by adding this line of code:
```php
require_once('./vendor/autoload.php');
```

**3.** Use the `Router` static class.

You can use `::get`, `::post`, `::put`, and `::delete`. BadRouter supports dynamic routes in `{braces}`.
```php
Router::get('/user/{id}', function ($id) {
  $locals = [
    "user_id" => $id
  ];
  Router::render('/user', $locals);
});
```

**4.** Configure the view and public directories.

The `public` directory is where the router will search for static files if the URI is not a route.
The `views` directry is where `Router::render` will search when rendering pages.
```php
Router::set_public('public');
Router::set_views('views');
```

**5.** After the router is configured, run it.
```php
Router::run();
```

# Full Usage example
```php
<?php
require_once('./vendor/autoload.php');

use BadRouter\Router;

Router::use(function() {
  // Middleware
});

// Setup routes
Router::get('/', function() {
  $locals = [
    'message' => 'Hello world!'
  ];

  Router::render('/home', $locals);
});

Router::get('/login', function() {
  Router::render('/login', [], null);
});

Router::get('/about', function() {
  $locals = [
    'message' => 'We are awesome!',
  ];

  Router::render('/about', $locals);
});

Router::get('/message', function() {
  Router::set_content_type('json');
  Router::json(['message' => 'About Us']);
});

Router::get('/redirectMe', function() {
  Router::redirect('/about');
});

Router::get('/user/{id}', function ($id) {
  $locals = [
    "id" => $id
  ];
  Router::render('/user', $locals);
});

// Restricted /admin route
function restricted() {
  if (!isset($_SESSION['user'])) {
    Router::redirect('/login');
    return false;
  }
}

Router::get('/admin', function() {
  restricted();
  Router::render('/admin/page');
});

// POST
Router::post('/api/login', function() {
  Router::json([
    'success' => true,
  ]);
});

// Set 404 page
Router::set_error(404, function() {
  $locals = [
    'route' => parse_url($_SERVER['REQUEST_URI'], PHP_URL_PATH),
  ];
  Router::render('/404', $locals, null);
});

// Configure directories
Router::set_public('public');
Router::set_views('views');

// Run
Router::run();
```

# Change Log
Log types:
* Added
* Changed
* Fixed
* Moved
* Removed
* Renamed
* Updated

## v0.4.5 (2023-07-26)
* Added styling rules

## v0.4.4 (2023-05-03)
* Fixed bug with fetching views using relative directory paths

## v0.4.3 (2023-05-03)
* Fixed incorrect Content-Type for 404

## v0.4.2 (2023-05-03)
* Fixed bug in URI parsing

## v0.4.1 (2023-05-02)
* Fixed file search path missing a slash

## v0.4.0 (2023-05-01)
* Added 74 MIME types
* Changed static files automatically fetched and no longer need PUBLIC_PATH to be specified

## v0.3.4 (2023-04-26)
* Removed Router::json datatype for input

## v0.3.3 (2023-04-24)
* Fixed warning `$server['HTTP_REFERER']` with null coalescence

## v0.3.2 (2023-04-18)
* Fixed Composer PHP requirements from "^7.4.33" to ">=7.4.33"

## v0.3.1 (2023-04-17)
* Fixed route not checking against the `Request` object
* Fixed `Router::render` `$layout` param not being nullable

## v0.3.0 (2023-04-17)
* Added class `Request`
* Added variable type specifications to method params
* Added variable type to method returns
* Updated middleware `use` to specify `Closure(Request)` type
* Updated documentation to include change logs

## v0.2.5 (2023-04-13)
* Updated documentation to mention https://badrouter.badluck.io

## v0.2.4 (2023-04-13)
* Fixed (another) bug in `Router::set_base_path`
* Moved example website from this package and into its own repo at https://github.com/BadLuckSoftware/BadRouter-Example
* Updated `$request` fields to be `null` if `$_SERVER` field was `null`
* Updated `Router::redirect` to use `BASE_PATH` instead of `self::$base_path`

## v0.2.3 (2023-04-13)
* Added GitHub Action to deploy example to https://badrouter.badluck.io
* Fixed bug in `Router::set_base_path`

## v0.2.2 (2023-04-13)
* Added `$request` parameter to middleware
* Added `VIEWS_PATH` global variable
* Added `Router::json` method
* Renamed directory `example_website` to `example`
* Updated example website to reflect the current features of `BadRouter`
* Updated `Router::set_base_path` to default empty paths to '`/`'

## v0.2.1 (meow)
* The cow goes moo

## v0.2.0 (2023-04-11)
* Added `BASE_PATH` global variable
* Added `Router::set_base_path` method
* Added Composer keywords
* Changed `PUBLIC_DIR` global variable to `PUBLIC_PATH`
* Moved `BadRouter` repo to `BadLuckSoftware` under `badluck` namespace
* Updated Composer description

## v0.1.3 (2023-04-06)
* Updated documentation

## v0.1.2 (2023-04-05)
* Renamed file `BadRouter.php` to `Router.php`
* Updated example in readme
* Updated example website to use the correct Router name

## v0.1.1 (2023-04-05)
* Added forgotten namespace
* Renamed class `BadRouter` to `Router`

## v0.1.0 (2023-04-05)
* Added initial BadRouter Composer package
