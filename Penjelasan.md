# PENJELASAN PROJECT SCMFashion
## Perancangan Sistem Informasi Pengolahan Data Pelanggan Berbasis Model WaterFall Pada Toko Mode Fashion

---

## 1. GAMBARAN UMUM PROJECT

### 1.1 Judul
**Perancangan Sistem Informasi Pengolahan Data Pelanggan Berbasis Model WaterFall Pada Toko Mode Fashion**

### 1.2 Latar Belakang
Toko Mode Fashion merupakan toko yang bergerak di bidang penjualan produk fashion seperti kemeja, dress, celana, jaket, rok, dan aksesoris. Seiring bertambahnya jumlah pelanggan dan produk, pengelolaan data secara manual menjadi tidak efisien dan rentan terhadap kesalahan. Oleh karena itu, dibutuhkan sebuah **Sistem Informasi** yang dapat mengelola data pelanggan, data produk, data kategori, serta mencatat transaksi penjualan secara terkomputerisasi.

### 1.3 Tujuan
1. Mempermudah proses **pengolahan data pelanggan** pada Toko Mode Fashion.
2. Mempermudah proses **pencatatan dan pengelolaan produk** fashion.
3. Mempermudah proses **transaksi penjualan** dengan perhitungan otomatis.
4. Menyediakan fitur **pencarian data** secara cepat dan akurat.
5. Menerapkan sistem **CRUD** (Create, Read, Update, Delete) pada setiap modul data.

### 1.4 Metode Pengembangan: WaterFall
Project ini dikembangkan menggunakan **Model WaterFall** dengan tahapan:

```
1. Analisis Kebutuhan  →  Menganalisis kebutuhan sistem toko fashion
         ↓
2. Desain Sistem       →  Merancang database & antarmuka (UI)
         ↓
3. Implementasi        →  Menulis kode program VB.NET + MySQL
         ↓
4. Pengujian           →  Testing semua fitur CRUD & Transaksi
         ↓
5. Pemeliharaan        →  Perbaikan bug & pengembangan lanjutan
```

---

## 2. TEKNOLOGI YANG DIGUNAKAN

| Komponen | Teknologi | Keterangan |
|---|---|---|
| Bahasa Pemrograman | **VB.NET** | Visual Basic .NET Framework 4 |
| IDE | **Microsoft Visual Studio** | Untuk menulis dan menjalankan kode |
| Database | **MySQL** | Menyimpan seluruh data aplikasi |
| Database Manager | **phpMyAdmin (XAMPP)** | Antarmuka web untuk mengelola MySQL |
| Konektor Database | **MySQL Connector .NET 6.6.4** | Library penghubung VB.NET ke MySQL |
| Kontrol Data | **ListView** | Menampilkan data dalam bentuk tabel |
| Kontrol Navigasi | **MenuStrip** | Menu navigasi utama aplikasi |

---

## 3. STRUKTUR DATABASE (db_scmfashion)

Database bernama **db_scmfashion** terdiri dari **5 tabel** dan **1 view**:

### 3.1 Tabel `tbluser` — Data Login
| Kolom | Tipe | Keterangan |
|---|---|---|
| id_user | INT (PK, Auto) | ID user otomatis |
| username | VARCHAR(50) | Nama pengguna untuk login |
| password | VARCHAR(50) | Kata sandi |
| nama_lengkap | VARCHAR(100) | Nama lengkap pengguna |
| level | VARCHAR(20) | Hak akses (Admin/Operator) |

**Akun bawaan:**
- `admin` / `admin123` (Admin)
- `operator` / `operator123` (Operator)

### 3.2 Tabel `tblpelanggan` — Data Pelanggan
| Kolom | Tipe | Keterangan |
|---|---|---|
| id_pelanggan | VARCHAR(10) (PK) | Kode pelanggan, contoh: PLG001 |
| nama | VARCHAR(100) | Nama pelanggan |
| tempat_lahir | VARCHAR(50) | Tempat lahir |
| tgl_lahir | DATE | Tanggal lahir |
| usia | VARCHAR(30) | Usia (dihitung otomatis) |
| jenis_kelamin | VARCHAR(15) | Laki-Laki / Perempuan |
| membership | VARCHAR(20) | Gold / Silver / Bronze / Regular / VIP |
| alamat | TEXT | Alamat lengkap |
| telp | VARCHAR(15) | Nomor telepon/HP |

### 3.3 Tabel `tblproduk` — Data Produk Fashion
| Kolom | Tipe | Keterangan |
|---|---|---|
| kd_produk | VARCHAR(10) (PK) | Kode produk, contoh: PRD001 |
| nm_produk | VARCHAR(100) | Nama produk |
| harga | VARCHAR(20) | Harga satuan (Rp) |
| stok | VARCHAR(10) | Jumlah stok tersedia |

### 3.4 Tabel `tblkategori` — Kategori Produk
| Kolom | Tipe | Keterangan |
|---|---|---|
| kd_kategori | VARCHAR(10) (PK) | Kode kategori, contoh: KAT001 |
| nm_kategori | VARCHAR(100) | Nama kategori (Atasan, Bawahan, dll) |
| deskripsi | VARCHAR(200) | Penjelasan kategori |

### 3.5 Tabel `tbltransaksi` — Data Transaksi Penjualan
| Kolom | Tipe | Keterangan |
|---|---|---|
| id_transaksi | INT (PK, Auto) | ID transaksi otomatis |
| tgl_transaksi | VARCHAR(20) | Bulan/Tahun transaksi |
| metode_bayar | VARCHAR(20) | Cash / Transfer / E-Wallet / dll |
| id_pelanggan | VARCHAR(10) (FK) | Merujuk ke tblpelanggan |
| kd_produk | VARCHAR(10) (FK) | Merujuk ke tblproduk |
| kd_kategori | VARCHAR(10) (FK) | Merujuk ke tblkategori |
| qty | INT | Jumlah barang dibeli |
| harga_satuan | DOUBLE | Harga per unit |
| diskon | DOUBLE | Persentase diskon (%) |
| total_harga | DOUBLE | Hasil perhitungan akhir |
| status | VARCHAR(20) | Premium / Lunas |

### 3.6 View `querytransaksi`
View ini menggabungkan data dari 4 tabel (transaksi + pelanggan + produk + kategori) menggunakan perintah **LEFT JOIN** untuk menampilkan informasi lengkap transaksi beserta nama pelanggan, nama produk, dan nama kategori.

### 3.7 Relasi Antar Tabel
```
tblpelanggan ──────┐
                    ├──→ tbltransaksi ──→ querytransaksi (VIEW)
tblproduk    ──────┤
                    │
tblkategori  ──────┘
```

---

## 4. STRUKTUR FILE PROJECT

```
SCMFashion/
├── SCMFashion.sln              ← File Solution (pembuka project)
├── db_scmfashion.sql           ← Script database MySQL
├── Penjelasan.md               ← Dokumen penjelasan ini
│
└── SCMFashion/                 ← Folder utama project
    ├── BukaKoneksi.vb          ← Modul koneksi database
    ├── FormLogin.vb            ← Form login pengguna
    ├── F_MenuUtama.vb          ← Menu utama (navigasi)
    ├── FormPelanggan.vb        ← CRUD data pelanggan
    ├── FormProduk.vb           ← CRUD data produk
    ├── FormKategori.vb         ← CRUD data kategori
    ├── FormTransaksi.vb        ← Proses transaksi penjualan
    ├── FormCariPelanggan.vb    ← Form pencarian pelanggan
    ├── FormCariProduk.vb       ← Form pencarian produk
    ├── FormCariKategori.vb     ← Form pencarian kategori
    ├── *.Designer.vb           ← Desain tampilan setiap form
    ├── *.resx                  ← Resource file setiap form
    ├── SCMFashion.vbproj       ← Konfigurasi project
    ├── app.config              ← Konfigurasi aplikasi
    ├── My Project/             ← File sistem project VB.NET
    ├── bin/Debug/              ← Folder output saat compile
    └── obj/                    ← Folder object saat compile
```

---

## 5. PENJELASAN SETIAP FORM / MODUL

### 5.1 BukaKoneksi.vb — Modul Koneksi Database
- Berisi **module** bernama `bukakoneksi` yang bersifat **global** (dapat diakses dari semua form).
- Fungsi utama: `koneksiKeDataBase()` — membuka koneksi ke MySQL menggunakan **connection string**:
  `server=localhost; user id=root; password=; database=db_scmfashion`
- Variabel global yang dideklarasikan: `conn`, `daData`, `dsData`, `query`, `RD`, `cmd`.

### 5.2 FormLogin — Halaman Login
- **Tampilan:** Field Username, Password (karakter tersembunyi dengan `*`), tombol LOGIN dan KELUAR.
- **Proses:** Mencocokkan input username & password dengan data di `tbluser`. Jika cocok, form login disembunyikan dan **F_MenuUtama** ditampilkan. Jika tidak cocok, muncul pesan error.
- **Fitur tambahan:** Menekan Enter pada field password akan otomatis menjalankan proses login.

### 5.3 F_MenuUtama — Menu Utama
- **Tampilan:** Form maximized dengan **MenuStrip** di bagian atas.
- **Menu navigasi:**
  - **Input Data** → Pelanggan, Produk, Kategori
  - **Proses Data** → Transaksi Penjualan
  - **Cari Data** → Pelanggan, Produk, Kategori
  - **Keluar** → Menutup aplikasi

### 5.4 FormPelanggan — CRUD Data Pelanggan
Form ini adalah yang **paling kompleks** karena menggunakan berbagai jenis kontrol:
- **TextBox** → ID Pelanggan, Nama, Tempat Lahir, Usia, Alamat, Telepon
- **DateTimePicker** → Tanggal Lahir (format yyyy/MM/dd)
- **RadioButton** → Jenis Kelamin (Laki-Laki / Perempuan)
- **CheckBox** → Membership (Gold, Silver, Bronze, Regular, VIP) — hanya boleh pilih satu
- **Fitur otomatis:** Saat tanggal lahir diubah, usia dihitung otomatis (tahun & bulan)
- **Pencarian realtime:** Mengetik di kolom pencarian langsung memfilter data

### 5.5 FormProduk — CRUD Data Produk
- **Field:** Kode Produk, Nama Produk, Harga, Stok
- Form sederhana dengan 4 field input dan ListView untuk menampilkan data
- Pencarian berdasarkan kode atau nama produk

### 5.6 FormKategori — CRUD Data Kategori
- **Field:** Kode Kategori, Nama Kategori, Deskripsi
- Form sederhana dengan 3 field input
- Pencarian berdasarkan kode atau nama kategori

### 5.7 FormTransaksi — Proses Transaksi Penjualan
Form ini adalah **inti proses bisnis** aplikasi:
- **ComboBox:** Tanggal Transaksi (bulan/tahun) dan Metode Bayar (Cash, Transfer Bank, E-Wallet, Kartu Kredit, Kartu Debit)
- **Button Cari:** Untuk mencari data Pelanggan, Produk, dan Kategori melalui form popup
- **Input:** Qty (jumlah beli) dan Diskon (%)
- **Tombol PROSES:** Menghitung total harga dan menentukan status

**Rumus Perhitungan:**
```
Subtotal     = Qty × Harga Satuan
Potongan     = Subtotal × (Diskon / 100)
Total Harga  = Subtotal - Potongan

Status:
  - Jika Total Harga ≥ 500.000  → "Premium"
  - Jika Total Harga ≥ 200.000  → "Lunas"
  - Jika Total Harga < 200.000  → "Lunas"
```

### 5.8 Form Pencarian (FormCariPelanggan, FormCariProduk, FormCariKategori)
- Form popup yang dipanggil dari **FormTransaksi**
- Menampilkan data dalam ListView dengan fitur pencarian
- Ketika user mengklik salah satu baris data, data tersebut otomatis terisi ke field di FormTransaksi, lalu form pencarian tertutup

---

## 6. POLA CRUD YANG DIGUNAKAN

Setiap form CRUD mengikuti pola yang **konsisten dan seragam**:

| No | Fungsi/Prosedur | Kegunaan |
|---|---|---|
| 1 | `posisilist()` | Menyiapkan kolom-kolom pada ListView |
| 2 | `Isilist()` | Mengambil data dari database dan menampilkan ke ListView |
| 3 | `caridataXXX()` | Mencari data berdasarkan kata kunci (pencarian realtime) |
| 4 | `AmbilDataDariListview()` | Mengisi form input dari baris ListView yang diklik |
| 5 | `Bersih()` | Mengosongkan semua field input |
| 6 | `btnSave_Click` | **CREATE** — Menyimpan data baru (`INSERT INTO`) |
| 7 | `btnEdit_Click` | **UPDATE** — Mengubah data yang dipilih (`UPDATE SET WHERE`) |
| 8 | `btnDelete_Click` | **DELETE** — Menghapus data dengan konfirmasi (`DELETE FROM WHERE`) |
| 9 | `btnRefresh_Click` | Me-refresh tampilan (Bersih + Isilist) |
| 10 | `btnExit_Click` | Menutup form (`Me.Close()`) |
| 11 | `Form_Load` | Inisialisasi: koneksi DB + setup kolom + load data |

---

## 7. CARA MENGGUNAKAN PROGRAM (PANDUAN LENGKAP)

### LANGKAH 1: Persiapan Database
1. Buka **XAMPP Control Panel**, klik **Start** pada **Apache** dan **MySQL**.
2. Buka browser, akses `http://localhost/phpmyadmin/`.
3. Klik **New** di sidebar kiri, buat database baru bernama `db_scmfashion`, klik **Create**.
4. Pilih database `db_scmfashion`, klik tab **Import**.
5. Klik **Choose File**, pilih file `db_scmfashion.sql` dari folder project.
6. Klik **Go**. Pastikan muncul pesan sukses dan 5 tabel terbuat.

### LANGKAH 2: Buka dan Jalankan Project
1. Buka file `SCMFashion.sln` menggunakan **Microsoft Visual Studio**.
2. Pastikan referensi **MySql.Data** tidak error (cek di Solution Explorer → References).
3. Tekan **F5** atau klik tombol **Start** (▶) untuk menjalankan program.

### LANGKAH 3: Login
1. Masukkan **Username**: `admin`
2. Masukkan **Password**: `admin123`
3. Klik tombol **LOGIN**.
4. Jika berhasil, akan muncul pesan "Login Berhasil!" dan masuk ke Menu Utama.

### LANGKAH 4: Input Data Pelanggan
1. Klik menu **Input Data** → **Pelanggan**.
2. Isi semua field: ID Pelanggan (contoh: PLG004), Nama, Tempat Lahir, pilih Tanggal Lahir (usia otomatis terhitung), pilih Jenis Kelamin, centang Membership, isi Alamat dan Telepon.
3. Klik **SAVE** untuk menyimpan.
4. Untuk mengedit: klik data di ListView → ubah field → klik **EDIT**.
5. Untuk menghapus: klik data di ListView → klik **DELETE** → konfirmasi OK.

### LANGKAH 5: Input Data Produk
1. Klik menu **Input Data** → **Produk**.
2. Isi: Kode Produk (contoh: PRD006), Nama Produk, Harga, Stok.
3. Klik **SAVE**. Proses Edit dan Delete sama seperti Pelanggan.

### LANGKAH 6: Input Data Kategori
1. Klik menu **Input Data** → **Kategori**.
2. Isi: Kode Kategori (contoh: KAT006), Nama Kategori, Deskripsi.
3. Klik **SAVE**.

### LANGKAH 7: Proses Transaksi Penjualan
1. Klik menu **Proses Data** → **Transaksi Penjualan**.
2. Pilih **Tgl Transaksi** dari ComboBox (contoh: 2026/05).
3. Pilih **Metode Bayar** dari ComboBox (contoh: Cash).
4. Klik **Cari Pelanggan** → pilih pelanggan dari popup → ID & Nama otomatis terisi.
5. Klik **Cari Produk** → pilih produk → Kode, Nama, dan Harga Satuan otomatis terisi.
6. Klik **Cari Kategori** → pilih kategori → Kode dan Nama Kategori otomatis terisi.
7. Masukkan **Qty** (jumlah beli, contoh: 2).
8. Masukkan **Diskon** (persentase, contoh: 10 untuk 10%, atau 0 jika tanpa diskon).
9. Klik **PROSES** → Total Harga dan Status dihitung otomatis.
10. Klik **SAVE** untuk menyimpan transaksi.

### LANGKAH 8: Cari Data
1. Klik menu **Cari Data** → pilih Pelanggan/Produk/Kategori.
2. Ketik kata kunci di kolom Pencarian → data langsung terfilter secara realtime.

---

## 8. CONTOH SKENARIO TRANSAKSI

### Skenario 1: Transaksi Tanpa Diskon
```
Pelanggan : PLG001 - Siti Rahma
Produk    : PRD001 - Kemeja Batik Premium (Rp 250.000)
Kategori  : KAT001 - Atasan
Qty       : 2
Diskon    : 0%

Perhitungan:
  Subtotal    = 2 × 250.000 = 500.000
  Potongan    = 500.000 × (0/100) = 0
  Total Harga = 500.000 - 0 = Rp 500.000
  Status      = Premium (karena ≥ 500.000)
```

### Skenario 2: Transaksi Dengan Diskon 10%
```
Pelanggan : PLG001 - Siti Rahma
Produk    : PRD001 - Kemeja Batik Premium (Rp 250.000)
Kategori  : KAT001 - Atasan
Qty       : 2
Diskon    : 10%

Perhitungan:
  Subtotal    = 2 × 250.000 = 500.000
  Potongan    = 500.000 × (10/100) = 50.000
  Total Harga = 500.000 - 50.000 = Rp 450.000
  Status      = Lunas (karena 450.000 ≥ 200.000 tapi < 500.000)
```

### Skenario 3: Transaksi Kecil
```
Pelanggan : PLG002 - Budi Santoso
Produk    : PRD005 - Rok Mini Plisket (Rp 120.000)
Kategori  : KAT002 - Bawahan
Qty       : 1
Diskon    : 0%

Perhitungan:
  Subtotal    = 1 × 120.000 = 120.000
  Total Harga = Rp 120.000
  Status      = Lunas (karena < 200.000)
```

---

## 9. KESIMPULAN

Sistem Informasi Pengolahan Data Pelanggan **SCMFashion** berhasil dikembangkan menggunakan bahasa pemrograman **VB.NET Framework 4** dengan database **MySQL**. Aplikasi ini menerapkan konsep **CRUD** secara menyeluruh pada setiap modul (Pelanggan, Produk, Kategori, dan Transaksi), dilengkapi dengan fitur **Login**, **pencarian data realtime**, dan **proses perhitungan transaksi otomatis**. Pengembangan dilakukan menggunakan metode **WaterFall** yang terstruktur dan sistematis, sehingga menghasilkan aplikasi yang mudah digunakan dan sesuai kebutuhan operasional Toko Mode Fashion.

---

*Dokumen ini dibuat sebagai penjelasan project UAS Mata Kuliah Pemrograman Visual.*
