# 💡 Tips & Trik Laravel - Dari Dasar Hingga Profesional

## 📌 Daftar Isi

1. [Arsitektur & Structure](#arsitektur)
2. [Database Query Optimization](#database)
3. [Routing Best Practices](#routing)
4. [Middleware Tips](#middleware)
5. [Blade Template Tricks](#blade)
6. [Model & Eloquent](#model)
7. [Controller Best Practices](#controller)
8. [Authentication & Authorization](#auth)
9. [Form & Validation](#validation)
10. [API Development](#api)
11. [Performance Optimization](#performance)
12. [Debugging & Testing](#debugging)
13. [Deployment & DevOps](#deployment)
14. [Real Case: POS Implementation](#realcase)

---

## 🏗️ Arsitektur & Structure

### 1. Service Layer Pattern

Pisahkan business logic dari controller.

```php
<?php

// ❌ BURUK - Logic di controller
class OrderController extends Controller
{
    public function store(StoreOrderRequest $request)
    {
        $order = Order::create($request->validated());

        foreach ($request->items as $item) {
            $product = Product::find($item['product_id']);

            OrderItem::create([
                'order_id' => $order->id,
                'product_id' => $product->id,
                'quantity' => $item['quantity'],
                'price' => $product->price,
            ]);

            $product->decrement('stock', $item['quantity']);
        }

        return redirect()->route('orders.show', $order);
    }
}

// ✅ BAIK - Logic di service
class OrderService
{
    public function createOrder(array $data): Order
    {
        return DB::transaction(function () use ($data) {
            $order = Order::create([
                'user_id' => $data['user_id'],
                'total_price' => 0,
            ]);

            $total = 0;
            foreach ($data['items'] as $item) {
                $product = Product::find($item['product_id']);
                $subtotal = $product->price * $item['quantity'];

                OrderItem::create([
                    'order_id' => $order->id,
                    'product_id' => $product->id,
                    'quantity' => $item['quantity'],
                    'price' => $product->price,
                    'subtotal' => $subtotal,
                ]);

                $product->decrement('stock', $item['quantity']);
                $total += $subtotal;
            }

            $order->update(['total_price' => $total]);

            event(new OrderCreated($order));

            return $order;
        });
    }
}

// Controller - Clean & Simple
class OrderController extends Controller
{
    protected $orderService;

    public function __construct(OrderService $orderService)
    {
        $this->orderService = $orderService;
    }

    public function store(StoreOrderRequest $request)
    {
        $order = $this->orderService->createOrder($request->validated());

        return redirect()->route('orders.show', $order)
            ->with('success', 'Order berhasil dibuat');
    }
}
?>
```

### 2. Repository Pattern

Untuk project besar, gunakan repository untuk data access.

```php
<?php

// app/Interfaces/OrderRepositoryInterface.php
interface OrderRepositoryInterface
{
    public function getLatest($limit = 10);
    public function findWithItems($id);
    public function create(array $data);
    public function update($id, array $data);
    public function getByUser($userId);
    public function getPending();
}

// app/Repositories/OrderRepository.php
class OrderRepository implements OrderRepositoryInterface
{
    protected $model;

    public function __construct(Order $model)
    {
        $this->model = $model;
    }

    public function getLatest($limit = 10)
    {
        return $this->model
            ->with('user', 'items.product')
            ->latest()
            ->limit($limit)
            ->get();
    }

    public function findWithItems($id)
    {
        return $this->model
            ->with('user', 'items.product')
            ->findOrFail($id);
    }

    public function create(array $data)
    {
        return $this->model->create($data);
    }

    public function getByUser($userId)
    {
        return $this->model
            ->where('user_id', $userId)
            ->latest()
            ->get();
    }

    public function getPending()
    {
        return $this->model
            ->where('status', 'pending')
            ->get();
    }
}

// app/Providers/RepositoryServiceProvider.php
class RepositoryServiceProvider extends ServiceProvider
{
    public function register()
    {
        $this->app->bind(
            OrderRepositoryInterface::class,
            OrderRepository::class
        );
    }
}

// Usage
class OrderController extends Controller
{
    protected $orderRepository;

    public function __construct(OrderRepositoryInterface $orderRepository)
    {
        $this->orderRepository = $orderRepository;
    }

    public function index()
    {
        $orders = $this->orderRepository->getLatest(15);
        return view('orders.index', compact('orders'));
    }
}
?>
```

---

## 🗄️ Database Query Optimization

### 1. Eager Loading (Prevent N+1)

```php
<?php

// ❌ BURUK - N+1 Query problem
$orders = Order::all(); // 1 query
foreach ($orders as $order) {
    echo $order->user->name; // Query setiap iterasi (N queries)
}
// Total: 1 + N queries = LAMBAT!

// ✅ BAIK - Eager loading
$orders = Order::with('user')->get(); // 2 queries
foreach ($orders as $order) {
    echo $order->user->name; // Tidak ada query tambahan
}

// ✅ BAIK - Multiple relations
$orders = Order::with(['user', 'items.product', 'payment'])->get();

// ✅ BAIK - Nested eager loading
$orders = Order::with('items.product.category')->get();

// ✅ BAIK - Conditional eager loading
$orders = Order::with([
    'user',
    'items' => function ($query) {
        $query->where('quantity', '>', 1);
    }
])->get();

// ✅ BAIK - Lazy eager loading (PHP 8.1+)
$orders = Order::all();
$orders->load('user', 'items.product');
?>
```

### 2. Select Specific Columns

```php
<?php

// ❌ BURUK - Select all columns
$products = Product::get();
// SELECT * FROM products (mengambil semua kolom)

// ✅ BAIK - Select hanya yang perlu
$products = Product::select('id', 'name', 'price')->get();
// SELECT id, name, price FROM products

// ✅ BAIK - Select dengan relation
$orders = Order::with(['user' => function ($query) {
    $query->select('id', 'name', 'email');
}])->get();

// ✅ BAIK - Select dengan counting
$users = User::withCount('orders')->get();
// SELECT users.*, COUNT(orders.id) as orders_count
?>
```

### 3. Chunking untuk Data Besar

```php
<?php

// ❌ BURUK - Load semua data ke memory
$products = Product::all();
foreach ($products as $product) {
    // Process
} // Memory ERROR jika data besar!

// ✅ BAIK - Gunakan chunk
Product::chunk(100, function ($products) {
    foreach ($products as $product) {
        // Process 100 items per chunk
    }
});

// ✅ BAIK - Chunk dengan condition
Order::where('status', 'completed')
    ->chunk(50, function ($orders) {
        foreach ($orders as $order) {
            // Send notification
        }
    });

// ✅ BAIK - Lazy loading (PHP 8.1+)
foreach (Product::lazy(100) as $product) {
    // Process dengan streaming
}
?>
```

### 4. Query Optimization Techniques

```php
<?php

// ✅ BAIK - Use whereIn daripada multiple orWhere
$products = Product::whereIn('id', [1, 2, 3, 4, 5])->get();

// ✅ BAIK - Use whereBetween
$orders = Order::whereBetween('total_price', [100, 1000])->get();

// ✅ BAIK - Use exists() untuk check
if (Order::where('user_id', $userId)->exists()) {
    // User punya order
}

// ✅ BAIK - Limit hasil jika tidak perlu semua
$latestOrders = Order::latest()->limit(10)->get();

// ✅ BAIK - Use pluck() untuk single column
$names = User::pluck('name'); // Array of names

// ✅ BAIK - Use first() instead of get()->first()
$user = User::find(1); // Direct
$user = User::where('email', $email)->first(); // Better

// ✅ BAIK - Use exists/doesntExist
if (User::where('email', $email)->doesntExist()) {
    // Create user
}

// ✅ BAIK - Use dirty() untuk cek changes
if ($user->isDirty('email')) {
    // Email berubah
}
?>
```

---

## 🛣️ Routing Best Practices

### 1. Organized Route Structure

```php
<?php

// routes/web.php

// ===== PUBLIC ROUTES =====
Route::get('/', [HomeController::class, 'index'])->name('home');
Route::get('/about', [PageController::class, 'about'])->name('about');
Route::get('/contact', [PageController::class, 'contact'])->name('contact');

// ===== PRODUCT ROUTES =====
Route::get('/products', [ProductController::class, 'index'])->name('products.index');
Route::get('/products/{product:slug}', [ProductController::class, 'show'])->name('products.show');

// ===== AUTH ROUTES =====
Route::middleware('guest')->group(function () {
    Route::get('/login', [AuthController::class, 'showLogin'])->name('login');
    Route::post('/login', [AuthController::class, 'login'])->name('login.post');
    Route::get('/register', [AuthController::class, 'showRegister'])->name('register');
    Route::post('/register', [AuthController::class, 'register'])->name('register.post');
});

Route::post('/logout', [AuthController::class, 'logout'])->name('logout')
    ->middleware('auth');

// ===== PROTECTED/AUTHENTICATED ROUTES =====
Route::middleware('auth')->group(function () {

    Route::get('/dashboard', [DashboardController::class, 'index'])->name('dashboard');

    // User Profile
    Route::prefix('profile')->name('profile.')->group(function () {
        Route::get('/', [ProfileController::class, 'show'])->name('show');
        Route::put('/', [ProfileController::class, 'update'])->name('update');
        Route::get('/edit', [ProfileController::class, 'edit'])->name('edit');
    });

    // My Orders
    Route::get('/orders', [OrderController::class, 'myOrders'])->name('orders.index');
    Route::get('/orders/{order}', [OrderController::class, 'show'])->name('orders.show');
    Route::post('/orders', [OrderController::class, 'store'])->name('orders.store');

    // Admin Routes
    Route::middleware('can:admin')->prefix('admin')->name('admin.')->group(function () {
        Route::get('/', [AdminController::class, 'dashboard'])->name('dashboard');

        Route::resource('products', ProductController::class)
            ->except(['show'])
            ->middleware('log.admin-action');

        Route::resource('orders', OrderController::class);

        Route::resource('users', UserController::class);
    });
});

// ===== API ROUTES =====
Route::prefix('api/v1')
    ->middleware(['api', 'throttle:60,1'])
    ->group(function () {

        Route::get('/products', [API\ProductController::class, 'index']);
        Route::get('/products/{product}', [API\ProductController::class, 'show']);

        Route::middleware('auth:sanctum')->group(function () {
            Route::post('/orders', [API\OrderController::class, 'store']);
            Route::get('/orders', [API\OrderController::class, 'index']);
        });
    });
?>
```

### 2. Route Model Binding

```php
<?php

// routes/web.php

// Implicit binding - find by id
Route::get('/products/{product}', [ProductController::class, 'show']);

// Explicit binding - find by custom column
Route::get('/products/{product:slug}', [ProductController::class, 'show']);

// Custom binding
Route::bind('order', function ($value) {
    return Order::where('uuid', $value)->orWhere('id', $value)->firstOrFail();
});

// In controller
public function show(Product $product)
{
    // $product automatically injected
}

// =======

// routes/RouteServiceProvider.php - Register custom binding

public function boot()
{
    $this->routes(function () {
        Route::bind('product', function ($value) {
            return Product::where('slug', $value)
                ->orWhere('id', $value)
                ->firstOrFail();
        });
    });
}
?>
```

---

## 🛡️ Middleware Tips

### 1. Middleware Benefits

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
        if (!auth()->check() || !auth()->user()->hasRole($role)) {
            abort(403, 'Unauthorized');
        }

        return $next($request);
    }
}

// Register in app/Http/Kernel.php
protected $routeMiddleware = [
    'role' => CheckRole::class,
];

// Usage
Route::middleware('role:admin')->group(function () {
    Route::resource('products', ProductController::class);
});
?>
```

### 2. Global Middleware

```php
<?php

// app/Http/Middleware/LogUserActivity.php
namespace App\Http\Middleware;

use Closure;
use Illuminate\Support\Facades\Log;

class LogUserActivity
{
    public function handle($request, Closure $next)
    {
        $response = $next($request);

        if (auth()->check()) {
            Log::info('User activity', [
                'user_id' => auth()->id(),
                'method' => $request->method(),
                'path' => $request->path(),
            ]);
        }

        return $response;
    }
}

// Register in app/Http/Kernel.php
protected $middleware = [
    LogUserActivity::class,
];
?>
```

---

## 🎨 Blade Template Tricks

### 1. Useful Directives

```blade
{{-- @isset - Check if variable set --}}
@isset($user)
    <p>{{ $user->name }}</p>
@endisset

{{-- @empty - Check if empty --}}
@empty($users)
    <p>No users</p>
@endempty

{{-- @auth & @guest --}}
@auth
    <span>Welcome {{ Auth::user()->name }}</span>
@endauth

@guest
    <a href="/login">Login</a>
@endguest

{{-- @once - Render once per request --}}
@once
    <script src="app.js"></script>
@endonce

{{-- @env - Check environment --}}
@env('production')
    <div>Production Mode</div>
@endenv

{{-- @switch --}}
@switch($user->role)
    @case('admin')
        <span>Administrator</span>
        @break
    @case('user')
        <span>Regular User</span>
        @break
@endswitch

{{-- @forelse - Loop with empty check --}}
@forelse($products as $product)
    <div>{{ $product->name }}</div>
@empty
    <p>No products</p>
@endforelse
```

### 2. Loop Variable

```blade
{{-- $loop variable tersedia dalam foreach --}}
@foreach($items as $item)
    @if($loop->first)
        <h2>First Item</h2>
    @endif

    <p>Item #{{ $loop->iteration }} of {{ $loop->count }}</p>
    <p>{{ $item->name }}</p>

    @if($loop->last)
        <h2>Last Item</h2>
    @endif
@endforeach

{{-- Loop properties --}}
$loop->index         {{-- 0-based --}}
$loop->iteration     {{-- 1-based --}}
$loop->count         {{-- Total items --}}
$loop->first         {{-- First iteration --}}
$loop->last          {{-- Last iteration --}}
$loop->even          {{-- Even iteration --}}
$loop->odd           {{-- Odd iteration --}}
$loop->depth         {{-- Nesting level --}}
$loop->parent        {{-- Parent loop --}}
```

### 3. Component & Slots

```blade
{{-- resources/views/components/button.blade.php --}}
<button {{ $attributes->merge(['class' => 'btn btn-primary']) }}>
    {{ $slot }}
</button>

{{-- Usage --}}
<x-button class="btn-lg">
    Click Me
</x-button>

{{-- Component with slots --}}
{{-- resources/views/components/card.blade.php --}}
<div class="card">
    <div class="card-header">
        {{ $header }}
    </div>
    <div class="card-body">
        {{ $slot }}
    </div>
    <div class="card-footer">
        {{ $footer ?? 'Default footer' }}
    </div>
</div>

{{-- Usage --}}
<x-card>
    <x-slot name="header">
        Card Title
    </x-slot>

    Card content here

    <x-slot name="footer">
        Custom footer
    </x-slot>
</x-card>
```

---

## 👨‍💻 Model & Eloquent

### 1. Scopes - Reusable Queries

```php
<?php

// app/Models/Product.php
class Product extends Model
{
    // Local scope
    public function scopeActive($query)
    {
        return $query->where('status', 'active');
    }

    public function scopeInStock($query)
    {
        return $query->where('stock', '>', 0);
    }

    public function scopeDiscounted($query)
    {
        return $query->where('discount', '>', 0);
    }

    public function scopeByCategory($query, $categoryId)
    {
        return $query->where('category_id', $categoryId);
    }

    // Global scope
    protected static function booted()
    {
        static::addGlobalScope('active', function (Builder $builder) {
            $builder->where('deleted_at', null);
        });
    }
}

// Usage
$activeProducts = Product::active()->get();
$inStockProducts = Product::active()->inStock()->get();
$discountedProducts = Product::discounted()->byCategory(1)->get();
?>
```

### 2. Accessors & Mutators

```php
<?php

class Order extends Model
{
    protected $casts = [
        'created_at' => 'datetime',
        'total_price' => 'decimal:2',
    ];

    // Accessor (computed property)
    public function getTotalFormatAttribute()
    {
        return 'Rp' . number_format($this->total_price, 0, ',', '.');
    }

    public function getStatusLabelAttribute()
    {
        return match($this->status) {
            'pending' => 'Menunggu',
            'completed' => 'Selesai',
            'cancelled' => 'Dibatalkan',
            default => 'Unknown',
        };
    }

    // Mutator (set value)
    public function setTotalPriceAttribute($value)
    {
        $this->attributes['total_price'] = (float) str_replace('.', '', $value);
    }
}

// Usage
echo $order->total_format; // Rp1.000.000
echo $order->status_label;  // Selesai
?>
```

### 3. Relationships Tips

```php
<?php

// Eager load dengan constraint
$users = User::with(['orders' => function ($query) {
    $query->where('status', 'completed')->latest();
}])->get();

// Check relation exists
$users = User::has('orders')->get(); // Only users with orders

// Count relations
$users = User::withCount('orders')->get();

// Relation data in array
$users = User::with('orders:id,user_id,total')->get();

// Polymorphic relations
class Comment extends Model
{
    public function commentable()
    {
        return $this->morphTo();
    }
}

// In Post/Product model
public function comments()
{
    return $this->morphMany(Comment::class, 'commentable');
}
?>
```

---

## 🎯 Controller Best Practices

### 1. Clean Controller

```php
<?php

// ✅ BAIK - Clean dan single responsibility
class ProductController extends Controller
{
    protected $productService;

    public function __construct(ProductService $productService)
    {
        $this->productService = $productService;
    }

    // Resource method - untuk CRUD
    public function index()
    {
        $products = $this->productService->getActive();
        return view('products.index', compact('products'));
    }

    public function create()
    {
        $categories = Category::all();
        return view('products.create', compact('categories'));
    }

    public function store(StoreProductRequest $request)
    {
        $product = $this->productService->create($request->validated());

        return redirect()->route('products.show', $product)
            ->with('success', 'Product created');
    }

    public function show(Product $product)
    {
        return view('products.show', compact('product'));
    }
}

// Register in routes
Route::resource('products', ProductController::class);
?>
```

---

## 🔐 Authentication & Authorization

### 1. Gates & Policies

```php
<?php

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

        // Simple gate
        Gate::define('admin', function (User $user) {
            return $user->role === 'admin';
        });

        Gate::define('edit-post', function (User $user, Post $post) {
            return $user->id === $post->user_id;
        });
    }
}

// app/Policies/OrderPolicy.php
class OrderPolicy
{
    public function view(User $user, Order $order)
    {
        return $user->id === $order->user_id || $user->isAdmin();
    }

    public function update(User $user, Order $order)
    {
        return $user->id === $order->user_id && $order->status === 'pending';
    }

    public function delete(User $user, Order $order)
    {
        return $user->isAdmin();
    }
}

// Usage in Controller
public function edit(Order $order)
{
    $this->authorize('update', $order);
    return view('orders.edit', compact('order'));
}

// Usage in Blade
@can('update', $order)
    <a href="{{ route('orders.edit', $order) }}">Edit</a>
@endcan

@cannot('delete', $order)
    <p>Cannot delete this order</p>
@endcannot
?>
```

---

## ✅ Form & Validation

### 1. Form Request Validation

```php
<?php

// app/Http/Requests/StoreProductRequest.php
class StoreProductRequest extends FormRequest
{
    public function authorize()
    {
        return auth()->user()->can('create', Product::class);
    }

    public function rules()
    {
        return [
            'name' => 'required|string|max:255|unique:products',
            'description' => 'nullable|string',
            'price' => 'required|numeric|min:0',
            'stock' => 'required|integer|min:0',
            'category_id' => 'required|exists:categories,id',
            'image' => 'nullable|image|max:2048',
        ];
    }

    public function messages()
    {
        return [
            'name.required' => 'Product name is required',
            'price.numeric' => 'Price must be a number',
        ];
    }

    public function validated()
    {
        $data = parent::validated();

        // Additional processing
        if ($this->hasFile('image')) {
            $data['image'] = $this->file('image')->store('products');
        }

        return $data;
    }
}

// Usage in Controller
public function store(StoreProductRequest $request)
{
    $product = Product::create($request->validated());
    return redirect()->route('products.show', $product);
}
?>
```

---

## 🌐 API Development

### 1. API Resources

```php
<?php

// app/Http/Resources/ProductResource.php
class ProductResource extends JsonResource
{
    public function toArray($request)
    {
        return [
            'id' => $this->id,
            'name' => $this->name,
            'price' => (float) $this->price,
            'category' => new CategoryResource($this->whenLoaded('category')),
            'created_at' => $this->created_at->toIso8601String(),
        ];
    }
}

// Usage
public function index()
{
    $products = Product::with('category')->paginate();
    return ProductResource::collection($products);
}

public function show(Product $product)
{
    return new ProductResource($product->load('category'));
}
?>
```

---

## ⚡ Performance Optimization

### 1. Caching

```php
<?php

// Cache simple result
$products = Cache::remember('all_products', 3600, function () {
    return Product::active()->get();
});

// Cache with tag
Cache::tags(['products'])->put('featured', $products, 3600);

// Invalidate cache
Cache::forget('all_products');
Cache::tags(['products'])->flush();

// In Model
class Product extends Model
{
    public static function getAllCached()
    {
        return Cache::remember('products.all', 3600, function () {
            return self::with('category')->get();
        });
    }
}
?>
```

### 2. Database Optimization

```php
<?php

// Use indexes pada kolom yang sering dicari
// Create index di migration
Schema::create('products', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('sku')->unique();
    $table->index(['category_id', 'status']); // Composite index
});

// Use paginate untuk data besar
$products = Product::paginate(15);

// Use pluck untuk single column
$ids = Product::pluck('id');
?>
```

---

## 🐛 Debugging & Testing

### 1. Laravel Tinker

```bash
# Interactive shell
php artisan tinker

# In tinker
>>> $user = User::find(1)
>>> $user->orders()->count()
>>> $user->update(['name' => 'Updated'])
>>> dd($user)
```

### 2. Logging

```php
<?php

use Illuminate\Support\Facades\Log;

// Different log levels
Log::debug('Debug message');
Log::info('Info message');
Log::warning('Warning message');
Log::error('Error message');
Log::critical('Critical error');

// With context
Log::info('User logged in', [
    'user_id' => $user->id,
    'ip' => request()->ip(),
]);

// In model
class Order extends Model
{
    public function update(array $attributes = [])
    {
        $result = parent::update($attributes);

        Log::info('Order updated', [
            'order_id' => $this->id,
            'changes' => $this->getChanges(),
        ]);

        return $result;
    }
}
?>
```

### 3. Testing

```php
<?php

// tests/Feature/OrderTest.php
class OrderTest extends TestCase
{
    use RefreshDatabase;

    public function test_can_create_order()
    {
        $user = User::factory()->create();
        $product = Product::factory()->create();

        $response = $this->actingAs($user)
            ->post('/orders', [
                'product_id' => $product->id,
                'quantity' => 2,
            ]);

        $response->assertRedirect();
        $this->assertDatabaseHas('orders', [
            'user_id' => $user->id,
        ]);
    }

    public function test_unauthorized_user_cannot_view_order()
    {
        $order = Order::factory()->create();
        $user = User::factory()->create();

        $response = $this->actingAs($user)
            ->get("/orders/{$order->id}");

        $response->assertStatus(403);
    }
}
?>
```

---

## 🚀 Deployment & DevOps

### 1. Pre-deployment Checklist

```bash
# Make sure your app is production-ready

# 1. Update environment
APP_ENV=production
APP_DEBUG=false

# 2. Generate app key
php artisan key:generate

# 3. Optimize for production
php artisan config:cache
php artisan route:cache
php artisan view:cache
php artisan event:cache

# 4. Run migrations
php artisan migrate --force

# 5. Seed database (if needed)
php artisan db:seed --force

# 6. Set permissions
chmod -R 755 storage bootstrap/cache
chown -R www-data:www-data storage bootstrap

# 7. Setup queue worker
php artisan queue:work --daemon

# 8. Schedule commands
* * * * * cd /path && php artisan schedule:run
```

---

## 🏪 Real Case: POS Implementation

### Complete POS Service

```php
<?php

// app/Services/POSService.php
namespace App\Services;

use App\Models\{Order, OrderItem, Product, User};
use Illuminate\Support\Facades\DB;

class POSService
{
    /**
     * Create POS order
     */
    public function createOrder(User $cashier, array $items): Order
    {
        return DB::transaction(function () use ($cashier, $items) {
            $order = Order::create([
                'user_id' => $cashier->id,
                'total_price' => 0,
            ]);

            $total = 0;

            foreach ($items as $item) {
                $product = Product::lock()->findOrFail($item['product_id']);

                // Validasi stock
                if ($product->stock < $item['quantity']) {
                    throw new \Exception("Insufficient stock: {$product->name}");
                }

                $subtotal = $product->price * $item['quantity'];

                OrderItem::create([
                    'order_id' => $order->id,
                    'product_id' => $product->id,
                    'quantity' => $item['quantity'],
                    'price' => $product->price,
                    'subtotal' => $subtotal,
                ]);

                $product->decrement('stock', $item['quantity']);
                $total += $subtotal;
            }

            $order->update(['total_price' => $total]);

            return $order->load('items.product');
        });
    }
}

// app/Http/Controllers/POSController.php
class POSController extends Controller
{
    protected $posService;

    public function __construct(POSService $posService)
    {
        $this->posService = $posService;
    }

    public function checkout(Request $request)
    {
        try {
            $order = $this->posService->createOrder(
                auth()->user(),
                $request->input('items')
            );

            return response()->json([
                'success' => true,
                'order_id' => $order->id,
                'total' => $order->total_price,
            ]);
        } catch (\Exception $e) {
            return response()->json([
                'success' => false,
                'message' => $e->getMessage(),
            ], 422);
        }
    }
}
?>
```

---

## ✅ Tips Checklist

- [ ] Pisahkan logic dengan Service Layer
- [ ] Use Repository Pattern untuk project besar
- [ ] Eager load relations (prevent N+1)
- [ ] Select specific columns
- [ ] Use chunking untuk data besar
- [ ] Gunakan route naming dan redirect
- [ ] Implement proper middleware
- [ ] Use Blade directives (@isset, @forelse, @auth)
- [ ] Use scopes di Model
- [ ] Implement authorization dengan Policies
- [ ] Use Form Request Validation
- [ ] Cache hasil query
- [ ] Use database indexes
- [ ] Implement proper logging
- [ ] Test your code
- [ ] Optimize untuk production
