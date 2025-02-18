---
subtitle: Learn how to configure the database layer.
---
# Database Configuration

The database configuration for your application is located in the `config/database.php` file. In this file you may define all of your database connections, as well as specify which connection should be used by default. Examples for all of the supported database systems are provided in this file.

## SQLite Configuration

SQLite databases use a single file on your filesystem. To create a new SQLite database, use the `touch` command.

```bash
touch storage/database.sqlite
```

After this you can configure your environment variables to use the database by placing the absolute path in the `DB_DATABASE` variable.

```text
DB_CONNECTION=sqlite
DB_DATABASE=/absolute/path/to/database.sqlite
```

## Read / Write Connections

Sometimes you may wish to use one database connection for SELECT statements, and another for INSERT, UPDATE, and DELETE statements. It is easy to specify which connection is used whether you are using raw queries, the query builder or a model.

To see how read / write connections should be configured, let's look at this example:

```php
'mysql' => [
    'read' => [
        'host' => '192.168.1.1',
    ],
    'write' => [
        'host' => '196.168.1.2'
    ],
    'driver'    => 'mysql',
    'database'  => 'database',
    'username'  => 'root',
    'password'  => '',
    'charset'   => 'utf8',
    'collation' => 'utf8_unicode_ci',
    'prefix'    => '',
],
```

Note that two keys have been added to the configuration array: `read` and `write`. Both of these keys have array values containing a single key: `host`. The rest of the database options for the `read` and `write` connections will be merged from the main `mysql` array.

We only need to place items in the `read` and `write` arrays if we wish to override the values in the main array. So, in this case, `192.168.1.1` will be used as the "read" connection, while `192.168.1.2` will be used as the "write" connection. The database credentials, prefix, character set, and all other options in the main `mysql` array will be shared across both connections.

<a id="oc-index-lengths-using-mysql-mariadb"></a>
## Index Lengths Using MySQL / MariaDB

By default, October CMS uses a `utf8mb4` character set. If running a version of MySQL older than v5.7.7 or MariaDB older than v10.2.2, you'll need to manually configure the default string length generated by migrations in order for MySQL to create indexes for them. To configure the default string length, add the following to your **config/database.php** configuration file under the key `connections.mysql`.

```php
'varcharmax' => 191,
```
