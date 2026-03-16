# Blueprint Sistem PPDB Online (Laravel)

Alur (flow) + desain sistem (arsitektur) Laravel yang lengkap untuk Sistem PPDB Online dengan fitur export PDF & Excel. Dokumen ini disusun dalam gaya blueprint agar bisa langsung jadi acuan implementasi.

## 1) Role & Modul Utama

### Role

#### Calon Siswa (Public)

- Isi form pendaftaran
- Upload berkas
- Cetak kartu pendaftaran
- Cek status verifikasi & seleksi
- Lihat pengumuman kelulusan
- Daftar ulang (jika lulus)

#### Admin PPDB (Backoffice)

- Kelola periode PPDB, kuota per jalur
- Verifikasi berkas & data
- Proses seleksi per jalur (zonasi/afirmasi/prestasi)
- Publish pengumuman
- Rekap pendaftar per jalur
- Export PDF/Excel

#### (Opsional) Verifikator

- Hanya akses modul verifikasi, tidak bisa publish hasil

## 2) Alur (Flow) End-to-End PPDB

### A. Persiapan (Admin)

- Admin membuat Periode PPDB (mis. “PPDB 2026/2027”):
  - tanggal buka/tutup pendaftaran
  - tanggal pengumuman
  - tanggal daftar ulang
- Admin set Jalur Pendaftaran:
  - Afirmasi: kuota, syarat dokumen (KIP/DTKS/dll)
  - Prestasi: kuota, bobot nilai/piagam
- Admin set Persyaratan Berkas (per jalur bisa berbeda)
- Admin set status sistem: `DIBUKA`

### B. Pendaftaran (Calon Siswa)

- Calon siswa buka halaman PPDB → pilih jalur (afirmasi/prestasi)
- Isi form:
  - data diri, alamat, sekolah asal
  - data orang tua/wali
  - pilihan jurusan/kelas (jika ada)
- Upload berkas:
  - KK, Akta, Raport, Piagam, KIP, dst sesuai jalur
- Sistem membuat:
  - Nomor Pendaftaran
  - Akun (opsional: login untuk cek status)
  - Status awal: `SUBMITTED`
- Calon siswa bisa Cetak Kartu Pendaftaran (PDF)

### C. Verifikasi (Admin)

- Admin melihat daftar pendaftar per jalur/periode
- Admin verifikasi:
  - kelengkapan data
  - validasi dokumen
- Status:
  - `VERIFIED` jika valid
  - `REJECTED` jika tidak valid (sertakan catatan)
  - (Opsional) `NEED_REVISION` jika perlu perbaikan

### D. Seleksi (Sistem/Admin)

Seleksi dilakukan hanya untuk `VERIFIED`.

- Validasi dokumen afirmasi
- Ranking bisa berdasarkan prioritas tertentu (aturan sekolah)

#### Prestasi

- Hitung skor: `(nilai raport × bobot) + (piagam × bobot)`
- Ranking berdasarkan skor

Output seleksi:

- Status seleksi: `PASSED` (lulus) / `FAILED` (tidak lulus) / `RESERVE` (cadangan)
- Simpan snapshot hasil seleksi agar tidak berubah saat rule berubah

### E. Pengumuman

- Admin klik “Publish Pengumuman”
- Calon siswa cek hasil dengan:
  - nomor pendaftaran + tanggal lahir (public)
  - atau login akun
- Jika lulus → muncul tombol “Daftar Ulang”

### F. Daftar Ulang

- Calon siswa isi form daftar ulang + upload bukti (jika diperlukan)
- Status daftar ulang:
  - `REREG_SUBMITTED`
  - `REREG_VERIFIED` (admin verifikasi)
- Cetak bukti daftar ulang (PDF)

### G. Rekap & Export

- Admin bisa rekap:
  - jumlah pendaftar per jalur
  - status (`submitted`/`verified`/`passed`/`failed`)
  - daftar lulus per jalur
- Export:
  - Excel untuk data tabular
  - PDF untuk laporan formal & kartu pendaftaran

## 3) Desain Arsitektur Laravel (Disarankan)

### Stack Laravel

- Laravel 10/11 (bebas), PHP 8.2+
- MySQL/MariaDB
- Queue (`database`/`redis`) untuk proses export besar
- Storage: public atau S3-compatible untuk berkas

### Pola Struktur

- MVC + Service Layer
- Form Request untuk validasi
- Policy/Gate untuk authorization
- Jobs untuk export async (opsional)
- Events untuk publish pengumuman (opsional)

## 4) Database Design (Entity & Relasi)

### Tabel inti (minimal)

#### `ppdb_periods`

- id, name, year
- registration_start, registration_end
- announcement_date
- rereg_start, rereg_end
- status (`DRAFT`/`OPEN`/`CLOSED`/`ANNOUNCED`)

#### `admission_tracks` (jalur)

- id, period_id
- code (`AFIRMASI`/`PRESTASI`)
- quota
- rules_json (aturan jalur)

#### `applicants`

- id, period_id, track_id
- registration_number (unik)
- full_name, nisn, birth_date, gender
- address fields, school_origin
- father/mother/guardian fields
- status: `SUBMITTED`/`VERIFIED`/`REJECTED`/`NEED_REVISION`
- verification_note

#### `applicant_documents`

- id, applicant_id
- type (`KK`/`AKTA`/`RAPORT`/`PIAGAM`/`KIP`/...)
- file_path, original_name
- status (`UPLOADED`/`VALID`/`INVALID`)
- note

#### `selection_results`

- id, applicant_id
- score_total (nullable)
- distance_km (nullable)
- rank (nullable)
- decision (`PASSED`/`FAILED`/`RESERVE`)
- decided_at

#### `re_registrations`

- id, applicant_id
- status (`SUBMITTED`/`VERIFIED`)
- fields sesuai kebutuhan

#### `users`

- admin/verifikator

#### (Opsional) `applicant_accounts`

- jika calon siswa login (email/phone + password)

### Catatan desain penting

- Simpan hasil seleksi di tabel `selection_results` (snapshot), jangan hitung ulang saat menampilkan pengumuman
- Berkas: simpan metadata di DB, file fisik di Storage

## 5) Route & URL Design

### Public

- `GET /ppdb` (landing)
- `GET /ppdb/register` (form)
- `POST /ppdb/register` (submit)
- `GET /ppdb/registration/{registration_number}/card` (PDF kartu)
- `GET /ppdb/announcement` (cek pengumuman)
- `POST /ppdb/announcement` (submit cek)
- `GET /ppdb/reregistration` + `POST` (daftar ulang)

### Admin (prefix: `/admin`)

- `GET /admin/dashboard`
- `CRUD /admin/periods`
- `CRUD /admin/tracks`
- `GET /admin/applicants` (filter: periode, jalur, status)
- `GET /admin/applicants/{id}`
- `POST /admin/applicants/{id}/verify`
- `POST /admin/applicants/{id}/reject`
- `POST /admin/selection/run` (jalankan seleksi per jalur/periode)
- `POST /admin/announcement/publish`
- `GET /admin/reports/recap` (rekap per jalur)
- `GET /admin/reports/recap.pdf`
- `GET /admin/reports/recap.xlsx`
- `GET /admin/reports/applicants.xlsx` (export data pendaftar)

## 6) UI/UX Design (Layar yang Wajib)

### Public

- Landing PPDB: info periode, jadwal, kuota jalur
- Form pendaftaran (wizard disarankan):
  - Step 1: Data pribadi
  - Step 2: Alamat & sekolah asal
  - Step 3: Orang tua/wali
  - Step 4: Upload berkas
  - Step 5: Review & submit
- Halaman sukses: nomor pendaftaran + tombol “Cetak Kartu”
- Cek pengumuman: input nomor pendaftaran + tanggal lahir
- Daftar ulang: form + upload (jika perlu)

### Admin

- List pendaftar (table + filter + bulk action)
- Detail pendaftar + viewer dokumen
- Verifikasi (valid/invalid per dokumen)
- Seleksi (preview ranking, kuota, tombol “Finalize”)
- Pengumuman: status publish + statistik
- Rekap + export

## 7) Export PDF & Excel (Implementasi Laravel)

### Excel

- Paket umum: `maatwebsite/excel`
- Use case:
  - Export daftar pendaftar (filter periode+jalur+status)
  - Export rekap per jalur (pivot sederhana)
- Output Excel biasanya:
  - Sheet 1: Applicants
  - Sheet 2: Summary/Recap

### PDF

- Paket umum:
  - `barryvdh/laravel-dompdf` (mudah, cocok laporan sederhana), atau
  - `snappy/wkhtmltopdf` (lebih presisi untuk layout kompleks)
- Use case PDF:
  - Kartu pendaftaran (dengan QR code)
  - Rekap laporan per jalur
  - Bukti daftar ulang
- Catatan:
  - Untuk kartu: buat template Blade khusus `resources/views/pdf/registration-card.blade.php`
  - QR: encode URL verifikasi kartu atau nomor pendaftaran

## 8) Business Rules (Harus Diputuskan Sejak Awal)

- Validasi upload: tipe file (pdf/jpg/png), limit size, scan mime type
- Simpan file pakai nama aman (UUID), jangan pakai nama asli
- Authorization admin pakai middleware auth + role
- Rate limit cek pengumuman (hindari brute force nomor pendaftaran)
- Audit log aksi admin (verifikasi/penolakan/publish)

## 9) Output Rekap yang Disarankan

Rekap per jalur (untuk PDF/Excel):

- Total pendaftar
- Total verified
- Total rejected
- Total lulus / tidak lulus / cadangan
- Total daftar ulang (verified)
