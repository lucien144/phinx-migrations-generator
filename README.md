# Phinx migrations generator

[![Latest Version](https://img.shields.io/github/release/odan/phinx-migrations-generator.svg?style=flat-square)](https://github.com/loadsys/odan/phinx-migrations-generator/releases)
[![Crutinizer](https://img.shields.io/scrutinizer/g/odan/phinx-migrations-generator.svg?style=flat-square)](https://scrutinizer-ci.com/g/odan/phinx-migrations-generator)
[![Build Status](https://travis-ci.org/odan/phinx-migrations-generator.svg?branch=master&style=flat-square)](https://travis-ci.org/odan/phinx-migrations-generator)
[![codecov](https://codecov.io/gh/odan/phinx-migrations-generator/branch/master/graph/badge.svg?style=flat-square)](https://codecov.io/gh/odan/phinx-migrations-generator)
[![Total Downloads](https://img.shields.io/packagist/dt/odan/phinx-migrations-generator.svg?style=flat-square)](https://packagist.org/packages/odan/phinx-migrations-generator)
[![Repo Size](https://reposs.herokuapp.com/?path=odan/phinx-migrations-generator&style=flat-square)](https://reposs.herokuapp.com/?path=odan/phinx-migrations-generator)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)

Currently Phinx (a migration library) cannot generate migrations automatically.
Phinx "only" generates a empty class with up and down functions. You still have to write the migration manually.

In reality, you should rarely need to write migrations manually, as the migrations library "should" generate migration classes automatically by comparing your schema mapping information (i.e. what your database should look like) with your actual current database structure.

![Screenshot](https://github.com/odan/phinx-migrations-generator/blob/master/docs/images/screenshot01.jpg "Screenshot")

Generated migration

![Screenshot 2](https://github.com/odan/phinx-migrations-generator/blob/master/docs/images/screenshot02.jpg "Screenshot 2")

THIS IS A DEVELOPMENT PREVIEW - DO NOT USE IT IN PRODUCTION!

# Features

* Framework independent
* DBMS: MySQL
* Initial schema, schema diff
* Database: character set, collation
* Tables: create, update, remove, engine, comment, character set, collation
* Columns: create, update, remove
* Indexes: create, remove
* Foreign keys (experimental): create, remove

### Not supported

* MySQL [double] is not supported by phinx https://github.com/robmorgan/phinx/issues/498
* MySQL [year] is not supported by phinx. https://github.com/robmorgan/phinx/issues/551 | https://github.com/robmorgan/phinx/pull/704
* MySQL [bit] is not supported by phinx. https://github.com/robmorgan/phinx/pull/778
* MySQL enum values with special characters: https://github.com/robmorgan/phinx/issues/887
* MySQL comments with special characters.

# Installation

```
composer require odan/phinx-migrations-generator
```

# Configuration

* Default configuration file: phinx-migrations-config.php

Example:

```php
<?php

return array(
    'dsn' => 'mysql:host=127.0.0.1;dbname=test',
    'username' => 'root',
    'password' => '',
    'options' => array(
        PDO::MYSQL_ATTR_INIT_COMMAND => 'SET NAMES utf8',
    ),
    //'pdo' => new PDO('mysql:host=127.0.0.1;dbname=test', 'username', 'password'),
    'schema_file' => __DIR__ . '/schema.php',
    'migration_path' => __DIR__
);
```

Name | Type | Default | Description
--- | --- | --- | ---
pdo | PDO | null | PDO object
dsn | string |  | Data source name (mysql:host=127.0.0.1;dbname=test)
username | string | | Database username
password | string | | Database password
options | array | | Database options
schema_file | string | schema.php | Database schema file (schema.php or schema.json)
migration_path | string | | Output directory for migration files
foreign_keys | int | 0 | Generate foreign keys (experimental)

# Usage

## Generating migrations

```
cd vendor/bin
phinx-migrations generate
```

## Load custom config file

```
phinx-migrations generate --config=myconfig.php
```

# Console Setup

* Create a console file: bin/phinx-migrations.php

```php
<?php

require_once __DIR__ . '/../vendor/autoload.php';
require_once __DIR__ . '/../vendor/odan/phinx-migrations-generator/bin/phinx-migrations';
```

* Create a config file: bin/phinx-migrations-config.php

```php
<?php

// include framework bootstrap code here
// ...

// Get PDO object (from framework) or create new instance
$pdo = new PDO('mysql:host=127.0.0.1;dbname=test', 'username', 'password'),

// Change this path!
$migrationPath = '/path/to/migrations';
$schemaFile = $migrationPath . '/schema.php';

return array(
    'pdo' => $pdo,
    'schema_file' => $schemaFile,
    'migration_path' => $migrationPath
);
```

* Open the console (cmd) and run:

```
cd bin
php phinx-migrations.php generate
```

## Similar projects

https://github.com/robmorgan/phinx/issues/109#issuecomment-255297913

