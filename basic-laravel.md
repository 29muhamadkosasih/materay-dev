# Materi Dasar-Dasar Laravel

Dokumen ini berisi pengenalan Laravel dari dasar, mulai dari konsep kerja, struktur folder, routing, controller, view, database, sampai praktik CRUD sederhana. Materi disusun agar lebih rapi, mudah dipahami, dan cocok untuk pemula yang ingin membangun fondasi Laravel dengan benar.

---

## 1. Apa Itu Laravel?

Laravel adalah framework PHP modern yang digunakan untuk membangun aplikasi web secara cepat, aman, dan terstruktur. Laravel menerapkan pola arsitektur **MVC (Model View Controller)** sehingga kode lebih mudah dipisahkan berdasarkan tugasnya masing-masing.

### Pengertian sederhananya

- **Laravel** membantu developer agar tidak menulis semuanya dari nol.
- Banyak kebutuhan umum aplikasi web sudah disediakan, seperti routing, autentikasi, validasi, ORM, middleware, dan sistem template.
- Sangat cocok untuk membuat:
  - website company profile,
  - sistem admin,
  - aplikasi sekolah,
  - inventory,
  - blog,
  - REST API,
  - hingga aplikasi skala besar.

### Kelebihan Laravel

- Sintaks mudah dibaca dan nyaman ditulis.
- Struktur project rapi dan konsisten.
- Memiliki banyak fitur bawaan.
- Keamanan dasar sudah sangat diperhatikan.
- Mendukung pembuatan REST API.
- Ekosistem besar dan dokumentasi lengkap.
- Cocok untuk pengembangan cepat maupun project jangka panjang.

---

## 2. Konsep Dasar Laravel

Laravel sangat identik dengan pola **MVC**.

### MVC (Model View Controller)

Alur sederhananya seperti ini:

```text
User → Route → Controller → Model → Database
                         ↓
                       View
```

### Penjelasan tiap bagian

#### 1. Model

Model bertugas mengelola data dan berinteraksi dengan database.

Contohnya:

- mengambil data user,
- menyimpan data post,
- menghapus data produk.

Biasanya satu model mewakili satu tabel database.

#### 2. View

View adalah bagian tampilan yang dilihat user di browser.

Contohnya:

- halaman dashboard,
- daftar user,
- form tambah data.

Di Laravel, view biasanya dibuat dengan **Blade Template Engine**.

#### 3. Controller

Controller bertugas mengatur logika aplikasi.

Controller menerima request dari route, memproses data, memanggil model bila perlu, lalu mengembalikan response berupa view atau data JSON.

---

## 3. Contoh Alur Sederhana MVC

### Route

```php
Route::get('/users', [UserController::class, 'index']);
```

### Controller

```php
class UserController extends Controller
{
    public function index()
    {
        $users = User::all();
        return view('users.index', compact('users'));
    }
}
```

### Model

```php
class User extends Model
{
    protected $fillable = ['name', 'email'];
}
```

### View

```blade
@foreach($users as $user)
    <p>{{ $user->name }}</p>
@endforeach
```

### Penjelasan alurnya

1. User membuka URL `/users`.
2. Route mengarahkan request ke `UserController@index`.
3. Controller mengambil semua data dari model `User`.
4. Data dikirim ke view `users.index`.
5. View menampilkan daftar user ke browser.

---

## 4. Instalasi Laravel

### Kebutuhan minimum

Sebelum menginstal Laravel, siapkan beberapa kebutuhan berikut:

- PHP versi **8.2** atau lebih baru
- Composer
- Database MySQL / PostgreSQL
- Node.js dan npm (opsional, tetapi umum dipakai untuk aset frontend)

### Cara instal Laravel

```bash
composer create-project laravel/laravel belajar-laravel
```

Masuk ke folder project:

```bash
cd belajar-laravel
```

Jalankan server development:

```bash
php artisan serve
```

Buka di browser:

```text
http://127.0.0.1:8000
```

### Pengertian tambahan

- `composer create-project` digunakan untuk membuat project Laravel baru.
- `php artisan serve` menjalankan server lokal bawaan Laravel.
- Setelah project berhasil dibuat, Laravel sudah memiliki struktur dasar yang siap dipakai.

---

## 5. Struktur Folder Laravel

Berikut folder penting yang sering dipakai:

```text
app/
bootstrap/
config/
database/
public/
resources/
routes/
storage/
```

### Penjelasan folder penting

#### `app/`

Berisi logika utama aplikasi.

Umumnya terdapat:

```text
app/
├── Models/
└── Http/
    ├── Controllers/
    ├── Middleware/
    └── Requests/
```

#### `routes/`

Tempat mendefinisikan semua route aplikasi.

File yang umum:

- `web.php` → route untuk halaman web
- `api.php` → route untuk API

#### `resources/`

Berisi file frontend seperti:

- `views/` untuk Blade
- `css/` untuk stylesheet
- `js/` untuk JavaScript

#### `database/`

Berisi hal-hal yang terkait database:

- `migrations/`
- `seeders/`
- `factories/`

#### `public/`

Folder yang diakses langsung oleh browser. Biasanya berisi file hasil build, gambar publik, dan `index.php`.

#### `storage/`

Berisi file log, cache, dan file yang diunggah aplikasi.

---

## 6. Routing Laravel

Routing adalah proses menghubungkan URL dengan logika tertentu.

Dengan route, Laravel tahu apa yang harus dilakukan ketika user mengakses suatu alamat.

### Basic Route

```php
Route::get('/', function () {
    return 'Hello Laravel';
});
```

### Route Method

```php
Route::get('/user', fn () => 'GET');
Route::post('/user', fn () => 'POST');
Route::put('/user', fn () => 'PUT');
Route::delete('/user', fn () => 'DELETE');
```

### Penjelasan method HTTP

- `GET` → mengambil atau menampilkan data
- `POST` → menyimpan data baru
- `PUT` / `PATCH` → mengubah data
- `DELETE` → menghapus data

### Route Parameter

```php
Route::get('/user/{id}', function ($id) {
    return $id;
});
```

Jika membuka URL berikut:

```text
/user/5
```

Maka output-nya:

```text
5
```

### Pengertian tambahan

`{id}` disebut parameter route. Nilainya diambil dari URL dan bisa dipakai langsung di function atau controller.

---

## 7. Controller

Controller digunakan agar logika aplikasi tidak ditulis langsung di file route.

Dengan controller, kode menjadi lebih rapi, mudah dirawat, dan lebih profesional.

### Membuat Controller

```bash
php artisan make:controller UserController
```

### Contoh Controller

```php
class UserController extends Controller
{
    public function index()
    {
        return 'Data User';
    }
}
```

### Route ke Controller

```php
Route::get('/users', [UserController::class, 'index']);
```

### Pengertian tambahan

- Route menerima request.
- Controller memproses request.
- Controller dapat mengembalikan string, view, redirect, atau JSON.

---

## 8. View dan Blade Template

Laravel menggunakan **Blade Template Engine** untuk membuat tampilan.

Blade memudahkan penulisan tampilan karena mendukung syntax sederhana untuk menampilkan data, kondisi, pengulangan, layout, dan komponen.

### Membuat View

File:

```text
resources/views/home.blade.php
```

Isi view:

```blade
<h1>Hello {{ $nama }}</h1>
```

Controller:

```php
return view('home', [
    'nama' => 'Kosasih'
]);
```

### Blade Syntax Dasar

#### Menampilkan variabel

```blade
{{ $data }}
```

#### Kondisi `if`

```blade
@if($user)
    Login
@endif
```

#### Perulangan `foreach`

```blade
@foreach($users as $user)
    {{ $user->name }}
@endforeach
```

### Pengertian tambahan

- `{{ }}` digunakan untuk menampilkan data secara aman.
- Blade otomatis membantu mengurangi risiko XSS pada output biasa.
- File Blade selalu berakhiran `.blade.php`.

---

## 9. Database dan Migration

Migration adalah cara Laravel mengelola struktur database melalui kode.

### Kenapa migration penting?

Karena struktur tabel tidak dibuat manual satu per satu di phpMyAdmin. Semua perubahan database bisa dicatat dalam file migration sehingga:

- mudah dilacak,
- mudah dibagikan ke tim,
- mudah dijalankan ulang,
- lebih aman dan konsisten.

### Konfigurasi database di `.env`

```env
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

### Membuat migration

```bash
php artisan make:migration create_posts_table
```

### Contoh migration

```php
Schema::create('posts', function (Blueprint $table) {
    $table->id();
    $table->string('title');
    $table->text('content');
    $table->timestamps();
});
```

### Menjalankan migration

```bash
php artisan migrate
```

### Pengertian tambahan

- `$table->id()` membuat primary key otomatis.
- `$table->timestamps()` membuat kolom `created_at` dan `updated_at`.
- `php artisan migrate` akan menjalankan semua migration yang belum dijalankan.

---

## 10. Model dan Eloquent ORM

Laravel memiliki ORM bernama **Eloquent**.

ORM adalah teknik untuk berinteraksi dengan database menggunakan object PHP, bukan query SQL mentah secara langsung.

### Membuat model

```bash
php artisan make:model Post
```

### Menambah data

```php
Post::create([
    'title' => 'Belajar Laravel',
    'content' => 'Laravel itu mudah'
]);
```

### Mengambil data

```php
Post::all();
Post::find(1);
```

### Mengubah data

```php
$post = Post::find(1);
$post->title = 'Update';
$post->save();
```

### Menghapus data

```php
Post::destroy(1);
```

### Pengertian tambahan

- `all()` mengambil semua data.
- `find(1)` mengambil data berdasarkan ID.
- `create()` menyimpan data baru.
- `save()` menyimpan perubahan object.
- Agar `create()` bisa dipakai, field harus diizinkan melalui `fillable` atau `guarded`.

Contoh:

```php
class Post extends Model
{
    protected $fillable = ['title', 'content'];
}
```

---

## 11. CRUD Lengkap Laravel

CRUD adalah singkatan dari:

- **Create** → menambah data
- **Read** → menampilkan data
- **Update** → mengubah data
- **Delete** → menghapus data

CRUD adalah dasar hampir semua aplikasi web.

### Route resource

```php
Route::resource('posts', PostController::class);
```

Route resource otomatis membuat route standar untuk CRUD seperti:

- `index`
- `create`
- `store`
- `show`
- `edit`
- `update`
- `destroy`

### Contoh Controller

```php
class PostController extends Controller
{
    public function index()
    {
        $posts = Post::all();
        return view('posts.index', compact('posts'));
    }

    public function create()
    {
        return view('posts.create');
    }

    public function store(Request $request)
    {
        Post::create($request->all());
        return redirect()->route('posts.index');
    }

    public function destroy(Post $post)
    {
        $post->delete();
        return back();
    }
}
```

### Contoh form create

```blade
<form method="POST" action="{{ route('posts.store') }}">
    @csrf

    <input name="title">
    <textarea name="content"></textarea>

    <button type="submit">Simpan</button>
</form>
```

### Catatan penting

Contoh di atas masih sederhana. Pada praktik nyata, sebaiknya jangan langsung memakai `$request->all()` tanpa validasi agar data lebih aman dan terkontrol.

---

## 12. Validation

Validation digunakan untuk memastikan data yang masuk sesuai aturan.

### Contoh validasi

```php
$request->validate([
    'title' => 'required|min:3'
]);
```

### Pengertian aturan di atas

- `required` → field wajib diisi
- `min:3` → minimal 3 karakter

### Kenapa validation penting?

- mencegah data kosong,
- menjaga format data tetap benar,
- membantu keamanan,
- mempermudah penanganan error input.

Contoh yang lebih baik:

```php
$request->validate([
    'title' => 'required|min:3|max:255',
    'content' => 'required'
]);
```

---

## 13. Middleware

Middleware adalah filter yang memeriksa request sebelum request tersebut masuk ke controller.

### Fungsi middleware

- mengecek apakah user sudah login,
- mengecek role admin,
- membatasi akses,
- memodifikasi request atau response.

### Contoh membuat middleware

```bash
php artisan make:middleware CheckAdmin
```

### Contoh logika middleware

```php
if (auth()->user()->role != 'admin') {
    abort(403);
}
```

### Pemakaian pada route

```php
Route::get('/admin', fn () => 'Admin')
    ->middleware('checkadmin');
```

### Pengertian tambahan

- `abort(403)` berarti akses ditolak.
- Middleware sangat penting untuk keamanan dan pembatasan hak akses.

---

## 14. Authentication

Authentication adalah sistem untuk mengenali siapa pengguna yang sedang masuk ke aplikasi.

Fitur ini biasanya meliputi:

- login,
- register,
- logout,
- reset password,
- session management.

### Cara modern yang umum dipakai

```bash
php artisan breeze:install
```

### Catatan

Perintah `php artisan make:auth` adalah cara lama pada versi Laravel terdahulu. Untuk Laravel modern, biasanya digunakan:

- Laravel Breeze
- Laravel Jetstream
- Laravel Fortify

### Pengertian tambahan

Authentication penting agar sistem bisa membedakan user biasa, admin, atau tamu yang belum login.

---

## 15. Relasi Database

Dalam aplikasi nyata, tabel biasanya saling berhubungan.

Contoh:

- satu user punya banyak post,
- satu post milik satu user,
- satu produk punya banyak transaksi.

### One To Many

Hubungan:

```text
User → Posts
```

### User Model

```php
public function posts()
{
    return $this->hasMany(Post::class);
}
```

### Post Model

```php
public function user()
{
    return $this->belongsTo(User::class);
}
```

### Pengertian tambahan

- `hasMany()` berarti satu data punya banyak data lain.
- `belongsTo()` berarti data tersebut dimiliki oleh satu data induk.

Contoh penggunaan:

```php
$user = User::find(1);
$user->posts;
```

Artinya: ambil semua post milik user dengan ID 1.

---

## 16. Artisan Command

Artisan adalah command line interface milik Laravel.

Artisan membantu developer menjalankan banyak tugas otomatis tanpa harus membuat semuanya secara manual.

### Perintah dasar

```bash
php artisan list
```

### Contoh command yang sering dipakai

```text
make:model
make:controller
migrate
serve
route:list
```

### Pengertian tambahan

- `make:model` membuat model baru
- `make:controller` membuat controller baru
- `migrate` menjalankan migration
- `serve` menjalankan server lokal
- `route:list` menampilkan daftar route yang tersedia

---

## 17. Security Laravel

Laravel sudah menyediakan banyak perlindungan dasar yang sangat membantu.

### Fitur keamanan bawaan

- CSRF Protection
- Password Hashing
- SQL Injection Protection
- XSS Protection pada output Blade biasa

### Contoh CSRF

```blade
@csrf
```

### Pengertian tambahan

#### CSRF Protection

Melindungi form agar tidak mudah disalahgunakan oleh request palsu dari situs lain.

#### Password Hashing

Password tidak disimpan dalam bentuk asli, tetapi diacak dengan aman.

#### SQL Injection Protection

Laravel membantu mengurangi risiko injeksi SQL saat memakai query builder atau Eloquent dengan benar.

#### XSS Protection

Blade secara default melakukan escaping pada output `{{ }}` sehingga script berbahaya tidak mudah dijalankan di halaman.

---

## 18. Best Practice Struktur Laravel

Saat project mulai besar, struktur kode perlu dibuat lebih teratur.

Contoh susunan yang sering dipakai:

```text
app/
├── Models
├── Http
│   ├── Controllers
│   ├── Requests
│   └── Middleware
├── Services
└── Repositories
```

### Penjelasan singkat

- `Controllers` → menangani request dan response
- `Requests` → tempat validasi yang lebih rapi
- `Middleware` → filter request
- `Services` → logika bisnis
- `Repositories` → pengelolaan akses data bila project membutuhkannya

### Tujuan best practice

- kode lebih mudah dipelihara,
- tanggung jawab tiap file lebih jelas,
- mempermudah kerja tim,
- lebih siap untuk pengembangan jangka panjang.

---

## 19. Flow Request Laravel

Bagian ini sangat penting untuk dipahami karena menjelaskan bagaimana request bergerak di dalam aplikasi Laravel.

```text
Browser
   ↓
Route
   ↓
Middleware
   ↓
Controller
   ↓
Model
   ↓
Database
   ↓
View → Browser
```

### Penjelasan alur

1. User mengakses halaman melalui browser.
2. Request masuk ke route.
3. Middleware memeriksa request jika ada aturan khusus.
4. Controller menjalankan logika aplikasi.
5. Model mengambil atau menyimpan data ke database.
6. Hasilnya dikirim ke view.
7. View ditampilkan kembali ke browser.

Jika aplikasi berbentuk API, biasanya response langsung berupa JSON tanpa view.

---

## 20. Tips Belajar Laravel Lebih Cepat

Urutan belajar yang disarankan:

1. Routing
2. Controller
3. Blade
4. Migration
5. Model dan Eloquent
6. CRUD
7. Validation
8. Authentication
9. API
10. Deployment

### Saran belajar

- jangan hanya membaca, langsung praktik,
- buat project kecil,
- biasakan membaca error message,
- pahami alur request,
- pelajari dokumentasi resmi Laravel.

---

## 21. Mini Project Latihan

Setelah memahami dasar, latih kemampuan dengan project sederhana berikut:

- Blog sederhana
- Sistem absensi
- Perpustakaan
- Inventory barang
- Todo list
- Kasir sederhana

### Tujuan mini project

Mini project membantu menggabungkan semua materi dasar sekaligus, seperti routing, controller, model, migration, validation, dan authentication.

---

## 22. Kesimpulan

Laravel adalah framework PHP yang sangat kuat untuk membangun aplikasi web modern.

### Inti yang perlu dipahami

- Laravel membuat proses coding lebih cepat.
- Struktur project menjadi lebih rapi dan profesional.
- Fitur bawaan membantu keamanan aplikasi.
- Cocok untuk project kecil maupun besar.
- Sangat penting memahami alur: **route → controller → model → database/view**.

### Penutup

Jika dasar-dasar Laravel ini sudah dipahami dengan baik, tahap berikutnya akan jauh lebih mudah, seperti mempelajari:

- REST API,
- upload file,
- pagination,
- export data,
- queue,
- testing,
- hingga deployment ke server.

---

## Ringkasan Singkat

Laravel bukan hanya alat untuk membuat website, tetapi juga framework yang membantu developer menulis kode dengan lebih terstruktur, aman, dan efisien. Semakin paham konsep dasarnya, semakin mudah membangun aplikasi yang stabil dan scalable.
