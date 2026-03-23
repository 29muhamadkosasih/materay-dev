# 📋 Middleware di Laravel - Panduan Lengkap & Detail

## 📌 Daftar Isi

1. [Pengertian Middleware](#pengertian)
2. [Alur Kerja Middleware](#alur-kerja)
3. [Struktur Folder & Pembuatan](#struktur)
4. [Perintah Membuat Middleware](#perintah)
5. [Struktur Dasar Middleware](#struktur-dasar)
6. [Tipe-Tipe Middleware](#tipe-middleware)
7. [Registrasi Middleware](#registrasi)
8. [Implementasi di Routes](#implementasi-routes)
9. [Implementasi di Controller](#implementasi-controller)
10. [Middleware dengan Parameter](#middleware-parameter)
11. [Before & After Processing](#before-after)
12. [Middleware Global](#middleware-global)
13. [Middleware Bawaan Laravel](#middleware-bawaan)
14. [Contoh Real Case - Sistem POS](#real-case)
15. [Best Practices](#best-practices)
16. [Debugging & Troubleshooting](#debugging)

---

## 🤔 Apa itu Middleware di Laravel?

Middleware adalah **lapisan filter/perantara** yang berjalan sebelum atau sesudah HTTP request diproses oleh controller. Middleware berfungsi sebagai security gate yang mengontrol akses dan memanipulasi request/response.

### Fungsi Utama Middleware:

| Fungsi                  | Penjelasan                               |
| ----------------------- | ---------------------------------------- |
| 🔐 **Authentication**   | Verifikasi user sudah login atau belum   |
| 👮 **Authorization**    | Cek role dan permission user             |
| 🌐 **IP Filtering**     | Batasi akses dari IP tertentu            |
| 🕒 **Maintenance Mode** | Graceful shutdown aplikasi               |
| 📜 **Logging**          | Catat setiap request yang masuk          |
| 🔑 **Token Validation** | Validasi API token                       |
| 🛡️ **CORS Handling**    | Cross-Origin Resource Sharing            |
| ⏱️ **Rate Limiting**    | Batasi jumlah request per waktu          |
| 🔒 **CSRF Protection**  | Proteksi dari Cross-Site Request Forgery |
| 📱 **Device Detection** | Deteksi jenis device user                |

---

## 🔄 Alur Kerja Middleware

### Visualisasi Alur Request-Response:

```
┌─────────────┐
│   CLIENT    │
│   REQUEST   │
└──────┬──────┘
       │
       ▼
┌─────────────────────────────────┐
│   MIDDLEWARE LAYER (Before)     │
│  ┌────────────────────────────┐ │
│  │ 1. CheckAuth Middleware    │ │
│  │ 2. CheckRole Middleware    │ │
│  │ 3. LogRequest Middleware   │ │
│  └────────────────────────────┘ │
└──────┬──────────────────────────┘
       │
       ▼
┌─────────────────────────────────┐
│      ROUTE DISPATCHER           │
│  ┌────────────────────────────┐ │
│  │ CONTROLLER METHOD          │ │
│  │ Business Logic             │ │
│  └────────────────────────────┘ │
└──────┬──────────────────────────┘
       │
       ▼
┌─────────────────────────────────┐
│   MIDDLEWARE LAYER (After)      │
│  ┌────────────────────────────┐ │
│  │ Add Custom Headers         │ │
│  │ Modify Response            │ │
│  │ Logging Response           │ │
│  └────────────────────────────┘ │
└──────┬──────────────────────────┘
       │
       ▼
┌─────────────┐
│   RESPONSE  │
│   TO CLIENT │
└─────────────┘
```

---

## 📁 Struktur Folder & Lokasi Middleware

Semua middleware Laravel berada di lokasi standar:

```
app/
├── Http/
│   ├── Middleware/          ← Folder middleware
│   │   ├── CheckRole.php
│   │   ├── VerifyToken.php
│   │   ├── LogActivity.php
│   │   └── ...
│   ├── Controllers/
│   ├── Requests/
│   └── Kernel.php           ← Registrasi middleware
```

---

## ⚙️ Cara Membuat Middleware

### Perintah Artisan:

```bash
php artisan make:middleware NamaMiddleware
```

### Contoh Membuat Beberapa Middleware:

```bash
# Middleware untuk cek role
php artisan make:middleware CheckRole

# Middleware untuk validasi token
php artisan make:middleware VerifyToken

# Middleware untuk logging
php artisan make:middleware LogActivity

# Middleware untuk cek maintenance
php artisan make:middleware CheckMaintenance
```

### Output File:

Setiap perintah akan membuat file di:

```
app/Http/Middleware/CheckRole.php
app/Http/Middleware/VerifyToken.php
app/Http/Middleware/LogActivity.php
app/Http/Middleware/CheckMaintenance.php
```

---

## 🏗️ Struktur Dasar Middleware

### Template Default:

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class CheckRole
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure(\Illuminate\Http\Request): (\Illuminate\Http\Response|\Illuminate\Http\RedirectResponse)  $next
     * @return \Illuminate\Http\Response|\Illuminate\Http\RedirectResponse
     */
    public function handle(Request $request, Closure $next)
    {
        // Logic sebelum request diproses (BEFORE)
        // ...

        $response = $next($request);

        // Logic setelah request diproses (AFTER)
        // ...

        return $response;
    }
}
```

### Penjelasan Parameter:

| Parameter  | Tipe      | Penjelasan                                                                      |
| ---------- | --------- | ------------------------------------------------------------------------------- |
| `$request` | `Request` | Object request yang berisi data HTTP request (GET, POST, headers, dll)          |
| `$next`    | `Closure` | Callback function untuk melanjutkan request ke middleware/controller berikutnya |
| `return`   | Response  | Wajib mengembalikan response, bisa dari `$next($request)` atau redirect         |

---

## 🎯 Tipe-Tipe Middleware

### 1. **Global Middleware** (Semua Request)

Berjalan di SETIAP request yang masuk ke aplikasi.

```php
// app/Http/Kernel.php
protected $middleware = [
    \App\Http\Middleware\TrustProxies::class,
    \App\Http\Middleware\CheckForMaintenanceMode::class,
    \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
];
```

### 2. **Route Middleware** (Per Route/Group)

Hanya berjalan untuk route tertentu yang didaftarkan.

```php
// app/Http/Kernel.php
protected $routeMiddleware = [
    'auth' => \App\Http\Middleware\Authenticate::class,
    'check.role' => \App\Http\Middleware\CheckRole::class,
    'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
];
```

### 3. **Middleware Groups** (Grup Route)

Kumpulan middleware yang berjalan bersamaan.

```php
// app/Http/Kernel.php
protected $middlewareGroups = [
    'web' => [
        \App\Http\Middleware\EncryptCookies::class,
        \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
        \Illuminate\Session\Middleware\StartSession::class,
        \Illuminate\View\Middleware\ShareErrorsFromSession::class,
        \App\Http\Middleware\VerifyCSRFToken::class,
    ],
    'api' => [
        \Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,
        'throttle:api',
        \Illuminate\Routing\Middleware\SubstituteBindings::class,
    ],
];
```

---

## 📋 Registrasi Middleware

File registrasi middleware ada di: **app/Http/Kernel.php**

### Step 1: Buka app/Http/Kernel.php

```php
<?php

namespace App\Http;

use Illuminate\Foundation\Http\Kernel as HttpKernel;

class Kernel extends HttpKernel
{
    // Global middleware untuk semua request
    protected $middleware = [
        // ...
    ];

    // Middleware groups (web & api)
    protected $middlewareGroups = [
        'web' => [
            // ...
        ],
        'api' => [
            // ...
        ],
    ];

    // Route middleware untuk route spesifik
    protected $routeMiddleware = [
        'auth' => \App\Http\Middleware\Authenticate::class,
        // TAMBAHKAN MIDDLEWARE BARU DI SINI
    ];
}
```

### Step 2: Tambahkan Middleware di $routeMiddleware

```php
protected $routeMiddleware = [
    'auth' => \App\Http\Middleware\Authenticate::class,
    'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
    'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,

    // MIDDLEWARE BUATAN SENDIRI
    'check.role' => \App\Http\Middleware\CheckRole::class,
    'check.admin' => \App\Http\Middleware\CheckAdmin::class,
    'verify.token' => \App\Http\Middleware\VerifyToken::class,
    'log.activity' => \App\Http\Middleware\LogActivity::class,
    'check.ip' => \App\Http\Middleware\CheckIP::class,
];
```

### Step 3: Middleware Siap Digunakan!

Setelah didaftarkan, middleware dapat digunakan dengan nama key-nya:

- `check.role`
- `check.admin`
- `verify.token`
- dll

---

## 🛣️ Implementasi Middleware di Routes

### 1. Per Route (Single Route)

```php
<?php

use App\Http\Controllers\AdminController;
use Illuminate\Support\Facades\Route;

// Middleware untuk satu route
Route::get('/admin/dashboard', [AdminController::class, 'dashboard'])
    ->middleware('check.role');

// Middleware multiple
Route::get('/admin/users', [AdminController::class, 'users'])
    ->middleware(['auth', 'check.role']);

// Dengan nama route
Route::get('/admin/reports', [AdminController::class, 'reports'])
    ->name('admin.reports')
    ->middleware(['auth', 'check.role', 'verify.token']);
```

### 2. Per Group Route (Multiple Routes)

```php
<?php

use App\Http\Controllers\AdminController;
use Illuminate\Support\Facades\Route;

// Middleware untuk group route
Route::middleware(['auth', 'check.role'])->group(function () {

    Route::get('/admin/dashboard', [AdminController::class, 'dashboard']);

    Route::get('/admin/users', [AdminController::class, 'users']);

    Route::post('/admin/users/create', [AdminController::class, 'store']);

    Route::get('/admin/reports', [AdminController::class, 'reports']);

    Route::delete('/admin/users/{id}', [AdminController::class, 'destroy']);

});
```

### 3. Nested Middleware Groups

```php
<?php

use App\Http\Controllers\AdminController;
use App\Http\Controllers\KasirController;
use Illuminate\Support\Facades\Route;

// GROUP ADMIN - Middleware auth + check.role
Route::middleware(['auth', 'check.role:admin'])->prefix('admin')->group(function () {

    Route::get('/dashboard', [AdminController::class, 'dashboard'])->name('admin.dashboard');
    Route::resource('users', AdminController::class);

    // SUB-GROUP SETTINGS - Tambah middleware log.activity
    Route::middleware('log.activity')->group(function () {
        Route::get('/settings', [AdminController::class, 'settings']);
        Route::post('/settings/update', [AdminController::class, 'updateSettings']);
    });
});

// GROUP KASIR - Middleware auth + check.role
Route::middleware(['auth', 'check.role:kasir'])->prefix('kasir')->group(function () {

    Route::get('/dashboard', [KasirController::class, 'dashboard'])->name('kasir.dashboard');
    Route::get('/transactions', [KasirController::class, 'transactions']);
    Route::post('/transactions/create', [KasirController::class, 'store']);

});
```

### 4. Exclude Route dari Middleware (Only & Except)

```php
<?php

use App\Http\Controllers\ProductController;
use Illuminate\Support\Facades\Route;

Route::middleware('check.role')->group(function () {

    Route::resource('products', ProductController::class)
        // Hanya method show dan index
        ->only(['show', 'index']);

    // Atau exclude method tertentu
    // ->except(['show']);

});
```

---

## 🎮 Implementasi Middleware di Controller

### 1. Global Middleware di Constructor

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Routing\Controller as BaseController;

class AdminController extends BaseController
{
    // Cara 1: Middleware di constructor
    public function __construct()
    {
        // Berlaku untuk SEMUA method di controller ini
        $this->middleware('auth');
        $this->middleware('check.role');
    }

    public function dashboard()
    {
        return view('admin.dashboard');
    }

    public function users()
    {
        return view('admin.users');
    }

    public function settings()
    {
        return view('admin.settings');
    }
}
```

### 2. Middleware Hanya untuk Method Tertentu

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Routing\Controller as BaseController;

class ProductController extends BaseController
{
    public function __construct()
    {
        // Middleware hanya untuk method: create, store, edit, update, destroy
        $this->middleware('auth')->only([
            'create', 'store', 'edit', 'update', 'destroy'
        ]);

        // Method show dan index tidak perlu auth
    }

    public function index()
    {
        // Tidak perlu auth - PUBLIK
        return view('products.index');
    }

    public function show($id)
    {
        // Tidak perlu auth - PUBLIK
        return view('products.show');
    }

    public function create()
    {
        // Perlu auth
        return view('products.create');
    }

    public function store()
    {
        // Perlu auth
        // Save logic...
    }

    public function edit($id)
    {
        // Perlu auth
        return view('products.edit');
    }

    public function update($id)
    {
        // Perlu auth
        // Update logic...
    }

    public function destroy($id)
    {
        // Perlu auth
        // Delete logic...
    }
}
```

### 3. Multiple Middleware dengan Only/Except

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Routing\Controller as BaseController;

class OrderController extends BaseController
{
    public function __construct()
    {
        // Middleware auth untuk semua
        $this->middleware('auth');

        // Middleware check.role hanya untuk create, store, update, destroy
        $this->middleware('check.role:admin')->only([
            'create', 'store', 'edit', 'update', 'destroy'
        ]);

        // Middleware log.activity untuk semua kecuali index
        $this->middleware('log.activity')->except(['index']);
    }

    public function index()
    {
        // Auth required, tidak perlu role admin, tidak di-log
    }

    public function show($id)
    {
        // Auth required, tidak perlu role admin, di-log
    }

    public function create()
    {
        // Auth required, perlu role admin, di-log
    }

    public function store()
    {
        // Auth required, perlu role admin, di-log
    }
}
```

---

## ⚙️ Middleware dengan Parameter

Middleware dapat menerima parameter dinamis untuk flexibility lebih.

### 1. Middleware dengan Parameter Single

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class CheckRole
{
    public function handle(Request $request, Closure $next, $role)
    {
        // $role adalah parameter yang diterima

        if (!auth()->check()) {
            return redirect('/login');
        }

        if (auth()->user()->role != $role) {
            abort(403, "Anda tidak memiliki akses ke role: {$role}");
        }

        return $next($request);
    }
}
```

### 2. Menggunakan Middleware dengan Parameter

```php
<?php

use App\Http\Controllers\AdminController;
use Illuminate\Support\Facades\Route;

// Middleware dengan parameter
Route::get('/admin', [AdminController::class, 'dashboard'])
    ->middleware('check.role:admin');

Route::get('/kasir', [AdminController::class, 'kasirDashboard'])
    ->middleware('check.role:kasir');

Route::get('/manager', [AdminController::class, 'managerDashboard'])
    ->middleware('check.role:manager');

// Multiple parameter
Route::get('/reports', [AdminController::class, 'reports'])
    ->middleware('check.role:admin,manager');
```

### 3. Middleware dengan Multiple Parameter

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class CheckRoleAndPermission
{
    public function handle(Request $request, Closure $next, $role, $permission)
    {
        // Cek role
        if (auth()->user()->role != $role) {
            abort(403, "Role tidak sesuai");
        }

        // Cek permission
        if (!auth()->user()->hasPermission($permission)) {
            abort(403, "Tidak memiliki permission: {$permission}");
        }

        return $next($request);
    }
}
```

### 4. Penggunaan Multiple Parameter

```php
<?php

use App\Http\Controllers\UserController;
use Illuminate\Support\Facades\Route;

// Multiple parameter: separated by comma
Route::post('/users', [UserController::class, 'store'])
    ->middleware('check.role.permission:admin,create_user');

Route::put('/users/{id}', [UserController::class, 'update'])
    ->middleware('check.role.permission:admin,edit_user');

Route::delete('/users/{id}', [UserController::class, 'destroy'])
    ->middleware('check.role.permission:admin,delete_user');
```

---

## ⏱️ Middleware Before & After Processing

Middleware dapat melakukan logic SEBELUM dan SESUDAH request diproses oleh controller.

### 1. Contoh Before & After Basic

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Log;

class LogActivity
{
    public function handle(Request $request, Closure $next)
    {
        // ========== BEFORE REQUEST ==========
        $startTime = microtime(true);

        Log::info('Request Masuk', [
            'method' => $request->getMethod(),
            'path' => $request->getPathInfo(),
            'ip' => $request->ip(),
            'user_id' => auth()->id() ?? 'guest',
            'timestamp' => now(),
        ]);

        // Lanjut ke controller
        $response = $next($request);

        // ========== AFTER RESPONSE =========
        $executionTime = microtime(true) - $startTime;

        Log::info('Response Keluar', [
            'status' => $response->getStatusCode(),
            'execution_time' => round($executionTime * 1000, 2) . 'ms',
            'timestamp' => now(),
        ]);

        // Add custom header ke response
        $response->header('X-Response-Time', round($executionTime * 1000, 2) . 'ms');

        return $response;
    }
}
```

### 2. Contoh: Cek Maintenance Mode

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class CheckMaintenance
{
    public function handle(Request $request, Closure $next)
    {
        // BEFORE: Cek apakah dalam maintenance mode
        if (env('APP_MAINTENANCE') === true) {
            // Allow access untuk admin only
            if (!auth()->check() || !auth()->user()->isAdmin()) {
                return response()->view('maintenance', [
                    'message' => 'Aplikasi sedang dalam maintenance'
                ], 503);
            }
        }

        // Lanjut ke controller
        $response = $next($request);

        // AFTER: Modify response jika perlu
        if (env('APP_MAINTENANCE') === true) {
            $response->header('X-Maintenance-Mode', 'true');
        }

        return $response;
    }
}
```

### 3. Contoh: Add Security Headers

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class AddSecurityHeaders
{
    public function handle(Request $request, Closure $next)
    {
        // BEFORE: Validasi request headers
        if (!$request->path() === '/api' && !$request->hasHeader('X-API-Key')) {
            // Optional: bisa throw error
            // return response()->json(['error' => 'Missing API Key'], 403);
        }

        // Lanjut ke controller
        $response = $next($request);

        // AFTER: Add security headers ke response
        $response->headers->set('X-Content-Type-Options', 'nosniff');
        $response->headers->set('X-Frame-Options', 'DENY');
        $response->headers->set('X-XSS-Protection', '1; mode=block');
        $response->headers->set('Strict-Transport-Security', 'max-age=31536000; includeSubDomains');
        $response->headers->set('Content-Security-Policy', "default-src 'self'");

        return $response;
    }
}
```

---

## 🌍 Middleware Global

Middleware global berjalan di SETIAP request yang masuk ke aplikasi, tanpa perlu didaftarkan di route.

### Cara Membuat Middleware Global:

#### Step 1: Buat Middleware

```bash
php artisan make:middleware CheckIP
```

#### Step 2: Tulis Logic Middleware

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class CheckIP
{
    public function handle(Request $request, Closure $next)
    {
        $allowedIPs = [
            '192.168.1.1',
            '192.168.1.2',
            '127.0.0.1', // Localhost
        ];

        if (!in_array($request->ip(), $allowedIPs)) {
            abort(403, 'IP anda tidak diizinkan');
        }

        return $next($request);
    }
}
```

#### Step 3: Daftarkan di app/Http/Kernel.php

```php
<?php

namespace App\Http;

use Illuminate\Foundation\Http\Kernel as HttpKernel;

class Kernel extends HttpKernel
{
    // Global middleware - BERJALAN DI SEMUA REQUEST
    protected $middleware = [
        \App\Http\Middleware\TrustProxies::class,
        \Illuminate\Foundation\Http\Middleware\CheckForMaintenanceMode::class,
        \App\Http\Middleware\CheckIP::class,  // ← MIDDLEWARE GLOBAL KAMI
    ];

    // ... rest of code
}
```

### ⚠️ Penting: Kapan Menggunakan Middleware Global?

```
✅ GUNAKAN GLOBAL MIDDLEWARE UNTUK:
   • Cek IP (security)
   • Logging semua request
   • Add global headers
   • Cek maintenance mode
   • Error handling global

❌ JANGAN GUNAKAN GLOBAL MIDDLEWARE UNTUK:
   • Cek authentication (terlalu berat)
   • Cek role/permission
   • Business logic spesifik
   • Parse request body khusus
```

---

## 📚 Middleware Bawaan Laravel

Laravel menyediakan beberapa middleware default yang siap pakai.

### 1. **auth** - Cek User Login

```php
<?php

use Illuminate\Support\Facades\Route;

Route::middleware('auth')->group(function () {
    Route::get('/dashboard', 'DashboardController@index');
    Route::get('/profile', 'ProfileController@show');
});

// Hanya untuk yang sudah login
Route::get('/protected', function () {
    return 'Hanya bisa diakses jika login';
})->middleware('auth');
```

### 2. **guest** - Hanya untuk User Belum Login

```php
<?php

use Illuminate\Support\Facades\Route;

// Hanya bisa diakses jika BELUM login
Route::middleware('guest')->group(function () {
    Route::get('/login', 'AuthController@showLogin');
    Route::post('/login', 'AuthController@login');
    Route::get('/register', 'AuthController@showRegister');
});
```

### 3. **verified** - Email Sudah Diverifikasi

```php
<?php

use Illuminate\Support\Facades\Route;
use App\Models\User;

// User sudah login AND email sudah diverifikasi
Route::middleware(['auth', 'verified'])->group(function () {
    Route::get('/premium-content', 'ContentController@premium');
});
```

### 4. **throttle** - Rate Limiting

```php
<?php

use Illuminate\Support\Facades\Route;

// Batasi 60 request per menit
Route::middleware('throttle:60,1')->group(function () {
    Route::post('/api/users', 'UserController@store');
});

// Rate limiting khusus API: 1000 request per menit
Route::middleware('throttle:1000,1')->prefix('/api')->group(function () {
    Route::get('/users', 'UserController@index');
    Route::post('/users', 'UserController@store');
});

// Different rates untuk authenticated vs guest
Route::middleware('throttle:60,1')->middleware('guest')->group(function () {
    Route::post('/login', 'AuthController@login'); // 60 req/menit untuk guest
});

Route::middleware('throttle:100,1')->middleware('auth')->group(function () {
    Route::post('/api/data', 'DataController@store'); // 100 req/menit untuk auth
});
```

### 5. **cors** - Cross-Origin Resource Sharing

```php
<?php

use Illuminate\Support\Facades\Route;

// Allow CORS untuk request dari domain lain
Route::middleware('cors')->group(function () {
    Route::get('/api/public-data', 'DataController@publicData');
});
```

---

## 💼 Contoh Real Case - Sistem POS

Jika kamu sedang buat sistem POS Laravel, berikut implementasi middleware yang realistis.

### 1. Buat Middleware POS

```bash
php artisan make:middleware CheckPOSRole
php artisan make:middleware LogPOSActivity
php artisan make:middleware VerifyAPIToken
```

### 2. Implementasi CheckPOSRole.php

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class CheckPOSRole
{
    public function handle(Request $request, Closure $next, $role = null)
    {
        // Cek user login
        if (!auth()->check()) {
            return redirect('/login');
        }

        $userRole = auth()->user()->role; // admin, kasir, manager

        // Jika parameter role diberikan
        if ($role && $userRole !== $role) {
            abort(403, "Anda tidak memiliki akses untuk role: {$role}");
        }

        // Set user info ke request (untuk digunakan di controller)
        $request->attributes->add(['user_role' => $userRole]);

        return $next($request);
    }
}
```

### 3. Implementasi LogPOSActivity.php

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;
use App\Models\ActivityLog;

class LogPOSActivity
{
    public function handle(Request $request, Closure $next)
    {
        $response = $next($request);

        // Log setiap activity
        ActivityLog::create([
            'user_id' => auth()->id(),
            'method' => $request->getMethod(),
            'path' => $request->path(),
            'status_code' => $response->getStatusCode(),
            'ip_address' => $request->ip(),
            'user_agent' => $request->header('User-Agent'),
            'timestamp' => now(),
        ]);

        return $response;
    }
}
```

### 4. Registrasi di app/Http/Kernel.php

```php
<?php

namespace App\Http;

use Illuminate\Foundation\Http\Kernel as HttpKernel;

class Kernel extends HttpKernel
{
    protected $routeMiddleware = [
        'auth' => \App\Http\Middleware\Authenticate::class,
        'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,

        // Middleware POS
        'check.pos.role' => \App\Http\Middleware\CheckPOSRole::class,
        'log.pos.activity' => \App\Http\Middleware\LogPOSActivity::class,
        'verify.api.token' => \App\Http\Middleware\VerifyAPIToken::class,
    ];
}
```

### 5. Implementasi di Routes

```php
<?php

use App\Http\Controllers\DashboardController;
use App\Http\Controllers\ProductController;
use App\Http\Controllers\KasirController;
use App\Http\Controllers\AdminController;
use Illuminate\Support\Facades\Route;

// ============ PUBLIC ROUTES ============
Route::get('/', function () {
    return view('welcome');
});

// ============ AUTH ROUTES ============
Route::middleware('guest')->group(function () {
    Route::get('/login', 'AuthController@showLogin')->name('login');
    Route::post('/login', 'AuthController@login');
});

Route::post('/logout', 'AuthController@logout')->name('logout')->middleware('auth');

// ============ AUTHENTICATED ROUTES ============
Route::middleware('auth')->group(function () {

    // Dashboard - semua role bisa akses
    Route::get('/dashboard', [DashboardController::class, 'index'])
        ->name('dashboard')
        ->middleware('log.pos.activity');

    // ===== GROUP ADMIN =====
    Route::middleware('check.pos.role:admin')->prefix('admin')->group(function () {

        Route::get('/dashboard', [AdminController::class, 'dashboard'])
            ->name('admin.dashboard');

        Route::resource('products', ProductController::class);

        Route::get('/reports', [AdminController::class, 'reports'])
            ->name('admin.reports')
            ->middleware('log.pos.activity');

        Route::get('/users', [AdminController::class, 'users'])
            ->name('admin.users')
            ->middleware('log.pos.activity');
    });

    // ===== GROUP KASIR =====
    Route::middleware('check.pos.role:kasir')->prefix('kasir')->group(function () {

        Route::get('/dashboard', [KasirController::class, 'dashboard'])
            ->name('kasir.dashboard');

        Route::get('/transactions', [KasirController::class, 'transactions'])
            ->name('kasir.transactions');

        Route::post('/transactions/create', [KasirController::class, 'store'])
            ->name('kasir.transaction.store')
            ->middleware('log.pos.activity');

        Route::get('/cash-drawer', [KasirController::class, 'cashDrawer'])
            ->name('kasir.cash');
    });

});

// ============ API ROUTES ============
Route::middleware(['api', 'throttle:60,1', 'verify.api.token'])->prefix('/api')->group(function () {

    Route::get('/products', 'API\ProductController@index');
    Route::get('/products/{id}', 'API\ProductController@show');

    Route::post('/transactions', 'API\TransactionController@store')->middleware('log.pos.activity');
    Route::get('/transactions', 'API\TransactionController@index');

});
```

### 6. Implementasi di Controller

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Routing\Controller as BaseController;

class KasirController extends BaseController
{
    public function __construct()
    {
        // Middleware untuk controller ini
        $this->middleware('auth');
        $this->middleware('check.pos.role:kasir');
    }

    public function dashboard()
    {
        return view('kasir.dashboard', [
            'user' => auth()->user(),
            'transactions_today' => 0,
        ]);
    }

    public function transactions()
    {
        return view('kasir.transactions');
    }

    public function store()
    {
        // Transaksi kasir disimpan dengan log activity middleware
        // Activity otomatis tercatat
        return response()->json(['success' => true]);
    }
}
```

---

## ✨ Best Practices Middleware

### 1. **Naming Convention** - Gunakan nama yang deskriptif dan jelas

```php
// ✅ BAIK
'verify.email'
'check.role:admin'
'rate.limit'
'log.activity'

// ❌ BURUK
'check1'
'middleware'
'test'
```

### 2. **Single Responsibility** - Satu middleware, satu tujuan

```php
// ❌ BURUK - Terlalu banyak logic
// ✅ BAIK - Pisah middleware
// CheckActive.php
// CheckRole.php
// LogActivity.php
```

### 3. **Error Handling** - Handle exception dengan baik

```php
try {
    // Logic
} catch (\Exception $e) {
    \Log::error('Middleware Error', [
        'error' => $e->getMessage(),
        'file' => $e->getFile(),
        'line' => $e->getLine(),
    ]);
    return response()->json(['error' => 'Server error'], 500);
}
```

### 4. **Performance Optimization** - Hindari query di middleware

```php
// ❌ JANGAN - Database query
// $users = User::all(); // LAMBAT!

// ✅ BAIK - Cache data
$users = cache()->remember('all_users', 3600, function () {
    return User::all();
});
```

### 5. **Documentation** - Dokumentasikan middleware dengan jelas

```php
/**
 * Middleware untuk verifikasi role user
 *
 * Contoh: Route::get('/admin', 'AdminController@index')->middleware('check.role:admin');
 * Parameter: $role - Role yang diizinkan
 * Return: 403 Forbidden jika tidak sesuai
 */
```

### 6. **Testing** - Test middleware dengan unit test

```php
public function test_middleware_allows_admin_access()
{
    $admin = User::factory()->create(['role' => 'admin']);
    $response = $this->actingAs($admin)->get('/admin/dashboard');
    $response->assertStatus(200);
}
```

---

## 🐛 Debugging & Troubleshooting

### 1. **Middleware Tidak Berjalan**

```bash
# Debug: Lihat semua routes dan middleware
php artisan route:list

# Cek apakah middleware sudah didaftarkan di Kernel.php
php artisan route:list | grep check.role
```

### 2. **Middleware Order Salah**

```bash
# Cek urutan middleware
php artisan route:list -v

# Solution: Atur order yang benar
# Authentication → Authorization → Business Logic
```

### 3. **Debug Mode - Log Middleware**

```php
\Log::debug('Middleware Start', [
    'middleware' => static::class,
    'path' => $request->path(),
    'user_id' => auth()->id() ?? 'guest',
]);

// Check log: tail -f storage/logs/laravel.log
```

---

## 📝 Summary

### Middleware Lifecycle:

```
1. Request masuk
   ↓
2. Global Middleware berjalan
   ↓
3. Route Middleware berjalan (BEFORE)
   ↓
4. Controller dijalankan
   ↓
5. Route Middleware berjalan (AFTER)
   ↓
6. Response di-send ke client
```

### Checklist Implementasi:

- [ ] Buat middleware dengan `php artisan make:middleware`
- [ ] Tulis logic di method `handle()`
- [ ] Daftarkan di `app/Http/Kernel.php`
- [ ] Gunakan di route, group route, atau controller
- [ ] Test middleware dengan benar
- [ ] Dokumentasikan middleware
- [ ] Monitor performance
- [ ] Setup logging/debugging
