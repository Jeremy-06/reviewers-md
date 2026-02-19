# 📚 COMPLETE LARAVEL PROJECT REVIEWER

## Comprehensive Study Guide - All Topics Combined

_This reviewer combines all Laravel topics from the Music Application and NS Shop E-Commerce projects, including CLI commands, core concepts, and advanced database topics._

---

<a id="table-of-contents"></a>

## 📑 Table of Contents

### Part 1: Core Laravel Concepts

1. [MVC Architecture](#1-mvc-architecture)
2. [Routing](#2-routing)
3. [Controllers](#3-controllers)
4. [Middleware](#4-middleware)
5. [Eloquent ORM (Models)](#5-eloquent-orm-models)
6. [Mass Assignment & Fillable](#6-mass-assignment--fillable)
7. [Blade Templating Engine](#7-blade-templating-engine)
8. [Database Migrations](#8-database-migrations)
9. [Database Seeders](#9-database-seeders)
10. [Factories](#10-factories)

### Part 2: Authentication & Security

11. [Authentication (Auth Facade & Laravel UI)](#11-authentication)
12. [Session Management](#12-session-management)
13. [Form Validation](#13-form-validation)
14. [CSRF Protection](#14-csrf-protection)
15. [Hashing (Password Encryption)](#15-hashing-password-encryption)

### Part 3: Database Advanced

16. [Query Builder (DB Facade)](#16-query-builder-db-facade)
17. [Pagination](#17-pagination)
18. [Soft Deletes](#18-soft-deletes)
19. [Foreign Key Constraints](#19-foreign-key-constraints)
20. [Schema Table Modifications](#20-schema-table-modifications)
21. [Column Types and Modifiers](#21-column-types-and-modifiers)
22. [Custom Primary Keys](#22-custom-primary-keys)
23. [Cascade Delete Operations](#23-cascade-delete-operations)
24. [Database Transactions](#24-database-transactions)
25. [Many-to-Many Relationships (Pivot Tables)](#25-many-to-many-relationships-pivot-tables)

### Part 4: Advanced Features

26. [Resource Controllers](#26-resource-controllers)
27. [Route Groups and Prefixes](#27-route-groups-and-prefixes)
28. [Named Routes](#28-named-routes)
29. [File Upload & Storage](#29-file-upload--storage)
30. [Service Providers](#30-service-providers)
31. [Facades](#31-facades)
32. [Traits](#32-traits)
33. [Request Handling](#33-request-handling)
34. [Environment Configuration](#34-environment-configuration)

### Part 5: Third-Party Packages

35. [Laravel Collective HTML Forms](#35-laravel-collective-html-forms)
36. [Excel Import (Maatwebsite Excel)](#36-excel-import-maatwebsite-excel)
37. [DataTables (Yajra DataTables)](#37-datatables-yajra-datatables)
38. [Carbon (Date/Time)](#38-carbon-datetime)
39. [Vite (Asset Bundling)](#39-vite-asset-bundling)
40. [Laravel Sanctum (API Authentication)](#40-laravel-sanctum-api-authentication)

### Part 6: Artisan CLI Commands

41. [What is Artisan?](#41-what-is-artisan)
42. [Migration Commands](#42-migration-commands)
43. [Make Commands (Code Generators)](#43-make-commands-code-generators)
44. [Database Seeding Commands](#44-database-seeding-commands)
45. [Cache Commands](#45-cache-commands)
46. [Server & Development Commands](#46-server--development-commands)
47. [Storage Commands](#47-storage-commands)
48. [Route Commands](#48-route-commands)
49. [Key & Environment Commands](#49-key--environment-commands)
50. [Queue & Testing Commands](#50-queue--testing-commands)

### Quick References

- [Summary Tables](#summary-tables)
- [Quick Artisan Cheat Sheet](#quick-artisan-cheat-sheet)
- [Common Command Patterns](#common-command-patterns)
- [📝 Answer Key for All Quizzes](#answer-key)

---

# PART 1: CORE LARAVEL CONCEPTS

---

<a id="1-mvc-architecture"></a>

## 1. MVC Architecture (Model-View-Controller)

### 📍 Where Used:

- **Models**: `app/Models/` - `Album.php`, `Artist.php`, `Song.php`, `Item.php`, `Stock.php`, `Customer.php`, `User.php`
- **Views**: `resources/views/` - All `.blade.php` files
- **Controllers**: `app/Http/Controllers/` - `AlbumController.php`, `ItemController.php`, `DashboardController.php`, etc.

### 🎯 Purpose in Laravel:

MVC (Model-View-Controller) is the architectural pattern Laravel follows. It separates the application into three parts:

- **Model** - Handles data and business logic
- **View** - Handles the presentation/UI
- **Controller** - Handles user requests and connects Model with View

### 📖 Detailed Explanation:

Think of MVC like a restaurant:

- **Model** = Kitchen (prepares the food/data)
- **View** = The plate presentation (how food looks to customers)
- **Controller** = Waiter (takes orders, brings food, connects kitchen and customers)

When a user visits your website:

1. The request goes to a **Controller**
2. The Controller asks the **Model** for data
3. The Controller passes data to the **View**
4. The View renders HTML and sends it back to the user

### 💡 Example from Project:

```php
// Controller (ArtistController.php) - processes data
public function index() {
    $artists = Artist::all();  // Model - get data
    return view('artist.index', compact('artists'));  // View - display
}

// Controller (ItemController.php) - using Query Builder
public function index()
{
    $items = DB::table('item as i')
        ->leftJoin('stock as s', 'i.item_id', '=', 's.item_id')
        ->select('i.*', 's.quantity')
        ->get();

    return view('item.index', compact('items')); // Passes data to View
}
```

### ❓ Quiz Questions:

1. What does MVC stand for?
2. Which component of MVC handles database interactions?
3. In Laravel, where are Views typically stored?
4. What is the role of a Controller in MVC?
5. Where are Models located in Laravel project structure?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="2-routing"></a>

## 2. Routing

### 📍 Where Used:

- `routes/web.php` - Web routes
- `routes/api.php` - API routes

### 🎯 Purpose in Laravel:

Routes define URLs and map them to specific controller methods or closures. They tell Laravel what to do when a user visits a specific URL. It's like the "address book" of your application.

### 📖 Detailed Explanation:

Routing is like a phone directory. When someone calls a number (visits a URL), the directory knows who to connect them to (which controller/action).

Laravel supports multiple HTTP methods:

- `GET` - Retrieve data
- `POST` - Submit data
- `PUT/PATCH` - Update data
- `DELETE` - Remove data

### 💡 Examples from Project:

**Basic Route with Closure:**

```php
Route::get('/', function () {
    return view('welcome');
});
```

**Route using Controller:**

```php
Route::get('/artists', [ArtistController::class, 'index'])->name('artist.index');
Route::post('/artists', [ArtistController::class, 'store'])->name('artists.store');
Route::get('/add-to-cart/{id}', [ItemController::class, 'addToCart'])->name('addToCart');
```

**Route::view (shortcut):**

```php
Route::view('/register', 'user.register');  // Direct to view, no controller needed
```

**Resource Routes:**

```php
Route::resource('songs', SongController::class);  // Auto-generate CRUD routes
Route::resource('albums', AlbumController::class);
Route::resource('items', ItemController::class);
```

**Route with Prefix:**

```php
Route::prefix('admin')->group(function () {
    Route::get('/customers', [DashboardController::class, 'getCustomers'])->name('admin.customers');
});
```

### ❓ Quiz Questions:

1. What file contains web routes in Laravel?
2. What is the difference between `Route::get` and `Route::post`?
3. What does `Route::resource()` do?
4. What is the purpose of `->name()` in route definition?
5. How do you define a route parameter in Laravel?
6. How do you create a route that directly displays a view?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="3-controllers"></a>

## 3. Controllers

### 📍 Where Used:

- `app/Http/Controllers/ItemController.php`
- `app/Http/Controllers/DashboardController.php`
- `app/Http/Controllers/HomeController.php`
- `app/Http/Controllers/Auth/` - Authentication controllers

### 🎯 Purpose in Laravel:

Controllers group related request handling logic into a single class. Instead of putting all logic in routes, you organize them in controllers.

### 📖 Detailed Explanation:

Controllers are like managers in an office. When a task (request) comes in, the manager (controller) decides what to do, gathers necessary resources (data from models), and produces output (returns views or responses).

### 💡 Example from Project:

```php
// app/Http/Controllers/ItemController.php
class ItemController extends Controller
{
    public function index()
    {
        $items = DB::table('item as i')
            ->leftJoin('stock as s', 'i.item_id', '=', 's.item_id')
            ->select('i.*', 's.quantity')
            ->get();
        return view('item.index', compact('items'));
    }

    public function create()
    {
        return view('item.create');
    }

    public function store(Request $request)
    {
        // Validation and saving logic
        $item = Item::create([...]);
        return redirect()->route('items.create')->with('success', 'item added');
    }
}
```

### ❓ Quiz Questions:

1. Where are controllers stored in Laravel?
2. What command creates a new controller?
3. What is a Resource Controller?
4. What class should a controller extend?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="4-middleware"></a>

## 4. Middleware

### 📍 Where Used:

- `routes/web.php` - `Route::middleware(['auth'])->group(...)`
- `app/Http/Kernel.php` - Middleware registration
- `app/Http/Middleware/` - Middleware classes (`Authenticate.php`, `VerifyCsrfToken.php`, etc.)

### 🎯 Purpose in Laravel:

Middleware filters HTTP requests entering your application. It acts as a "filter" or "checkpoint" before a request reaches the controller. It can perform actions before or after a request reaches your controller.

### 📖 Detailed Explanation:

Think of middleware as security guards or checkpoints. Before a request reaches your controller, it passes through middleware that can:

- Check if user is logged in
- Verify CSRF tokens
- Log requests
- Modify the request/response

Middleware is like a "security guard" - it checks first before allowing users in. For example: if not logged in, you cannot access protected pages.

### 💡 Examples from Project:

```php
// All routes inside this group are protected - must be logged in
Route::middleware(['auth'])->group(function () {
    Route::get('/artists', [ArtistController::class, 'index']);
    Route::resource('songs', SongController::class);
    Route::resource('albums', AlbumController::class);
    // ...more protected routes
});

// app/Http/Middleware/Authenticate.php
class Authenticate extends Middleware
{
    protected function redirectTo(Request $request): ?string
    {
        return $request->expectsJson() ? null : route('login');
    }
}

// app/Http/Controllers/HomeController.php - Using middleware
class HomeController extends Controller
{
    public function __construct()
    {
        $this->middleware('auth');  // Requires authentication
    }
}

// app/Http/Kernel.php - Middleware groups
protected $middlewareGroups = [
    'web' => [
        \App\Http\Middleware\EncryptCookies::class,
        \Illuminate\Session\Middleware\StartSession::class,
        \App\Http\Middleware\VerifyCsrfToken::class,
        // ...
    ],
];
```

### ❓ Quiz Questions:

1. What is middleware and what is its function?
2. What does the `auth` middleware mean?
3. How do you apply middleware to a group of routes?
4. Where are middleware classes stored?
5. What file registers middleware in Laravel?
6. How do you apply middleware to a controller?
7. What are middleware groups?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="5-eloquent-orm-models"></a>

## 5. Eloquent ORM (Models)

### 📍 Where Used:

- `app/Models/` - All Model files (`Album.php`, `Artist.php`, `Song.php`, `Item.php`, `Stock.php`, `Customer.php`, `User.php`)
- Controllers that use Models

### 🎯 Purpose in Laravel:

Eloquent is Laravel's ORM (Object-Relational Mapping). It allows you to interact with database tables using PHP objects instead of writing raw SQL queries.

### 📖 Detailed Explanation:

Think of Eloquent as a translator. Instead of speaking SQL (database language), you speak PHP, and Eloquent translates it to SQL for you.

Each Model represents a database table:

- `Item` model → `item` table
- `User` model → `users` table

### 💡 Examples from Project:

**Model Definition:**

```php
// app/Models/Item.php
class Item extends Model
{
    use HasFactory;

    protected $table = 'item';           // Custom table name
    public $timestamps = false;          // Disable timestamps
    protected $primaryKey = 'item_id';   // Custom primary key
    protected $fillable = ['description', 'cost_price', 'sell_price', 'img_path'];
}
```

**Retrieving Data:**

```php
$artists = Artist::all();         // SELECT * FROM artists
$song = Song::find($id);          // SELECT * FROM songs WHERE id = ?
$listener = Listener::where('user_id', Auth::id())->first();
$item = Item::find($id);          // Find by custom primary key
```

**Creating Data:**

```php
$album = Album::create([
    'title' => $request->title,
    'genre' => $request->genre,
    'date_released' => $request->date_released,
    'artist_id' => $request->artist_id,
]);
```

**Updating Data:**

```php
Album::where('id', $id)->update([
    'title' => $request->title,
    // ...
]);
```

**Deleting Data:**

```php
Artist::destroy($id);
```

### ❓ Quiz Questions:

1. What is Eloquent ORM?
2. What is the difference between `find()` and `where()->first()`?
3. What does the `::all()` method do in Eloquent?
4. How do you update a record using Eloquent?
5. What property defines which columns can be mass-assigned?
6. How do you disable timestamps in a model?
7. What method retrieves a record by primary key?
8. How do you specify a custom table name in a model?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="6-mass-assignment--fillable"></a>

## 6. Mass Assignment & Fillable

### 📍 Where Used:

- `app/Models/Album.php`
- `app/Models/Song.php`
- `app/Models/Listener.php`
- `app/Models/Item.php`
- `app/Models/User.php`

### 🎯 Purpose in Laravel:

Security feature to control which columns can be mass assigned (to prevent unwanted data manipulation).

### 📖 Detailed Explanation:

The `$fillable` array specifies which fields can be filled using `create()` or `update()` method. It's like a "whitelist" of allowed fields. This protects against mass assignment vulnerabilities where malicious users could inject unwanted data.

### 💡 Examples from Project:

```php
// Album.php
protected $fillable = ['title', 'genre', 'date_released', 'artist_id'];

// Song.php
protected $fillable = ['title', 'description', 'album_id'];

// Listener.php
protected $fillable = ['fname', 'lname', 'address', 'img_path', 'user_id'];

// Item.php
protected $fillable = ['description', 'cost_price', 'sell_price', 'img_path'];
```

### ❓ Quiz Questions:

1. What is the purpose of the `$fillable` property in a Model?
2. What is mass assignment vulnerability?
3. What is the alternative to `$fillable`? (Answer: `$guarded`)

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="7-blade-templating-engine"></a>

## 7. Blade Templating Engine

### 📍 Where Used:

- All `.blade.php` files in `resources/views/`
- `resources/views/layouts/app.blade.php`
- `resources/views/layouts/base.blade.php`

### 🎯 Purpose in Laravel:

Laravel's template engine for dynamic HTML generation. It provides convenient shortcuts for common PHP structures like loops, conditionals, and template inheritance.

### 📖 Detailed Explanation:

Blade is Laravel's way of combining PHP and HTML. Instead of mixing messy PHP with HTML, you use clean Blade syntax:

- `{{ }}` - Echo and escape output
- `{!! !!}` - Echo raw output (be careful - no escaping)
- `@if`, `@foreach`, `@extends`, `@section`, `@yield` - Control structures

### 💡 Examples from Project:

**Template Inheritance:**

```php
{{-- Layout file: layouts/base.blade.php --}}
<!DOCTYPE html>
<html>
<head>
    @section('head')
        <link href="bootstrap.css" rel="stylesheet">
    @show
</head>
<body>
    @include('layouts.header')
    @yield('body')
</body>
</html>

{{-- Child view: artist/index.blade.php --}}
@extends('layouts.base')

@section('body')
    @include('layouts.flash-messages')

    @foreach ($artists as $artist)
        <td>{{ $artist->name }}</td>
    @endforeach
@endsection
```

**Blade Directives:**

```php
@foreach ($artists as $artist)
    <td>{{ $artist->name }}</td>
@endforeach

@if ($song->deleted_at === null)
    // Show edit button
@else
    // Show restore button
@endif
```

**CSRF Token:**

```php
<form method="POST">
    @csrf   <!-- Required for POST requests -->
</form>
```

**Method Spoofing:**

```php
<form method="POST">
    @method('DELETE')  <!-- For DELETE request -->
    @csrf
</form>
```

**Error Display:**

```php
@error('email')
    <div class="alert alert-danger">{{ $message }}</div>
@enderror

@if ($errors->any())
    @foreach ($errors->all() as $error)
        <li>{{ $error }}</li>
    @endforeach
@endif
```

**Old Input:**

```php
<input type="text" name="email" value="{{ old('email') }}">
```

### ❓ Quiz Questions:

1. What is the file extension for Blade templates?
2. What is the difference between `{{ }}` and `{!! !!}` in Blade?
3. What is the purpose of the `@csrf` directive?
4. What are `@extends` and `@section`?
5. How do you display validation errors in Blade?
6. What is the purpose of `@method('DELETE')`?
7. What directive is used to include another Blade file?
8. What directive creates a section that child views can fill?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="8-database-migrations"></a>

## 8. Database Migrations

### 📍 Where Used:

- `database/migrations/` - All migration files

### 🎯 Purpose in Laravel:

Version control for database schema. Provides a way to create and modify database tables using PHP code instead of raw SQL.

### 📖 Detailed Explanation:

Instead of manually creating tables in the database, you create migration files. These are the "blueprints" of your tables. Each migration file describes:

- `up()` method - What to do when running the migration (create table)
- `down()` method - What to do when rolling back (drop table)

### 💡 Examples from Project:

**Creating Table:**

```php
// 2025_12_16_014903_create_artists_table.php
Schema::create('artists', function (Blueprint $table) {
    $table->id();
    $table->string('name', 50);
    $table->text('country');
    $table->text('img_path');
    $table->timestamps();
});

// 2026_01_27_013920_create_item_table.php
Schema::create('item', function (Blueprint $table) {
    $table->integer('item_id')->primary();
    $table->string('item_name');
    $table->decimal('unit_price', 10, 2);
    $table->text('description')->nullable();
});
```

**Foreign Key Constraint:**

```php
// 2026_01_06_022756_create_albums_table.php
$table->foreignId('artist_id')->constrained('artists');
```

**Modifying Existing Table:**

```php
// 2026_01_13_010911_add_description_to_song.php
Schema::table('songs', function (Blueprint $table) {
    $table->string('description')->after('title');
});
```

**Soft Deletes Column:**

```php
$table->softDeletes();  // Adds deleted_at column
```

**Pivot Table (Many-to-Many):**

```php
// 2026_01_20_005928_create_table_album_listener.php
Schema::create('album_listener', function (Blueprint $table) {
    $table->unsignedBigInteger('album_id')->index();
    $table->foreign('album_id')->references('id')->on('albums')->onDelete('cascade');
    $table->unsignedBigInteger('listener_id')->index();
    $table->foreign('listener_id')->references('id')->on('listeners')->onDelete('cascade');
    $table->primary(['album_id', 'listener_id']);
});
```

### ❓ Quiz Questions:

1. What is database migration?
2. What command runs the migrations?
3. What is the purpose of the `timestamps()` method?
4. What does `foreignId()->constrained()` do?
5. What is the purpose of the `down()` method in migration?
6. Where are migration files stored?
7. What command creates a new migration file?
8. What command rolls back the last migration?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="9-database-seeders"></a>

## 9. Database Seeders

### 📍 Where Used:

- `database/seeders/DatabaseSeeder.php`
- `database/seeders/AlbumSeeder.php`
- `database/seeders/SongSeeder.php`
- `database/seeders/ItemSeeder.php`
- `database/seeders/UserSeeder.php`

### 🎯 Purpose in Laravel:

To populate the database with dummy/test data for development and testing.

### 📖 Detailed Explanation:

Seeders are like sample data generators. Instead of manually inserting data into your database, you write code that automatically creates the data for you. Faker generates random fake data (names, emails, words, etc.)

### 💡 Examples from Project:

```php
// AlbumSeeder.php
public function run(Faker $faker): void {
    for ($i = 0; $i < 30; $i++) {
        $album = new Album();
        $album->title = $faker->words(3, true);
        $album->genre = $faker->randomElement(['kpop', 'punk', 'pop', 'grunge', 'hiphop']);
        $album->date_released = $faker->date();
        $album->artist_id = $faker->numberBetween(4, 7);
        $album->save();
    }
}

// ItemSeeder.php
public function run(Faker $faker): void
{
    for ($i = 0; $i < 30; $i++) {
        $item = new Item();
        $item->description = $faker->realText(20);
        $item->cost_price = $faker->randomFloat(2, 0, 6);
        $item->sell_price = $faker->randomFloat(2, 0, 6);
        $item->img_path = 'default.jpg';
        $item->save();

        $stock = new Stock;
        $stock->item_id = $item->item_id;
        $stock->quantity = 20;
        $stock->save();
    }
}

// DatabaseSeeder.php - calling other seeders
public function run(): void
{
    $this->call(UserSeeder::class);
    $this->call(SongSeeder::class);
}
```

### ❓ Quiz Questions:

1. What are Database Seeders for?
2. What is the Faker library?
3. What command runs the seeders?
4. How do you call another seeder from DatabaseSeeder?
5. What command creates a new seeder?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="10-factories"></a>

## 10. Factories

### 📍 Where Used:

- `database/factories/UserFactory.php`

### 🎯 Purpose in Laravel:

Factories define templates for generating model instances with fake data. They work with Eloquent models to create test data efficiently.

### 📖 Detailed Explanation:

Factories are like templates or molds for creating model instances. You define what a "typical" user or item looks like, and the factory can create many of them automatically.

### 💡 Example from Project:

```php
// database/factories/UserFactory.php
class UserFactory extends Factory
{
    protected static ?string $password;

    public function definition(): array
    {
        return [
            'name' => fake()->name(),
            'email' => fake()->unique()->safeEmail(),
            'email_verified_at' => now(),
            'password' => static::$password ??= Hash::make('password'),
            'remember_token' => Str::random(10),
        ];
    }

    public function unverified(): static
    {
        return $this->state(fn (array $attributes) => [
            'email_verified_at' => null,
        ]);
    }
}

// Usage
User::factory()->count(10)->create();  // Creates 10 users
```

### ❓ Quiz Questions:

1. What trait must a model use to work with factories?
2. What command creates a new factory?
3. What method defines the default state of a factory?
4. How do you create multiple instances using a factory?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

# PART 2: AUTHENTICATION & SECURITY

---

<a id="11-authentication"></a>

## 11. Authentication (Auth Facade & Laravel UI)

### 📍 Where Used:

- `app/Http/Controllers/UserController.php`
- `app/Http/Controllers/ListenerController.php`
- `app/Models/User.php`
- `routes/web.php` - `Auth::routes();`
- `app/Http/Controllers/Auth/` - `LoginController.php`, `RegisterController.php`
- `resources/views/auth/` - `login.blade.php`, `register.blade.php`

### 🎯 Purpose in Laravel:

To manage user login, logout, and sessions. Laravel UI provides pre-built authentication scaffolding.

### 📖 Detailed Explanation:

Authentication handles "who are you?" - login, logout, and checking if you're logged in. `Auth::routes()` is a shortcut that registers all authentication routes automatically:

- Login/Logout
- Registration
- Password reset
- Email verification

### 💡 Examples from Project:

**User Model extends Authenticatable:**

```php
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable {
    use HasApiTokens, HasFactory, Notifiable;
    // ...
}
```

**Login:**

```php
if (auth()->attempt(['email' => $request->email, 'password' => $request->password])) {
    return redirect('/artists');
}
```

**Manual Login after Registration:**

```php
Auth::login($user);
```

**Logout:**

```php
Auth::logout();
```

**Get Current User:**

```php
$user = Auth::user();      // Get user object
$userId = Auth::id();      // Get user ID
```

**Auth Routes:**

```php
// routes/web.php
Auth::routes();  // Registers all auth routes
```

**Login Controller:**

```php
class LoginController extends Controller
{
    use AuthenticatesUsers;  // Trait with login logic

    protected $redirectTo = '/home';

    public function __construct()
    {
        $this->middleware('guest')->except('logout');
        $this->middleware('auth')->only('logout');
    }
}
```

### ❓ Quiz Questions:

1. What does the `auth()->attempt()` method do?
2. How do you get the currently logged-in user?
3. What is the difference between `Auth::login()` and `auth()->attempt()`?
4. What is the role of the `Authenticatable` trait?
5. What command installs Laravel UI authentication?
6. What does `Auth::routes()` do?
7. What helper checks if a user is logged in?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="12-session-management"></a>

## 12. Session Management

### 📍 Where Used:

- `app/Http/Controllers/ItemController.php` - Cart management
- `app/Cart.php` - Cart class
- `config/session.php` - Session configuration

### 🎯 Purpose in Laravel:

Sessions store user data across multiple HTTP requests. Laravel provides a unified API for various session backends.

### 📖 Detailed Explanation:

HTTP is stateless - each request is independent. Sessions allow you to remember information about a user across requests (like a shopping cart). Laravel stores session data and gives each user a unique session ID (stored in a cookie).

### 💡 Examples from Project:

```php
// app/Http/Controllers/ItemController.php
public function addToCart($id)
{
    $item = Item::find($id);

    // Check if cart exists in session
    $oldCart = Session::has('cart') ? Session::get('cart') : null;

    $cart = new Cart($oldCart);
    $cart->add($item, $id);

    // Store cart in session
    Session::put('cart', $cart);

    return redirect('/')->with('success', 'item added to cart');
}

public function getRemoveItem($id)
{
    $oldCart = Session::has('cart') ? Session::get('cart') : null;
    $cart = new Cart($oldCart);
    $cart->removeItem($id);

    if (count($cart->items) > 0) {
        Session::put('cart', $cart);
    } else {
        Session::forget('cart');  // Remove cart from session
    }
    return redirect()->route('getCart');
}
```

### ❓ Quiz Questions:

1. What facade is used to work with sessions in Laravel?
2. How do you store data in a session?
3. How do you retrieve data from a session?
4. How do you delete a session variable?
5. What method checks if a session key exists?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="13-form-validation"></a>

## 13. Form Validation

### 📍 Where Used:

- `app/Http/Controllers/UserController.php`
- `app/Http/Controllers/SongController.php`
- `app/Http/Controllers/ItemController.php`
- `app/Http/Controllers/Auth/RegisterController.php`

### 🎯 Purpose in Laravel:

To validate user input before saving to the database.

### 📖 Detailed Explanation:

Before accepting form data, it is checked first to see if it is valid (has email, minimum characters, etc.). Validation is like a bouncer at a club - it checks if the data is acceptable before letting it in.

### 💡 Examples from Project:

**Method 1 - Using Validator Facade:**

```php
// UserController.php
$rules = [
    'email' => 'required|email',
    'password' => 'required|min:6',
    'lname' => 'required|alpha',
    'img_path' => 'required|mimes:jpg,png'
];
$messages = [
    'required' => 'The :attribute must have content',
    'email' => 'The :attribute format is wrong',
];

$validator = Validator::make($request->all(), $rules, $messages);

if ($validator->fails()) {
    return redirect()->back()
        ->withErrors($validator)
        ->withInput();
}
```

**Method 2 - Using $request->validate():**

```php
// SongController.php
$validated = $request->validate([
    'title' => ['required', 'max:30'],
    'description' => ['required', 'min:5', 'max:200'],
    'album_id' => 'required'
]);
```

**Displaying Errors in Blade:**

```php
@error('image')
    <div class="alert alert-danger">{{ $message }}</div>
@enderror

@if ($errors->any())
    <div class="alert alert-danger">
        @foreach ($errors->all() as $message)
            {{ $message }}
        @endforeach
    </div>
@endif
```

### ❓ Quiz Questions:

1. What are the two ways to validate form input?
2. What are `withErrors()` and `withInput()` for?
3. What does the validation rule `required|email|min:6` mean?
4. How do you customize error messages?
5. What directive displays validation errors in Blade?
6. What does the 'confirmed' validation rule expect?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="14-csrf-protection"></a>

## 14. CSRF Protection

### 📍 Where Used:

- All forms in Blade templates
- `app/Http/Middleware/VerifyCsrfToken.php`
- `resources/views/layouts/app.blade.php` - CSRF token meta tag

### 🎯 Purpose in Laravel:

Security feature to protect forms from Cross-Site Request Forgery attacks. Laravel automatically generates and verifies CSRF tokens.

### 📖 Detailed Explanation:

The CSRF token is like a "secret code" or security badge that proves the form submission really came from your website, not from another malicious site. Every form gets a unique token, and Laravel checks if the token is valid when the form is submitted.

### 💡 Examples from Project:

```php
{{-- Meta tag for AJAX --}}
<meta name="csrf-token" content="{{ csrf_token() }}">

{{-- Form with CSRF --}}
<form action="{{ route('user.register') }}" method="POST">
    @csrf   <!-- CSRF Token -->
    <!-- form fields -->
</form>

{{-- DELETE form --}}
<form action="{{ route('items.destroy', $item->item_id) }}" method="POST">
    @method('DELETE')
    @csrf
    <button>Delete</button>
</form>
```

### ❓ Quiz Questions:

1. What is CSRF and why is it important?
2. What happens if the @csrf token is missing?
3. Where is the CSRF token validated?
4. What Blade directive adds a CSRF token to a form?
5. What HTTP methods require CSRF verification by default?
6. Where can you exclude routes from CSRF protection?
7. What error do you get if CSRF token is missing?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="15-hashing-password-encryption"></a>

## 15. Hashing (Password Encryption)

### 📍 Where Used:

- `app/Http/Controllers/UserController.php`
- `app/Models/User.php`

### 🎯 Purpose in Laravel:

To encrypt passwords so they are not readable in the database.

### 📖 Detailed Explanation:

Passwords should not be saved as plain text. They should be encrypted using `bcrypt()` or automatic casting.

### 💡 Examples from Project:

```php
// Manual hashing using bcrypt
$user = User::create([
    'password' => bcrypt($request->password),
]);

// Auto-hashing using casts (User.php)
protected $casts = [
    'password' => 'hashed',
];
```

### ❓ Quiz Questions:

1. Why do we need to hash passwords?
2. What is the `bcrypt()` function?
3. How do you auto-hash using the `$casts` property?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

# PART 3: DATABASE ADVANCED

---

<a id="16-query-builder-db-facade"></a>

## 16. Query Builder (DB Facade)

### 📍 Where Used:

- `app/Http/Controllers/AlbumController.php`
- `app/Http/Controllers/ListenerController.php`
- `app/Http/Controllers/ItemController.php`
- `app/DataTables/CustomersDataTable.php`

### 🎯 Purpose in Laravel:

An alternative way to query the database, useful for complex queries and joins. Provides a convenient, fluent interface for building and executing database queries.

### 📖 Detailed Explanation:

Query Builder is the more "SQL-like" approach to database queries. Useful when you need complex joins that are difficult to do with Eloquent. It works like building with blocks - you chain methods together to build your SQL query.

### 💡 Examples from Project:

```php
// AlbumController.php - Join query with pagination
$albums = DB::table('artists AS ar')
    ->join('albums AS al', 'ar.id', '=', 'al.artist_id')
    ->select('al.id', 'al.title', 'ar.name', 'al.genre', 'al.date_released')
    ->orderBy('al.id', 'DESC')
    ->paginate(10);

// ListenerController.php - Insert to pivot table
DB::table('album_listener')->insert([
    'album_id' => $album_id,
    'listener_id' => $listener->id,
    'created_at' => now()
]);

// ItemController.php - Left Join
$items = DB::table('item as i')
    ->leftJoin('stock as s', 'i.item_id', '=', 's.item_id')
    ->select('i.*', 's.quantity')
    ->get();

// DataTables query
$users = DB::table('users')
    ->join('customer', 'users.id', '=', 'customer.user_id')
    ->select(
        'users.id AS id',
        'users.name',
        'users.email',
        'customer.addressline',
        'customer.phone',
        'users.created_at'
    );
```

### ❓ Quiz Questions:

1. What is the difference between Eloquent and Query Builder?
2. How do you join tables using Query Builder?
3. What is the `DB::table()` method for?
4. What facade is used for Query Builder?
5. What method starts a query on a table?
6. What method executes the query and returns results?
7. What is the difference between `join` and `leftJoin`?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="17-pagination"></a>

## 17. Pagination

### 📍 Where Used:

- `app/Http/Controllers/AlbumController.php` - `->paginate(10)`
- `resources/views/album/index.blade.php` - `{{ $albums->links() }}`

### 🎯 Purpose in Laravel:

To split results into multiple pages so the data is not overwhelming.

### 📖 Detailed Explanation:

When there are many records, not all are displayed at once. They are split into pages (10 per page, etc.) to make it more manageable.

### 💡 Examples from Project:

```php
// Controller
$albums = DB::table('artists AS ar')
    ->join('albums AS al', 'ar.id', '=', 'al.artist_id')
    ->paginate(10);  // 10 items per page

// Blade View
{{ $albums->links() }}  // Display pagination links
```

### ❓ Quiz Questions:

1. What is the difference between `get()` and `paginate()`?
2. How do you display pagination links in Blade?
3. What is the default number of items per page if `paginate()` has no parameter?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="18-soft-deletes"></a>

## 18. Soft Deletes

### 📍 Where Used:

- `app/Models/Song.php` - `use SoftDeletes;`
- `app/Models/Listener.php` - `use SoftDeletes;`
- `app/Models/Item.php` - `use SoftDeletes;`
- `app/Http/Controllers/SongController.php`
- `app/Http/Controllers/ListenerController.php`
- `database/migrations/2026_01_27_020914_add_soft_deletes_to_item_table.php`

### 🎯 Purpose in Laravel:

Instead of actually deleting the record, it just adds a `deleted_at` timestamp. The record can still be restored. Useful for:

- Keeping historical data
- Recovering accidentally deleted records
- Audit trails

### 📖 Detailed Explanation:

It's like a "Recycle Bin" - data doesn't disappear immediately, it can be recovered.

- Normal delete = Record is **permanently removed** from database
- Soft delete = Record **stays** in database but is **marked as deleted**

When you query a model with soft deletes:

- Soft-deleted records are automatically **excluded** from results
- You can include or only get soft-deleted records with special methods

### 💡 Examples from Project:

**Migration to Add Soft Deletes:**

```php
// database/migrations/2026_01_27_020914_add_soft_deletes_to_item_table.php
public function up(): void
{
    Schema::table('item', function (Blueprint $table) {
        $table->softDeletes();  // Adds 'deleted_at' column (nullable timestamp)
    });
}

public function down(): void
{
    Schema::table('item', function (Blueprint $table) {
        $table->dropColumn('deleted_at');  // Remove the column on rollback
    });
}
```

**Model Setup:**

```php
use Illuminate\Database\Eloquent\SoftDeletes;

class Song extends Model {
    use SoftDeletes;
}

class Item extends Model
{
    use HasFactory, SoftDeletes;  // Add SoftDeletes trait

    protected $table = 'item';
    public $timestamps = false;
    protected $primaryKey = 'item_id';
    protected $fillable = ['description', 'cost_price', 'sell_price', 'img_path'];
}
```

**Using Soft Deletes in Code:**

```php
// Delete an item (soft delete - sets deleted_at)
$item = Item::find(1);
$item->delete();  // Record still exists, but deleted_at is set

// Query items (soft-deleted items are automatically excluded)
$items = Item::all();  // Only non-deleted items

// Include soft-deleted items
$allItems = Item::withTrashed()->get();

// Only get soft-deleted items
$deletedItems = Item::onlyTrashed()->get();

// Restore a soft-deleted item
$item = Item::withTrashed()->find(1);
$item->restore();  // Clears deleted_at

// Permanently delete (force delete)
$item->forceDelete();  // Actually removes from database

// Check if item is soft-deleted
if ($item->trashed()) {
    echo "This item has been deleted";
}
```

**Restoring Soft Deleted Records:**

```php
public function restore($id) {
    Song::withTrashed()->where('id', $id)->restore();
    return redirect()->route('songs.index');
}
```

**In Blade Template:**

```php
@foreach ($items as $item)
    @if ($item->deleted_at === null)
        {{-- Show normal actions --}}
        <a href="{{ route('items.edit', $item->item_id) }}">Edit</a>
        <form action="{{ route('items.destroy', $item->item_id) }}" method="POST">
            @method('DELETE')
            @csrf
            <button>Delete</button>
        </form>
    @else
        {{-- Show restore option for soft-deleted items --}}
        <span style="color: gray">Deleted</span>
        <a href="{{ route('items.restore', $item->item_id) }}">Restore</a>
    @endif
@endforeach
```

### ❓ Quiz Questions:

1. What is Soft Deletes?
2. What is the column name used by Soft Deletes?
3. How do you retrieve all records including soft deleted ones?
4. How do you restore a soft deleted record?
5. What method adds a soft deletes column in a migration?
6. What trait must a model use to enable soft deletes?
7. How do you permanently delete a soft-deleted record?
8. What method checks if a model is soft-deleted?
9. Are soft-deleted records included in normal queries?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="19-foreign-key-constraints"></a>

## 19. Foreign Key Constraints

### 📍 Where Used:

- `database/migrations/2026_02_10_032706_create_customer_table.php`
- `database/migrations/2026_02_10_032710_create_orderinfo_table.php`
- `database/migrations/2026_02_10_032741_create_orderline_table.php`
- `database/migrations/2026_01_27_011136_add_item_id_foreign_unique_to_stock_table.php`

### 🎯 Purpose in Laravel:

Foreign key constraints ensure **referential integrity** between related tables. They prevent orphaned records by ensuring that a value in a column must exist in another table.

### 📖 Detailed Explanation:

Think of foreign keys as a relationship guarantee. If you have an order that belongs to a customer, the foreign key ensures:

- You can't create an order for a customer that doesn't exist
- You can't delete a customer who still has orders (unless you specify what to do)

Laravel provides **THREE ways** to define foreign keys in migrations:

#### Method 1: Using `foreignId()->constrained()` (Recommended - Short Syntax)

```php
$table->foreignId('user_id')->constrained('users');
```

This is the modern, shorthand way. It:

- Creates an `unsignedBigInteger` column
- Assumes the foreign table's primary key is `id`
- Automatically names the constraint

#### Method 2: Using `foreign()->references()->on()` (Traditional - Full Control)

```php
$table->foreign('customer_id')
    ->references('customer_id')  // Column in the foreign table
    ->on('customer');            // Foreign table name
```

This gives you full control when:

- Column names don't follow conventions
- Primary keys are custom named (not `id`)

#### Method 3: Creating Column First, Then Adding Constraint

```php
$table->unsignedBigInteger('customer_id');
$table->foreign('customer_id')->references('customer_id')->on('customer');
```

### 💡 Examples from Project:

**Using `foreignId()->constrained()` (Short Syntax):**

```php
// database/migrations/2026_02_10_032706_create_customer_table.php
Schema::create('customer', function (Blueprint $table) {
    $table->id('customer_id');
    $table->string('fname', 50);
    $table->string('lname', 50);
    // Short syntax - assumes 'users' table has 'id' column
    $table->foreignId('user_id')->constrained('users');
});
```

**Using `foreign()->references()->on()` (Traditional):**

```php
// database/migrations/2026_02_10_032710_create_orderinfo_table.php
Schema::create('orderinfo', function (Blueprint $table) {
    $table->id('orderinfo_id');
    $table->unsignedBigInteger('customer_id');  // Create column first
    $table->dateTime('date_placed');
    // Traditional syntax - for custom column names
    $table->foreign('customer_id')
        ->references('customer_id')
        ->on('customer');
});
```

**Multiple Foreign Keys in One Table:**

```php
// database/migrations/2026_02_10_032741_create_orderline_table.php
Schema::create('orderline', function (Blueprint $table) {
    $table->id('orderline_id');
    $table->unsignedBigInteger('orderinfo_id');
    $table->integer('item_id');
    $table->integer('quantity')->default(1);

    // Foreign key to orderinfo table
    $table->foreign('orderinfo_id')
        ->references('orderinfo_id')
        ->on('orderinfo');

    // Foreign key to item table
    $table->foreign('item_id')
        ->references('item_id')
        ->on('item');
});
```

### Visual Representation of Relationships

```
┌─────────────────┐     ┌─────────────────┐
│     users       │     │    customer     │
├─────────────────┤     ├─────────────────┤
│ id (PK)         │◄────│ user_id (FK)    │
│ name            │     │ customer_id (PK)│
│ email           │     │ fname           │
└─────────────────┘     │ lname           │
                        └────────┬────────┘
                                 │
                                 ▼
                        ┌─────────────────┐
                        │   orderinfo     │
                        ├─────────────────┤
                        │ orderinfo_id(PK)│
                        │ customer_id(FK) │◄─┐
                        │ date_placed     │  │
                        └────────┬────────┘  │
                                 │           │
                                 ▼           │
                        ┌─────────────────┐  │
                        │   orderline     │  │
                        ├─────────────────┤  │
                        │ orderline_id(PK)│  │
                        │ orderinfo_id(FK)│──┘
                        │ item_id (FK)    │
                        │ quantity        │
                        └────────┬────────┘
                                 │
                                 ▼
                        ┌─────────────────┐
                        │      item       │
                        ├─────────────────┤
                        │ item_id (PK)    │
                        │ description     │
                        └─────────────────┘
```

### ❓ Quiz Questions:

1. What is the purpose of a foreign key constraint?
2. What is the difference between `foreignId()->constrained()` and `foreign()->references()->on()`?
3. What happens if you try to insert a record with a foreign key value that doesn't exist in the referenced table?
4. Given this code: `$table->foreignId('user_id')->constrained('users');` - what column type is created?
5. Why must you create the `customer` table before the `orderinfo` table?
6. What does `constrained('users')` do?
7. How do you drop a foreign key in a migration?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="20-schema-table-modifications"></a>

## 20. Schema Table Modifications (Alter Tables)

### 📍 Where Used:

- `database/migrations/2026_01_27_011136_add_item_id_foreign_unique_to_stock_table.php`
- `database/migrations/2026_01_27_020914_add_soft_deletes_to_item_table.php`
- `database/migrations/2026_01_13_010911_add_description_to_song.php`

### 🎯 Purpose in Laravel:

`Schema::table()` allows you to modify existing tables after they've been created. This is essential for:

- Adding new columns to existing tables
- Adding indexes or foreign keys
- Modifying column properties
- Removing columns

### 📖 Detailed Explanation:

There are two main Schema methods:

- `Schema::create()` - Creates a NEW table
- `Schema::table()` - Modifies an EXISTING table

### 💡 Examples from Project:

**Adding a Foreign Key to an Existing Table:**

```php
// database/migrations/2026_01_27_011136_add_item_id_foreign_unique_to_stock_table.php
public function up(): void
{
    Schema::table('stock', function (Blueprint $table) {
        // Add foreign key with cascade delete and unique constraint
        $table->foreign('item_id')
            ->references('item_id')
            ->on('item')
            ->onDelete('cascade')
            ->unique();
    });
}

public function down(): void
{
    Schema::table('stock', function (Blueprint $table) {
        $table->dropForeign(['item_id']);  // Remove the foreign key
    });
}
```

**Adding a Column to an Existing Table:**

```php
// database/migrations/2026_01_27_020914_add_soft_deletes_to_item_table.php
public function up(): void
{
    Schema::table('item', function (Blueprint $table) {
        $table->softDeletes();  // Adds deleted_at column
    });
}

public function down(): void
{
    Schema::table('item', function (Blueprint $table) {
        $table->dropColumn('deleted_at');  // Remove the column
    });
}
```

### Common Table Modification Operations

```php
// Adding columns
$table->string('new_column')->after('existing_column');
$table->integer('quantity')->default(0);
$table->text('notes')->nullable();

// Modifying columns (requires doctrine/dbal package)
$table->string('name', 100)->change();  // Change varchar length

// Renaming columns
$table->renameColumn('old_name', 'new_name');

// Dropping columns
$table->dropColumn('column_name');
$table->dropColumn(['column1', 'column2']);

// Adding indexes
$table->unique('email');
$table->index('lastname');

// Adding foreign keys
$table->foreign('user_id')->references('id')->on('users');

// Dropping foreign keys
$table->dropForeign(['user_id']);  // By column name
$table->dropForeign('posts_user_id_foreign');  // By constraint name
```

### ❓ Quiz Questions:

1. What is the difference between `Schema::create()` and `Schema::table()`?
2. How do you add a column after a specific column?
3. What package is required to modify columns in Laravel?
4. How do you drop a foreign key constraint?
5. What method is used to remove a column?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="21-column-types-and-modifiers"></a>

## 21. Column Types and Modifiers

### 📍 Where Used:

- All migration files in `database/migrations/`

### 🎯 Purpose in Laravel:

Laravel provides many column types and modifiers to define your database schema precisely. Understanding these is crucial for proper database design.

### Column Types Used in Project

| Method                 | Description                              | Example from Project                        |
| ---------------------- | ---------------------------------------- | ------------------------------------------- |
| `id()`                 | Auto-incrementing BIGINT for primary key | `$table->id('customer_id')`                 |
| `integer()`            | INTEGER column                           | `$table->integer('item_id')`                |
| `string()`             | VARCHAR column                           | `$table->string('fname', 50)`               |
| `text()`               | TEXT column                              | `$table->text('description')`               |
| `decimal()`            | DECIMAL column with precision            | `$table->decimal('shipping', 10, 2)`        |
| `dateTime()`           | DATETIME column                          | `$table->dateTime('date_placed')`           |
| `unsignedBigInteger()` | Unsigned BIGINT (for foreign keys)       | `$table->unsignedBigInteger('customer_id')` |
| `foreignId()`          | Creates unsigned BIGINT for foreign key  | `$table->foreignId('user_id')`              |
| `softDeletes()`        | Adds nullable `deleted_at` timestamp     | `$table->softDeletes()`                     |
| `timestamps()`         | Adds `created_at` and `updated_at`       | `$table->timestamps()`                      |

### Column Modifiers Used in Project

| Modifier          | Description                | Example                                       |
| ----------------- | -------------------------- | --------------------------------------------- |
| `->primary()`     | Set as primary key         | `$table->integer('item_id')->primary()`       |
| `->nullable()`    | Allow NULL values          | `$table->string('title', 10)->nullable()`     |
| `->default()`     | Set default value          | `$table->integer('quantity')->default(0)`     |
| `->unique()`      | Add unique constraint      | `->unique()`                                  |
| `->constrained()` | Add foreign key constraint | `$table->foreignId('user_id')->constrained()` |
| `->after()`       | Place column after another | `$table->string('desc')->after('title')`      |

### 💡 Examples from Project:

```php
// database/migrations/2026_02_10_032706_create_customer_table.php
Schema::create('customer', function (Blueprint $table) {
    $table->id('customer_id');                        // BIGINT auto-increment primary key
    $table->string('title', 10)->nullable();          // VARCHAR(10), can be NULL
    $table->string('fname', 50);                      // VARCHAR(50), required
    $table->string('lname', 50);                      // VARCHAR(50), required
    $table->string('addressline', 255)->nullable();   // VARCHAR(255), optional
    $table->string('town', 100)->nullable();          // VARCHAR(100), optional
    $table->string('zipcode', 20)->nullable();        // VARCHAR(20), optional
    $table->string('phone', 20)->nullable();          // VARCHAR(20), optional
    $table->foreignId('user_id')->constrained('users'); // Foreign key to users
});

// database/migrations/2026_02_10_032710_create_orderinfo_table.php
Schema::create('orderinfo', function (Blueprint $table) {
    $table->id('orderinfo_id');                                    // Primary key
    $table->unsignedBigInteger('customer_id');                     // Foreign key column
    $table->dateTime('date_placed');                               // Order date
    $table->dateTime('date_shipped')->nullable();                  // Ship date (optional)
    $table->decimal('shipping', 10, 2)->default(0);                // Decimal with default
    $table->string('status', 50)->default('Processing');           // Status with default
    $table->foreign('customer_id')->references('customer_id')->on('customer');
});
```

### Decimal Precision Explained

```php
$table->decimal('shipping', 10, 2);
//                          ↑   ↑
//                          |   └── Scale (digits after decimal)
//                          └────── Precision (total digits)
// Allows values like: 12345678.99 (8 digits before, 2 after)
```

### ❓ Quiz Questions:

1. What column type would you use for a price field?
2. What does `$table->id('custom_id')` create?
3. How do you make a column optional (allow NULL)?
4. What is the second parameter in `$table->string('fname', 50)`?
5. What does `->default(0)` do?
6. What is the difference between `integer()` and `unsignedBigInteger()`?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="22-custom-primary-keys"></a>

## 22. Custom Primary Keys

### 📍 Where Used:

- `app/Models/Item.php` - `protected $primaryKey = 'item_id'`
- `app/Models/Stock.php` - `protected $primaryKey = 'item_id'`
- `app/Models/Customer.php` - `protected $primaryKey = 'customer_id'`
- `app/Models/Order.php` - `protected $primaryKey = 'orderinfo_id'`

### 🎯 Purpose in Laravel:

By default, Eloquent expects the primary key to be named `id`. When using different column names, you must tell Eloquent what the primary key is called.

### 📖 Detailed Explanation:

If your table uses a custom primary key name instead of `id`, you need to set the `$primaryKey` property in your model. Otherwise, methods like `find()`, `findOrFail()`, and route model binding won't work correctly.

### 💡 Examples from Project:

**Migration with Custom Primary Key:**

```php
// Using custom primary key name
Schema::create('item', function (Blueprint $table) {
    $table->integer('item_id')->primary();  // Not 'id'
    $table->string('item_name');
});

// Or using id() with custom name
Schema::create('customer', function (Blueprint $table) {
    $table->id('customer_id');  // Auto-increment BIGINT named 'customer_id'
});
```

**Model Configuration:**

```php
// app/Models/Item.php
class Item extends Model
{
    protected $table = 'item';
    protected $primaryKey = 'item_id';    // Tell Eloquent the PK name
    public $incrementing = true;          // Is it auto-incrementing? (default: true)
    protected $keyType = 'int';           // Data type (default: 'int')
}

// app/Models/Order.php
class Order extends Model
{
    protected $table = 'orderinfo';
    protected $primaryKey = 'orderinfo_id';
}
```

**Using Custom Primary Keys:**

```php
// This works because we set $primaryKey
$item = Item::find(5);  // Finds where item_id = 5
$item->item_id;         // Access the primary key value

// These also work
$item = Item::findOrFail(5);
$items = Item::whereIn('item_id', [1, 2, 3])->get();
```

### ❓ Quiz Questions:

1. What is the default primary key column name in Eloquent?
2. How do you specify a custom primary key in a model?
3. What property determines if the primary key is auto-incrementing?
4. If your primary key is a UUID string, what should `$keyType` be set to?
5. What happens if you don't set `$primaryKey` when using a custom primary key name?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="23-cascade-delete-operations"></a>

## 23. Cascade Delete Operations

### 📍 Where Used:

- `database/migrations/2026_01_27_011136_add_item_id_foreign_unique_to_stock_table.php`
- `database/migrations/2026_01_20_005928_create_table_album_listener.php`

### 🎯 Purpose in Laravel:

Cascade operations define what happens to related records when a parent record is deleted or updated. This maintains referential integrity automatically.

### 📖 Detailed Explanation:

When you delete a record that is referenced by another table's foreign key, the database needs to know what to do. Options include:

| Option      | What Happens                                 |
| ----------- | -------------------------------------------- |
| `CASCADE`   | Delete/update child records too              |
| `RESTRICT`  | Prevent deletion if children exist (default) |
| `SET NULL`  | Set foreign key to NULL in children          |
| `NO ACTION` | Similar to RESTRICT                          |

### 💡 Examples from Project:

```php
// database/migrations/2026_01_27_011136_add_item_id_foreign_unique_to_stock_table.php
Schema::table('stock', function (Blueprint $table) {
    $table->foreign('item_id')
        ->references('item_id')
        ->on('item')
        ->onDelete('cascade');  // If item is deleted, delete stock record too
});

// Pivot table with cascade
Schema::create('album_listener', function (Blueprint $table) {
    $table->unsignedBigInteger('album_id')->index();
    $table->foreign('album_id')->references('id')->on('albums')->onDelete('cascade');
    $table->unsignedBigInteger('listener_id')->index();
    $table->foreign('listener_id')->references('id')->on('listeners')->onDelete('cascade');
});
```

### All Cascade Options

```php
// Cascade delete - delete children when parent is deleted
$table->foreign('item_id')
    ->references('item_id')
    ->on('item')
    ->onDelete('cascade');

// Set null - set foreign key to null when parent is deleted
$table->foreign('user_id')
    ->references('id')
    ->on('users')
    ->onDelete('set null');  // Column must be nullable!

// Restrict - prevent deletion if children exist (default)
$table->foreign('category_id')
    ->references('id')
    ->on('categories')
    ->onDelete('restrict');

// Cascade update - update foreign key when parent key changes
$table->foreign('item_id')
    ->references('item_id')
    ->on('item')
    ->onUpdate('cascade');

// Shorthand with foreignId
$table->foreignId('user_id')
    ->constrained()
    ->cascadeOnDelete();

// Or
$table->foreignId('user_id')
    ->constrained()
    ->nullOnDelete();  // Sets to null on delete
```

### Practical Example

```
Before delete:
┌─────────────┐          ┌─────────────┐
│    item     │          │   stock     │
├─────────────┤          ├─────────────┤
│ item_id: 1  │◄─────────│ item_id: 1  │
│ item_id: 2  │◄─────────│ item_id: 2  │
└─────────────┘          └─────────────┘

DELETE FROM item WHERE item_id = 1;

After delete (with CASCADE):
┌─────────────┐          ┌─────────────┐
│    item     │          │   stock     │
├─────────────┤          ├─────────────┤
│ item_id: 2  │◄─────────│ item_id: 2  │
└─────────────┘          └─────────────┘
(Both item 1 and its stock are deleted!)
```

### ❓ Quiz Questions:

1. What does `onDelete('cascade')` do?
2. What is the default behavior when deleting a parent with child records?
3. When would you use `onDelete('set null')`?
4. What requirement must a column meet to use `set null`?
5. What is the difference between CASCADE and RESTRICT?
6. How do you apply cascade delete with the shorthand `foreignId` method?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="24-database-transactions"></a>

## 24. Database Transactions

### 📍 Where Used:

- `app/Http/Controllers/ItemController.php`

### 🎯 Purpose in Laravel:

Transactions ensure that a series of database operations either all succeed or all fail together. This maintains data integrity.

### 📖 Detailed Explanation:

Think of transactions like an all-or-nothing deal:

- Start transaction: "I'm going to make several changes"
- If all succeed: Commit (save all changes)
- If any fails: Rollback (undo all changes)

### 💡 Example from Project:

```php
// app/Http/Controllers/ItemController.php
public function postCheckout()
{
    try {
        DB::beginTransaction();  // Start transaction

        $order = new Order();
        $order->customer_id = 1;
        $order->date_placed = now();
        $order->save();

        foreach ($cart->items as $items) {
            DB::table('orderline')->insert([
                'item_id' => $items['item']['item_id'],
                'orderinfo_id' => $order->orderinfo_id,
                'quantity' => $items['qty']
            ]);

            $stock = Stock::find($items['item']['item_id']);
            $stock->quantity = $stock->quantity - $items['qty'];
            $stock->save();
        }

        DB::commit();  // Save all changes
        Session::forget('cart');
        return redirect('/')->with('success', 'Successfully Purchased!');

    } catch (\Exception $e) {
        DB::rollback();  // Undo all changes on error
        return redirect()->route('getCart')->with('error', $e->getMessage());
    }
}
```

### ❓ Quiz Questions:

1. What method starts a database transaction?
2. What method saves all transaction changes?
3. What method undoes all transaction changes?
4. Why use transactions?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="25-many-to-many-relationships-pivot-tables"></a>

## 25. Many-to-Many Relationships (Pivot Tables)

### 📍 Where Used:

- `database/migrations/2026_01_20_005928_create_table_album_listener.php`
- `app/Http/Controllers/ListenerController.php`

### 🎯 Purpose in Laravel:

To represent a many-to-many relationship between tables.

### 📖 Detailed Explanation:

When a listener can subscribe to many albums, and an album can have many listeners - you need a pivot table to store the relationship.

### 💡 Examples from Project:

```php
// Migration - Pivot table
Schema::create('album_listener', function (Blueprint $table) {
    $table->unsignedBigInteger('album_id')->index();
    $table->foreign('album_id')->references('id')->on('albums')->onDelete('cascade');
    $table->unsignedBigInteger('listener_id')->index();
    $table->foreign('listener_id')->references('id')->on('listeners')->onDelete('cascade');
    $table->primary(['album_id', 'listener_id']);
});

// Controller - Inserting to pivot table
foreach ($request->album_id as $album_id) {
    DB::table('album_listener')->insert([
        'album_id' => $album_id,
        'listener_id' => $listener->id,
        'created_at' => now()
    ]);
}
```

### ❓ Quiz Questions:

1. What is a pivot table?
2. What is the naming convention for pivot tables in Laravel?
3. How do you insert a record into a pivot table?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

# PART 4: ADVANCED FEATURES

---

<a id="26-resource-controllers"></a>

## 26. Resource Controllers

### 📍 Where Used:

- `routes/web.php` - `Route::resource()`
- `app/Http/Controllers/SongController.php`
- `app/Http/Controllers/AlbumController.php`
- `app/Http/Controllers/ItemController.php`

### 🎯 Purpose in Laravel:

Auto-generates all CRUD routes and controller methods. With just one line, all routes for Create, Read, Update, Delete operations are generated.

### 📖 Detailed Explanation:

Instead of defining 7 separate routes, you use one line:

| Verb      | URI              | Action  | Route Name    |
| --------- | ---------------- | ------- | ------------- |
| GET       | /items           | index   | items.index   |
| GET       | /items/create    | create  | items.create  |
| POST      | /items           | store   | items.store   |
| GET       | /items/{id}      | show    | items.show    |
| GET       | /items/{id}/edit | edit    | items.edit    |
| PUT/PATCH | /items/{id}      | update  | items.update  |
| DELETE    | /items/{id}      | destroy | items.destroy |

### 💡 Examples from Project:

```php
// web.php
Route::resource('songs', SongController::class);
Route::resource('albums', AlbumController::class);
Route::resource('items', ItemController::class);
Route::resource('listeners', ListenerController::class);

// app/Http/Controllers/ItemController.php
class ItemController extends Controller
{
    public function index() { /* List all items */ }
    public function create() { /* Show create form */ }
    public function store(Request $request) { /* Save new item */ }
    public function show(string $id) { /* Show single item */ }
    public function edit(string $id) { /* Show edit form */ }
    public function update(Request $request, string $id) { /* Update item */ }
    public function destroy(string $id) { /* Delete item */ }
}
```

### ❓ Quiz Questions:

1. What are the 7 default methods of a Resource Controller?
2. What is the command to create a Resource Controller?
3. What is the HTTP method for the destroy action?
4. How many routes does `Route::resource()` create?
5. What is the route name for the edit action of a 'posts' resource?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="27-route-groups-and-prefixes"></a>

## 27. Route Groups and Prefixes

### 📍 Where Used:

- `routes/web.php`
- `app/Providers/RouteServiceProvider.php`

### 🎯 Purpose in Laravel:

Route groups allow you to share route attributes (middleware, prefixes, namespaces) across multiple routes without defining them on each individual route.

### 📖 Detailed Explanation:

Route groups are like folders for routes. All routes inside a group share common properties:

- `prefix()` - Adds a URL prefix
- `middleware()` - Applies middleware to all routes
- `namespace()` - Sets controller namespace

### 💡 Examples from Project:

```php
// routes/web.php
Route::prefix('admin')->group(function () {
    Route::get('/customers', [DashboardController::class, 'getCustomers'])
        ->name('admin.customers');
    // All routes here will have /admin prefix
    // /admin/customers
});

// Middleware with group
Route::middleware(['auth'])->group(function () {
    Route::get('/artists', [ArtistController::class, 'index']);
    Route::resource('songs', SongController::class);
});

// app/Providers/RouteServiceProvider.php
$this->routes(function () {
    Route::middleware('api')
        ->prefix('api')
        ->group(base_path('routes/api.php'));

    Route::middleware('web')
        ->group(base_path('routes/web.php'));
});
```

### ❓ Quiz Questions:

1. What method adds a URL prefix to a route group?
2. How do you apply middleware to a route group?
3. Can you nest route groups?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="28-named-routes"></a>

## 28. Named Routes

### 📍 Where Used:

- `routes/web.php` - `->name('...')`
- Blade templates using `route()` helper

### 🎯 Purpose in Laravel:

To give routes an alias, making them easier to reference and maintain. Named routes allow you to generate URLs or redirects based on route names instead of hardcoded URLs.

### 📖 Detailed Explanation:

Instead of hardcoding URLs, you use the route name. If the URL changes, you don't need to change all references.

Instead of:

```php
<a href="/add-to-cart/5">Add</a>  // Hardcoded URL
```

You use:

```php
<a href="{{ route('addToCart', 5) }}">Add</a>  // Named route
```

### 💡 Examples from Project:

```php
// Defining named routes
Route::post('/user/register', [UserController::class, 'register'])->name('user.register');
Route::get('/add-to-cart/{id}', [ItemController::class, 'addToCart'])->name('addToCart');
Route::get('/shopping-cart', [ItemController::class, 'getCart'])->name('getCart');
Route::get('/checkout', [ItemController::class, 'postCheckout'])->name('checkout');

// Using named route in Blade
<form action="{{ route('user.register') }}">
<a href="{{ route('addToCart', $item->item_id) }}">Add to Cart</a>
<a href="{{ route('checkout') }}">Checkout</a>

// Using named route with parameter
<a href="{{ route('songs.edit', ['song' => $song->id]) }}">

// In Controller
return redirect()->route('getCart');
return redirect()->route('items.create')->with('success', 'item added');
```

### ❓ Quiz Questions:

1. What are named routes for?
2. How do you reference a named route in Blade?
3. How do you pass a parameter to the `route()` helper?
4. How do you name a route in Laravel?
5. What is the advantage of named routes?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="29-file-upload--storage"></a>

## 29. File Upload & Storage

### 📍 Where Used:

- `app/Http/Controllers/UserController.php`
- `app/Http/Controllers/ItemController.php`
- `resources/views/user/profile.blade.php`
- `config/filesystems.php` - Storage configuration

### 🎯 Purpose in Laravel:

To upload and store files (images, documents, etc.). Laravel provides a powerful filesystem abstraction using Flysystem.

### 📖 Detailed Explanation:

Laravel has a built-in file storage system. You can save uploaded files and access them later. The Storage facade makes file operations simple:

- Store uploaded files
- Retrieve file URLs
- Delete files
- Move/copy files

Files are typically stored in `storage/app/` directory.

### 💡 Examples from Project:

```php
// UserController.php - Uploading File
$path = $request->file('img_path')->storeAs(
    'public/images',
    $request->file('img_path')->hashName()
);

// ItemController.php - Using Storage facade
$path = Storage::putFileAs(
    'public/images',                           // Directory
    $request->file('image'),                   // File
    $request->file('image')->hashName()        // Filename (hashed)
);

$item = Item::create([
    'description' => trim($request->description),
    'cost_price' => $request->cost_price,
    'sell_price' => $request->sell_price,
    'img_path' => $path
]);

// Displaying Stored Image in Blade
<img src="{{ Storage::url($listener->img_path) }}" />
<img src="{{ Storage::url($item->img_path) }}" alt="item image">

// Form with File Upload
<form enctype="multipart/form-data">
    <input type="file" name="img_path">
</form>
```

### ❓ Quiz Questions:

1. What is the required attribute on a form for file upload?
2. What does the `hashName()` method do?
3. How do you access the stored file URL using the Storage facade?
4. What facade is used for file operations in Laravel?
5. Where are public files typically stored?
6. What command creates a symbolic link for public storage?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="30-service-providers"></a>

## 30. Service Providers

### 📍 Where Used:

- `app/Providers/` - `RouteServiceProvider.php`, `AppServiceProvider.php`, `AuthServiceProvider.php`
- `config/app.php` - Provider registration

### 🎯 Purpose in Laravel:

Central place to register and bootstrap application services. Service Providers are the "bootstrappers" of Laravel - they prepare everything before the event (application) starts.

### 📖 Detailed Explanation:

Service Providers are like organizers at an event. They:

- Register services in the container
- Register routes
- Register middleware
- Bootstrap any services

### 💡 Examples from Project:

```php
// RouteServiceProvider.php
class RouteServiceProvider extends ServiceProvider
{
    public const HOME = '/home';

    public function boot(): void
    {
        RateLimiter::for('api', function (Request $request) {
            return Limit::perMinute(60)->by($request->user()?->id ?: $request->ip());
        });

        $this->routes(function () {
            Route::middleware('api')
                ->prefix('api')
                ->group(base_path('routes/api.php'));

            Route::middleware('web')
                ->group(base_path('routes/web.php'));
        });
    }
}

// AppServiceProvider.php
class AppServiceProvider extends ServiceProvider
{
    public function register(): void
    {
        // Register bindings
    }

    public function boot(): void
    {
        // Bootstrap services
    }
}
```

### ❓ Quiz Questions:

1. What is a Service Provider in Laravel?
2. What are the two main methods of a Service Provider?
3. Where are Service Providers registered?
4. When is the `register()` method called?
5. What command creates a new service provider?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="31-facades"></a>

## 31. Facades

### 📍 Where Used:

Throughout the project:

- `Route::`, `DB::`, `Auth::`, `Session::`, `Storage::`, `Validator::`

### 🎯 Purpose in Laravel:

Facades provide a "static" interface to classes available in the service container. They offer a simple, expressive syntax while still being testable.

### 📖 Detailed Explanation:

Facades are like shortcuts. Instead of:

```php
$container->make('session')->get('key');
```

You can write:

```php
Session::get('key');
```

Behind the scenes, Laravel resolves the actual class from the service container.

### 💡 Examples from Project:

```php
// Using various facades
use Illuminate\Support\Facades\Route;
use Illuminate\Support\Facades\DB;
use Illuminate\Support\Facades\Storage;

// Route facade
Route::get('/', [ItemController::class, 'getItems']);

// DB facade
DB::table('item')->join('stock', ...)->get();

// Storage facade
Storage::putFileAs('public/images', $file, $name);

// Session facade
Session::put('cart', $cart);
Session::get('cart');

// Auth facade (via helper)
Auth::check();
Auth::user();
```

### ❓ Quiz Questions:

1. What is a Facade in Laravel?
2. Where are facades located?
3. Can facades be mocked for testing?
4. Name 5 commonly used facades.

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="32-traits"></a>

## 32. Traits

### 📍 Where Used:

- `app/Models/User.php` - `HasApiTokens`, `HasFactory`, `Notifiable`
- `app/Http/Controllers/Controller.php` - `AuthorizesRequests`, `ValidatesRequests`
- `app/Http/Controllers/Auth/LoginController.php` - `AuthenticatesUsers`

### 🎯 Purpose in Laravel:

Traits allow you to reuse code across multiple classes. Laravel uses traits extensively to provide optional functionality to models and controllers.

### 📖 Detailed Explanation:

Traits are like plugins for classes. You "use" them to add functionality:

- `HasFactory` - Adds factory methods to models
- `Notifiable` - Adds notification capabilities
- `SoftDeletes` - Adds soft delete functionality
- `AuthenticatesUsers` - Adds login functionality to controllers

### 💡 Examples from Project:

```php
// app/Models/User.php
class User extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable;
    // HasApiTokens - Can create API tokens
    // HasFactory - Can use factories
    // Notifiable - Can receive notifications
}

// app/Http/Controllers/Controller.php
class Controller extends BaseController
{
    use AuthorizesRequests, ValidatesRequests;
}

// app/Http/Controllers/Auth/LoginController.php
class LoginController extends Controller
{
    use AuthenticatesUsers;  // Provides login/logout methods
}
```

### ❓ Quiz Questions:

1. What PHP feature allows code reuse across classes?
2. What trait adds factory support to a model?
3. How do you add a trait to a class?
4. What trait adds notification support to a model?
5. What trait adds soft delete functionality?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="33-request-handling"></a>

## 33. Request Handling

### 📍 Where Used:

- `app/Http/Controllers/ItemController.php`
- All controller methods that accept `Request $request`

### 🎯 Purpose in Laravel:

The Request class provides an object-oriented way to access HTTP request data including input, files, cookies, and headers.

### 📖 Detailed Explanation:

The Request object contains everything about the incoming HTTP request:

- Form input: `$request->input('name')` or `$request->name`
- Files: `$request->file('image')`
- Query strings: `$request->query('page')`
- All data: `$request->all()`

### 💡 Examples from Project:

```php
// app/Http/Controllers/ItemController.php
public function store(Request $request)
{
    // Get input values
    $description = $request->description;
    $cost = $request->cost_price;
    $sell = $request->sell_price;
    $quantity = $request->quantity;

    // Get all input
    $allInput = $request->all();

    // Get uploaded file
    $image = $request->file('image');
    $originalName = $request->file('image')->getClientOriginalName();
    $hashedName = $request->file('image')->hashName();

    // Validate input
    $validator = Validator::make($request->all(), $rules);
}
```

### ❓ Quiz Questions:

1. What class represents HTTP requests in Laravel?
2. How do you get an input value from a request?
3. How do you get an uploaded file?
4. How do you get all request input?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="34-environment-configuration"></a>

## 34. Environment Configuration

### 📍 Where Used:

- `config/app.php`
- `config/database.php`
- `config/session.php`
- `.env` file (not in repo)

### 🎯 Purpose in Laravel:

Environment configuration allows you to have different settings for different environments (development, testing, production) without changing code.

### 📖 Detailed Explanation:

The `.env` file stores environment-specific settings:

- Database credentials
- API keys
- Debug mode
- App URL

The `env()` helper retrieves these values in config files.

### 💡 Examples from Project:

```php
// config/app.php
'name' => env('APP_NAME', 'Laravel'),      // Gets APP_NAME from .env, defaults to 'Laravel'
'env' => env('APP_ENV', 'production'),
'debug' => (bool) env('APP_DEBUG', false),

// config/database.php
'default' => env('DB_CONNECTION', 'mysql'),
'mysql' => [
    'host' => env('DB_HOST', '127.0.0.1'),
    'port' => env('DB_PORT', '3306'),
    'database' => env('DB_DATABASE', 'forge'),
    'username' => env('DB_USERNAME', 'forge'),
    'password' => env('DB_PASSWORD', ''),
],

// config/session.php
'driver' => env('SESSION_DRIVER', 'file'),
'lifetime' => env('SESSION_LIFETIME', 120),
```

### ❓ Quiz Questions:

1. What file stores environment variables in Laravel?
2. What helper retrieves environment variables?
3. Should the .env file be committed to version control?
4. What file is used as a template for .env?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

# PART 5: THIRD-PARTY PACKAGES

---

<a id="35-laravel-collective-html-forms"></a>

## 35. Laravel Collective HTML Forms

### 📍 Where Used:

- `resources/views/item/create.blade.php`
- `resources/views/item/edit.blade.php`
- `composer.json` - `laravelcollective/html`

### 🎯 Purpose in Laravel:

Laravel Collective provides a clean syntax for creating HTML forms in Blade. It handles form opening, closing, model binding, and various input types.

### 📖 Detailed Explanation:

Instead of writing raw HTML for forms, you use helper methods:

- `Form::open()` - Opens a form with proper CSRF
- `Form::text()` - Creates text input
- `Form::model()` - Binds form to a model (auto-fills values)

### 💡 Examples from Project:

```php
{{-- resources/views/item/create.blade.php --}}
{!! Form::open(['route' => 'items.store', 'files' => true]) !!}
    {!! Form::label('desc', 'item name', ['class' => 'form-label']) !!}
    {!! Form::text('description', null, ['class' => 'form-control', 'id' => 'desc']) !!}

    {!! Form::label('cost', 'cost price', ['class' => 'form-label']) !!}
    {!! Form::number('cost_price', 0.0, ['min' => 0.0, 'step' => 0.01, 'class' => 'form-control']) !!}

    {!! Form::file('image', ['class' => 'form-control']) !!}
    {!! Form::submit('Add item', ['class' => 'btn btn-primary']) !!}
{!! Form::close() !!}

{{-- resources/views/item/edit.blade.php --}}
{!! Form::model($item, ['route' => ['items.update', $item->item_id], 'method' => 'PUT', 'files' => true]) !!}
    {{-- Fields are auto-filled with $item values --}}
    {!! Form::text('description', null, ['class' => 'form-control']) !!}
{!! Form::close() !!}
```

### ❓ Quiz Questions:

1. What package provides Form helpers in Laravel?
2. What does `Form::model()` do?
3. How do you specify a PUT method in a form?
4. What option enables file uploads in a form?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="36-excel-import-maatwebsite-excel"></a>

## 36. Excel Import (Maatwebsite Excel)

### 📍 Where Used:

- `app/Imports/ItemImport.php`
- `app/Imports/ItemStockImport.php`
- `app/Http/Controllers/ItemController.php`
- `composer.json` - `maatwebsite/excel`

### 🎯 Purpose in Laravel:

Maatwebsite Excel allows you to easily import and export Excel and CSV files. It integrates with Eloquent models for seamless data handling.

### 📖 Detailed Explanation:

This package acts as a bridge between Excel files and your database:

- Import: Read Excel → Create database records
- Export: Query database → Create Excel file

### 💡 Examples from Project:

```php
// app/Imports/ItemStockImport.php
class ItemStockImport implements ToCollection, WithHeadingRow
{
    public function collection(Collection $rows)
    {
        foreach ($rows as $row) {
            $item = Item::create([
                'description' => $row["product_name"],
                'cost_price' => $row["cost_price"],
                'sell_price' => $row["sell_price"],
                'img_path' => $row["image"],
            ]);
        }
    }
}

// app/Http/Controllers/ItemController.php
public function import()
{
    Excel::import(
        new ItemStockImport,
        request()->file('item_upload')->storeAs(
            'files',
            request()->file('item_upload')->getClientOriginalName()
        )
    );
    return redirect()->back()->with('success', 'Excel file Imported Successfully');
}
```

### ❓ Quiz Questions:

1. What package is used for Excel operations in Laravel?
2. What interface is used to import Excel to a collection?
3. What does `WithHeadingRow` interface do?
4. How do you import an Excel file?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="37-datatables-yajra-datatables"></a>

## 37. DataTables (Yajra DataTables)

### 📍 Where Used:

- `app/DataTables/CustomersDataTable.php`
- `app/Http/Controllers/DashboardController.php`
- `resources/views/dashboard/customers.blade.php`
- `composer.json` - `yajra/laravel-datatables`

### 🎯 Purpose in Laravel:

Yajra DataTables provides server-side processing for jQuery DataTables. It handles pagination, searching, and sorting efficiently for large datasets.

### 📖 Detailed Explanation:

DataTables transforms a regular HTML table into an interactive table with:

- Server-side pagination (loads data as needed)
- Searching
- Sorting
- Export buttons (PDF, Excel, CSV)

### 💡 Examples from Project:

```php
// app/DataTables/CustomersDataTable.php
class CustomersDataTable extends DataTable
{
    public function dataTable($query)
    {
        return datatables()->query($query)
            ->addColumn('role',  function ($row) {
                return view('users.role', compact('row'));
            })
            ->rawColumns(['role'])
            ->setRowId('id');
    }

    public function query()
    {
        $users = DB::table('users')
            ->join('customer', 'users.id', '=', 'customer.user_id')
            ->select('users.id AS id', 'users.name', 'users.email', ...);
        return $users;
    }

    public function html(): HtmlBuilder
    {
        return $this->builder()
            ->setTableId('customers-table')
            ->columns($this->getColumns())
            ->minifiedAjax()
            ->orderBy(1)
            ->parameters([
                'dom' => 'Bfrtip',
                'buttons' => ['pdf', 'excel', 'csv', 'reload', 'print'],
            ]);
    }
}

// app/Http/Controllers/DashboardController.php
public function getCustomers(CustomersDataTable $dataTable)
{
    return $dataTable->render('dashboard.customers');
}
```

### ❓ Quiz Questions:

1. What package provides DataTables integration?
2. What method renders the DataTable in a view?
3. How do you add a custom column in DataTables?
4. What does `rawColumns()` do?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="38-carbon-datetime"></a>

## 38. Carbon (Date/Time)

### 📍 Where Used:

- `app/Http/Controllers/ItemController.php`

### 🎯 Purpose in Laravel:

Carbon is a powerful PHP library for date and time manipulation. It extends PHP's DateTime class with a fluent, intuitive API.

### 📖 Detailed Explanation:

Carbon makes working with dates easy:

- Get current date: `Carbon::now()`
- Add/subtract time: `now()->addDays(5)`
- Format dates: `$date->format('Y-m-d')`
- Compare dates: `$date1->diffInDays($date2)`

### 💡 Examples from Project:

```php
// app/Http/Controllers/ItemController.php
use Carbon\Carbon;

public function postCheckout()
{
    $order = new Order();
    $order->customer_id = 1;
    $order->date_placed = now();                    // Current timestamp
    $order->date_shipped = Carbon::now()->addDays(5); // 5 days from now
    $order->save();
}
```

### ❓ Quiz Questions:

1. What library is used for date/time in Laravel?
2. How do you get the current date/time in Laravel?
3. How do you add days to a date?
4. Is Carbon included in Laravel by default?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="39-vite-asset-bundling"></a>

## 39. Vite (Asset Bundling)

### 📍 Where Used:

- `vite.config.js`
- `resources/views/layouts/app.blade.php`
- `resources/sass/`
- `resources/js/`

### 🎯 Purpose in Laravel:

Vite is Laravel's default build tool for compiling and bundling frontend assets (JavaScript, CSS/SCSS). It provides fast hot module replacement during development.

### 📖 Detailed Explanation:

Vite takes your raw assets and:

- Compiles SCSS to CSS
- Bundles JavaScript modules
- Minifies files for production
- Provides hot reloading during development

### 💡 Examples from Project:

```javascript
// vite.config.js
import { defineConfig } from "vite";
import laravel from "laravel-vite-plugin";

export default defineConfig({
  plugins: [
    laravel({
      input: ["resources/sass/app.scss", "resources/js/app.js"],
      refresh: true,
    }),
  ],
});
```

```php
{{-- resources/views/layouts/app.blade.php --}}
@vite(['resources/sass/app.scss', 'resources/js/app.js'])
```

### ❓ Quiz Questions:

1. What is the default asset bundler in Laravel?
2. What Blade directive includes Vite assets?
3. What command runs Vite in development mode?
4. What command builds assets for production?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="40-laravel-sanctum-api-authentication"></a>

## 40. Laravel Sanctum (API Authentication)

### 📍 Where Used:

- `routes/api.php`
- `app/Models/User.php` - `HasApiTokens` trait
- `composer.json` - `laravel/sanctum`

### 🎯 Purpose in Laravel:

Sanctum provides a lightweight authentication system for SPAs, mobile applications, and simple token-based APIs.

### 📖 Detailed Explanation:

Sanctum offers:

- Token-based authentication for APIs
- Session-based authentication for SPAs
- Simple, secure token management

### 💡 Examples from Project:

```php
// routes/api.php
Route::middleware('auth:sanctum')->get('/user', function (Request $request) {
    return $request->user();
});

// app/Models/User.php
class User extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable;  // HasApiTokens for Sanctum

    // ...
}
```

### ❓ Quiz Questions:

1. What package provides lightweight API authentication?
2. What trait enables Sanctum tokens in a User model?
3. What middleware protects API routes with Sanctum?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

# PART 6: ARTISAN CLI COMMANDS

---

<a id="41-what-is-artisan"></a>

## 41. What is Artisan?

Artisan is Laravel's built-in command-line interface (CLI). It provides helpful commands for development tasks like creating files, running migrations, clearing cache, and more.

### Basic Syntax:

```bash
php artisan <command> [options] [arguments]
```

### Get Help on Any Command:

```bash
php artisan help <command>
php artisan <command> --help
```

### List All Available Commands:

```bash
php artisan list
```

[⬆ Back to Top](#table-of-contents)

---

<a id="42-migration-commands"></a>

## 42. Migration Commands

Migrations are like version control for your database schema.

### `php artisan migrate`

**What it does:** Runs all pending migrations (creates/modifies database tables)

```bash
php artisan migrate
```

- Executes the `up()` method in each migration file
- Creates tables defined in `database/migrations/`
- Tracks which migrations have run in `migrations` table

### `php artisan migrate:rollback`

**What it does:** Reverts the last batch of migrations

```bash
php artisan migrate:rollback
php artisan migrate:rollback --step=3  # Rollback last 3 batches
```

- Executes the `down()` method in migrations
- Rolls back only the last batch

### `php artisan migrate:reset`

**What it does:** Rolls back ALL migrations (empties all tables)

```bash
php artisan migrate:reset
```

- ⚠️ WARNING: You will lose all data!

### `php artisan migrate:refresh`

**What it does:** Rolls back all migrations then re-runs them

```bash
php artisan migrate:refresh
php artisan migrate:refresh --seed  # Refresh and seed data
```

- Same as: `migrate:reset` + `migrate`

### `php artisan migrate:fresh`

**What it does:** Drops ALL tables then runs all migrations

```bash
php artisan migrate:fresh
php artisan migrate:fresh --seed  # Fresh migrate and seed
```

- Faster than `migrate:refresh`
- Doesn't run `down()` methods - just drops tables directly
- ⚠️ WARNING: You will lose all data!

### `php artisan migrate:status`

**What it does:** Shows which migrations have run and which are pending

```bash
php artisan migrate:status
```

### ❓ Migration Quiz Questions:

1. What is the difference between `migrate:refresh` and `migrate:fresh`?
2. What does `migrate:rollback --step=2` do?
3. Which command shows pending migrations?
4. What happens when you run `php artisan migrate`?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="43-make-commands-code-generators"></a>

## 43. Make Commands (Code Generators)

These commands generate boilerplate code files.

### `php artisan make:model`

**What it does:** Creates a new Eloquent Model class

```bash
php artisan make:model Album
php artisan make:model Album -m           # Model + Migration
php artisan make:model Album -mc          # Model + Migration + Controller
php artisan make:model Album -mcr         # Model + Migration + Resource Controller
php artisan make:model Album --all        # Model + Migration + Factory + Seeder + Controller
```

**Flags:**
| Flag | What it creates |
|------|-----------------|
| `-m` | Migration file |
| `-c` | Controller |
| `-r` | Resource Controller (with CRUD methods) |
| `-f` | Factory |
| `-s` | Seeder |
| `--all` | All of the above |

### `php artisan make:controller`

**What it does:** Creates a new Controller class

```bash
php artisan make:controller ArtistController
php artisan make:controller ArtistController -r      # Resource controller
php artisan make:controller ArtistController --resource  # Same as above
php artisan make:controller ArtistController --api   # API resource (no create/edit)
```

**Resource Controller generates these methods:**

- `index()` - Display list
- `create()` - Show create form
- `store()` - Save new record
- `show()` - Display single record
- `edit()` - Show edit form
- `update()` - Update record
- `destroy()` - Delete record

### `php artisan make:migration`

**What it does:** Creates a new database migration file

```bash
php artisan make:migration create_songs_table
php artisan make:migration add_description_to_songs_table --table=songs
php artisan make:migration create_listeners_table --create=listeners
```

**Naming Convention:**

- `create_xxx_table` - For creating new tables
- `add_xxx_to_yyy_table` - For adding columns
- `remove_xxx_from_yyy_table` - For removing columns

### `php artisan make:seeder`

**What it does:** Creates a new database seeder class

```bash
php artisan make:seeder AlbumSeeder
```

### `php artisan make:factory`

**What it does:** Creates a new model factory

```bash
php artisan make:factory AlbumFactory
php artisan make:factory AlbumFactory --model=Album  # Link to model
```

### `php artisan make:middleware`

**What it does:** Creates a new middleware class

```bash
php artisan make:middleware CheckAge
```

### `php artisan make:request`

**What it does:** Creates a Form Request class for validation

```bash
php artisan make:request StoreSongRequest
```

### ❓ Make Commands Quiz Questions:

1. What does `php artisan make:model Album -mcr` create?
2. Where is the controller file created?
3. What is the difference between `-r` and `--api` in make:controller?
4. What naming convention should you use for migrations?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="44-database-seeding-commands"></a>

## 44. Database Seeding Commands

### `php artisan db:seed`

**What it does:** Runs the database seeders to populate data

```bash
php artisan db:seed
php artisan db:seed --class=AlbumSeeder  # Run specific seeder only
```

- Runs `database/seeders/DatabaseSeeder.php` by default

### ❓ Seeding Quiz Questions:

1. What file does `db:seed` run by default?
2. How do you run only a specific seeder?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="45-cache-commands"></a>

## 45. Cache Commands

Laravel caches various things for performance.

### `php artisan cache:clear`

**What it does:** Clears the application cache

```bash
php artisan cache:clear
```

- Clears cached data stored by your application

### `php artisan config:clear`

**What it does:** Clears the configuration cache

```bash
php artisan config:clear
```

- Use after changing `.env` or config files

### `php artisan config:cache`

**What it does:** Creates a cache file for faster config loading

```bash
php artisan config:cache
```

- ⚠️ Must run again after changing any config

### `php artisan route:clear`

**What it does:** Clears the route cache

```bash
php artisan route:clear
```

### `php artisan route:cache`

**What it does:** Creates a cache file for faster route registration

```bash
php artisan route:cache
```

- ⚠️ Cannot use with Closure routes (must use controllers)

### `php artisan view:clear`

**What it does:** Clears compiled view files

```bash
php artisan view:clear
```

### `php artisan view:cache`

**What it does:** Compiles all Blade views

```bash
php artisan view:cache
```

### `php artisan optimize:clear`

**What it does:** Clears ALL cached files

```bash
php artisan optimize:clear
```

- Clears: config, route, view, and general cache
- One command to clear everything

### ❓ Cache Commands Quiz Questions:

1. Which command clears all types of cache?
2. When should you run `config:clear`?
3. What's the difference between `cache:clear` and `config:clear`?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="46-server--development-commands"></a>

## 46. Server & Development Commands

### `php artisan serve`

**What it does:** Starts the Laravel development server

```bash
php artisan serve
php artisan serve --port=8080           # Custom port
php artisan serve --host=192.168.1.100  # Custom host
```

- Starts server at `http://127.0.0.1:8000`
- For development only (not production!)

### `php artisan tinker`

**What it does:** Opens an interactive PHP shell (REPL)

```bash
php artisan tinker
```

- Interact with your application from command line
- Test Eloquent queries, create records, etc.

```php
>>> User::all()                    // Get all users
>>> Album::create(['title'=>'Test'])  // Create album
>>> $user = User::find(1)          // Find user
>>> exit                           // Exit tinker
```

### ❓ Server Commands Quiz Questions:

1. What is the default port for `php artisan serve`?
2. What is Tinker used for?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="47-storage-commands"></a>

## 47. Storage Commands

### `php artisan storage:link`

**What it does:** Creates a symbolic link from `public/storage` to `storage/app/public`

```bash
php artisan storage:link
```

- Makes uploaded files accessible from the web
- Required for file uploads to work with `Storage::url()`
- Creates: `public/storage` → `storage/app/public`

### ❓ Storage Commands Quiz Questions:

1. Why do we need to run `storage:link`?
2. Where does the symbolic link point to?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="48-route-commands"></a>

## 48. Route Commands

### `php artisan route:list`

**What it does:** Displays all registered routes

```bash
php artisan route:list
php artisan route:list --name=user      # Filter by name
php artisan route:list --path=api       # Filter by path
php artisan route:list --method=GET     # Filter by method
php artisan route:list -v               # Verbose (show middleware)
```

- Shows: Method, URI, Name, Action, Middleware
- Very useful for debugging routes

### ❓ Route Commands Quiz Questions:

1. How do you see all registered routes?
2. How do you filter routes by name?

📝 [View Answer Key](#answer-key)

[⬆ Back to Top](#table-of-contents)

---

<a id="49-key--environment-commands"></a>

## 49. Key & Environment Commands

### `php artisan key:generate`

**What it does:** Generates a new application encryption key

```bash
php artisan key:generate
```

- Sets `APP_KEY` in `.env` file
- Required for encryption, sessions, cookies
- Run once when setting up new project

### `php artisan env`

**What it does:** Displays the current environment

```bash
php artisan env
```

- Shows: `local`, `production`, `testing`, etc.

[⬆ Back to Top](#table-of-contents)

---

<a id="50-queue--testing-commands"></a>

## 50. Queue & Testing Commands

### `php artisan queue:work`

**What it does:** Processes jobs in the queue

```bash
php artisan queue:work
```

- Processes queued jobs (emails, notifications, etc.)
- Runs continuously until stopped

### `php artisan queue:listen`

**What it does:** Listens for queue jobs (reloads on code changes)

```bash
php artisan queue:listen
```

- Better for development

### `php artisan test`

**What it does:** Runs the application tests

```bash
php artisan test
php artisan test --filter=UserTest  # Run specific test
```

- Runs PHPUnit tests
- Tests in `tests/` directory

[⬆ Back to Top](#table-of-contents)

---

<a id="summary-tables"></a>

## 📋 Summary Tables

### Core Topics Summary

| Topic                | Files/Location             | Key Concepts           |
| -------------------- | -------------------------- | ---------------------- |
| MVC                  | Models, Views, Controllers | Separation of concerns |
| Routing              | routes/web.php             | URL handling           |
| Middleware           | Kernel.php, web.php        | Request filtering      |
| Eloquent ORM         | app/Models/                | Database operations    |
| Mass Assignment      | $fillable in Models        | Security               |
| Blade                | .blade.php files           | Templating             |
| Migrations           | database/migrations/       | Database schema        |
| Soft Deletes         | SoftDeletes trait          | Data recovery          |
| Seeding              | database/seeders/          | Test data              |
| Query Builder        | DB facade                  | Complex queries        |
| Pagination           | paginate()                 | Data splitting         |
| Validation           | Controllers                | Input checking         |
| Authentication       | Auth facade                | User login             |
| Resource Controllers | Route::resource()          | CRUD automation        |
| File Upload          | Storage facade             | File handling          |
| CSRF                 | @csrf                      | Security               |
| Hashing              | bcrypt()                   | Password security      |
| Named Routes         | ->name()                   | URL management         |
| Service Providers    | app/Providers/             | App bootstrapping      |
| Pivot Tables         | album_listener             | Many-to-Many           |
| Foreign Keys         | Migrations                 | Referential integrity  |
| Transactions         | DB::transaction()          | Data integrity         |

### Advanced Database Topics

| Topic                      | Migration File                                | Key Concept                      |
| -------------------------- | --------------------------------------------- | -------------------------------- |
| Foreign Keys               | create_customer_table.php                     | `foreignId()->constrained()`     |
| Foreign Keys (Traditional) | create_orderinfo_table.php                    | `foreign()->references()->on()`  |
| Multiple FKs               | create_orderline_table.php                    | Multiple references in one table |
| Soft Deletes               | add_soft_deletes_to_item_table.php            | `$table->softDeletes()`          |
| Table Modifications        | add_item_id_foreign_unique_to_stock_table.php | `Schema::table()`                |
| Cascade Delete             | add_item_id_foreign_unique_to_stock_table.php | `->onDelete('cascade')`          |
| Custom Primary Keys        | All models                                    | `protected $primaryKey`          |

[⬆ Back to Top](#table-of-contents)

---

<a id="quick-artisan-cheat-sheet"></a>

## 🎯 Quick Artisan Cheat Sheet

### Most Used Commands

| Command                               | Description                           |
| ------------------------------------- | ------------------------------------- |
| `php artisan serve`                   | Start dev server                      |
| `php artisan migrate`                 | Run migrations                        |
| `php artisan migrate:fresh --seed`    | Reset DB and seed                     |
| `php artisan make:model Name -mcr`    | Create model + migration + controller |
| `php artisan make:controller Name -r` | Create resource controller            |
| `php artisan db:seed`                 | Run seeders                           |
| `php artisan route:list`              | Show all routes                       |
| `php artisan tinker`                  | Interactive shell                     |
| `php artisan optimize:clear`          | Clear all cache                       |
| `php artisan storage:link`            | Link storage folder                   |

### Quick Command Groups

**Migrations:**

```bash
php artisan migrate              # Run migrations
php artisan migrate:rollback     # Rollback last migration
php artisan migrate:fresh        # Drop all tables and re-run migrations
```

**Seeders:**

```bash
php artisan db:seed              # Run seeders
php artisan db:seed --class=SongSeeder  # Run specific seeder
```

**Make Commands:**

```bash
php artisan make:model ModelName -m     # Create model with migration
php artisan make:controller ControllerName -r  # Create resource controller
php artisan make:migration create_table_name   # Create migration
php artisan make:seeder SeederName      # Create seeder
```

**Cache:**

```bash
php artisan config:cache         # Cache configuration
php artisan route:cache          # Cache routes
php artisan cache:clear          # Clear cache
php artisan optimize:clear       # Clear all cache
```

**Storage:**

```bash
php artisan storage:link         # Create symbolic link for storage
```

[⬆ Back to Top](#table-of-contents)

---

<a id="common-command-patterns"></a>

## 📝 Common Command Patterns

### Creating a Complete Resource

```bash
# Option 1: All at once
php artisan make:model Album -mcr

# Option 2: Step by step
php artisan make:model Album
php artisan make:migration create_albums_table
php artisan make:controller AlbumController -r
php artisan make:seeder AlbumSeeder
php artisan make:factory AlbumFactory
```

### Database Reset Workflow

```bash
# Method 1: Fresh (faster, drops all tables)
php artisan migrate:fresh --seed

# Method 2: Refresh (runs down() methods)
php artisan migrate:refresh --seed

# Method 3: Manual
php artisan migrate:reset
php artisan migrate
php artisan db:seed
```

### Development Workflow

```bash
# 1. Create new feature (Model + Migration + Controller)
php artisan make:model Song -mcr

# 2. Edit migration file, then run it
php artisan migrate

# 3. Create seeder for test data
php artisan make:seeder SongSeeder

# 4. Run seeder
php artisan db:seed --class=SongSeeder

# 5. Check routes
php artisan route:list

# 6. Start server
php artisan serve
```

### When Things Go Wrong

```bash
# Clear everything
php artisan optimize:clear

# Or clear individually:
php artisan cache:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear

# Rebuild database
php artisan migrate:fresh --seed
```

[⬆ Back to Top](#table-of-contents)

---

## 💡 Pro Tips

1. **Use Tab Completion**: Type `php artisan` and press Tab to see available commands

2. **Get Help**: Add `--help` to any command to see options

   ```bash
   php artisan make:model --help
   ```

3. **Combine Flags**: Use multiple flags together

   ```bash
   php artisan make:model Post -mcrfs  # model, migration, controller, resource, factory, seeder
   ```

4. **Use Aliases**: Create bash/terminal aliases for common commands

   ```bash
   alias pa="php artisan"
   alias pam="php artisan migrate"
   alias pamf="php artisan migrate:fresh --seed"
   ```

5. **Interactive Mode**: Use `tinker` to test queries before putting them in code

---

## 📋 Quick Reference Card

### Foreign Key Syntax

```php
// Modern shorthand
$table->foreignId('user_id')->constrained('users');

// Traditional with full control
$table->foreign('custom_id')
    ->references('other_id')
    ->on('other_table')
    ->onDelete('cascade');

// With cascade
$table->foreignId('user_id')
    ->constrained()
    ->cascadeOnDelete()
    ->cascadeOnUpdate();
```

### Soft Deletes

```php
// Migration
$table->softDeletes();

// Model
use SoftDeletes;  // trait

// Queries
Model::withTrashed()->get();    // Include deleted
Model::onlyTrashed()->get();    // Only deleted
$model->restore();              // Restore
$model->forceDelete();          // Permanent delete
```

### Table Modification

```php
// Add to existing table
Schema::table('table', function ($table) {
    $table->string('new_column');
});

// Drop from existing table
Schema::table('table', function ($table) {
    $table->dropColumn('column');
    $table->dropForeign(['column']);
});
```

---

[⬆ Back to Top](#table-of-contents)

---

<a id="answer-key"></a>

# 📝 ANSWER KEY FOR ALL QUIZZES

---

## Part 1: Core Concepts Answers

### 1. MVC Architecture Answers

1. **What does MVC stand for?** - Model-View-Controller
2. **What is the role of the Model?** - Handles data logic and interacts with the database
3. **What is the role of the View?** - Displays data to users (UI/presentation layer)
4. **What is the role of the Controller?** - Handles user input, coordinates between Model and View
5. **Where are Models located in Laravel?** - `app/Models/`

### 2. Routing Answers

1. **Where are web routes defined?** - `routes/web.php`
2. **What file handles API routes?** - `routes/api.php`
3. **What does `Route::resource()` generate?** - All 7 CRUD routes (index, create, store, show, edit, update, destroy)
4. **What are the HTTP methods for CRUD operations?** - GET (Read), POST (Create), PUT/PATCH (Update), DELETE (Delete)
5. **How do you pass a parameter in a route?** - `{parameter}` e.g., `Route::get('/user/{id}')`
6. **What is the difference between PUT and PATCH?** - PUT replaces the entire resource, PATCH updates partial data

### 3. Controllers Answers

1. **Where are controllers stored?** - `app/Http/Controllers/`
2. **What command creates a resource controller?** - `php artisan make:controller ControllerName -r` or `--resource`
3. **What is a resource controller?** - A controller with pre-defined CRUD methods (index, create, store, show, edit, update, destroy)
4. **How do you inject a model in a controller method?** - Type-hint the model in the method parameter (Route Model Binding)

### 4. Middleware Answers

1. **What is the purpose of middleware?** - Filters HTTP requests entering your application
2. **Where is middleware registered?** - `app/Http/Kernel.php`
3. **What middleware function checks if user is logged in?** - `auth` middleware
4. **How do you apply middleware to a route?** - `->middleware('middleware_name')`
5. **What does the `guest` middleware do?** - Redirects authenticated users away (used for login pages)
6. **Can you create custom middleware?** - Yes, using `php artisan make:middleware`
7. **Where are custom middleware files stored?** - `app/Http/Middleware/`

### 5. Eloquent ORM Answers

1. **What does ORM stand for?** - Object-Relational Mapping
2. **What does `$fillable` do?** - Specifies which attributes can be mass-assigned
3. **What is the difference between `find()` and `findOrFail()`?** - `find()` returns null if not found, `findOrFail()` throws a 404 exception
4. **How do you define a one-to-many relationship?** - `hasMany()` in parent model, `belongsTo()` in child model
5. **What is the default primary key name in Eloquent?** - `id`
6. **What is the default table name convention?** - Plural, snake_case of model name (User → users)
7. **How do you create a new record?** - `Model::create([])` or `$model = new Model; $model->save();`
8. **What method updates a record?** - `$model->update([])` or `$model->save()` after modifying

### 6. Mass Assignment Answers

1. **What is mass assignment?** - Assigning multiple attributes at once (e.g., `Model::create($request->all())`)
2. **What happens without `$fillable`?** - Mass assignment exception is thrown (security protection)
3. **Alternative to `$fillable`?** - `$guarded` - specifies attributes that cannot be mass-assigned

### 7. Blade Templating Answers

1. **What is Blade?** - Laravel's templating engine
2. **What does `{{ }}` do?** - Echoes escaped data (XSS safe)
3. **What does `{!! !!}` do?** - Echoes unescaped/raw HTML
4. **What directive extends a layout?** - `@extends('layout.name')`
5. **What directive defines a section?** - `@section('name')` / `@endsection`
6. **What directive includes content from parent section?** - `@yield('name')` or `@parent`
7. **How do you loop in Blade?** - `@foreach`, `@for`, `@while`, `@forelse`
8. **What is `@forelse` used for?** - Loop with empty fallback (handles empty collections)

### 8. Migrations Answers

1. **What method runs when migrating?** - `up()`
2. **What method runs when rolling back?** - `down()`
3. **What command runs migrations?** - `php artisan migrate`
4. **What command rolls back last migration?** - `php artisan migrate:rollback`
5. **Where are migrations stored?** - `database/migrations/`
6. **How do you create a migration?** - `php artisan make:migration migration_name`
7. **What is `Schema::create()` vs `Schema::table()`?** - `create()` makes new table, `table()` modifies existing
8. **What does `nullable()` do?** - Allows column to have NULL values

### 9. Seeders Answers

1. **What are seeders used for?** - Populating the database with test/sample data
2. **How do you run seeders?** - `php artisan db:seed`
3. **Where are seeders located?** - `database/seeders/`
4. **What is `DatabaseSeeder.php`?** - The main seeder that calls other seeders
5. **How do you run a specific seeder?** - `php artisan db:seed --class=SeederName`

### 10. Factories Answers

1. **What are factories for?** - Generating fake data for testing using Faker
2. **Where are factories located?** - `database/factories/`
3. **How do you generate multiple records?** - `Model::factory()->count(50)->create()`
4. **What library does Factory use?** - Faker PHP library

---

## Part 2: Security & Authentication Answers

### 11. Authentication Answers

1. **What facade handles authentication?** - `Auth`
2. **How do you get the authenticated user?** - `Auth::user()` or `auth()->user()`
3. **How do you check if user is logged in?** - `Auth::check()` or `@auth` directive
4. **What does `Auth::attempt()` do?** - Attempts to log in a user with credentials, returns boolean
5. **How do you logout a user?** - `Auth::logout()`
6. **What middleware protects authenticated routes?** - `auth`
7. **What Blade directive shows content to logged-in users?** - `@auth` / `@endauth`

### 12. Session Management Answers

1. **How do you store session data?** - `session(['key' => 'value'])` or `Session::put()`
2. **How do you retrieve session data?** - `session('key')` or `Session::get('key')`
3. **How do you delete session data?** - `Session::forget('key')` or `Session::flush()` for all
4. **What is the default session driver?** - `file`
5. **Where is session configuration?** - `config/session.php`

### 13. Form Validation Answers

1. **How do you validate requests?** - `$request->validate([rules])`
2. **What happens on validation failure?** - Redirects back with errors in `$errors` bag
3. **What directive shows validation errors?** - `@error('field')` / `@enderror`
4. **How do you create custom validation rules?** - `php artisan make:rule RuleName`
5. **What is a Form Request?** - Separate class for complex validation (`php artisan make:request`)
6. **How do you display all errors?** - `$errors->all()` or loop through `$errors`

### 14. CSRF Protection Answers

1. **What does CSRF stand for?** - Cross-Site Request Forgery
2. **What directive adds CSRF token to forms?** - `@csrf`
3. **Where is CSRF middleware registered?** - `app/Http/Kernel.php` (web middleware group)
4. **What happens without CSRF token?** - 419 Page Expired error
5. **How does CSRF protection work?** - Token is generated per session, verified on each POST/PUT/DELETE request
6. **Can you exclude routes from CSRF?** - Yes, in `VerifyCsrfToken` middleware's `$except` array
7. **What HTTP methods require CSRF?** - POST, PUT, PATCH, DELETE (state-changing methods)

### 15. Hashing Answers

1. **What function hashes passwords?** - `bcrypt('password')` or `Hash::make('password')`
2. **How do you verify a hashed password?** - `Hash::check('plain', $hashedPassword)`
3. **Should you ever store plain passwords?** - NO, never! Always hash passwords

---

## Part 3: Database Operations Answers

### 16. Query Builder Answers

1. **What generates raw SQL SELECT?** - `DB::select()` or `DB::table()->select()`
2. **What is the difference between DB and Eloquent?** - DB is query builder (raw), Eloquent is ORM (models)
3. **How do you join tables?** - `->join('table', 'a.id', '=', 'b.a_id')`
4. **What method selects specific columns?** - `->select('col1', 'col2')`
5. **What method groups results?** - `->groupBy('column')`
6. **What does `pluck()` return?** - Collection of single column values
7. **How do you order results?** - `->orderBy('column', 'asc/desc')`

### 17. Pagination Answers

1. **How do you paginate results?** - `Model::paginate(10)`
2. **What method renders pagination links?** - `{{ $items->links() }}`
3. **What is default items per page?** - 15

### 18. Soft Deletes Answers

1. **What trait enables soft deletes?** - `SoftDeletes`
2. **What column stores deletion timestamp?** - `deleted_at`
3. **How do you restore a soft-deleted record?** - `$model->restore()`
4. **How do you include soft-deleted records in queries?** - `->withTrashed()`
5. **How do you get only soft-deleted records?** - `->onlyTrashed()`
6. **How do you permanently delete?** - `->forceDelete()`
7. **What migration method adds soft delete column?** - `$table->softDeletes()`
8. **Does `Model::all()` include soft-deleted?** - No
9. **What method checks if record is soft-deleted?** - `$model->trashed()`

### 19. Foreign Key Constraints Answers

1. **What does `foreignId()` create?** - An unsigned bigInteger column for foreign key
2. **What does `constrained()` do?** - Automatically references the related table's id
3. **What is the difference between `constrained()` and `references()->on()`?** - `constrained()` is shorthand, `references()->on()` gives more control
4. **How do you add cascade delete?** - `->onDelete('cascade')` or `->cascadeOnDelete()`
5. **What happens on foreign key violation?** - Database throws integrity constraint violation error
6. **How do you drop a foreign key?** - `$table->dropForeign(['column_name'])`
7. **Can a table have multiple foreign keys?** - Yes, unlimited

### 20. Schema Table Modifications Answers

1. **What method modifies existing tables?** - `Schema::table()`
2. **What method creates new tables?** - `Schema::create()`
3. **How do you add a column after another?** - `->after('column_name')`
4. **How do you rename a table?** - `Schema::rename('old', 'new')`
5. **How do you drop a table?** - `Schema::dropIfExists('table')`

### 21. Column Types Answers

1. **What stores long text?** - `text()`, `mediumText()`, `longText()`
2. **What stores decimal money?** - `decimal('price', 8, 2)`
3. **What stores true/false?** - `boolean()`
4. **What stores dates?** - `date()`, `dateTime()`, `timestamp()`
5. **What does `timestamps()` create?** - `created_at` and `updated_at` columns
6. **How do you add `remember_token`?** - `$table->rememberToken()`

### 22. Custom Primary Keys Answers

1. **How do you set custom primary key name?** - `protected $primaryKey = 'column_name';`
2. **What happens if PK is non-incrementing?** - Set `public $incrementing = false;`
3. **What if PK is not integer?** - Set `protected $keyType = 'string';`
4. **Does Laravel require primary keys?** - Not required but highly recommended
5. **Can you use composite primary keys?** - Limited support through `primaryKey` array

### 23. Cascade Delete Answers

1. **What does `onDelete('cascade')` do?** - Deletes related records when parent is deleted
2. **What does `onDelete('set null')` do?** - Sets foreign key to NULL when parent is deleted
3. **What does `onDelete('restrict')` do?** - Prevents deletion if related records exist
4. **Where is cascade defined?** - In the migration when creating foreign key
5. **Modern method for cascade?** - `->cascadeOnDelete()` and `->cascadeOnUpdate()`
6. **When to use cascade vs manual delete?** - Cascade for strict relationships, manual for soft deletes or complex logic

---

## Part 4: Advanced Features Answers

### 24. Database Transactions Answers

1. **What does `DB::transaction()` do?** - Wraps queries in a transaction (all succeed or all fail)
2. **When should you use transactions?** - When multiple related database operations must succeed together
3. **What happens on error in a transaction?** - Everything is rolled back automatically
4. **Manual transaction methods?** - `DB::beginTransaction()`, `DB::commit()`, `DB::rollBack()`

### 25. Pivot Tables Answers

1. **What are pivot tables for?** - Many-to-many relationships
2. **Naming convention for pivot tables?** - Singular, alphabetical order (e.g., `album_listener`)
3. **What methods define many-to-many?** - `belongsToMany()` in both models

### 26. Resource Controllers Answers

1. **What are the 7 resource methods?** - index, create, store, show, edit, update, destroy
2. **How do you register resource routes?** - `Route::resource('name', Controller::class)`
3. **How do you limit resource methods?** - `->only(['index', 'show'])` or `->except(['destroy'])`
4. **What is API resource?** - Resource without create/edit views (`--api` flag)
5. **Route names convention?** - `resource.action` (e.g., `songs.index`, `songs.store`)

### 27. Route Groups Answers

1. **What does `prefix()` do?** - Adds URL prefix to all routes in group
2. **What does `name()` do?** - Adds name prefix to all routes in group
3. **What does `middleware()` do on group?** - Applies middleware to all routes in group

### 28. Named Routes Answers

1. **What does `->name()` do?** - Assigns a name to a route for easy reference
2. **How do you generate URL from named route?** - `route('route.name')` or `route('route.name', $id)`
3. **Why use named routes?** - URLs update automatically if URI changes
4. **What helper generates URLs?** - `route()` function
5. **Can you pass parameters to named routes?** - Yes, `route('route.name', ['id' => 1])`

### 29. File Upload/Storage Answers

1. **What facade handles file storage?** - `Storage`
2. **Where are uploaded files stored?** - `storage/app/public/` (or configured disk)
3. **What command makes storage accessible?** - `php artisan storage:link`
4. **How do you store uploaded file?** - `$request->file('input')->store('folder')` or `Storage::put()`
5. **How do you get file URL?** - `Storage::url('path')` or `asset('storage/path')`
6. **How do you delete a file?** - `Storage::delete('path')`

### 30. Service Providers Answers

1. **What are service providers?** - Bootstrap classes that register and configure application services
2. **Where are they registered?** - `config/app.php` in `providers` array
3. **What are the two main methods?** - `register()` and `boot()`
4. **When is `register()` called?** - First, before any services are available
5. **When is `boot()` called?** - After all services are registered

### 31. Facades Answers

1. **What are Facades?** - Static-like interfaces to classes in the service container
2. **Name 3 common facades** - `Auth`, `DB`, `Route`, `Cache`, `Session`, `Storage`, `Mail`, etc.
3. **Where are facades defined?** - `Illuminate\Support\Facades\`
4. **Can you use facades anywhere?** - Yes, globally available

### 32. Traits Answers

1. **What are traits?** - Reusable code blocks that can be inserted into classes
2. **How do you use a trait?** - `use TraitName;` inside a class
3. **Name 3 Laravel traits** - `SoftDeletes`, `HasApiTokens`, `HasFactory`, `Notifiable`, `Authenticatable`
4. **Can a class use multiple traits?** - Yes, `use Trait1, Trait2;`
5. **Where do you typically use traits?** - In Models for shared functionality

### 33. Request Handling Answers

1. **How do you get all input?** - `$request->all()` or `$request->input()`
2. **How do you get specific input?** - `$request->input('key')` or `$request->key`
3. **How do you check if input exists?** - `$request->has('key')` or `$request->filled('key')`
4. **What is Request injection?** - Type-hinting `Request $request` in controller methods

### 34. Environment Configuration Answers

1. **What file stores environment variables?** - `.env`
2. **How do you access env values?** - `env('KEY')` or `config('key')`
3. **Should `.env` be in version control?** - NO! It contains sensitive data
4. **What file is an example?** - `.env.example` (safe to commit)

---

## Part 5: Packages & Tools Answers

### 35. Laravel Collective Answers

1. **What does Laravel Collective provide?** - HTML and Form helpers/components
2. **How to create a form?** - `{!! Form::open(['route' => 'route.name']) !!}`
3. **How to create form input?** - `{!! Form::text('name', $value, ['class' => 'form-control']) !!}`
4. **Benefits of Collective?** - Auto CSRF, model binding, cleaner syntax

### 36. Excel Import/Export Answers

1. **What package handles Excel?** - `maatwebsite/excel`
2. **What interface is used for imports?** - `ToModel` (or `ToCollection`)
3. **How do you start import from row 2?** - `implements WithHeadingRow` interface
4. **Where is import logic defined?** - In Import class (e.g., `app/Imports/ItemImport.php`)

### 37. DataTables Answers

1. **What package for server-side tables?** - `yajra/laravel-datatables`
2. **Benefits of server-side?** - Better performance with large datasets, pagination, search, sort
3. **Where is DataTable logic?** - In DataTable classes or controller methods
4. **What library does it integrate with?** - jQuery DataTables

### 38. Carbon (Date/Time) Answers

1. **What is Carbon?** - DateTime library for PHP (extended DateTime class)
2. **How do you get current time?** - `Carbon::now()` or `now()`
3. **How do you format dates?** - `$date->format('Y-m-d')` or `$date->toDateString()`
4. **Common methods?** - `addDays()`, `subMonths()`, `diffForHumans()`, `isPast()`, `isFuture()`

### 39. Vite (Asset Bundling) Answers

1. **What is the default asset bundler in Laravel?** - Vite
2. **What Blade directive includes Vite assets?** - `@vite(['resources/css/app.css', 'resources/js/app.js'])`
3. **What command runs Vite in development mode?** - `npm run dev`
4. **What command builds assets for production?** - `npm run build`

### 40. Laravel Sanctum Answers

1. **What package provides lightweight API authentication?** - Laravel Sanctum
2. **What trait enables Sanctum tokens in a User model?** - `HasApiTokens`
3. **What middleware protects API routes with Sanctum?** - `auth:sanctum`

---

## Part 6: Artisan CLI Answers

### 42. Migration Commands Quiz Answers

1. **What is the difference between `migrate:refresh` and `migrate:fresh`?** - `refresh` runs down() then up() methods; `fresh` drops all tables directly (faster)
2. **What does `migrate:rollback --step=2` do?** - Rolls back the last 2 migration batches
3. **Which command shows pending migrations?** - `php artisan migrate:status`
4. **What happens when you run `php artisan migrate`?** - Runs the `up()` method in all pending migration files

### 43. Make Commands Quiz Answers

1. **What does `php artisan make:model Album -mcr` create?** - Model, Migration, and Resource Controller
2. **Where is the controller file created?** - `app/Http/Controllers/`
3. **What is the difference between `-r` and `--api` in make:controller?** - `-r` creates full resource (7 methods), `--api` excludes create/edit (5 methods)
4. **What naming convention should you use for migrations?** - `create_tablename_table`, `add_column_to_table_table`

### 44. Seeding Commands Quiz Answers

1. **What file does `db:seed` run by default?** - `database/seeders/DatabaseSeeder.php`
2. **How do you run only a specific seeder?** - `php artisan db:seed --class=SeederName`

### 45. Cache Commands Quiz Answers

1. **Which command clears all types of cache?** - `php artisan optimize:clear`
2. **When should you run `config:clear`?** - After changing `.env` or config files
3. **What's the difference between `cache:clear` and `config:clear`?** - `cache:clear` clears application cache, `config:clear` clears configuration cache

### 46. Server Commands Quiz Answers

1. **What is the default port for `php artisan serve`?** - 8000 (http://127.0.0.1:8000)
2. **What is Tinker used for?** - Interactive PHP shell to test queries and interact with your application

### 47. Storage Commands Quiz Answers

1. **Why do we need to run `storage:link`?** - To make files in `storage/app/public` accessible from the web
2. **Where does the symbolic link point to?** - `public/storage` → `storage/app/public`

### 48. Route Commands Quiz Answers

1. **How do you see all registered routes?** - `php artisan route:list`
2. **How do you filter routes by name?** - `php artisan route:list --name=keyword`

---

## 🎯 Quick Answer Reference

### One-Word/Short Answers

| Question                           | Answer                     |
| ---------------------------------- | -------------------------- |
| MVC stands for?                    | Model-View-Controller      |
| ORM stands for?                    | Object-Relational Mapping  |
| CSRF stands for?                   | Cross-Site Request Forgery |
| Default session driver?            | file                       |
| Default pagination per page?       | 15                         |
| Default serve port?                | 8000                       |
| Models location?                   | app/Models/                |
| Controllers location?              | app/Http/Controllers/      |
| Migrations location?               | database/migrations/       |
| Seeders location?                  | database/seeders/          |
| Views location?                    | resources/views/           |
| Routes location?                   | routes/                    |
| Password hashing function?         | bcrypt()                   |
| Soft delete column?                | deleted_at                 |
| CSRF Blade directive?              | @csrf                      |
| Timestamps columns?                | created_at, updated_at     |
| Get authenticated user?            | Auth::user()               |
| Check if logged in?                | Auth::check()              |
| Default Eloquent primary key?      | id                         |
| Resource controller methods count? | 7                          |

---

[⬆ Back to Top](#table-of-contents)

---

_Good luck on your exam! 🎓_

_This comprehensive reviewer covers all Laravel concepts from the Music Application and NS Shop E-Commerce projects. Study each section carefully and understand how they work together to create complete web applications._
