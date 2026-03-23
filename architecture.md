# 🏗️ Arsitektur Laravel - Panduan Lengkap & Profesional

## 📌 Daftar Isi

1. [Pengenalan Arsitektur Laravel](#pengenalan)
2. [MVC Pattern](#mvc)
3. [Struktur Folder Laravel](#struktur)
4. [Service Container & Dependency Injection](#container)
5. [Service Provider](#provider)
6. [Routing Advanced](#routing)
7. [Middleware](#middleware)
8. [Controller & Request](#controller)
9. [Model & Relationships](#model)
10. [Repository Pattern](#repository)
11. [Service Layer](#service)
12. [Database Design & ERD](#database)
13. [Authentication & Authorization](#auth)
14. [Validation & Form Request](#validation)
15. [API Design](#api)
16. [Caching Strategy](#cache)
17. [Queue & Jobs](#queue)
18. [Testing](#testing)
19. [Production Deployment](#production)
20. [Best Practices](#best-practices)

---

## 🎯 Pengenalan Arsitektur Laravel

### Apa itu Arsitektur Laravel?

Arsitektur Laravel adalah cara mengorganisir kode dan struktur aplikasi agar:

- **Scalable**: Mudah di-scale untuk project besar
- **Maintainable**: Mudah di-maintain dan debug
- **Testable**: Mudah di-test dengan unit/feature test
- **Reusable**: Komponen dapat di-reuse di tempat lain

### Prinsip-Prinsip Utama Laravel:

```
┌─────────────────────────────────────────┐
│   LARAVEL ARCHITECTURE PRINCIPLES       │
├─────────────────────────────────────────┤
│ 1. Convention over Configuration       │
│ 2. DRY (Don't Repeat Yourself)          │
│ 3. SOLID Principles                    │
│ 4. Separation of Concerns               │
│ 5. Dependency Injection                 │
│ 6. Elegant & Readable Code              │
└─────────────────────────────────────────┘
```

---

## 🔄 MVC Pattern (Model-View-Controller)

### Konsep MVC:

```
REQUEST FROM CLIENT
        │
        ▼
    ROUTER
        │
        ▼
   MIDDLEWARE
        │
        ▼
   CONTROLLER
        │
        ├──► MODEL (Database Logic)
        │
        ├──► SERVICE (Business Logic)
        │
        └──► VIEW (Response/JSON)
        │
        ▼
   RESPONSE TO CLIENT
```

### Penjelasan Setiap Layer:

#### 1. **Route** - Endpoint Definition

```php
<?php

// routes/web.php
Route::get('/products', [ProductController::class, 'index'])->name('products.index');
Route::post('/products', [ProductController::class, 'store'])->name('products.store');
Route::get('/products/{product}', [ProductController::class, 'show'])->name('products.show');
Route::put('/products/{product}', [ProductController::class, 'update'])->name('products.update');
Route::delete('/products/{product}', [ProductController::class, 'destroy'])->name('products.destroy');
```

#### 2. **Middleware** - Request Filter

```php
<?php

// app/Http/Middleware/CheckRole.php
namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class CheckRole
{
    public function handle(Request $request, Closure $next, $role)
    {
        if (!auth()->check() || auth()->user()->role !== $role) {
            abort(403, 'Unauthorized');
        }

        return $next($request);
    }
}
```

#### 3. **Controller** - Request Handler

```php
<?php

// app/Http/Controllers/ProductController.php
namespace App\Http\Controllers;

use App\Models\Product;
use App\Services\ProductService;
use Illuminate\Http\Request;

class ProductController extends Controller
{
    protected $productService;

    public function __construct(ProductService $productService)
    {
        $this->productService = $productService;
    }

    public function index()
    {
        $products = $this->productService->getAllProducts();
        return view('products.index', compact('products'));
    }

    public function store(Request $request)
    {
        $validated = $request->validate([
            'name' => 'required|string',
            'price' => 'required|numeric',
        ]);

        $product = $this->productService->createProduct($validated);

        return redirect()->route('products.show', $product)->with('success', 'Product created');
    }
}
```

#### 4. **Model** - Database Abstraction

```php
<?php

// app/Models/Product.php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\HasMany;

class Product extends Model
{
    protected $fillable = ['name', 'price', 'description'];

    public function orders(): HasMany
    {
        return $this->hasMany(Order::class);
    }
}
```

#### 5. **View/Response** - Output Format

```php
// resources/views/products/index.blade.php
@foreach($products as $product)
    <div class="product-card">
        <h3>{{ $product->name }}</h3>
        <p>{{ $product->price }}</p>
    </div>
@endforeach

// Atau JSON Response untuk API
return response()->json($products);
```

---

## 📁 Struktur Folder Laravel - Penjelasan Detail

### Struktur Lengkap:

```
laravel-project/
├── app/
│   ├── Console/
│   │   └── Commands/         ← Artisan commands
│   ├── Events/
│   │   └── OrderCreated.php  ← Event handler
│   ├── Exceptions/
│   │   └── Handler.php       ← Exception handling
│   ├── Http/
│   │   ├── Controllers/      ← Semua controller
│   │   ├── Middleware/       ← Custom middleware
│   │   ├── Requests/         ← Form validation request
│   │   └── Resources/        ← API resource
│   ├── Listeners/
│   │   └── SendNotification.php ← Event listener
│   ├── Mail/
│   │   └── OrderShipped.php  ← Mailable class
│   ├── Models/
│   │   ├── User.php
│   │   ├── Product.php
│   │   └── Order.php
│   ├── Notifications/
│   │   └── InvoicePaid.php   ← Notification class
│   ├── Policies/
│   │   └── PostPolicy.php    ← Authorization policy
│   ├── Providers/
│   │   ├── AppServiceProvider.php
│   │   ├── EventServiceProvider.php
│   │   └── AuthServiceProvider.php
│   ├── Services/             ← Business logic layer
│   │   └── ProductService.php
│   ├── Repositories/         ← Data access layer
│   │   └── ProductRepository.php
│   ├── Interfaces/           ← Contract/Interface
│   │   └── ProductRepositoryInterface.php
│   └── DTOs/                 ← Data Transfer Object
│       └── CreateProductDTO.php
│
├── bootstrap/
│   └── app.php               ← Bootstrap application
│
├── config/
│   ├── app.php               ← Application config
│   ├── database.php          ← Database config
│   ├── cache.php             ← Cache config
│   ├── queue.php             ← Queue config
│   └── ...
│
├── database/
│   ├── migrations/           ← Table schema
│   │   ├── 2024_01_01_000000_create_users_table.php
│   │   ├── 2024_01_02_000000_create_products_table.php
│   │   └── 2024_01_03_000000_create_orders_table.php
│   ├── seeders/              ← Database seeders
│   │   └── DatabaseSeeder.php
│   └── factories/            ← Model factory
│       └── UserFactory.php
│
├── resources/
│   ├── views/
│   │   ├── layouts/
│   │   │   └── app.blade.php     ← Main layout
│   │   ├── components/
│   │   │   └── alert.blade.php   ← Reusable component
│   │   └── products/
│   │       ├── index.blade.php
│   │       ├── create.blade.php
│   │       └── show.blade.php
│   ├── css/
│   │   └── app.css
│   ├── js/
│   │   └── app.js
│   └── lang/                 ← Language files
│       └── en/
│
├── routes/
│   ├── web.php               ← Web routes
│   ├── api.php               ← API routes
│   ├── console.php           ← Console routes
│   └── channels.php          ← Broadcasting channels
│
├── storage/
│   ├── app/
│   ├── logs/
│   └── framework/
│
├── tests/
│   ├── Unit/                 ← Unit tests
│   │   └── ProductServiceTest.php
│   ├── Feature/              ← Feature tests
│   │   └── ProductControllerTest.php
│   └── TestCase.php
│
├── .env                      ← Environment variables
├── .env.example              ← Environment template
├── .gitignore
├── composer.json             ← PHP dependencies
├── package.json              ← Node dependencies
├── phpunit.xml               ← Test configuration
└── artisan                   ← CLI tool
```

### Penjelasan Folder Penting:

| Folder                 | Fungsi                                    |
| ---------------------- | ----------------------------------------- |
| `app/Models`           | Tempat semua Model (database abstraction) |
| `app/Http/Controllers` | Tempat semua Controller (request handler) |
| `app/Services`         | Business logic yang reusable              |
| `app/Repositories`     | Data access layer (query builder)         |
| `app/Http/Requests`    | Form validation rules                     |
| `routes/`              | Definisi semua routes                     |
| `database/migrations`  | Database schema definition                |
| `resources/views`      | Blade template files                      |
| `tests/`               | Unit & Feature tests                      |
| `config/`              | Application configuration                 |
| `storage/logs`         | Log files                                 |

---

## 🔧 Service Container & Dependency Injection

### Konsep Service Container:

Service Container adalah "IoC (Inversion of Control) Container" yang mengelola dependency aplikasi.

```php
<?php

// Bootstrap: app/Providers/AppServiceProvider.php
namespace App\Providers;

use App\Repositories\ProductRepository;
use App\Interfaces\ProductRepositoryInterface;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    public function register()
    {
        // Binding interface ke implementation
        $this->app->bind(
            ProductRepositoryInterface::class,
            ProductRepository::class
        );

        // Singleton binding (instance tunggal)
        $this->app->singleton('payment.service', function () {
            return new PaymentService();
        });
    }

    public function boot()
    {
        // Boot logic
    }
}
```

### Dependency Injection dalam Controller:

```php
<?php

// app/Http/Controllers/ProductController.php
namespace App\Http\Controllers;

use App\Interfaces\ProductRepositoryInterface;
use App\Services\ProductService;

class ProductController extends Controller
{
    protected $productService;
    protected $productRepository;

    // Constructor Injection
    public function __construct(
        ProductService $productService,
        ProductRepositoryInterface $productRepository
    ) {
        $this->productService = $productService;
        $this->productRepository = $productRepository;
    }

    public function index()
    {
        // Service Container otomatis inject dependency
        $products = $this->productRepository->all();
        return view('products.index', compact('products'));
    }
}
```

### Method Injection:

```php
<?php

public function store(
    StoreProductRequest $request,
    ProductService $productService
) {
    // Dependency di-inject otomatis
    $product = $productService->create($request->validated());

    return redirect()->route('products.show', $product);
}
```

---

## 🏭 Service Provider

Service Provider adalah "bootstrapper" aplikasi Laravel.

```php
<?php

// app/Providers/AppServiceProvider.php
namespace App\Providers;

use App\Models\User;
use App\Observers\UserObserver;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    // Step 1: Register - Register service ke container
    public function register()
    {
        // Bind interface ke concrete class
        $this->app->bind(
            'App\Interfaces\PaymentInterface',
            'App\Services\StripePaymentService'
        );

        // Singleton - sama instance untuk semua request
        $this->app->singleton('config.custom', function () {
            return new CustomConfig();
        });

        // Deferred provider
        if ($this->app->environment('production')) {
            // Load production service
        }
    }

    // Step 2: Boot - Aplikasi sudah di-boot
    public function boot()
    {
        // Register observer
        User::observe(UserObserver::class);

        // Share data ke semua view
        view()->share('appName', config('app.name'));

        // Register macro
        \Illuminate\Support\Str::macro('capitalize', function ($string) {
            return ucfirst($string);
        });
    }
}
```

---

## 🛣️ Routing Advanced

### Struktur Route Lengkap:

```php
<?php

// routes/web.php

use App\Http\Controllers\ProductController;
use App\Http\Controllers\OrderController;
use App\Http\Controllers\AdminController;

// ============ PUBLIC ROUTES ============
Route::get('/', function () {
    return view('welcome');
})->name('home');

// ============ PRODUCT ROUTES ============
Route::resource('products', ProductController::class)
    ->only(['index', 'show']); // Hanya index dan show

// ============ AUTH ROUTES ============
Route::middleware('guest')->group(function () {
    Route::get('/login', [AuthController::class, 'showLogin'])->name('login');
    Route::post('/login', [AuthController::class, 'login']);
});

Route::post('/logout', [AuthController::class, 'logout'])->name('logout')->middleware('auth');

// ============ PROTECTED ROUTES ============
Route::middleware('auth')->group(function () {

    // User Dashboard
    Route::get('/dashboard', [DashboardController::class, 'index'])->name('dashboard');

    // My Products
    Route::resource('my-products', ProductController::class)->except(['index', 'show']);

    // Admin Routes
    Route::middleware('can:admin')->prefix('admin')->name('admin.')->group(function () {

        Route::get('/', [AdminController::class, 'dashboard'])->name('dashboard');

        // Products Management
        Route::resource('products', ProductController::class)
            ->middleware('log.admin-action');

        // Orders Management
        Route::resource('orders', OrderController::class);
    });
});

// ============ API ROUTES ============
Route::prefix('api')->middleware(['api', 'throttle:60,1'])->group(function () {

    // Public API
    Route::get('/products', [ProductController::class, 'index']);
    Route::get('/products/{product}', [ProductController::class, 'show']);

    // Protected API
    Route::middleware('auth:sanctum')->group(function () {
        Route::post('/products', [ProductController::class, 'store']);
        Route::put('/products/{product}', [ProductController::class, 'update']);
        Route::delete('/products/{product}', [ProductController::class, 'destroy']);
    });
});
```

### Route Model Binding:

```php
<?php

// routes/web.php

// Implicit binding (automatically find by id)
Route::get('/products/{product}', [ProductController::class, 'show']);

// Explicit binding (custom column)
Route::get('/products/{product:slug}', [ProductController::class, 'show']);

// Custom binding
Route::bind('product', function ($value) {
    return Product::where('uuid', $value)
        ->orWhere('slug', $value)
        ->firstOrFail();
});
```

---

## 📊 Database Design & ERD

### Sistem POS - Entity Relationship Diagram:

```
┌─────────────────────────────────────────────────────────────┐
│                    ERD SISTEM POS                          │
└─────────────────────────────────────────────────────────────┘

    ┌─────────────────┐
    │     USERS       │
    ├─────────────────┤
    │ id              │
    │ name            │
    │ email           │
    │ password        │
    │ role (admin│    │
    │  kasir)         │
    │ created_at      │
    └────────┬────────┘
             │
             │ 1:N
             ▼
    ┌─────────────────┐
    │    PRODUCTS     │
    ├─────────────────┤
    │ id              │
    │ name            │
    │ price           │
    │ stock           │
    │ category_id FK  │
    │ created_at      │
    └────────┬────────┘
             │
             │ N:M (through order_items)
             │
    ┌────────▼────────┐
    │     ORDERS      │
    ├─────────────────┤
    │ id              │
    │ user_id FK      │
    │ total_price     │
    │ status          │
    │ created_at      │
    └────────┬────────┘
             │
             │ 1:N
             ▼
    ┌─────────────────┐
    │  ORDER_ITEMS    │
    ├─────────────────┤
    │ id              │
    │ order_id FK     │
    │ product_id FK   │
    │ quantity        │
    │ price           │
    │ subtotal        │
    └─────────────────┘
```

### Migration Files:

```php
<?php

// database/migrations/2024_01_01_000001_create_users_table.php
Schema::create('users', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('email')->unique();
    $table->string('password');
    $table->enum('role', ['admin', 'kasir', 'customer'])->default('customer');
    $table->timestamps();
    $table->softDeletes();

    // Indexes
    $table->index('email');
    $table->index('role');
});

// database/migrations/2024_01_02_000002_create_products_table.php
Schema::create('products', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->text('description')->nullable();
    $table->decimal('price', 12, 2);
    $table->integer('stock')->default(0);
    $table->foreignId('category_id')->constrained('categories')->onDelete('cascade');
    $table->string('sku')->unique();
    $table->timestamps();
    $table->softDeletes();

    $table->index(['sku', 'category_id']);
});

// database/migrations/2024_01_03_000003_create_orders_table.php
Schema::create('orders', function (Blueprint $table) {
    $table->id();
    $table->foreignId('user_id')->constrained('users')->onDelete('cascade');
    $table->decimal('total_price', 12, 2);
    $table->enum('status', ['pending', 'completed', 'cancelled'])->default('pending');
    $table->text('notes')->nullable();
    $table->timestamps();

    $table->index(['user_id', 'status', 'created_at']);
});

// database/migrations/2024_01_04_000004_create_order_items_table.php
Schema::create('order_items', function (Blueprint $table) {
    $table->id();
    $table->foreignId('order_id')->constrained('orders')->onDelete('cascade');
    $table->foreignId('product_id')->constrained('products')->onDelete('restrict');
    $table->integer('quantity');
    $table->decimal('price', 12, 2);
    $table->decimal('subtotal', 12, 2);
    $table->timestamps();

    $table->index(['order_id', 'product_id']);
});
```

---

## 🏛️ Model & Relationships

### Model Relationships - Detail:

```php
<?php

// app/Models/User.php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\HasMany;

class User extends Model
{
    protected $fillable = ['name', 'email', 'password', 'role'];

    // One to Many: User -> Orders
    public function orders(): HasMany
    {
        return $this->hasMany(Order::class);
    }

    // Query optimization
    public function scopeAdmin($query)
    {
        return $query->where('role', 'admin');
    }

    public function scopeKasir($query)
    {
        return $query->where('role', 'kasir');
    }
}

// app/Models/Product.php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\{
    BelongsTo,
    HasMany,
    BelongsToMany
};

class Product extends Model
{
    protected $fillable = ['name', 'price', 'stock', 'category_id', 'sku'];
    protected $casts = [
        'price' => 'decimal:2',
    ];

    // Many to One: Product -> Category
    public function category(): BelongsTo
    {
        return $this->belongsTo(Category::class);
    }

    // Many to Many: Product -> OrderItems
    public function orders(): BelongsToMany
    {
        return $this->belongsToMany(Order::class, 'order_items')
            ->withPivot(['quantity', 'price', 'subtotal'])
            ->withTimestamps();
    }

    // One to Many: Product -> OrderItems
    public function orderItems(): HasMany
    {
        return $this->hasMany(OrderItem::class);
    }

    // Scope
    public function scopeInStock($query)
    {
        return $query->where('stock', '>', 0);
    }

    public function scopeByCategory($query, $categoryId)
    {
        return $query->where('category_id', $categoryId);
    }
}

// app/Models/Order.php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\{
    BelongsTo,
    HasMany,
    HasManyThrough
};

class Order extends Model
{
    protected $fillable = ['user_id', 'total_price', 'status', 'notes'];
    protected $casts = [
        'total_price' => 'decimal:2',
    ];

    // Many to One: Order -> User
    public function user(): BelongsTo
    {
        return $this->belongsTo(User::class);
    }

    // One to Many: Order -> OrderItems
    public function items(): HasMany
    {
        return $this->hasMany(OrderItem::class);
    }

    // Through relationship: Order -> Products (through OrderItems)
    public function products(): HasManyThrough
    {
        return $this->hasManyThrough(
            Product::class,
            OrderItem::class,
            'order_id',      // Foreign key on OrderItem
            'id',            // Foreign key on Product
            'id',            // Local key on Order
            'product_id'     // Local key on OrderItem
        );
    }

    // Scope
    public function scopeCompleted($query)
    {
        return $query->where('status', 'completed');
    }

    public function scopeRecent($query)
    {
        return $query->orderByDesc('created_at');
    }
}

// app/Models/OrderItem.php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\BelongsTo;

class OrderItem extends Model
{
    protected $fillable = ['order_id', 'product_id', 'quantity', 'price', 'subtotal'];
    protected $casts = [
        'price' => 'decimal:2',
        'subtotal' => 'decimal:2',
    ];

    public function order(): BelongsTo
    {
        return $this->belongsTo(Order::class);
    }

    public function product(): BelongsTo
    {
        return $this->belongsTo(Product::class);
    }
}
```

---

## 🏗️ Repository Pattern

Repository Pattern memisahkan data access logic dari business logic.

### Struktur Repository:

```
app/
├── Interfaces/
│   └── ProductRepositoryInterface.php
├── Repositories/
│   └── ProductRepository.php
└── Models/
    └── Product.php
```

### Implementasi:

```php
<?php

// app/Interfaces/ProductRepositoryInterface.php
namespace App\Interfaces;

interface ProductRepositoryInterface
{
    public function all();
    public function paginate($perPage = 15);
    public function find($id);
    public function create(array $data);
    public function update($id, array $data);
    public function delete($id);
    public function findBySlug($slug);
    public function getInStock();
}

// app/Repositories/ProductRepository.php
namespace App\Repositories;

use App\Models\Product;
use App\Interfaces\ProductRepositoryInterface;

class ProductRepository implements ProductRepositoryInterface
{
    protected $model;

    public function __construct(Product $model)
    {
        $this->model = $model;
    }

    public function all()
    {
        return $this->model->all();
    }

    public function paginate($perPage = 15)
    {
        return $this->model->paginate($perPage);
    }

    public function find($id)
    {
        return $this->model->findOrFail($id);
    }

    public function create(array $data)
    {
        return $this->model->create($data);
    }

    public function update($id, array $data)
    {
        $product = $this->find($id);
        $product->update($data);
        return $product;
    }

    public function delete($id)
    {
        $product = $this->find($id);
        return $product->delete();
    }

    public function findBySlug($slug)
    {
        return $this->model->where('slug', $slug)->firstOrFail();
    }

    public function getInStock()
    {
        return $this->model->inStock()->get();
    }
}

// app/Providers/AppServiceProvider.php - Register Repository
public function register()
{
    $this->app->bind(
        ProductRepositoryInterface::class,
        ProductRepository::class
    );
}

// app/Http/Controllers/ProductController.php - Use Repository
class ProductController extends Controller
{
    protected $productRepository;

    public function __construct(ProductRepositoryInterface $productRepository)
    {
        $this->productRepository = $productRepository;
    }

    public function index()
    {
        $products = $this->productRepository->paginate(15);
        return view('products.index', compact('products'));
    }

    public function show(Product $product)
    {
        $product = $this->productRepository->find($product->id);
        return view('products.show', compact('product'));
    }
}
```

---

## 💼 Service Layer

Service Layer berisi business logic yang independent dari framework.

```php
<?php

// app/Services/OrderService.php
namespace App\Services;

use App\Models\Order;
use App\Models\OrderItem;
use App\Interfaces\ProductRepositoryInterface;
use Illuminate\Support\Facades\DB;

class OrderService
{
    protected $productRepository;

    public function __construct(ProductRepositoryInterface $productRepository)
    {
        $this->productRepository = $productRepository;
    }

    /**
     * Create order dengan items
     */
    public function createOrder(array $orderData, array $items)
    {
        return DB::transaction(function () use ($orderData, $items) {
            // Create order
            $order = Order::create($orderData);

            // Create order items dan update stock
            $totalPrice = 0;

            foreach ($items as $item) {
                $product = $this->productRepository->find($item['product_id']);
                $subtotal = $product->price * $item['quantity'];

                // Validasi stock
                if ($product->stock < $item['quantity']) {
                    throw new \Exception("Insufficient stock for {$product->name}");
                }

                // Create order item
                OrderItem::create([
                    'order_id' => $order->id,
                    'product_id' => $product->id,
                    'quantity' => $item['quantity'],
                    'price' => $product->price,
                    'subtotal' => $subtotal,
                ]);

                // Update product stock
                $product->decrement('stock', $item['quantity']);

                $totalPrice += $subtotal;
            }

            // Update order total
            $order->update(['total_price' => $totalPrice]);

            return $order->load('items.product');
        });
    }

    /**
     * Complete order
     */
    public function completeOrder($orderId)
    {
        $order = Order::findOrFail($orderId);
        $order->update(['status' => 'completed']);

        // Send notification, email, etc
        event(new OrderCompleted($order));

        return $order;
    }

    /**
     * Cancel order dan restore stock
     */
    public function cancelOrder($orderId)
    {
        return DB::transaction(function () use ($orderId) {
            $order = Order::findOrFail($orderId);

            if ($order->status === 'completed') {
                throw new \Exception('Cannot cancel completed order');
            }

            // Restore stock
            foreach ($order->items as $item) {
                $item->product->increment('stock', $item->quantity);
            }

            $order->update(['status' => 'cancelled']);

            return $order;
        });
    }
}

// Usage in Controller
class OrderController extends Controller
{
    protected $orderService;

    public function __construct(OrderService $orderService)
    {
        $this->orderService = $orderService;
    }

    public function store(StoreOrderRequest $request)
    {
        try {
            $order = $this->orderService->createOrder(
                $request->only(['user_id', 'notes']),
                $request->input('items')
            );

            return redirect()->route('orders.show', $order)
                ->with('success', 'Order created successfully');
        } catch (\Exception $e) {
            return back()->withErrors(['error' => $e->getMessage()]);
        }
    }
}
```

---

## 🔐 Authentication & Authorization

### Authentication Setup:

```php
<?php

// config/auth.php
return [
    'defaults' => [
        'guard' => env('AUTH_GUARD', 'web'),
        'passwords' => 'users'
    ],

    'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],
        'api' => [
            'driver' => 'token',
            'provider' => 'users',
            'hash' => true,
        ],
    ],

    'providers' => [
        'users' => [
            'driver' => 'eloquent',
            'model' => App\Models\User::class,
        ],
    ],

    'passwords' => [
        'users' => [
            'provider' => 'users',
            'table' => 'password_resets',
            'expire' => 60,
        ],
    ],
];
```

### Authorization dengan Policy:

```php
<?php

// app/Policies/OrderPolicy.php
namespace App\Policies;

use App\Models\User;
use App\Models\Order;

class OrderPolicy
{
    public function view(User $user, Order $order)
    {
        return $user->id === $order->user_id || $user->isAdmin();
    }

    public function update(User $user, Order $order)
    {
        return $user->is Admin() && $order->status !== 'completed';
    }

    public function delete(User $user, Order $order)
    {
        return $user->isAdmin();
    }
}

// app/Providers/AuthServiceProvider.php
class AuthServiceProvider extends ServiceProvider
{
    protected $policies = [
        Order::class => OrderPolicy::class,
        Product::class => ProductPolicy::class,
    ];

    public function boot()
    {
        $this->registerPolicies();

        Gate::define('admin', function (User $user) {
            return $user->role === 'admin';
        });
    }
}

// Usage in Controller
public function update(Request $request, Order $order)
{
    $this->authorize('update', $order);

    $order->update($request->validated());
    return redirect()->route('orders.show', $order);
}

// Usage in Blade
@can('update', $order)
    <a href="{{ route('orders.edit', $order) }}">Edit</a>
@endcan
```

---

## ✅ Validation & Form Request

### Form Request Validation:

```php
<?php

// app/Http/Requests/StoreOrderRequest.php
namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StoreOrderRequest extends FormRequest
{
    public function authorize()
    {
        return auth()->check();
    }

    public function rules()
    {
        return [
            'user_id' => 'required|exists:users,id',
            'notes' => 'nullable|string|max:500',
            'items' => 'required|array|min:1',
            'items.*.product_id' => 'required|exists:products,id',
            'items.*.quantity' => 'required|integer|min:1|max:100',
        ];
    }

    public function messages()
    {
        return [
            'items.required' => 'Order must have at least one item',
            'items.*.product_id.required' => 'Product ID is required',
            'items.*.quantity.min' => 'Quantity must be at least 1',
        ];
    }

    public function validated()
    {
        return array_merge(parent::validated(), [
            'created_by' => auth()->id(),
        ]);
    }
}

// Usage in Controller
public function store(StoreOrderRequest $request)
{
    $order = OrderService::createOrder($request->validated());
    return redirect()->route('orders.show', $order);
}
```

---

## 🌐 API Design

### RESTful API Setup:

```php
<?php

// routes/api.php
Route::prefix('v1')->middleware(['api', 'throttle:60,1'])->group(function () {

    // Public endpoints
    Route::get('/products', [ProductController::class, 'index']);
    Route::get('/products/{product}', [ProductController::class, 'show']);

    // Protected endpoints
    Route::middleware('auth:sanctum')->group(function () {
        // User endpoints
        Route::get('/me', [AuthController::class, 'me']);
        Route::post('/logout', [AuthController::class, 'logout']);

        // Order endpoints
        Route::post('/orders', [OrderController::class, 'store']);
        Route::get('/orders', [OrderController::class, 'index']);
        Route::get('/orders/{order}', [OrderController::class, 'show']);
        Route::put('/orders/{order}', [OrderController::class, 'update']);
        Route::delete('/orders/{order}', [OrderController::class, 'destroy']);
    });
});

// app/Http/Resources/OrderResource.php
namespace App\Http\Resources;

use Illuminate\Http\Resources\Json\JsonResource;

class OrderResource extends JsonResource
{
    public function toArray($request)
    {
        return [
            'id' => $this->id,
            'user_id' => $this->user_id,
            'total_price' => (float) $this->total_price,
            'status' => $this->status,
            'items' => OrderItemResource::collection($this->whenLoaded('items')),
            'created_at' => $this->created_at->toIso8601String(),
            'updated_at' => $this->updated_at->toIso8601String(),
        ];
    }
}

// Controller
public function index()
{
    $orders = Order::where('user_id', auth()->id())->paginate();
    return OrderResource::collection($orders);
}

public function show(Order $order)
{
    $this->authorize('view', $order);
    return new OrderResource($order->load('items.product'));
}
```

---

## ⚡ Caching Strategy

### Caching Implementation:

```php
<?php

// app/Services/ProductService.php
namespace App\Services;

use App\Models\Product;
use Illuminate\Support\Facades\Cache;

class ProductService
{
    public function getAllProducts($forceRefresh = false)
    {
        $cacheKey = 'products.all';
        $cacheDuration = 3600; // 1 hour

        if ($forceRefresh) {
            Cache::forget($cacheKey);
        }

        return Cache::remember($cacheKey, $cacheDuration, function () {
            return Product::with('category')->get();
        });
    }

    public function getProductsByCategory($categoryId)
    {
        $cacheKey = "products.category.{$categoryId}";

        return Cache::remember($cacheKey, 3600, function () use ($categoryId) {
            return Product::where('category_id', $categoryId)
                ->inStock()
                ->get();
        });
    }

    public function createProduct(array $data)
    {
        $product = Product::create($data);

        // Invalidate related caches
        Cache::forget('products.all');
        Cache::forget("products.category.{$product->category_id}");

        return $product;
    }

    public function updateProduct($id, array $data)
    {
        $product = Product::find($id);
        $product->update($data);

        // Invalidate caches
        Cache::forget('products.all');
        Cache::forget("products.category.{$product->category_id}");
        if (isset($data['category_id'])) {
            Cache::forget("products.category.{$data['category_id']}");
        }

        return $product;
    }
}
```

---

## 🔄 Queue & Jobs

### Background Job Processing:

```php
<?php

// php artisan make:job SendOrderEmail
namespace App\Jobs;

use App\Models\Order;
use App\Mail\OrderCreated;
use Illuminate\Bus\Queueable;
use Illuminate\Queue\SerializesModels;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Support\Facades\Mail;

class SendOrderEmail implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    public $order;
    public $tries = 3;
    public $timeout = 60;

    public function __construct(Order $order)
    {
        $this->order = $order;
    }

    public function handle()
    {
        Mail::to($this->order->user->email)
            ->send(new OrderCreated($this->order));
    }

    public function failed(\Exception $exception)
    {
        // Log or notify on failure
        \Log::error('Failed to send order email', [
            'order_id' => $this->order->id,
            'error' => $exception->getMessage(),
        ]);
    }
}

// Usage - Dispatch job
public function store(StoreOrderRequest $request)
{
    $order = $this->orderService->createOrder($request->validated());

    // Dispatch as async job
    SendOrderEmail::dispatch($order)
        ->onQueue('emails')
        ->delay(now()->addMinutes(5));

    return redirect()->route('orders.show', $order);
}

// Run queue worker
// php artisan queue:work --queue=emails
```

---

## 🧪 Testing

### Feature Test:

```php
<?php

// tests/Feature/OrderControllerTest.php
namespace Tests\Feature;

use App\Models\User;
use App\Models\Order;
use App\Models\Product;
use Illuminate\Foundation\Testing\RefreshDatabase;
use Tests\TestCase;

class OrderControllerTest extends TestCase
{
    use RefreshDatabase;

    protected $user;

    protected function setUp(): void
    {
        parent::setUp();
        $this->user = User::factory()->create(['role' => 'customer']);
    }

    public function test_can_view_orders_index()
    {
        $response = $this->actingAs($this->user)->get('/orders');
        $response->assertStatus(200);
        $response->assertViewIs('orders.index');
    }

    public function test_can_create_order()
    {
        $products = Product::factory(2)->create();

        $response = $this->actingAs($this->user)->post('/orders', [
            'user_id' => $this->user->id,
            'items' => [
                [
                    'product_id' => $products[0]->id,
                    'quantity' => 2,
                ],
                [
                    'product_id' => $products[1]->id,
                    'quantity' => 1,
                ],
            ],
        ]);

        $response->assertRedirect();
        $this->assertDatabaseHas('orders', [
            'user_id' => $this->user->id,
        ]);
    }

    public function test_unauthorized_user_cannot_view_others_order()
    {
        $otherUser = User::factory()->create();
        $order = Order::factory()->create(['user_id' => $otherUser->id]);

        $response = $this->actingAs($this->user)->get("/orders/{$order->id}");
        $response->assertStatus(403);
    }
}
```

---

## 🚀 Production Deployment

### Environment Configuration:

```bash
# .env (production)
APP_ENV=production
APP_DEBUG=false
APP_KEY=base64:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
APP_URL=https://example.com

DB_CONNECTION=mysql
DB_HOST=localhost
DB_DATABASE=productiondb
DB_USERNAME=root
DB_PASSWORD=strongpassword

CACHE_DRIVER=redis
QUEUE_CONNECTION=redis
SESSION_DRIVER=cookie

MAIL_MAILER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=your-email@gmail.com
MAIL_PASSWORD=your-password

LOG_CHANNEL=stack
LOG_LEVEL=info
```

### Deployment Checklist:

```markdown
## Pre-Deployment

- [ ] Set APP_ENV=production
- [ ] Set APP_DEBUG=false
- [ ] Generate APP_KEY
- [ ] Run migrations
- [ ] Run seeders
- [ ] Optimize code (config/routes/classes)
- [ ] Clear cache
- [ ] Set file permissions

## Environment Setup

- [ ] Configure database connection
- [ ] Setup Redis cache
- [ ] Configure queue driver
- [ ] Setup email service
- [ ] Configure storage disk
- [ ] Setup SSL certificate

## Monitoring

- [ ] Setup application monitoring (New Relic/DataDog)
- [ ] Setup log aggregation
- [ ] Setup error tracking (Sentry)
- [ ] Setup uptime monitoring
- [ ] Setup database backups

## Optimization

- [ ] Enable query caching
- [ ] Configure CDN
- [ ] Setup gzip compression
- [ ] Optimize database indexes
- [ ] Configure rate limiting
```

---

## ⭐ Best Practices

### 1. Code Organization

```
✅ Teratur dan konsisten
✅ Satu file = satu class
✅ Gunakan namespace yang jelas
✅ Gunakan PSR-12 coding standard
✅ Pisahkan business logic dari framework logic
```

### 2. Naming Convention

```php
// ✅ BAIK
class ProductController extends Controller {}
interface ProductRepositoryInterface {}
trait CreatedAtScope {}
const DEFAULT_PAGE_SIZE = 15;

// ❌ BURUK
class pc extends Controller {}
interface ProductRepo {}
trait scope {}
define('DPS', 15);
```

### 3. Error Handling

```php
// ✅ BAIK
try {
    // Process
} catch (\Exception $e) {
    \Log::error('Processing failed', ['error' => $e]);
    return back()->withErrors(['error' => 'Processing failed']);
}

// ❌ BURUK
try {
    // Process
} catch (\Exception $e) {
    die($e->getMessage());
}
```

### 4. Database Performance

```php
// ✅ BAIK - Eager loading
$orders = Order::with(['user', 'items.product'])->get();

// ❌ BURUK - N+1 query
$orders = Order::all();
foreach ($orders as $order) {
    $user = $order->user; // Query setiap iterasi
}
```

### 5. Security

```php
// ✅ BAIK - Hash password
$user = User::create([
    'password' => Hash::make($request->password),
]);

// ❌ BURUK - Store plaintext
$user = User::create([
    'password' => $request->password,
]);
```

---

## 📝 Summary

### Arsitektur Laravel Flow:

```
HTTP REQUEST
    ↓
ROUTING (Match route)
    ↓
MIDDLEWARE (Filter request)
    ↓
SERVICE PROVIDER (Setup dependencies)
    ↓
CONTROLLER (Handle request)
    ↓
REPOSITORY (Get data)
    ↓
SERVICE (Process business logic)
    ↓
MODEL (Database interaction)
    ↓
RESPONSE (Return result)
```

### Checklist Arsitektur yang Baik:

- [ ] Clear separation of concerns
- [ ] Dependency injection everywhere
- [ ] Proper error handling
- [ ] Database optimization
- [ ] Proper validation
- [ ] API resources
- [ ] Comprehensive testing
- [ ] Proper documentation
- [ ] Security best practices
- [ ] Performance optimization
