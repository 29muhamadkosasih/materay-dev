# Software Development Lifecycle (SDLC)

Dalam proses pembuatan website atau aplikasi, ada rangkaian tahapan kerja yang dilakukan secara sistematis agar hasil akhirnya lebih profesional, terstruktur, aman, mudah dipelihara, dan mudah dikembangkan di masa depan. Rangkaian proses ini sering disebut **Software Development Lifecycle (SDLC)** atau **siklus hidup pengembangan perangkat lunak**.

Dokumen ini menjelaskan tahapan SDLC dari awal sampai aplikasi siap digunakan dan terus dikembangkan.

---

## 1. Pengertian Software Development Lifecycle

**Software Development Lifecycle (SDLC)** adalah proses terstruktur yang digunakan untuk merancang, membuat, menguji, menerapkan, dan memelihara sebuah sistem atau aplikasi.

### Tujuan SDLC

SDLC digunakan agar proses pengembangan aplikasi tidak dilakukan secara asal. Dengan adanya tahapan yang jelas, tim dapat:

- memahami kebutuhan dengan lebih tepat,
- mengurangi kesalahan saat coding,
- menghemat waktu dan biaya,
- menjaga kualitas aplikasi,
- mempermudah kerja tim,
- mempermudah pengembangan lanjutan.

### Pengertian sederhananya

Jika diibaratkan membangun rumah, maka SDLC adalah urutan kerja mulai dari:

- menentukan rumah akan dibuat seperti apa,
- membuat gambar desain,
- membangun pondasi dan struktur,
- mengecek hasil bangunan,
- hingga rumah siap ditempati dan dirawat.

Pada aplikasi, prosesnya kurang lebih sama: dimulai dari ide, lalu perencanaan, desain, pembangunan, pengujian, peluncuran, hingga pemeliharaan.

---

## 2. Perencanaan (Planning)

Tahap ini adalah langkah paling awal untuk menentukan arah project.

### Tujuan planning

Tahap perencanaan bertujuan menjawab pertanyaan dasar seperti:

- aplikasi ini dibuat untuk apa,
- siapa target penggunanya,
- masalah apa yang ingin diselesaikan,
- fitur apa saja yang dibutuhkan,
- berapa estimasi waktu dan biaya.

### Kegiatan yang biasanya dilakukan

- analisis masalah,
- menentukan tujuan project,
- menentukan target user,
- menentukan platform: web, mobile, desktop,
- menentukan teknologi yang akan dipakai,
- menyusun timeline kerja.

### Contoh

Misalnya membuat **Aplikasi Perpustakaan Sekolah**.

#### Target pengguna

- Admin
- Petugas
- Siswa

#### Fitur awal

- Login
- Data buku
- Peminjaman buku
- Pengembalian buku
- Laporan

### Pengertian tambahan

Tahap planning sangat penting karena jika dari awal arah project sudah salah, maka proses berikutnya akan ikut bermasalah. Banyak project gagal bukan karena coding-nya jelek, tetapi karena perencanaannya kurang matang.

---

## 3. Analisis Kebutuhan (Requirement Analysis)

Tahap ini mengubah ide awal menjadi daftar kebutuhan yang lebih jelas dan lebih teknis.

### Tujuan requirement analysis

Agar tim pengembang, designer, tester, dan stakeholder memiliki pemahaman yang sama tentang sistem yang akan dibuat.

### Jenis requirement

#### A. Functional Requirement

Functional requirement adalah kebutuhan tentang **fitur yang harus dimiliki sistem**.

Contoh:

- user bisa login,
- admin bisa menambah data,
- user bisa mencari data,
- sistem bisa mencetak laporan.

#### B. Non-Functional Requirement

Non-functional requirement adalah kebutuhan tentang **kualitas sistem**.

Contoh:

- aplikasi harus cepat,
- sistem harus aman,
- tampilan harus mobile friendly,
- sistem mampu menangani banyak user,
- downtime harus minim.

### Contoh requirement

#### Functional

- User login
- CRUD data buku
- Search data buku
- Cetak laporan peminjaman

#### Non-functional

- Response time cepat
- Data aman
- Tampilan responsif
- Support 1000 user per hari

### Pengertian tambahan

Jika planning menjawab **apa tujuan aplikasi**, maka requirement analysis menjawab **apa saja kebutuhan detail agar tujuan itu tercapai**.

---

## 4. Perancangan Sistem (System Design)

Tahap ini adalah proses membuat blueprint atau rancangan sistem sebelum coding dimulai.

### Tujuan system design

Agar tim memiliki gambaran teknis yang jelas tentang bagaimana sistem akan dibangun.

### Bagian yang biasanya dirancang

#### A. Arsitektur Sistem

Menentukan susunan komponen utama aplikasi.

Contoh:

```text
Client → Server → Database
(Vue)   (Laravel)   (MySQL)
```

Artinya:

- **Client** menangani tampilan yang digunakan user,
- **Server** menangani logika aplikasi,
- **Database** menyimpan data.

#### B. Database Design

Merancang tabel, relasi, dan struktur data.

Biasanya menggunakan:

- ERD (Entity Relationship Diagram)
- Diagram relasi tabel

Contoh tabel:

- `users`
- `books`
- `borrowings`

#### C. Alur Sistem

Menentukan bagaimana data bergerak dari user ke sistem dan kembali lagi ke user.

Contoh:

- user login,
- sistem memverifikasi akun,
- user masuk dashboard,
- user melakukan transaksi,
- data disimpan ke database.

### Pengertian tambahan

System design membantu mengurangi kebingungan saat development. Dengan rancangan yang jelas, coding menjadi lebih cepat dan kesalahan desain bisa dikurangi sejak awal.

---

## 5. Design UI/UX (Interface Design)

Tahap ini fokus pada tampilan aplikasi dan pengalaman pengguna.

### Pengertian UI dan UX

- **UI (User Interface)** adalah tampilan visual aplikasi, seperti warna, tombol, layout, dan ikon.
- **UX (User Experience)** adalah pengalaman user saat menggunakan aplikasi, apakah mudah dipahami, nyaman, dan efisien.

### Komponen UI/UX

- Layout
- Warna
- Typography
- Tombol
- Form input
- Navigasi
- Responsivitas tampilan

### Contoh halaman yang biasanya didesain

- Login page
- Dashboard
- Data table
- Form input
- Detail halaman
- Halaman laporan

### Tools yang sering dipakai

- Figma
- Adobe XD
- Sketch

### Tujuan UI/UX

- aplikasi mudah digunakan,
- user tidak bingung,
- tampilan lebih profesional,
- proses kerja user lebih cepat.

### Pengertian tambahan

Aplikasi yang bagus bukan hanya yang kodenya benar, tetapi juga yang nyaman dipakai. Karena itu UI/UX punya peran penting dalam keberhasilan aplikasi.

---

## 6. Development (Coding / Implementation)

Tahap development adalah proses menerjemahkan desain menjadi kode program.

Pada tahap ini, developer mulai membangun semua bagian aplikasi berdasarkan hasil planning, requirement, dan design.

Secara umum development dibagi menjadi tiga bagian utama.

---

### A. Frontend Development

Frontend adalah bagian aplikasi yang langsung dilihat dan digunakan user.

#### Tugas frontend

- membuat tampilan halaman,
- menampilkan data,
- menangani interaksi user,
- menghubungkan tampilan dengan API atau backend.

#### Teknologi yang sering dipakai

- HTML
- CSS
- JavaScript
- Vue.js
- React
- Bootstrap / Tailwind CSS

#### Contoh

```html
<button @click="saveData">Simpan</button>
```

### Pengertian tambahan

Frontend berfokus pada pengalaman user di sisi tampilan. Jika backend adalah otak sistem, maka frontend adalah wajah sistem.

---

### B. Backend Development

Backend adalah bagian yang menangani logika, aturan bisnis, proses data, autentikasi, validasi, dan komunikasi dengan database.

#### Tugas backend

- menerima request,
- memproses logika,
- menyimpan atau mengambil data,
- mengatur hak akses,
- mengirim response ke frontend.

#### Teknologi yang sering dipakai

- Laravel
- Node.js
- Django
- Spring Boot

#### Contoh Laravel

```php
public function store(Request $request)
{
    User::create($request->all());
}
```

### Pengertian tambahan

Backend adalah pusat kendali aplikasi. Hampir semua aturan penting sistem dijalankan di sini.

---

### C. Database Development

Database development adalah proses merancang dan membangun tempat penyimpanan data aplikasi.

#### Tugas database development

- membuat tabel,
- menentukan relasi,
- mengatur struktur kolom,
- menjaga konsistensi data,
- mendukung performa query.

#### Contoh migration Laravel

```php
Schema::create('books', function (Blueprint $table) {
    $table->id();
    $table->string('title');
});
```

### Pengertian tambahan

Database adalah fondasi data aplikasi. Jika struktur database buruk, maka aplikasi akan sulit dikembangkan dan performanya bisa bermasalah.

---

## 7. Integrasi dan Security

Setelah komponen utama dibuat, tahap berikutnya adalah menghubungkan semua bagian sistem dan memastikan sistem cukup aman.

### Komponen penting

- Authentication (Login)
- Authorization (Role dan Permission)
- API Integration
- Validation input
- Encryption password
- Session management
- Proteksi terhadap request berbahaya

### Contoh

- Admin memiliki akses berbeda dengan user biasa.
- Password tidak disimpan dalam bentuk asli.
- Input form harus divalidasi sebelum disimpan.

### Pengertian tambahan

Tahap ini sangat penting karena aplikasi yang berjalan tetapi tidak aman tetap berbahaya. Sistem harus bisa melindungi data user dan mencegah akses yang tidak sah.

---

## 8. Testing (Pengujian)

Testing adalah proses memeriksa apakah aplikasi berjalan sesuai kebutuhan dan bebas dari kesalahan besar.

### Tujuan testing

- menemukan bug,
- memastikan fitur berjalan benar,
- memastikan sistem stabil,
- memastikan aplikasi sesuai kebutuhan user.

### Jenis testing

#### A. Unit Testing

Menguji bagian kecil dari program, misalnya satu function atau satu method.

#### B. Integration Testing

Menguji hubungan antar bagian sistem, misalnya form login dengan database.

#### C. User Acceptance Test (UAT)

User atau pihak terkait mencoba aplikasi secara langsung untuk memastikan sistem sesuai kebutuhan nyata.

### Contoh skenario testing

- Login gagal menampilkan pesan error.
- Data berhasil disimpan menampilkan notifikasi sukses.
- User tanpa hak akses tidak bisa masuk halaman admin.
- Form tidak boleh dikirim jika data belum lengkap.

### Pengertian tambahan

Testing bukan berarti mencari kesalahan developer, tetapi memastikan kualitas aplikasi sebelum dipakai user secara luas.

---

## 9. Deployment (Publish ke Server)

Deployment adalah proses memasang aplikasi ke server agar bisa diakses secara online atau digunakan di lingkungan produksi.

### Proses deployment

1. Menyiapkan VPS atau hosting
2. Upload project ke server
3. Konfigurasi environment
4. Konfigurasi database
5. Menyambungkan domain
6. Memasang SSL
7. Menjalankan aplikasi dalam mode production

### Contoh stack server

- Nginx
- PHP
- MySQL
- Ubuntu Server

### Pengertian tambahan

Saat masih di laptop developer, aplikasi hanya berjalan di lingkungan lokal. Deployment membuat aplikasi bisa diakses oleh user sebenarnya melalui internet atau jaringan internal.

---

## 10. Monitoring dan Maintenance

Setelah aplikasi live, pekerjaan belum selesai. Sistem harus terus dipantau dan dirawat.

### Kegiatan maintenance

- memperbaiki bug,
- update fitur,
- backup database,
- memantau error,
- optimasi performa,
- update keamanan,
- memperbarui dependensi jika diperlukan.

### Tools yang sering dipakai

- log monitoring,
- error tracking,
- analytics,
- backup system,
- server monitoring.

### Pengertian tambahan

Maintenance penting agar aplikasi tetap stabil, aman, dan relevan dengan kebutuhan pengguna yang terus berubah.

---

## 11. Improvement (Continuous Development)

Aplikasi profesional tidak berhenti setelah rilis pertama. Biasanya aplikasi akan terus dikembangkan berdasarkan masukan user, kebutuhan bisnis, dan perkembangan teknologi.

### Contoh improvement

- menambah fitur mobile,
- integrasi AI,
- dashboard analytics,
- notifikasi real-time,
- optimasi performa,
- integrasi payment gateway.

### Pengertian tambahan

Tahap improvement membuat aplikasi tetap kompetitif dan tidak tertinggal. Inilah alasan kenapa software sering memiliki versi 1.0, 2.0, dan seterusnya.

---

## 12. Gambaran Alur Lengkap SDLC

Berikut alur umum pengembangan aplikasi dari awal sampai pengembangan lanjutan:

```text
IDEA
 ↓
Planning
 ↓
Requirement Analysis
 ↓
System Design
 ↓
UI/UX Design
 ↓
Development
 ↓
Testing
 ↓
Deployment
 ↓
Maintenance
 ↓
Improvement
```

### Penjelasan singkat alur

- **Idea** → muncul kebutuhan atau masalah yang ingin diselesaikan.
- **Planning** → menentukan arah project.
- **Requirement Analysis** → merinci kebutuhan.
- **System Design** → membuat rancangan sistem.
- **UI/UX Design** → membuat rancangan tampilan dan pengalaman pengguna.
- **Development** → menulis kode program.
- **Testing** → mengecek kualitas sistem.
- **Deployment** → mempublikasikan aplikasi.
- **Maintenance** → merawat aplikasi yang sudah live.
- **Improvement** → mengembangkan versi yang lebih baik.

---

## 13. Komponen Utama Website atau Aplikasi

Selain memahami tahapan pengerjaan, penting juga memahami komponen teknis utama dalam sebuah sistem.

### 1. Frontend

Bagian tampilan yang berinteraksi langsung dengan user.

### 2. Backend

Bagian logika aplikasi yang memproses data dan aturan sistem.

### 3. Database

Tempat penyimpanan seluruh data aplikasi.

### 4. Server

Tempat aplikasi dijalankan agar bisa diakses oleh user.

### 5. API

Penghubung antar sistem atau antara frontend dan backend.

### 6. Security Layer

Lapisan keamanan untuk melindungi data, akses, dan proses aplikasi.

### Pengertian tambahan

Semua komponen di atas saling terhubung. Jika salah satu komponen bermasalah, maka sistem secara keseluruhan bisa terganggu.

---

## 14. Contoh Penerapan SDLC pada Project Sederhana

Misalnya akan dibuat **Aplikasi Absensi Sekolah**.

### Planning

Menentukan bahwa aplikasi dipakai oleh admin, guru, dan siswa.

### Requirement Analysis

Menentukan fitur:

- login,
- absensi harian,
- rekap kehadiran,
- laporan bulanan.

### System Design

Membuat tabel seperti:

- `users`,
- `students`,
- `attendance`.

### UI/UX Design

Membuat desain halaman:

- login,
- dashboard,
- form absensi,
- laporan.

### Development

Frontend membuat tampilan.
Backend membuat logika absensi.
Database menyimpan data kehadiran.

### Testing

Menguji apakah data absensi tersimpan dengan benar dan laporan muncul sesuai tanggal.

### Deployment

Aplikasi diunggah ke server sekolah atau hosting.

### Maintenance

Jika ada bug atau kebutuhan baru, sistem diperbaiki dan ditingkatkan.

---

## 15. Kesimpulan

Software Development Lifecycle adalah proses penting dalam pembuatan aplikasi agar hasilnya tidak asal jadi, tetapi benar-benar terencana, rapi, aman, dan siap digunakan.

### Inti yang perlu dipahami

- Pengembangan aplikasi memiliki tahapan yang jelas.
- Setiap tahap punya tujuan masing-masing.
- Coding hanyalah satu bagian dari keseluruhan proses.
- Testing, deployment, maintenance, dan improvement sama pentingnya dengan development.
- Semakin baik proses SDLC dijalankan, semakin baik pula kualitas aplikasi yang dihasilkan.

### Ringkasan singkat

Jika ingin membuat aplikasi yang profesional, jangan langsung lompat ke coding. Mulailah dari perencanaan, pahami kebutuhan, rancang sistem dengan baik, lalu bangun, uji, rilis, dan rawat aplikasi secara berkelanjutan.
