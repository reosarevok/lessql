# LessQL

LessQL is a thin but powerful data access layer for SQL databases using PDO (PHP Data Objects).
It provides an intuitive API for efficient traversal of related database tables.

Inspired mainly by NotORM, it was written from scratch to provide a clean API and simplified concepts.

http://lessql.net


## Features

- Traverse related tables with a minimal amount of queries
- Save related structures with one method call
- Convention over configuration
- Work closely to your database: LessQL is not an ORM
- Does not attempt to analyze the database, instead relies on conventions and minimal user hints
- Clean, readable source code so forks and extensions are easy to develop
- Fully tested with MySQL and SQLite3
- MIT license

For full documentation and examples, see the [homepage](http://lessql.net).


## Quick Tour

Traversing related tables efficiently is a killer feature.
The following example only needs four queries (one for each table) to retrieve the data:

```php
$pdo = new \PDO( 'sqlite:blog.sqlite3' );
$db = new \LessQL\Database( $pdo );

foreach ( $db->post()->where( 'is_published', 1 )
		->order( 'date_published', 'DESC' ) as $post ) {

	$author = $post->author()->fetch();

	foreach ( $post->categorizationList()->category() as $category ) {

		// ...

	}

	// ...

}
```

Saving is also a breeze. Row objects can be saved with all its associated structures in one call.
For instance, you can create a `Row` from a plain array and save it:

```php
$row = $db->createRow( 'user', array(
	'name' => 'GitHub User',
	'address' => array(
		'location' => 'Berlin',
		'street' => '...'
	)
);

$row->save(); // creates a user, an address and connects them via user.address_id
```


## Status

LessQL has not been used in production yet, but it's fully tested.
It is therefore currently in beta status.

If you want to contribute, please do! Feedback is welcome, too.


## Installation

LessQL requires at least PHP 5.3 and PDO.
The composer package name is `morris/lessql`.
You can also download or fork the repository.


## Tests

Run `composer update` in the `lessql` directory.
This will install development dependencies like PHPUnit.
Run the tests with `vendor/bin/phpunit tests`.