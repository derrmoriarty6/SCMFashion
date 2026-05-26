# Walkthrough - Project SCMFashion

## Ringkasan
Project **SCMFashion** (Sistem Informasi Pengolahan Data Pelanggan - Toko Mode Fashion) telah selesai dibuat berdasarkan referensi project `AplNilaiMhs`. Total **40+ file** berhasil dibuat.

## Struktur Project

```
SCMFashion/
├── SCMFashion.sln                    ← Solution file
├── db_scmfashion.sql                 ← SQL script untuk database
└── SCMFashion/
    ├── SCMFashion.vbproj             ← Project file (.NET Framework 4.0)
    ├── app.config
    ├── BukaKoneksi.vb                ← Module koneksi ke db_scmfashion
    ├── FormLogin.vb + .Designer.vb   ← 🔐 Form Login (BARU)
    ├── F_MenuUtama.vb + .Designer.vb ← 📋 Menu Utama
    ├── FormPelanggan.vb + .Designer  ← 👤 CRUD Pelanggan
    ├── FormProduk.vb + .Designer     ← 👕 CRUD Produk
    ├── FormKategori.vb + .Designer   ← 📂 CRUD Kategori
    ├── FormTransaksi.vb + .Designer  ← 💰 Transaksi Penjualan
    ├── FormCariPelanggan.vb          ← 🔍 Popup Cari Pelanggan
    ├── FormCariProduk.vb             ← 🔍 Popup Cari Produk
    ├── FormCariKategori.vb           ← 🔍 Popup Cari Kategori
    ├── My Project/ (7 files)
    ├── bin/Debug/
    └── obj/
```

## Mapping dari AplNilaiMhs

| Referensi | SCMFashion | Kontrol Khusus |
|---|---|---|
| FormMahasiswa | **FormPelanggan** | RadioButton, CheckBox, DateTimePicker |
| FormDosen | **FormProduk** | TextBox (4 field) |
| FormMataKuliah | **FormKategori** | TextBox (3 field) |
| FormNilai | **FormTransaksi** | ComboBox, Button Cari, Proses |
| FormCariMhs | **FormCariPelanggan** | ListView + Search |
| FormCariDosen | **FormCariProduk** | ListView + Search |
| FormCariMtk | **FormCariKategori** | ListView + Search |
| _(tidak ada)_ | **FormLogin** | Login baru dgn tbluser |

## Database: `db_scmfashion`

5 tabel + 1 view:
- `tbluser` - Login (admin/admin123, operator/operator123)
- `tblpelanggan` - 9 kolom (id, nama, tempat_lahir, tgl_lahir, usia, jenis_kelamin, membership, alamat, telp)
- `tblproduk` - 4 kolom (kd_produk, nm_produk, harga, stok)
- `tblkategori` - 3 kolom (kd_kategori, nm_kategori, deskripsi)
- `tbltransaksi` - 11 kolom (id_transaksi, tgl_transaksi, metode_bayar, id_pelanggan, kd_produk, kd_kategori, qty, harga_satuan, diskon, total_harga, status)
- `querytransaksi` - VIEW join 4 tabel

## Cara Menggunakan

### 1. Import Database
1. Buka **PHPMyAdmin** (http://localhost/phpmyadmin)
2. Klik **Import** → pilih file `db_scmfashion.sql`
3. Klik **Go** untuk eksekusi

### 2. Buka Project di Visual Studio
1. Buka file `SCMFashion.sln` di **Visual Studio 2010/2012+**
2. Pastikan **MySQL Connector .NET** sudah terinstal
3. Jika referensi MySql.Data error, klik kanan References → Add Reference → Browse → pilih `MySql.Data.dll`
4. Tekan **F5** untuk run

### 3. Login
- Username: `admin` | Password: `admin123`
- Username: `operator` | Password: `operator123`

### 4. Flow Aplikasi
```
Login → Menu Utama → Input Data (Pelanggan/Produk/Kategori)
                    → Proses Data (Transaksi Penjualan)
                    → Cari Data (Pelanggan/Produk/Kategori)
                    → Keluar
```

## Pola Code CRUD (Konsisten dengan AplNilaiMhs)
Setiap form mengikuti pola identik:
1. `posisilist()` → setup kolom ListView
2. `Isilist()` → load data dari DB ke ListView
3. `caridataXXX()` → pencarian realtime (TextChanged)
4. `AmbilDataDariListview()` → klik ListView mengisi form
5. `bersih()` → reset semua field
6. `btnSave` → INSERT INTO + validasi + MsgBox
7. `btnEdit` → UPDATE WHERE + validasi
8. `btnDelete` → DELETE FROM + konfirmasi OK/Cancel
9. `btnRefresh` → bersih() + Isilist()
10. `btnExit` → Me.Close()
