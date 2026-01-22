# 📚 LARAVEL CLI & ARTISAN COMMANDS REVIEWER

## Complete Guide to Laravel Command Line Interface

---

## 🔧 What is Artisan?

Artisan is Laravel's built-in command-line interface (CLI). It provides helpful commands for development tasks like creating files, running migrations, clearing cache, and more.

**Basic Syntax:**

```bash
php artisan <command> [options] [arguments]
```

**Get Help on Any Command:**

```bash
php artisan help <command>
php artisan <command> --help
```

**List All Available Commands:**

```bash
php artisan list
```

---

## 1. MIGRATION COMMANDS

Migrations are like version control for your database schema.

### `php artisan migrate`

**What it does:** Runs all pending migrations (creates/modifies database tables)

```bash
php artisan migrate
```

- Executes the `up()` method in each migration file
- Creates tables defined in `database/migrations/`
- Tracks which migrations have run in `migrations` table

---

### `php artisan migrate:rollback`

**What it does:** Reverts the last batch of migrations

```bash
php artisan migrate:rollback
```

- Executes the `down()` method in migrations
- Rolls back only the last batch (group of migrations run together)
- Use `--step=2` to rollback 2 batches

```bash
php artisan migrate:rollback --step=3  # Rollback last 3 batches
```

---

### `php artisan migrate:reset`

**What it does:** Rolls back ALL migrations (empties all tables)

```bash
php artisan migrate:reset
```

- Reverts every migration that has ever run
- Executes `down()` method on all migrations
- ⚠️ WARNING: You will lose all data!

---

### `php artisan migrate:refresh`

**What it does:** Rolls back all migrations then re-runs them

```bash
php artisan migrate:refresh
```

- Same as: `migrate:reset` + `migrate`
- Useful when you want to rebuild the database
- Add `--seed` to also run seeders after

```bash
php artisan migrate:refresh --seed  # Refresh and seed data
```

---

### `php artisan migrate:fresh`

**What it does:** Drops ALL tables then runs all migrations

```bash
php artisan migrate:fresh
```

- Faster than `migrate:refresh`
- Doesn't run `down()` methods - just drops tables directly
- ⚠️ WARNING: You will lose all data!

```bash
php artisan migrate:fresh --seed  # Fresh migrate and seed
```

---

### `php artisan migrate:status`

**What it does:** Shows which migrations have run and which are pending

```bash
php artisan migrate:status
```

- Displays a table of all migrations
- Shows "Ran" or "Pending" status
- Useful to check migration state

---

### ❓ Migration Quiz Questions:

1. What is the difference between `migrate:refresh` and `migrate:fresh`?
2. What does `migrate:rollback --step=2` do?
3. Which command shows pending migrations?
4. What happens when you run `php artisan migrate`?

---

## 2. MAKE COMMANDS (Code Generators)

These commands generate boilerplate code files.

### `php artisan make:model`

**What it does:** Creates a new Eloquent Model class

```bash
php artisan make:model Album
```

- Creates `app/Models/Album.php`
- Add `-m` to also create a migration

```bash
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

---

### `php artisan make:controller`

**What it does:** Creates a new Controller class

```bash
php artisan make:controller ArtistController
```

- Creates `app/Http/Controllers/ArtistController.php`
- Add `-r` for resource controller with CRUD methods

```bash
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

---

### `php artisan make:migration`

**What it does:** Creates a new database migration file

```bash
php artisan make:migration create_songs_table
```

- Creates file in `database/migrations/`
- Filename includes timestamp: `2026_01_22_123456_create_songs_table.php`

```bash
# Create table migration
php artisan make:migration create_albums_table

# Modify existing table
php artisan make:migration add_description_to_songs_table --table=songs

# Create table with pre-filled Schema::create
php artisan make:migration create_listeners_table --create=listeners
```

**Naming Convention:**

- `create_xxx_table` - For creating new tables
- `add_xxx_to_yyy_table` - For adding columns
- `remove_xxx_from_yyy_table` - For removing columns

---

### `php artisan make:seeder`

**What it does:** Creates a new database seeder class

```bash
php artisan make:seeder AlbumSeeder
```

- Creates `database/seeders/AlbumSeeder.php`
- Used to populate database with test/dummy data

---

### `php artisan make:factory`

**What it does:** Creates a new model factory

```bash
php artisan make:factory AlbumFactory
php artisan make:factory AlbumFactory --model=Album  # Link to model
```

- Creates `database/factories/AlbumFactory.php`
- Used with Faker to generate fake data for testing

---

### `php artisan make:middleware`

**What it does:** Creates a new middleware class

```bash
php artisan make:middleware CheckAge
```

- Creates `app/Http/Middleware/CheckAge.php`
- Must register in `app/Http/Kernel.php` to use

---

### `php artisan make:request`

**What it does:** Creates a Form Request class for validation

```bash
php artisan make:request StoreSongRequest
```

- Creates `app/Http/Requests/StoreSongRequest.php`
- Used to separate validation logic from controller

---

### `php artisan make:mail`

**What it does:** Creates a new Mailable class

```bash
php artisan make:mail WelcomeEmail
```

- Creates `app/Mail/WelcomeEmail.php`
- Used for sending emails

---

### `php artisan make:command`

**What it does:** Creates a custom Artisan command

```bash
php artisan make:command SendEmails
```

- Creates `app/Console/Commands/SendEmails.php`
- You can create your own CLI commands

---

### ❓ Make Commands Quiz Questions:

1. What does `php artisan make:model Album -mcr` create?
2. Where is the controller file created?
3. What is the difference between `-r` and `--api` in make:controller?
4. What naming convention should you use for migrations?

---

## 3. DATABASE SEEDING COMMANDS

### `php artisan db:seed`

**What it does:** Runs the database seeders to populate data

```bash
php artisan db:seed
```

- Runs `database/seeders/DatabaseSeeder.php`
- Populates database with test data

```bash
php artisan db:seed --class=AlbumSeeder  # Run specific seeder only
```

---

### ❓ Seeding Quiz Questions:

1. What file does `db:seed` run by default?
2. How do you run only a specific seeder?

---

## 4. CACHE COMMANDS

Laravel caches various things for performance.

### `php artisan cache:clear`

**What it does:** Clears the application cache

```bash
php artisan cache:clear
```

- Clears cached data stored by your application
- Use when cached data is stale

---

### `php artisan config:clear`

**What it does:** Clears the configuration cache

```bash
php artisan config:clear
```

- Removes cached config files
- Use after changing `.env` or config files

---

### `php artisan config:cache`

**What it does:** Creates a cache file for faster config loading

```bash
php artisan config:cache
```

- Combines all config files into one cached file
- Faster loading in production
- ⚠️ Must run again after changing any config

---

### `php artisan route:clear`

**What it does:** Clears the route cache

```bash
php artisan route:clear
```

- Removes cached routes
- Use after changing routes

---

### `php artisan route:cache`

**What it does:** Creates a cache file for faster route registration

```bash
php artisan route:cache
```

- Speeds up route registration
- Good for production
- ⚠️ Cannot use with Closure routes (must use controllers)

---

### `php artisan view:clear`

**What it does:** Clears compiled view files

```bash
php artisan view:clear
```

- Removes compiled Blade templates
- Use when views aren't updating

---

### `php artisan view:cache`

**What it does:** Compiles all Blade views

```bash
php artisan view:cache
```

- Pre-compiles all Blade templates
- Faster first page loads in production

---

### `php artisan optimize:clear`

**What it does:** Clears ALL cached files

```bash
php artisan optimize:clear
```

- Clears: config, route, view, and general cache
- One command to clear everything
- Equivalent to running all clear commands

---

### ❓ Cache Commands Quiz Questions:

1. Which command clears all types of cache?
2. When should you run `config:clear`?
3. What's the difference between `cache:clear` and `config:clear`?

---

## 5. SERVER & DEVELOPMENT COMMANDS

### `php artisan serve`

**What it does:** Starts the Laravel development server

```bash
php artisan serve
```

- Starts server at `http://127.0.0.1:8000`
- For development only (not production!)

```bash
php artisan serve --port=8080           # Custom port
php artisan serve --host=192.168.1.100  # Custom host
```

---

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

---

### ❓ Server Commands Quiz Questions:

1. What is the default port for `php artisan serve`?
2. What is Tinker used for?

---

## 6. STORAGE COMMANDS

### `php artisan storage:link`

**What it does:** Creates a symbolic link from `public/storage` to `storage/app/public`

```bash
php artisan storage:link
```

- Makes uploaded files accessible from the web
- Required for file uploads to work with `Storage::url()`
- Creates: `public/storage` → `storage/app/public`

---

### ❓ Storage Commands Quiz Questions:

1. Why do we need to run `storage:link`?
2. Where does the symbolic link point to?

---

## 7. ROUTE COMMANDS

### `php artisan route:list`

**What it does:** Displays all registered routes

```bash
php artisan route:list
```

- Shows: Method, URI, Name, Action, Middleware
- Very useful for debugging routes

```bash
php artisan route:list --name=user      # Filter by name
php artisan route:list --path=api       # Filter by path
php artisan route:list --method=GET     # Filter by method
php artisan route:list -v               # Verbose (show middleware)
```

---

### ❓ Route Commands Quiz Questions:

1. How do you see all registered routes?
2. How do you filter routes by name?

---

## 8. KEY & ENVIRONMENT COMMANDS

### `php artisan key:generate`

**What it does:** Generates a new application encryption key

```bash
php artisan key:generate
```

- Sets `APP_KEY` in `.env` file
- Required for encryption, sessions, cookies
- Run once when setting up new project

---

### `php artisan env`

**What it does:** Displays the current environment

```bash
php artisan env
```

- Shows: `local`, `production`, `testing`, etc.
- Based on `APP_ENV` in `.env`

---

## 9. QUEUE COMMANDS (Advanced)

### `php artisan queue:work`

**What it does:** Processes jobs in the queue

```bash
php artisan queue:work
```

- Processes queued jobs (emails, notifications, etc.)
- Runs continuously until stopped

---

### `php artisan queue:listen`

**What it does:** Listens for queue jobs (reloads on code changes)

```bash
php artisan queue:listen
```

- Similar to `queue:work` but reloads on changes
- Better for development

---

## 10. TESTING COMMANDS

### `php artisan test`

**What it does:** Runs the application tests

```bash
php artisan test
```

- Runs PHPUnit tests
- Tests in `tests/` directory

```bash
php artisan test --filter=UserTest  # Run specific test
```

---

## 📋 QUICK REFERENCE CHEAT SHEET

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

---

## 📝 COMMON COMMAND PATTERNS

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

---

## ❓ COMPREHENSIVE QUIZ QUESTIONS

1. What is the purpose of `php artisan`?
2. How do you create a model with its migration and controller in one command?
3. What is the difference between `migrate:fresh` and `migrate:refresh`?
4. What does `php artisan tinker` do?
5. Which command shows all registered routes?
6. What does `php artisan storage:link` create?
7. How do you run a specific seeder?
8. What is the purpose of `php artisan key:generate`?
9. Which command clears all types of cache?
10. What flags create a resource controller?

---

## 💡 PRO TIPS

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

Good luck on your exam! 🎓
