# Laravel Project Reviewer - Part 2
## Newly Added Topics and Concepts
### NS Shop - E-Commerce Application

This reviewer covers the **NEW Laravel topics/concepts** that were recently added to the project. These are advanced database and migration topics that build upon the fundamentals.

---

## Table of Contents
1. [Foreign Key Constraints](#1-foreign-key-constraints)
2. [Soft Deletes](#2-soft-deletes)
3. [Schema Table Modifications (Alter Tables)](#3-schema-table-modifications-alter-tables)
4. [Column Types and Modifiers](#4-column-types-and-modifiers)
5. [Migration Dependencies and Execution Order](#5-migration-dependencies-and-execution-order)
6. [Database Schema Design for E-Commerce](#6-database-schema-design-for-e-commerce)
7. [Custom Primary Keys](#7-custom-primary-keys)
8. [Cascade Delete Operations](#8-cascade-delete-operations)

---

## 1. Foreign Key Constraints

### Where Used
- [database/migrations/2026_02_10_032706_create_customer_table.php](database/migrations/2026_02_10_032706_create_customer_table.php)
- [database/migrations/2026_02_10_032710_create_orderinfo_table.php](database/migrations/2026_02_10_032710_create_orderinfo_table.php)
- [database/migrations/2026_02_10_032741_create_orderline_table.php](database/migrations/2026_02_10_032741_create_orderline_table.php)
- [database/migrations/2026_01_27_011136_add_item_id_foreign_unique_to_stock_table.php](database/migrations/2026_01_27_011136_add_item_id_foreign_unique_to_stock_table.php)

### Purpose in Laravel
Foreign key constraints ensure **referential integrity** between related tables. They prevent orphaned records by ensuring that a value in a column must exist in another table.

### Detailed Explanation
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

### Example from Project

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

### Quiz Questions

1. **Q: What is the purpose of a foreign key constraint?**
   - A: To ensure referential integrity between related tables, preventing orphaned records.

2. **Q: What is the difference between `foreignId()->constrained()` and `foreign()->references()->on()`?**
   - A: `foreignId()->constrained()` is shorthand that auto-creates the column and assumes standard conventions. `foreign()->references()->on()` gives full control over column and table names.

3. **Q: What happens if you try to insert a record with a foreign key value that doesn't exist in the referenced table?**
   - A: The database will throw a constraint violation error and reject the insert.

4. **Q: Given this code: `$table->foreignId('user_id')->constrained('users');` - what column type is created?**
   - A: `unsignedBigInteger`

5. **Q: Why must you create the `customer` table before the `orderinfo` table?**
   - A: Because `orderinfo` has a foreign key referencing `customer`. The referenced table must exist first.

6. **Q: What does `constrained('users')` do?**
   - A: It creates a foreign key constraint that references the `id` column of the `users` table.

7. **Q: How do you drop a foreign key in a migration?**
   - A: Using `$table->dropForeign(['column_name']);` or `$table->dropForeign('table_column_foreign');`

---

## 2. Soft Deletes

### Where Used
- [database/migrations/2026_01_27_020914_add_soft_deletes_to_item_table.php](database/migrations/2026_01_27_020914_add_soft_deletes_to_item_table.php)

### Purpose in Laravel
Soft deletes allow you to "delete" records without actually removing them from the database. Instead, a `deleted_at` timestamp is set. This is useful for:
- Keeping historical data
- Recovering accidentally deleted records
- Audit trails

### Detailed Explanation
Normal delete = Record is **permanently removed** from database
Soft delete = Record **stays** in database but is **marked as deleted**

When you query a model with soft deletes:
- Soft-deleted records are automatically **excluded** from results
- You can include or only get soft-deleted records with special methods

### Example from Project

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

**Model Implementation (What needs to be added):**
```php
// app/Models/Item.php - Add SoftDeletes trait
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\SoftDeletes;  // Import the trait

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

### Practical Usage in Views
```php
{{-- resources/views/item/index.blade.php --}}
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

### Quiz Questions

1. **Q: What method adds a soft deletes column in a migration?**
   - A: `$table->softDeletes();`

2. **Q: What column is created by `softDeletes()`?**
   - A: `deleted_at` (nullable timestamp)

3. **Q: What trait must a model use to enable soft deletes?**
   - A: `Illuminate\Database\Eloquent\SoftDeletes`

4. **Q: How do you include soft-deleted records in a query?**
   - A: Using `Model::withTrashed()->get()`

5. **Q: How do you permanently delete a soft-deleted record?**
   - A: Using `$model->forceDelete()`

6. **Q: How do you restore a soft-deleted record?**
   - A: Using `$model->restore()`

7. **Q: What method checks if a model is soft-deleted?**
   - A: `$model->trashed()`

8. **Q: Are soft-deleted records included in normal queries?**
   - A: No, they are automatically excluded unless you use `withTrashed()` or `onlyTrashed()`

---

## 3. Schema Table Modifications (Alter Tables)

### Where Used
- [database/migrations/2026_01_27_011136_add_item_id_foreign_unique_to_stock_table.php](database/migrations/2026_01_27_011136_add_item_id_foreign_unique_to_stock_table.php)
- [database/migrations/2026_01_27_020914_add_soft_deletes_to_item_table.php](database/migrations/2026_01_27_020914_add_soft_deletes_to_item_table.php)

### Purpose in Laravel
`Schema::table()` allows you to modify existing tables after they've been created. This is essential for:
- Adding new columns to existing tables
- Adding indexes or foreign keys
- Modifying column properties
- Removing columns

### Detailed Explanation
There are two main Schema methods:
- `Schema::create()` - Creates a NEW table
- `Schema::table()` - Modifies an EXISTING table

Common modifications:
- Add columns: `$table->string('new_column')`
- Add foreign keys: `$table->foreign('column')`
- Add indexes: `$table->unique('column')`
- Drop columns: `$table->dropColumn('column')`
- Drop foreign keys: `$table->dropForeign(['column'])`

### Example from Project

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

### Quiz Questions

1. **Q: What is the difference between `Schema::create()` and `Schema::table()`?**
   - A: `Schema::create()` creates a new table; `Schema::table()` modifies an existing table.

2. **Q: How do you add a column after a specific column?**
   - A: Using `->after('column_name')`, e.g., `$table->string('new')->after('existing');`

3. **Q: What package is required to modify columns in Laravel?**
   - A: `doctrine/dbal`

4. **Q: How do you drop a foreign key constraint?**
   - A: Using `$table->dropForeign(['column_name']);`

5. **Q: What method is used to remove a column?**
   - A: `$table->dropColumn('column_name');`

---

## 4. Column Types and Modifiers

### Where Used
- All migration files in [database/migrations/](database/migrations/)

### Purpose in Laravel
Laravel provides many column types and modifiers to define your database schema precisely. Understanding these is crucial for proper database design.

### Column Types Used in Project

| Method | Description | Example from Project |
|--------|-------------|---------------------|
| `id()` | Auto-incrementing BIGINT for primary key | `$table->id('customer_id')` |
| `integer()` | INTEGER column | `$table->integer('item_id')` |
| `string()` | VARCHAR column | `$table->string('fname', 50)` |
| `text()` | TEXT column | `$table->text('description')` |
| `decimal()` | DECIMAL column with precision | `$table->decimal('shipping', 10, 2)` |
| `dateTime()` | DATETIME column | `$table->dateTime('date_placed')` |
| `unsignedBigInteger()` | Unsigned BIGINT (for foreign keys) | `$table->unsignedBigInteger('customer_id')` |
| `foreignId()` | Creates unsigned BIGINT for foreign key | `$table->foreignId('user_id')` |
| `softDeletes()` | Adds nullable `deleted_at` timestamp | `$table->softDeletes()` |

### Column Modifiers Used in Project

| Modifier | Description | Example |
|----------|-------------|---------|
| `->primary()` | Set as primary key | `$table->integer('item_id')->primary()` |
| `->nullable()` | Allow NULL values | `$table->string('title', 10)->nullable()` |
| `->default()` | Set default value | `$table->integer('quantity')->default(0)` |
| `->unique()` | Add unique constraint | `->unique()` |
| `->constrained()` | Add foreign key constraint | `$table->foreignId('user_id')->constrained()` |

### Examples from Project

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

### Quiz Questions

1. **Q: What column type would you use for a price field?**
   - A: `decimal($column, $precision, $scale)`, e.g., `$table->decimal('price', 10, 2)`

2. **Q: What does `$table->id('custom_id')` create?**
   - A: An auto-incrementing BIGINT primary key column named `custom_id`

3. **Q: How do you make a column optional (allow NULL)?**
   - A: Using the `->nullable()` modifier

4. **Q: What is the second parameter in `$table->string('fname', 50)`?**
   - A: The maximum length of the VARCHAR column (50 characters)

5. **Q: What does `->default(0)` do?**
   - A: Sets the default value to 0 if no value is provided during insert

6. **Q: What is the difference between `integer()` and `unsignedBigInteger()`?**
   - A: `integer()` creates a signed INT; `unsignedBigInteger()` creates an unsigned BIGINT (no negative values, commonly used for foreign keys)

---

## 5. Migration Dependencies and Execution Order

### Where Used
- All migrations in [database/migrations/](database/migrations/)

### Purpose in Laravel
Migrations must be created and run in the correct order when tables have foreign key relationships. A table cannot reference another table that doesn't exist yet.

### Detailed Explanation
Laravel executes migrations in **alphabetical order** based on the filename timestamp. This is why migrations have timestamps like `2026_01_27_000001_`.

**Rule:** Tables that are referenced by foreign keys must be created FIRST.

### Project Migration Order

```
Order  | Timestamp            | Table      | Dependencies
-------|---------------------|------------|--------------------------------
1      | 2014_10_12_000000   | users      | None (base table)
2      | 2026_01_27_000001   | item       | None
3      | 2026_01_27_000002   | stock      | None (FK added later)
4      | 2026_01_27_011136   | (alter)    | item (adds FK to stock)
5      | 2026_01_27_020914   | (alter)    | item (adds soft deletes)
6      | 2026_02_10_032706   | customer   | users (references users.id)
7      | 2026_02_10_032710   | orderinfo  | customer (references customer.customer_id)
8      | 2026_02_10_032741   | orderline  | orderinfo, item (references both)
```

### Visual Dependency Chain
```
users
  └── customer (depends on users)
        └── orderinfo (depends on customer)
              └── orderline (depends on orderinfo and item)
                        ↓
item ─────────────────────┘
  └── stock (depends on item via foreign key)
```

### Creating Migrations with Correct Timestamps
```bash
# Create migrations in order (timestamps will be sequential)
php artisan make:migration create_users_table
# Wait a few seconds or manually adjust timestamp
php artisan make:migration create_customer_table
php artisan make:migration create_orderinfo_table
php artisan make:migration create_orderline_table
```

### Handling Migration Errors

If you get a foreign key error:
```
SQLSTATE[HY000]: General error: 1215 Cannot add foreign key constraint
```

Check:
1. Referenced table exists (correct migration order)
2. Column types match (both should be same type, e.g., `unsignedBigInteger`)
3. Referenced column has an index (primary keys are automatically indexed)

### Quiz Questions

1. **Q: In what order does Laravel run migrations?**
   - A: Alphabetically by filename (timestamp order)

2. **Q: Why does the timestamp in migration filenames matter?**
   - A: It determines the execution order; tables with foreign keys must be created after the tables they reference.

3. **Q: What error do you get if you try to create a foreign key to a non-existent table?**
   - A: "Cannot add foreign key constraint" error

4. **Q: Given tables A, B, C where B references A and C references B, what is the correct creation order?**
   - A: A → B → C

5. **Q: How can you fix migration order issues?**
   - A: Rename migration files to adjust timestamps, or create new migrations in the correct order.

---

## 6. Database Schema Design for E-Commerce

### Where Used
- All table migrations representing the e-commerce structure

### Purpose in Laravel
Understanding database design helps you create efficient, normalized schemas. This project demonstrates a typical e-commerce database structure.

### Project Schema Overview

**Users & Customers:**
- `users` - Laravel's default authentication table
- `customer` - Extended customer profile linked to users

**Products & Inventory:**
- `item` - Product catalog
- `stock` - Inventory levels (1-to-1 with item)

**Orders:**
- `orderinfo` - Order header (who, when, status)
- `orderline` - Order details (what items, quantities)

### Entity Relationship Diagram (ERD)
```
┌─────────────────────────────────────────────────────────────────────┐
│                         E-COMMERCE SCHEMA                           │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────┐     1:1      ┌─────────────┐
│   users     │─────────────►│  customer   │
├─────────────┤              ├─────────────┤
│ id (PK)     │              │ customer_id │
│ name        │              │ user_id(FK) │
│ email       │              │ fname, lname│
│ password    │              │ address     │
└─────────────┘              └──────┬──────┘
                                    │
                                    │ 1:N
                                    ▼
                             ┌─────────────┐
                             │  orderinfo  │
                             ├─────────────┤
                             │ orderinfo_id│
                             │customer_id  │
                             │ date_placed │
                             │ status      │
                             └──────┬──────┘
                                    │
                                    │ 1:N
                                    ▼
┌─────────────┐              ┌─────────────┐
│    item     │◄────N:1──────│  orderline  │
├─────────────┤              ├─────────────┤
│ item_id(PK) │              │orderline_id │
│ description │              │orderinfo_id │
│ cost_price  │              │ item_id(FK) │
│ sell_price  │              │ quantity    │
└──────┬──────┘              └─────────────┘
       │
       │ 1:1
       ▼
┌─────────────┐
│   stock     │
├─────────────┤
│ item_id(FK) │
│ quantity    │
└─────────────┘
```

### Relationship Types in This Project

| Relationship | Tables | Type | Description |
|--------------|--------|------|-------------|
| User → Customer | users → customer | 1:1 | Each user has one customer profile |
| Customer → Orders | customer → orderinfo | 1:N | A customer can have many orders |
| Order → OrderLines | orderinfo → orderline | 1:N | An order can have many items |
| Item → OrderLines | item → orderline | 1:N | An item can be in many orders |
| Item → Stock | item → stock | 1:1 | Each item has one stock record |

### Quiz Questions

1. **Q: What is the relationship between `users` and `customer` in this schema?**
   - A: One-to-One (1:1) - each user has one customer profile

2. **Q: Why is `orderline` a separate table from `orderinfo`?**
   - A: To support multiple items per order (one-to-many relationship)

3. **Q: What is a junction/pivot table and which table in this project acts like one?**
   - A: `orderline` acts like a junction table connecting `orderinfo` and `item`

4. **Q: Why use separate `item` and `stock` tables instead of putting quantity in `item`?**
   - A: Separation of concerns - stock changes frequently, item details rarely change. Also allows for multiple stock locations in the future.

---

## 7. Custom Primary Keys

### Where Used
- [app/Models/Item.php](app/Models/Item.php) - `protected $primaryKey = 'item_id'`
- [app/Models/Stock.php](app/Models/Stock.php) - `protected $primaryKey = 'item_id'`
- [app/Models/Customer.php](app/Models/Customer.php) - `protected $primaryKey = 'customer_id'`
- [app/Models/Order.php](app/Models/Order.php) - `protected $primaryKey = 'orderinfo_id'`

### Purpose in Laravel
By default, Eloquent expects the primary key to be named `id`. When using different column names, you must tell Eloquent what the primary key is called.

### Detailed Explanation
If your table uses a custom primary key name instead of `id`, you need to set the `$primaryKey` property in your model. Otherwise, methods like `find()`, `findOrFail()`, and route model binding won't work correctly.

### Examples from Project

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

### Quiz Questions

1. **Q: What is the default primary key column name in Eloquent?**
   - A: `id`

2. **Q: How do you specify a custom primary key in a model?**
   - A: Set the `$primaryKey` property: `protected $primaryKey = 'custom_id';`

3. **Q: What property determines if the primary key is auto-incrementing?**
   - A: `$incrementing` (boolean, default: true)

4. **Q: If your primary key is a UUID string, what should `$keyType` be set to?**
   - A: `'string'`

5. **Q: What happens if you don't set `$primaryKey` when using a custom primary key name?**
   - A: `find()` and other methods will look for `id` column and fail.

---

## 8. Cascade Delete Operations

### Where Used
- [database/migrations/2026_01_27_011136_add_item_id_foreign_unique_to_stock_table.php](database/migrations/2026_01_27_011136_add_item_id_foreign_unique_to_stock_table.php)

### Purpose in Laravel
Cascade operations define what happens to related records when a parent record is deleted or updated. This maintains referential integrity automatically.

### Detailed Explanation
When you delete a record that is referenced by another table's foreign key, the database needs to know what to do. Options include:

| Option | What Happens |
|--------|--------------|
| `CASCADE` | Delete/update child records too |
| `RESTRICT` | Prevent deletion if children exist (default) |
| `SET NULL` | Set foreign key to NULL in children |
| `NO ACTION` | Similar to RESTRICT |

### Example from Project

```php
// database/migrations/2026_01_27_011136_add_item_id_foreign_unique_to_stock_table.php
Schema::table('stock', function (Blueprint $table) {
    $table->foreign('item_id')
        ->references('item_id')
        ->on('item')
        ->onDelete('cascade');  // If item is deleted, delete stock record too
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

### Quiz Questions

1. **Q: What does `onDelete('cascade')` do?**
   - A: Automatically deletes related child records when the parent is deleted.

2. **Q: What is the default behavior when deleting a parent with child records?**
   - A: RESTRICT - the deletion is prevented (error thrown).

3. **Q: When would you use `onDelete('set null')`?**
   - A: When you want to keep child records but remove their association with the deleted parent.

4. **Q: What requirement must a column meet to use `set null`?**
   - A: The column must be `nullable()`.

5. **Q: What is the difference between CASCADE and RESTRICT?**
   - A: CASCADE deletes children automatically; RESTRICT prevents deletion if children exist.

6. **Q: How do you apply cascade delete with the shorthand `foreignId` method?**
   - A: Using `->cascadeOnDelete()`, e.g., `$table->foreignId('user_id')->constrained()->cascadeOnDelete()`

---

## Summary Table of New Topics

| Topic | Migration File | Key Concept |
|-------|---------------|-------------|
| Foreign Keys | create_customer_table.php | `foreignId()->constrained()` |
| Foreign Keys (Traditional) | create_orderinfo_table.php | `foreign()->references()->on()` |
| Multiple FKs | create_orderline_table.php | Multiple references in one table |
| Soft Deletes | add_soft_deletes_to_item_table.php | `$table->softDeletes()` |
| Table Modifications | add_item_id_foreign_unique_to_stock_table.php | `Schema::table()` |
| Cascade Delete | add_item_id_foreign_unique_to_stock_table.php | `->onDelete('cascade')` |
| Custom Primary Keys | All models | `protected $primaryKey` |
| Column Types | All migrations | Various types and modifiers |

---

## Quick Reference Card

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

*This reviewer covers advanced database concepts added to the NS Shop project. Make sure you understand foreign keys and soft deletes as these are commonly asked in Laravel exams!*