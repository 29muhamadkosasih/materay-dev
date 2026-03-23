# 🔀 Percabangan, Array & Looping - Panduan Lengkap di PHP & Laravel

## 📌 Daftar Isi

1. [Percabangan (Conditional) - PHP](#percabangan-php)
2. [Percabangan di Blade Template](#percabangan-blade)
3. [Array - Tipe & Operasi](#array)
4. [Looping (Iterasi)](#looping)
5. [Praktik Terbaik](#best-practices)
6. [Real Case: POS System](#real-case)

---

## 🔀 Percabangan (Conditional) - PHP

### 1. If Else Statement

```php
<?php

// Simple if
if ($user->role === 'admin') {
    echo "Anda adalah admin";
}

// If else
if ($user->role === 'admin') {
    echo "Admin access";
} else {
    echo "Limited access";
}

// If elseif else
if ($user->role === 'admin') {
    $permission = 'full';
} elseif ($user->role === 'manager') {
    $permission = 'moderate';
} elseif ($user->role === 'user') {
    $permission = 'limited';
} else {
    $permission = 'none';
}

// Multiple conditions (AND)
if ($user->role === 'admin' && $user->active === true) {
    echo "Active admin";
}

// Multiple conditions (OR)
if ($user->role === 'admin' || $user->role === 'manager') {
    echo "High privilege";
}

// Complex conditions (NOT)
if (!empty($user->email) && $user->verified === true) {
    echo "Valid user";
}

// Nested if
if ($user) {
    if ($user->role === 'admin') {
        if ($user->active) {
            echo "Active admin user";
        }
    }
}
?>
```

### 2. Ternary Operator (Short If)

```php
<?php

// Basic ternary
$status = $user->active ? 'Aktif' : 'Nonaktif';

// Nested ternary (tidak disarankan - sulit dibaca)
$level = $user->role === 'admin' ? 'Admin' :
         $user->role === 'manager' ? 'Manager' :
         'User';

// Better way - gunakan match
$level = match($user->role) {
    'admin' => 'Admin',
    'manager' => 'Manager',
    default => 'User',
};

// Ternary dengan variable
$permission = (auth()->check() && auth()->user()->isAdmin()) ? 'allow' : 'deny';
?>
```

### 3. Null Coalescing Operator

```php
<?php

// Basic null coalescing (??)
$name = $user->name ?? 'Guest';

// Multiple fallback
$value = $var1 ?? $var2 ?? $var3 ?? 'default';

// With array
$email = $_GET['email'] ?? 'no-email@example.com';

// Null coalescing assignment (PHP 7.4+)
$user->name ??= 'Default Name';
// Jika $user->name adalah null, set ke 'Default Name'

// Common use case - config fallback
$timeout = config('app.timeout') ?? 30;
?>
```

### 4. Match Expression (PHP 8+) - REKOMENDASI

```php
<?php

// Match menggantikan switch dan ternary
$status = match($statusCode) {
    200 => 'OK',
    404 => 'Not Found',
    500 => 'Server Error',
    default => 'Unknown',
};

// Match dengan multiple values
$message = match($priority) {
    'high', 'urgent' => 'Prioritas tinggi',
    'medium', 'normal' => 'Prioritas sedang',
    'low' => 'Prioritas rendah',
    default => 'Tidak ada',
};

// Match dengan conditions
$discount = match(true) {
    $purchase >= 1000000 => 0.20,    // 20%
    $purchase >= 500000 => 0.15,      // 15%
    $purchase >= 100000 => 0.10,      // 10%
    default => 0,                      // Tidak ada diskon
};

// Match dengan method call
$result = match($user->role) {
    'admin' => $this->getAdminDashboard(),
    'manager' => $this->getManagerDashboard(),
    'user' => $this->getUserDashboard(),
};
?>
```

### 5. Switch Statement

```php
<?php

// Switch dengan multiple cases (fallthrough)
switch ($status) {
    case 'pending':
        echo "Menunggu";
        break;

    case 'processing':
        echo "Sedang diproses";
        break;

    case 'completed':
    case 'shipped':
        echo "Selesai";
        break;

    case 'cancelled':
    case 'failed':
        echo "Dibatalkan";
        break;

    default:
        echo "Status tidak dikenal";
}

// Important: gunakan break untuk stop case
// Tanpa break, kode akan terus ke case berikutnya (fallthrough)
?>
```

---

## 🎨 Percabangan di Blade Template

### 1. @if Directive

```blade
{{-- Simple if --}}
@if($user->role === 'admin')
    <span class="badge badge-admin">Admin</span>
@endif

{{-- If else --}}
@if($order->status === 'completed')
    <span class="success">Selesai</span>
@else
    <span class="pending">Menunggu</span>
@endif

{{-- Elseif chain --}}
@if($user->role === 'admin')
    <div>Admin Panel</div>
@elseif($user->role === 'manager')
    <div>Manager Dashboard</div>
@elseif($user->role === 'user')
    <div>User Dashboard</div>
@else
    <div>Guest</div>
@endif

{{-- Multiple conditions --}}
@if($user->active && $user->verified)
    <span class="verified">✓ Verified</span>
@endif

@if($user->role === 'admin' || $user->role === 'manager')
    <button>Edit</button>
@endif
```

### 2. @unless - Negasi If

```blade
{{-- @unless = @if NOT --}}

{{-- Jangan tampilkan jika user aktif --}}
@unless($user->active)
    <div class="alert">User tidak aktif</div>
@endunless

{{-- Equivalent dengan @if --}}
@if(!$user->active)
    <div class="alert">User tidak aktif</div>
@endif
```

### 3. @isset & @empty

```blade
{{-- Check isset --}}
@isset($user->email)
    <p>Email: {{ $user->email }}</p>
@endisset

{{-- Check empty --}}
@empty($data)
    <div class="alert">Data tidak ada</div>
@endempty

@empty($searchResults)
    <p>No products found</p>
@else
    <ul>
        @foreach($searchResults as $product)
            <li>{{ $product->name }}</li>
        @endforeach
    </ul>
@endempty

{{-- Check null --}}
@ifnull($value)
    <p>Value is null</p>
@endifnull
```

### 4. @auth & @guest

```blade
{{-- Check if authenticated --}}
@auth
    <div>Welcome {{ Auth::user()->name }}</div>
@endauth

{{-- Check if guest --}}
@guest
    <div>Please login</div>
@endguest

{{-- With guard specification --}}
@auth('sanctum')
    <div>API User</div>
@endauth

@guest('web')
    <div>Web Guest</div>
@endguest
```

### 5. @switch & @case (Blade)

```blade
{{-- Switch statement di Blade --}}
@switch($userRole)
    @case('admin')
        <span class="role-admin">Administrator</span>
        @break

    @case('manager')
        <span class="role-manager">Manager</span>
        @break

    @case('user')
        <span class="role-user">User</span>
        @break

    @default
        <span class="role-unknown">Unknown</span>
@endswitch
```

---

## 📦 Array - Tipe & Operasi

### 1. Array Dasar

```php
<?php

// Indexed array
$fruits = ['Apple', 'Banana', 'Orange'];
echo $fruits[0]; // Apple

// Associative array (key => value)
$person = [
    'name' => 'Kosasih',
    'age' => 25,
    'email' => 'kosasih@mail.com'
];
echo $person['name']; // Kosasih

// Multi-dimensional array
$users = [
    ['id' => 1, 'name' => 'Ali', 'role' => 'admin'],
    ['id' => 2, 'name' => 'Budi', 'role' => 'user'],
    ['id' => 3, 'name' => 'Citra', 'role' => 'manager']
];

echo $users[0]['name']; // Ali
echo $users[1]['role']; // user

// Nested array
$data = [
    'company' => [
        'name' => 'PT ABC',
        'address' => [
            'street' => 'Jl. Sudirman',
            'city' => 'Jakarta',
            'country' => 'Indonesia'
        ]
    ]
];

echo $data['company']['address']['city']; // Jakarta
?>
```

### 2. Array Manipulation Functions

```php
<?php

$arr = [3, 1, 4, 1, 5, 9, 2, 6];

// ===== COUNT & LENGTH =====
count($arr);      // 8
sizeof($arr);     // 8 (alias)

// ===== ADD ELEMENTS =====
array_push($arr, 10);        // Add last: [3,1,4,1,5,9,2,6,10]
array_unshift($arr, 0);      // Add first: [0,3,1,4,1,5,9,2,6,10]
$arr[] = 11;                 // Add last: [3,1,4,1,5,9,2,6,11]

// ===== REMOVE ELEMENTS =====
array_pop($arr);             // Remove last
array_shift($arr);           // Remove first
array_splice($arr, 2, 1);    // Remove 1 element at index 2
unset($arr[0]);              // Remove specific key

// ===== SORTING =====
sort($arr);                  // Ascending: [1,1,2,3,4,5,6,9]
rsort($arr);                 // Descending: [9,6,5,4,3,2,1,1]
asort($arr);                 // Sort maintain key
arsort($arr);                // Reverse sort maintain key
ksort($arr);                 // Sort by key
krsort($arr);                // Reverse sort by key

// ===== SEARCH =====
in_array(5, $arr);           // true
array_key_exists('name', $person); // true
array_search(5, $arr);       // Index of value: 4

// ===== SLICE & SPLICE =====
array_slice($arr, 2, 3);     // Get 3 elements from index 2
array_splice($arr, 1, 2);    // Remove 2 elements from index 1

// ===== MERGE & COMBINE =====
$combined = array_merge($arr1, $arr2); // Combine 2 arrays
array_combine(['a','b','c'], [1,2,3]); // ['a'=>1, 'b'=>2, 'c'=>3]

// ===== MAP & FILTER =====
array_map(fn($x) => $x * 2, [1,2,3]);     // [2,4,6]
array_filter([1,2,3,4], fn($x) => $x > 2); // [3,4]
array_reduce([1,2,3], fn($carry, $item) => $carry + $item, 0); // 6

// ===== REVERSE & FLIP =====
array_reverse($arr);         // Reverse order
array_flip($person);         // Flip key & value

// ===== CHUNK =====
array_chunk([1,2,3,4,5], 2); // [[1,2], [3,4], [5]]

// ===== KEYS & VALUES =====
array_keys($person);         // ['name', 'age', 'email']
array_values([1=>'a', 3=>'b']); // ['a', 'b']
?>
```

### 3. Array di Blade Template

```blade
{{-- Display array element --}}
<p>Name: {{ $person['name'] }}</p>

{{-- Check if key exists --}}
@if(isset($person['email']))
    <p>{{ $person['email'] }}</p>
@endif

{{-- Loop array --}}
@foreach($users as $user)
    <p>{{ $user['name'] }} - {{ $user['role'] }}</p>
@endforeach

{{-- Loop with key --}}
@foreach($person as $key => $value)
    <p>{{ $key }}: {{ $value }}</p>
@endforeach

{{-- Nested array in blade --}}
@foreach($users as $user)
    <h3>{{ $user['name'] }}</h3>
    <p>Email: {{ $user['email'] ?? 'No email' }}</p>
@endforeach
```

---

## 🔁 Looping (Iterasi)

### 1. Foreach Loop

```php
<?php

// ===== BASIC FOREACH =====

// Only values
$fruits = ['Apple', 'Banana', 'Orange'];

foreach ($fruits as $fruit) {
    echo "$fruit ";
}
// Output: Apple Banana Orange

// Key and value
$person = ['name' => 'Kosasih', 'age' => 25, 'email' => 'kosasih@mail.com'];

foreach ($person as $key => $value) {
    echo "$key: $value\n";
}
// Output:
// name: Kosasih
// age: 25
// email: kosasih@mail.com

// ===== BY REFERENCE (modify original) =====

$numbers = [1, 2, 3, 4, 5];

foreach ($numbers as &$num) {
    $num = $num * 2; // Multiply by 2
}

unset($num); // Important - break reference

echo implode(',', $numbers); // 2,4,6,8,10

// ===== NESTED FOREACH =====

$data = [
    ['id' => 1, 'name' => 'Ali'],
    ['id' => 2, 'name' => 'Budi'],
];

foreach ($data as $item) {
    foreach ($item as $key => $value) {
        echo "$key: $value | ";
    }
    echo "\n";
}

// ===== DESTRUCTURING (PHP 7.1+) =====

$pairs = [
    ['key1', 'value1'],
    ['key2', 'value2'],
];

foreach ($pairs as [$key, $value]) {
    echo "$key => $value\n";
}
?>
```

### 2. For Loop

```php
<?php

// Standard for
for ($i = 0; $i < 5; $i++) {
    echo $i; // 0 1 2 3 4
}

// Reverse loop
for ($i = 10; $i > 0; $i--) {
    echo $i; // 10 9 8 ... 1
}

// Skip elements (increment by 2)
for ($i = 0; $i <= 10; $i += 2) {
    echo $i; // 0 2 4 6 8 10
}

// Nested for
for ($i = 1; $i <= 3; $i++) {
    for ($j = 1; $j <= 3; $j++) {
        echo "$i-$j ";
    }
}
// Output: 1-1 1-2 1-3 2-1 2-2 2-3 3-1 3-2 3-3
?>
```

### 3. While & Do-While

```php
<?php

// While loop
$i = 0;
while ($i < 5) {
    echo $i;
    $i++;
}

// Do-while (execute at least once)
$count = 0;
do {
    echo "Count: $count\n";
    $count++;
} while ($count < 3);

// Example: read from database until condition
$page = 1;
while (true) {
    $users = User::where('page', $page)->get();

    if ($users->isEmpty()) {
        break; // Exit loop
    }

    foreach ($users as $user) {
        // Process
    }

    $page++;
}
?>
```

### 4. Break & Continue

```php
<?php

// ===== BREAK - Stop loop =====

for ($i = 0; $i < 10; $i++) {
    if ($i === 5) {
        break; // Stop loop when i = 5
    }
    echo $i; // 0 1 2 3 4
}

// Break multiple levels
for ($i = 0; $i < 3; $i++) {
    for ($j = 0; $j < 3; $j++) {
        if ($j === 1) {
            break 2; // Break from 2 loops
        }
        echo "$i-$j ";
    }
}
// Output: 0-0

// ===== CONTINUE - Skip iteration =====

for ($i = 0; $i < 5; $i++) {
    if ($i === 2) {
        continue; // Skip when i = 2
    }
    echo $i; // 0 1 3 4
}

// Continue with level
for ($i = 0; $i < 3; $i++) {
    for ($j = 0; $j < 3; $j++) {
        if ($j === 1) {
            continue 2; // Continue outer loop
        }
        echo "$i-$j ";
    }
    echo "| ";
}
// Output: 0-0 | 1-0 | 2-0 |
?>
```

### 5. Looping di Blade Template

```blade
{{-- Basic foreach --}}
@foreach($products as $product)
    <div class="product">
        <h3>{{ $product->name }}</h3>
        <p>Rp{{ number_format($product->price) }}</p>
    </div>
@endforeach

{{-- Foreach dengan index dan iteration --}}
@foreach($users as $user)
    <tr>
        <td>{{ $loop->iteration }}</td>
        <td>{{ $user->name }}</td>
        <td>{{ $loop->count }}</td>
    </tr>
@endforeach

{{-- Loop variable properties --}}
@foreach($items as $item)
    @if($loop->first)
        <h2>First Item</h2>
    @endif

    <p>{{ $item->name }} ({{ $loop->iteration }} of {{ $loop->count }})</p>

    @if($loop->last)
        <h2>Last Item</h2>
    @endif
@endforeach

{{-- Forelse - foreach with empty check --}}
@forelse($orders as $order)
    <div>Order #{{ $order->id }} - Rp{{ $order->total }}</div>
@empty
    <p class="alert">Belum ada order</p>
@endforelse

{{-- For loop --}}
@for($i = 1; $i <= 10; $i++)
    <p>Item #{{ $i }}</p>
@endfor

{{-- While loop --}}
@while($count < 5)
    <p>{{ $count }}</p>
    {{ $count++ }}
@endwhile
```

---

## ✅ Best Practices

### 1. Percabangan yang Baik

```php
<?php

// ✅ BAIK - Simple dan readable
if ($user->role === 'admin') {
    return true;
}

return false;

// ❌ BURUK - Unnecessary else
if ($user->role === 'admin') {
    return true;
} else {
    return false;
}

// ✅ BAIK - Early return
public function processOrder($order)
{
    if (!$order->isValid()) {
        return false; // Exit early
    }

    if (!$order->hasStock()) {
        return false; // Exit early
    }

    // Main logic
    return $order->process();
}

// ❌ BURUK - Deep nesting
public function processOrder($order)
{
    if ($order->isValid()) {
        if ($order->hasStock()) {
            if ($order->canProcess()) {
                return $order->process();
            }
        }
    }
}

// ✅ BAIK - Use default/fallback
$state = $user->state ?? 'pending';
$permission = $this->getPermission() ?: 'user';

// ✅ BAIK - Use match untuk simple check
$status = match($code) {
    200 => 'OK',
    404 => 'Not Found',
    500 => 'Error',
    default => 'Unknown',
};
?>
```

### 2. Array Best Practices

```php
<?php

// ✅ BAIK - Use modern array syntax
$data = [
    'name' => 'value',
    'age' => 25,
];

// ❌ BURUK - Old syntax
$data = array(
    'name' => 'value',
    'age' => 25,
);

// ✅ BAIK - Type checking
if (isset($user['email']) && !empty($user['email'])) {
    echo $user['email'];
}

// ✅ BAIK - Use helper function
if (array_key_exists('email', $user)) {
    echo $user['email'];
}

// ✅ BAIK - Null coalesce untuk array
$email = $user['email'] ?? 'no-email@example.com';

// ✅ BAIK - Spread operator untuk merge
$merged = [...$array1, ...$array2];

// Lebih baik daripada
$merged = array_merge($array1, $array2);
?>
```

### 3. Loop Best Practices

```php
<?php

// ✅ BAIK - Use foreach untuk associative array
foreach ($products as $id => $product) {
    echo "$id: {$product->name}";
}

// ✅ BAIK - Never modify array dalam loop
foreach ($items as $item) {
    // Don't: unset($item), $item = null
    // Instead: create new array
}

// ✅ BAIK - Use collection methods di Laravel
$products = Product::active()
    ->where('stock', '>', 0)
    ->get();

foreach ($products as $product) {
    // ...
}

// ✅ BAIK - Use chunk untuk data besar
Order::chunk(100, function ($orders) {
    foreach ($orders as $order) {
        // Process
    }
});

// ✅ BAIK - Avoid deep nesting
foreach ($data as $item) {
    if (!$this->isValid($item)) {
        continue; // Skip
    }

    // Process valid item
}
?>
```

---

## 🏪 Real Case: POS System

### Order Status Checker

```php
<?php

class OrderController extends Controller
{
    public function show(Order $order)
    {
        // Determine status message
        $statusMessage = match($order->status) {
            'pending' => 'Pesanan Anda sedang diproses',
            'confirmed' => 'Pesanan telah dikonfirmasi',
            'paid' => 'Pembayaran diterima',
            'shipped' => 'Pesanan sedang dalam pengiriman',
            'delivered' => 'Pesanan telah sampai',
            'cancelled' => 'Pesanan telah dibatalkan',
            default => 'Status tidak diketahui',
        };

        // Check user permission
        if (auth()->user()->cannot('view', $order)) {
            abort(403, 'Unauthorized');
        }

        return view('orders.show', [
            'order' => $order,
            'statusMessage' => $statusMessage,
            'canCancel' => in_array($order->status, ['pending', 'confirmed']),
            'canPay' => $order->status === 'confirmed' && !$order->isPaid(),
        ]);
    }
}
```

### Product Filter & Search

```php
<?php

class ProductController extends Controller
{
    public function index(Request $request)
    {
        $query = Product::query();

        // Filter by category
        if ($request->filled('category_id')) {
            $query->where('category_id', $request->category_id);
        }

        // Filter by price range
        if ($request->filled('min_price') && $request->filled('max_price')) {
            $query->whereBetween('price', [
                $request->min_price,
                $request->max_price
            ]);
        }

        // Filter in stock only
        if ($request->boolean('in_stock')) {
            $query->where('stock', '>', 0);
        }

        // Search by name
        if ($request->filled('search')) {
            $search = $request->search;
            $query->where('name', 'like', "%$search%");
        }

        // Sort
        $sortBy = match($request->input('sort', 'newest')) {
            'price_low' => 'price',
            'price_high' => ['price', 'desc'],
            'popular' => 'rating',
            default => ['created_at', 'desc'],
        };

        if (is_array($sortBy)) {
            $query->orderBy($sortBy[0], $sortBy[1]);
        } else {
            $query->orderBy($sortBy);
        }

        $products = $query->paginate(12);

        return view('products.index', [
            'products' => $products,
            'categories' => Category::all(),
        ]);
    }
}
```

### Blade Template - Order List

```blade
<div class="orders-container">
    @forelse($orders as $order)
        <div class="order-card">
            <div class="order-header">
                <h3>Order #{{ $order->id }}</h3>

                {{-- Status Badge --}}
                @if($order->status === 'completed')
                    <span class="badge badge-success">Selesai</span>
                @elseif($order->status === 'pending')
                    <span class="badge badge-warning">Menunggu</span>
                @elseif($order->status === 'cancelled')
                    <span class="badge badge-danger">Dibatalkan</span>
                @else
                    <span class="badge badge-info">{{ ucfirst($order->status) }}</span>
                @endif
            </div>

            <div class="order-body">
                <p>Total: <strong>Rp{{ number_format($order->total_price) }}</strong></p>
                <p>Tanggal: {{ $order->created_at->format('d M Y H:i') }}</p>
            </div>

            <div class="order-items">
                <h4>Items:</h4>
                @foreach($order->items as $item)
                    <div class="item">
                        <span>{{ $item->product->name }}</span>
                        <span>x{{ $item->quantity }}</span>
                        <span>Rp{{ number_format($item->subtotal) }}</span>
                    </div>
                @endforeach
            </div>

            <div class="order-actions">
                @can('view', $order)
                    <a href="{{ route('orders.show', $order) }}" class="btn btn-sm">
                        Lihat Detail
                    </a>
                @endcan

                @unless($order->status === 'completed' || $order->status === 'cancelled')
                    @can('update', $order)
                        <a href="{{ route('orders.edit', $order) }}" class="btn btn-sm btn-warning">
                            Edit
                        </a>
                    @endcan
                @endunless
            </div>
        </div>
    @empty
        <div class="alert alert-info">
            Belum ada order.
            <a href="{{ route('products.index') }}">Mulai berbelanja</a>
        </div>
    @endforelse
</div>

{{-- Pagination --}}
{{ $orders->links() }}
```

---

## 📝 Summary Checklist

- [ ] Use **match** untuk simple conditional (PHP 8+)
- [ ] Use **early return** untuk menghindari nested if
- [ ] Use **null coalescing** (??) untuk default values
- [ ] Use **foreach** untuk array iteration
- [ ] Use **@forelse** di Blade untuk empty check
- [ ] Gunakan **break/continue** untuk loop control
- [ ] Use **array functions** untuk manipulasi array
- [ ] Avoid **deep nesting** di conditional
- [ ] Use **collection methods** di Laravel
- [ ] Implement **proper permissions** dengan @can/@cannot
