Laravel Modules
===============

[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/neilherbertuk/modules/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/neilherbertuk/modules/?branch=master) [![Build Status](https://scrutinizer-ci.com/g/neilherbertuk/modules/badges/build.png?b=master)](https://scrutinizer-ci.com/g/neilherbertuk/modules/build-status/master)

A package to add modules to a [Laravel](https://laravel.com/) 5 application. This package allows you to separate out code for parts of your application into their own dedicated "modules" or folders, allowing all code related to a specific section or function of your application to be stored in one place.

Compatible with:
 - Laravel 5.4
 - Laravel 5.5
 
Currently supports:
 - Controllers
 - Migrations
 - Models
 - Routes, web and api
 - Service Providers
 - Views

Currently not supported:
 - Database Seeding
 - Module Middleware

This package will soon have commands to assist in making each of these.
 
## Example
An admin panel at *domain.com/admin* - all functionality related to the admin panel could be turned into a module and stored together in a single location.

Example Structure

    Admin
     - Admin/controllers
     - - Admin/controllers/AdminController.php
     - Admin/database
     - - Admin/database/migrations
     - - - Admin/database/migrations/Create_A_Table_Migrtion.php
     - Admin/models
     - - Admin/models/statistics.php
     - Admin/views
     - - Admin/views/dashboard.blade.php
     - Admin/web.php

## Installation

This version has been tested with Laravel 5.4 and 5.5, other versions will be tested in the future.

```bash
$ composer require neilherbertuk/laravel-modules:dev-master
```

### Laravel 5.4
If you are using Laravel 5.4, you will need to register the service provider in your `config/app.php`

```php
        neilherbertuk\modules\ModuleServiceProvider::class,
```

### Laravel 5.5
If you are using Laravel 5.5 or up, Automatic Package Discovery should autoload the `ModuleServiceProvider`, if this does not happen run the following commands from the root of your laravel project:

```bash
$ composer dump-autoload
$ php artisan package:discover
```

### Configuration

If you would like to make changes to the default configuration, publish the package's config file.

```bash
$ php artisan vendor:publish --provider="neilherbertuk\modules\ModuleServiceProvider" --tag=config
```

This will create a `config/modules.php` file in your app that you can modify to set your configuration.

The package can be configured to work in several ways. By default the package will autoload modules under the `app\Modules` folder.

In my opinion (correct me if I am wrong) auto-loading is great in development, but not recommended in production due to the expensive nature of finding each available module.

#### Enable Autoload

This is the default behaviour, but can be added to your `.env` file

```dotenv
MODULES_AUTOLOAD=true
```
#### Disable Autoload

Add the following to your `.env` file

```dotenv
MODULES_AUTOLOAD=false
```

#### Manually Loading Modules

Manual loading of modules will occur when Autoload is disabled. 
Within your `config/modules.php` file you will find a blank `enabled` array. Simply add the name of each module you wish to manually load as a new entry. Each name is the name of the folder your module sits in.

```php
'enabled' => [
                'Admin',
                'Forum'
            ],
```

#### Disabling Modules

When using Autoloading, it is possible to disable certain modules. Within your `config/modules.php` file is a blank `disabled` array. Any modules listed here will not be loaded.

```php
'disabled' => [
                'Admin'
            ],
```

### Usage

To be completed.

The Laravel-Modules package comes with a handy console command to help build new modules.
It's usage can be seen by running the following command:

```bash
    $ php artisan make:module
```

#### Creating a New Module

To create a new module, run the following command from the root of your Laravel project

```bash
    $ php artisan make:module --create ModuleName [--webroute | --apiroute]
```

This will create a new folder structure as shown in the example above within your `app/Module` folder.

##### New Module Routes #####
Unless an option is given, by default, the create command will also create a web routes file (`web.php`).
The routes file generated by this will include a group prefix to help separate your module from other parts of your application.   

If you would prefer to create an api route file (`api.php`) add `--apiroute` to the command.

```bash
    $ php artisan make:module --create ModuleName --apiroute
```

If you would like to add both a web and api routes file, add both `--webroute` and `--apiroute` to the create command,
 alternatively you can use these options without `--create` if the module has already been created.

```bash
    $ php artisan make:module --create ModuleName --apiroute --webroute
```  

The module name will automatically be converted to lower case and used as a prefix for any routes file created.
If you create a new module named "Dashboard", anything within the module will be available at *domain.com/dashboard/*

##### New Module Controller #####
You can create a new module controller using the --controller option, optionally you can also include --resource

```bash
    $ php artisan make:module ModuleName --controller NameOfController [--resource]
```

This will place a new controller into your ```app\Modules\ModuleName\Controllers``` folder. By default this will create a plain controller.
If you would like to create a resource controller add --resource which will include all of the basic CRUD methods for you.

#####  Module Namespacing #####
All files related to the module must belong to the same namespace. This is done automatically for you if you use the provided console commands.

```
namespace app\Modules\ModuleName\Controllers;
namespace app\Modules\ModuleName\Models;
namespace app\Modules\ModuleName\Migrations;
```

### TODO
 - [ ] Working On - Complete Documentation - Usage Section
 - [ ] Working On - Create commands to easily make modules and various parts such as controllers and views within a module.
 - [ ] Working On - Create example project
 - [ ] Working On - Test support for other versions of Laravel 5
 - [ ] Create Unit Tests

## Bugs
Please report any bugs by opening an issue on [github](https://github.com/neilherbertuk/modules/issues).

## Security Issues
Please email any security issues directly to neil@ea3.co.uk.
