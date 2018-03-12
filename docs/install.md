# About

    This package is fork of duxet/laravel-rethinkdb with fix and how to for Lumen 5.6 

# Installation

## Requirements.

1. Rethinkdb : You need to make sure that you have installed [rethinkdb](http://www.rethinkdb.com) successfully, you can reffer to rethinkdb [documentation](https://rethinkdb.com/docs/) for the full instruction of how to install rethinkdb.

1. Lumen 5.6 

## Installation

Composer command to install:

`composer require "phillx/lumen-rethinkdb:dev-master"`

This will install the package and all other required packages needed to work.

## Service Provider

After you install the library you will need to register the `Service Provider` file in your `bootstrap/app.php` file like :

    $app->register(Phillx\Rethinkdb\RethinkdbServiceProvider::class);
    $app->withEloquent();


## Database configuration

Now you need to create file `config/database.php` with content like:


      <?php
      return [
          'default' => 'rethinkdb',
          'connections' => [
                  'rethinkdb' => [
                      'name'      => 'rethinkdb',
                      'driver'    => 'rethinkdb',
                      'host'      => env('DB_HOST', 'localhost'),
                      'port'      => env('DB_PORT', 28015),
                      'database'  => env('DB_DATABASE', 'homestead'),
                  ]
              ],
          'migrations' => 'migrations',
      ];

After you create it, you can just edit your `.env` file:

	DB_HOST=localhost
	DB_DATABASE=rethinkdb_name
	DB_CONNECTION=rethinkdb

but you can always update your `DB_HOST` to point to the IP where you have installed rethinkdb.