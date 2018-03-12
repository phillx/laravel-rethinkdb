# RethinkDB for Lumen 5.6

    This package is fork of duxet/laravel-rethinkdb with fix and how-to for Lumen 5.6 

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
# Migration

## Create a Migration File

You can easily create a migration file using the following command which will create a migration file for you to create the users table and use the package schema instead of Laravel schema:

`php artisan make:rethink-migration Users --create`

Please note that you can use the same options that you use in `make:migration` with `make:rethink-migration`, as its based on laravel `make:migration`

Be aware that Laravel Schema API is not fully implemented.  For example, ID columns using increments will not be auto-incremented unsigned integers, and will instead be a UUID unless explicitly set.  The easiest solution is to maintain UUID use within RethinkDB, turn off incremental IDs in Laravel, and finally implement UUID use in Laravel.


## Running The Migrations

Nothing will change here, you will keep using the same laravel commands which you are used to execute to run the migration.

## Example of Laravel Users Migration file

This is an example of how the laravel Users Migration file has become

	<?php

	use Phillx\Rethinkdb\Schema\Blueprint;
	use Illuminate\Database\Migrations\Migration;

	class CreateUsersTable extends Migration
	{
	    /**
	     * Run the migrations.
	     *
	     * @return void
	     */
	    public function up()
	    {
	        Schema::create('users', function (Blueprint $table) {
	            $table->increments('id');
	            $table->string('name');
	            $table->string('email')->unique();
	            $table->string('password', 60);
	            $table->rememberToken();
	            $table->timestamps();
	        });
	    }

	    /**
	     * Reverse the migrations.
	     *
	     * @return void
	     */
	    public function down()
	    {
	        Schema::drop('users');
	    }
	}


# Model

## Create a Model Class

You can easily create a model class using the following command which will create it for you and use the package model instead of Laravel model:

`php artisan make:rethink-model News`

Please note that you can use the same options that you use in `make:model` with `make:rethink-model`, as its based on laravel `make:model`

## Example of Laravel News Model Class

This is an example of how the laravel model class has become

	<?php

	namespace App;

	use \Phillx\Rethinkdb\Eloquent\Model;

	class News extends Model
	{
	    //
	}

## Update a Model Class

Be aware that any model that Laravel generates during its initial installation will need to be updated manually in order for them to work properly.  For example, the User model extends `Illuminate\Foundation\Auth\User`, which further extends `Illuminate\Database\Eloquent\Model` instead of `\duxet\Rethinkdb\Eloquent\Model;`. The import `Illuminate\Foundation\Auth\User` needs to be removed from the User model and replaced with `\duxet\Rethinkdb\Eloquent\Model;`, and any interfaces and associated traits implemented in `Illuminate\Foundation\Auth\User` that are required will need to be ported to the User model.

## Example of Laravel User Model Class

This is an example of how the laravel User model class has become

```
use Illuminate\Auth\Authenticatable;
use \Phillx\Rethinkdb\Eloquent\Model;
use Illuminate\Auth\Passwords\CanResetPassword;
use Illuminate\Foundation\Auth\Access\Authorizable;
use Illuminate\Contracts\Auth\Authenticatable as AuthenticatableContract;
use Illuminate\Contracts\Auth\Access\Authorizable as AuthorizableContract;
use Illuminate\Contracts\Auth\CanResetPassword as CanResetPasswordContract;

class User extends Model implements
    AuthenticatableContract,
    AuthorizableContract,
    CanResetPasswordContract
{
    use Authenticatable, Authorizable, CanResetPassword;
    
    //
}
```
