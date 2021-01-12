# Route View and Layout
## Views and Layout:
```
  Resources → views → layout → app.blade.php
  <body >
      @yield('content')  //-- index.blade.php injected here --
  </body>

  Resources → views → posts → index.blade.php

  @extends('layouts.app')

  @section('content')

  Index files and all

  @endsection

```
## Routes:
```
  Routes → web.php → 
  Route::get('/posts', function () {
      return view('posts.index');
  });
```

## Tailwinds.css installation
```
Laravel shipped with package.json → Laravel.wix pull all the compiler
Npm install → npm install tailwind → npm run dev → npm watch

	→ laravel.mix → require('tailwindcss') → Go to the recourse → css → app.css
  → @tailwind base;
    @tailwind components;
    @tailwind utilities;
 copy the 3 lists above and paste it to the resources → css → and run npm run dev → it will link up to the public css link to it

```
## Running Migration:
```
.env file → change hostname and password
Php artisan migrate 

Q.  add column username to users table
    Php artisan make:migration add_username-to-users-table
  --> Automatically create scaffolding code and it determines → go to the database → migration → you find the file created. Lets add the table user name
  
  In Up migration :        
Schema::table('users', function (Blueprint $table) {
            $table->string('username');
        });

And down migration : $table → dropColumn(‘username’);
Schema::table('users', function (Blueprint $table) {
            	$table->dropColumn('username');
        	});

    Then → php artisan migrate

To roll this back:
  Php artisan migrate:rollback

If you want to make migration app then
Php artisan make:migration add_username_to_users_table --table=abc

```
## Controller and modules practice
```
  Web.php → define the Route
  Route::get('/register', [RegisterController::class, 'index']) -> name('register');
Inside the bracket [Controller and method name] we want to use. So not quet about that point yet. Comment this out and go to Command line

Command line
 → Php artisan make:controller Auth\\RegisterController
→ Http → Controllers → Auth → RegisterController  → create index method here
````
  Public function index(){
	  Return view(‘auth.register’);
    }
````
```


