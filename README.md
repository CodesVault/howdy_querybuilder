# WP Query Builder
<p>
Relational Database Query builder for WordPress.
WP Query Builder uses <code>PDO</code> for database queries. It has <strong>zero dependencies</strong> with third-party query builders or any other composer library.
</p>

<br/>

## Dev Envirenment Setup for Contributors
Want to contribute to this package? Please follow the steps below.

<ul>
    <li>Create a local WordPress envirenment setup.</li>
    <li>Create a basic plugin.</li>
    <li>Run <code>composer init</code> into the plugin.</li>
    <li>Clone <code>git@github.com:CodesVault/wp_querybuilder.git</code> into vendor folder.</li>
    <li>
        Add repository for local package in plugin's <code>composer.json</code>.
        <pre>
        "repositories": [
            {
                "type": "path",
                "url": "./vendor/wp_querybuilder"
            }
        ],
        </pre>
    </li>
    <li>Require this package. <code>composer require "codesvault/wpqb @dev"</code></li>
</ul>

<br/>
<br/>

## Examples

### Create Table
``` php
DB::create('querybuilder')
    ->column('ID')->bigInt()->unsigned()->autoIncrement()->primary()->required()
    ->column('name')->string(255)->required()
    ->column('email')->string(255)->default('NULL')
    ->index(['ID'])
    ->execute();
```

<br/>

### Insert Statement
``` php
DB::insert('querybuilder', [
    [
        'name' => 'Keramot UL Islam',
        'email' => 'keramotul.@gmail.com',
    ]
]);
```

<br/>

### Update Statement

``` php
DB::update('querybuilders', [
    'name' => 'Keramot UL',
    'email' => 'keramotul.islam@gmail.com'
])
->where('ID', '=', 10)
->andWhere('name', '=', 'Abm Sourav')
->execute();
```

<br>

### Select Statement

``` php
$result =
DB::select('qb.ID', 'qb.name, qb.email')
    ->from('querybuilders')
    ->alias('qb')
    ->groupBy('name')
    ->get();


// *** where clouse
$result =
DB::select('posts.ID', 'posts.post_title')
    ->distinct()
    ->from('posts posts')
    ->where('posts.post_status', '=', 'publish')
    ->orderBy('post_title', 'DESC')
    ->limit(10)->offset(2)
    ->get();

$result =
DB::select('posts.ID', 'posts.post_title')
    ->distinct()
    ->from('posts posts')
    ->where(function($query) {
        $query->where('posts.post_status', '=', 'publish')
            ->andWhere('posts.post_type', '=', 'page');
    })
    ->orderBy('post_title', 'DESC')
    ->get();


// *** JOIN
DB::select('users.display_name name')
    ->count('posts.ID', 'posts')
    ->from('users users')
    ->join('posts posts')
    ->where('posts.post_status', '=', 'publish')
    ->andWhere('posts.post_type', '=', 'post')
    ->get();

DB::select('posts.post_title')
    ->from('posts posts')
    ->innerJoin('term_relationships term_rel', 'posts.ID', 'term_rel.object_id')
    ->where('posts.post_status', '=', 'publish')
    ->get();
```

<br>

<p>
Expressions also can be exicuted with one instence of <code>DB</code> class. By doing this database connection will be stablished only once.
</p>

``` php
$db = new DB();

$result =
$db::select('posts.ID', 'posts.post_title')
    ...

$db::create('meta')
    ...
```
