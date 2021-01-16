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

  Public function index(){
	  Return view(‘auth.register’);
    }

```
## Define the route out to the web.php
```
	Route → web.php         //fully qualified class name for this and define the method
	Route::get(‘/register’, [RegisterController::class, ‘index’]);
	Then import the class into the top here.

```
## Views
```
→ views → auth → register.blade.php then write same layout blade it works on your browser
In register form action=”{{ route(‘register’) }}”  method post  :- @csrf

Web.php → Route::get('/register', [RegisterController::class, ‘store’]) 

```

## RegisterController:
```
Public function store(Request $request){
//dd(‘abc’) 
// Validation //already pullout request object.
dd($request)
dd($request->email) shorthand property or dd($request->get(‘email’)); refer to get message
//store user
//redirect
}
Within laravel Base controller we have a validate method
$this → validate ($request, [
		// ‘name’ => [‘required’, ‘max’]
	‘Name’ => ‘required|max:255’,
	‘Username’ => ‘required|max:255’,
	‘Email’ => ‘required|email|max:255’,
	‘Password’ => ‘required|confirmed’,

]);
Make Sure you import user model and create person of array
User::create([
	‘Name’ => $request → name,
	email => $request → name,
‘username’ => $request → name,
password => Hass::make($request → password), Facades
]);
Return redirect() -> route(‘dashboard’); → #

//Sign in
auth() ->attempt($request->only(‘email’, ‘password’);

))

Go to the register.blade.php and write error
@error(‘name’)
	<div class =”danger”>
	{{ $message }}
</div>
@enderror


```

## Dashboard controller
```
# Route::get('/dashboard, [DashboardController::class, 'index']) -> name('dashboard');
Php artisan make:controller DashboardController
	Open it and 
	Public function index(){
		Return view(‘dashboard’);
	}
	Import the DashboardController
	Then create views
	Dashboard.blade.php
	Layout copy and paste


```
### dashboard signed in two method Fasad or Auth helper
```
 auth()->attempt([
    		'email' => $request -> email,
    		'password' => $request -> password,
    	]);

OR
For die dump => dd($request->only('email', 'password'));

auth()->attempt($request->only('email', 'password'));

To check weather you sign in or not then go to
→ DashboardController.php → inside public function index(){
	dd(auth()->user());
    	
    	return view('dashboard');

}
→ Click on the attribute to find all the sql arrays: now we know auth→ user returning its object.

```
### Authenticated state: if (auth()→ check()) or auth() → user())
```
@if(auth()->user())
		<li><a href="" class="p-3">Britu</a></li>
		<li><a href="" class="p-3">Logout</a></li>
	@else
		<li><a href="" class="p-3">Log out</a></li>
		<li><a href="{{ route('register') }}" class="p-3">Register</a></li>
	@endif

Better way to do this:
@auth
@endauth
@guest
@endguest

```
### Let’s Login, done form and login 
```
Go to the Command line
php artisan make:controller Auth\\LoginController
→ web.php :- make a Route get and post  login and import the LoginController:
Route::get('/login', [LoginController::class, 'index']) -> name('login');
Route::post('/login', [LoginController::class, 'store']);
→ LoginController.php:- 
   public function index()
    {return view('auth.login');}

    public function store()
    {//for user sign in
    	dd('ok');
    }

Go to the resources→ views→ auth→ create login.blade.php 
Copy all the code from register.blade.php and paste it and amend it for Login form
class LoginController extends Controller
{
    public function index()
    {
    	return view('auth.login');
    }

    public function store(Request $request)
    {
    	$this -> validate($request, [
    		'email' => 'required|email',
    		'password' => 'required',
    		]);

    	if(!auth()->attempt($request->only('email', 'password'))){
    		return back()->with('status', 'Invalid login details');
    	}

    	return redirect()-> route('dashboard');
    }
}

→ then go to the login.blade.php To get a session helper to checkIn key in our session and flashing messages. Put them in a temporary session.
@if (session('status'))
{{ session('status') }}
@endif

```
## Log Out user 
```
Command prompt: php artisan make:controller Auth\\LogoutController
→ LogoutController.php 
class LogoutController extends Controller
{
    public function store(){
    	//dd('logout');
auth()->logout();
    }
}

→ web.php:
use App\Http\Controllers\Auth\LogoutController;
Create the home route::
Route:: get('/', function() {
	return view('home');
})-> name('home');


Route::post('/logout', [LogoutController::class, 'store']) -> name('logout');

→ app.blade.php → <li><a href="{{ route('logout') }}" class="p-3">Logout</a></li>

→ Then create the home.blade.php and copy the Dashboard page.
But we want it CRFP -> not to be inceptible logout →
Create a form and put the logout li there and make it button
<li>
<form action="{{ route('logout') }}" method="post" class="inline p-3">
@csrf
<button type="submit">Log Out</button>
</form>
</li>

```
## Middleware -> use for dashboard, login, and logout controller
```
@Dashboard => Dashboard controller:
public function __construct()
    {
        $this->middleware(['auth']);
    }

@Auth => Logout Controller:
public function store(){
       auth()->logout();

@Guest => RegisterController and LoginController
public function __construct(){
        $this->middleware(['guest']);
    }
```

## Remember me
```
   <div class="mb-4">
	<div class="flex items-center">
	    <input type="checkbox" name="remember" id="remember" class="mr-2">
	    <label for="remember"> Remember me </label>
	</div>
   </div>
   
   -> auth -> LoginController->
       
       public function store(request $request){

		$this ->validate($request, [
		    'email' => 'required|email',
		    'password'=> 'required',

		]);

		if(!auth()->attempt($request->only('email', 'password'), $request -> remember)){
		    //for remember me we should pass to the second argument
		    return back()->with('status', 'Invalid login details');
		}

		return redirect()->route('dashboard');
    }

```
## Posts Forms and controller
```
view->posts->index.blade.php
@extends('layouts.app')

@section('content')

<div class="flex justify-center">
    <div class="w-8/12 bg-white p-6 rounded-lg">

        <form action="{{ route('posts')}}" method="post">
            @csrf
            <div class="mb-4">
                <label for="body" class="sr-only">Body</label>
                <textarea name="body" id="body" cols="30" rows="4" class="bg-gray-100 border-2 w-full p-4 rounded-lg @error('body') border-red-500 @enderror" placeholder="Post Something!"></textarea>
                @error('body')
                    <div class="text-red-500 mt-2 text-sm">
                        {{$message}}
                    </div>
                @enderror
            </div>

            <button type="submit" class="bg-blue-500 text-white px-4 py-3 rounded font-medium">Post</button>
        </form>


    </div>
</div>

@endsection


Create PostController:
class PostController extends Controller
{
    public function index()
    {
        return view('posts.index');
    }
    public function store(Request $request){
        dd('ok');
    }
}

Web.php
Route::get('/posts', [PostController::class, 'index'])-> name('posts');
Route::post('/posts', [PostController::class, 'store']);
```

## Setting Up Post [Model]
```
	php artisan make:model => there is more easy way
	php artisan make:model Post --help
must do:- php artisan make:model Post -m -f [Create table and migration]

--> Go to the migrations and amend the matter and run the migration
 public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->foreignId('user_id')->constrained()->OnDelete('cascade');
            $table->text('body');
            $table->timestamps();
        });
    }

Description:
  Create a new Eloquent model class

Usage:
  make:model [options] [--] <name>

Arguments:
  name                  The name of the class

Options: ability to do anythings
  -a, --all             Generate a migration, seeder, factory, and resource controller for the model
  -c, --controller      Create a new controller for the model
  -f, --factory         Create a new factory for the model
      --force           Create the class even if the model already exists
  -m, --migration       Create a new migration file for the model
  -s, --seed            Create a new seeder file for the model
  -p, --pivot           Indicates if the generated model should be a custom intermediate table model
  -r, --resource        Indicates if the generated controller should be a resource controller
      --api             Indicates if the generated controller should be an API controller
  -h, --help            Display help for the given command. When no command is given display help for the list command
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi            Force ANSI output
      --no-ansi         Disable ANSI output
  -n, --no-interaction  Do not ask any interactive question
      --env[=ENV]       The environment the command should run under
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

```

## User Post Relationship
```
Go to the User Model -> and write 
	
	public function posts(){
        	return $this->hasMany(Post::class);
    	}
- Go to the DashboardController-> 
	dd(auth()->user()->posts); [has many collection return]-> we shoud do this.
	dd(auth()->user()->posts()); [Has many object return]
Try it using sign in then see the power of collection of posts by filling the database
```
## Creating a Post




