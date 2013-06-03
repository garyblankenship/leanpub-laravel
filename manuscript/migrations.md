# Migrations & Seeding {#migrations}

- [Introduction](#migrations-introduction)
- [Creating Migrations](#creating-migrations)
- [Running Migrations](#running-migrations)
- [Rolling Back Migrations](#rolling-back-migrations)
- [Database Seeding](#database-seeding)

## Introduction {#migrations-introduction}

Migrations are a type of version control for your database. They allow a team to modify the database schema and stay up to date on the current schema state. Migrations are typically paired with the [Schema Builder](#schema) to easily manage your application's scheme.

## Creating Migrations {#creating-migrations}

To create a migration, you may use the `migrate:make` command on the Artisan CLI:

**Creating A Migration**

	php artisan migrate:make create_users_table

The migration will be placed in your `app/database/migrations` folder, and will contain a timestamp which allows the framework to determine the order of the migrations.

You may also specify a `--path` option when creating the migration. The path should be relative to the root directory of your installation:

	php artisan migrate:make foo --path=app/migrations

The `--table` and `--create` options may also be used to indicate the name of the table, and whether the migration will be creating a new table:

	php artisan migrate:make create_users_table --table=users --create

## Running Migrations {#running-migrations}

**Running All Outstanding Migrations**

	php artisan migrate

**Running All Outstanding Migrations For A Path**

	php artisan migrate --path=app/foo/migrations

**Running All Outstanding Migrations For A Package**

	php artisan migrate --package=vendor/package

> **Note:** If you receive a "class not found" error when running migrations, try running the `composer update` command.

## Rolling Back Migrations {#rolling-back-migrations}

**Rollback The Last Migration Operation**

	php artisan migrate:rollback

**Rollback all migrations**

	php artisan migrate:reset

**Rollback all migrations and run them all again**

	php artisan migrate:refresh

	php artisan migrate:refresh --seed

## Database Seeding {#database-seeding}

Laravel also includes a simple way to seed your database with test data using seed classes. All seed classes are stored in `app/database/seeds`. Seed classes may have any name you wish, but probably should follow some sensible convention, such as `UserTableSeeder`, etc. By default, a `DatabaseSeeder` class is defined for you. From this class, you may use the `call` method to run other seed classes, allowing you to control the seeding order.

**Example Database Seed Class**

	class DatabaseSeeder extends Seeder {

		public function run()
		{
			$this->call('UserTableSeeder');

			$this->command->info('User table seeded!');
		}

	}

	class UserTableSeeder extends Seeder {

		public function run()
		{
			DB::table('users')->delete();

			User::create(array('email' => 'foo@bar.com'));
		}

	}

To seed your database, you may use the `db:seed` command on the Artisan CLI:

	php artisan db:seed

You may also seed your database using the `migrate:refresh` command, which will also rollback and re-run all of your migrations:

	php artisan migrate:refresh --seed
