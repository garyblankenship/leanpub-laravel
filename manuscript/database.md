# Basic Database Usage {#database}

- [Configuration](#database-configuration)
- [Running Queries](#running-queries)
- [Database Transactions](#database-transactions)
- [Accessing Connections](#accessing-connections)
- [Query Logging](#query-logging)

## Configuration {#database-configuration}

Laravel makes connecting with databases and running queries extremely simple. The database configuration file is `app/config/database.php`. In this file you may define all of your database connections, as well as specify which connection should be used by default. Examples for all of the supported database systems are provided in this file.

Currently Laravel supports four database systems: MySQL, Postgres, SQLite, and SQL Server.

## Running Queries {#running-queries}

Once you have configured your database connection, you may run queries using the `DB` class.

**Running A Select Query**

	$results = DB::select('select * from users where id = ?', array(1));

The `select` method will always return an `array` of results.

**Running An Insert Statement**

	DB::insert('insert into users (id, name) values (?, ?)', array(1, 'Dayle'));

**Running An Update Statement**

	DB::update('update users set votes = 100 where name = ?', array('John'));

**Running A Delete Statement**

	DB::delete('delete from users');

> **Note:** The `update` and `delete` statements return the number of rows affected by the operation.

**Running A General Statement**

	DB::statement('drop table users');

You may listen for query events using the `DB::listen` method:

**Listening For Query Events**

	DB::listen(function($sql, $bindings, $time)
	{
		//
	});

## Database Transactions {#database-transactions}

To run a set of operations within a database transaction, you may use the `transaction` method:

	DB::transaction(function()
	{
		DB::table('users')->update(array('votes' => 1));

		DB::table('posts')->delete();
	});

## Accessing Connections {#accessing-connections}

When using multiple connections, you may access them via the `DB::connection` method:

	$users = DB::connection('foo')->select(...);

You may also access the raw, underlying PDO instance:

	$pdo = DB::connection()->getPdo();

Sometimes you may need to reconnect to a given database:

	DB::reconnect('foo');

## Query Logging {#query-logging}

By default, Laravel keeps a log in memory of all queries that have been run for the current request. However, in some cases, such as when inserting a large number of rows, this can cause the application to use excess memory. To disable the log, you may use the `disableQueryLog` method:

	DB::connection()->disableQueryLog();