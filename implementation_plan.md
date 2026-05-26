# Perancangan Sistem Informasi Pengolahan Data Pelanggan - Toko Mode Fashion (SCMFashion)

## Deskripsi Project
Project UAS Pemrograman Visual: Sistem Informasi Pengolahan Data Pelanggan Berbasis Model WaterFall pada **Toko Mode Fashion**. Dibuat menggunakan VB.NET Framework 4 dengan database MySQL/PHPMyAdmin, mengikuti pola code dari project referensi `AplNilaiMhs`.

## Mapping Referensi: AplNilaiMhs → SCMFashion

| AplNilaiMhs (Referensi) | SCMFashion (Project Baru) | Keterangan |
|---|---|---|
| `tblmahasiswa` | `tblpelanggan` | Data master pelanggan (dengan RadioButton & CheckBox) |
| `tbldosen` | `tblproduk` | Data master produk fashion |
| `tblmatakuliah` | `tblkategori` | Data master kategori produk |
| `tblnilai` / `querynilai` | `tbltransaksi` / `querytransaksi` | Data transaksi penjualan (proses data) |
| `FormMahasiswa` | `FormPelanggan` | CRUD Pelanggan (lengkap dgn RadioButton, CheckBox, DatePicker) |
| `FormDosen` | `FormProduk` | CRUD Produk |
| `FormMataKuliah` | `FormKategori` | CRUD Kategori |
| `FormNilai` | `FormTransaksi` | Proses Transaksi (dgn ComboBox, Pencarian lookup) |
| `FormCariMhs` | `FormCariPelanggan` | Popup pencarian pelanggan |
| `FormCariDosen` | `FormCariProduk` | Popup pencarian produk |
| `FormCariMtk` | `FormCariKategori` | Popup pencarian kategori |
| `F_MenuUtama` | `F_MenuUtama` | Menu utama aplikasi |
| `BukaKoneksi` | `BukaKoneksi` | Module koneksi database |

## Desain Database MySQL (`db_scmfashion`)

### Tabel `tblpelanggan` (mapping dari tblmahasiswa)
| Kolom | Tipe Data | Keterangan |
|---|---|---|
| id_pelanggan | VARCHAR(10) | PK, kode pelanggan (PLG001) |
| nama | VARCHAR(100) | Nama pelanggan |
| tempat_lahir | VARCHAR(50) | Tempat lahir |
| tgl_lahir | DATE | Tanggal lahir |
| usia | VARCHAR(30) | Usia (auto-hitung) |
| jenis_kelamin | VARCHAR(15) | Laki-Laki / Perempuan (**RadioButton**) |
| membership | VARCHAR(20) | Gold/Silver/Bronze/Regular/VIP (**CheckBox**) |
| alamat | TEXT | Alamat |
| telp | VARCHAR(15) | Telepon/HP |

### Tabel `tblproduk` (mapping dari tbldosen)
| Kolom | Tipe Data | Keterangan |
|---|---|---|
| kd_produk | VARCHAR(10) | PK, kode produk (PRD001) |
| nm_produk | VARCHAR(100) | Nama produk |
| harga | VARCHAR(20) | Harga produk |
| stok | VARCHAR(10) | Jumlah stok |

### Tabel `tblkategori` (mapping dari tblmatakuliah)
| Kolom | Tipe Data | Keterangan |
|---|---|---|
| kd_kategori | VARCHAR(10) | PK, kode kategori (KAT001) |
| nm_kategori | VARCHAR(100) | Nama kategori (Baju, Celana, Aksesoris, dll) |
| deskripsi | VARCHAR(200) | Deskripsi kategori |

### Tabel `tbltransaksi` (mapping dari tblnilai)
| Kolom | Tipe Data | Keterangan |
|---|---|---|
| id_transaksi | INT AUTO_INCREMENT | PK |
| tgl_transaksi | VARCHAR(20) | Tanggal transaksi (**ComboBox** bulan/tahun) |
| metode_bayar | VARCHAR(20) | Cash/Transfer/E-Wallet (**ComboBox**) |
| id_pelanggan | VARCHAR(10) | FK ke tblpelanggan |
| kd_produk | VARCHAR(10) | FK ke tblproduk |
| kd_kategori | VARCHAR(10) | FK ke tblkategori |
| qty | INT | Jumlah beli |
| harga_satuan | DOUBLE | Harga satuan |
| diskon | DOUBLE | Diskon (%) |
| total_harga | DOUBLE | Total = (qty * harga) - diskon |
| status | VARCHAR(20) | Lunas/Belum Lunas |

### View `querytransaksi` (mapping dari querynilai)
```sql
CREATE VIEW querytransaksi AS
SELECT t.*, p.nama, p.telp, p.membership,
       pr.nm_produk, pr.harga AS harga_produk, pr.stok,
       k.nm_kategori, k.deskripsi
FROM tbltransaksi t
JOIN tblpelanggan p ON t.id_pelanggan = p.id_pelanggan
JOIN tblproduk pr ON t.kd_produk = pr.kd_produk
JOIN tblkategori k ON t.kd_kategori = k.kd_kategori;
```

## Struktur File Project SCMFashion

```
SCMFashion/
├── SCMFashion.sln                    ← Solution file
└── SCMFashion/
    ├── SCMFashion.vbproj             ← Project file
    ├── SCMFashion.vbproj.user        ← User settings
    ├── app.config                    ← Konfigurasi aplikasi
    ├── BukaKoneksi.vb                ← Module koneksi DB (db_scmfashion)
    │
    ├── F_MenuUtama.vb                ← Menu Utama (code-behind)
    ├── F_MenuUtama.Designer.vb       ← Menu Utama (designer)
    ├── F_MenuUtama.resx              ← Menu Utama (resource)
    │
    ├── FormPelanggan.vb              ← CRUD Pelanggan (code-behind)
    ├── FormPelanggan.Designer.vb     ← CRUD Pelanggan (designer)
    ├── FormPelanggan.resx            ← CRUD Pelanggan (resource)
    │
    ├── FormProduk.vb                 ← CRUD Produk (code-behind)
    ├── FormProduk.Designer.vb        ← CRUD Produk (designer)
    ├── FormProduk.resx               ← CRUD Produk (resource)
    │
    ├── FormKategori.vb               ← CRUD Kategori (code-behind)
    ├── FormKategori.Designer.vb      ← CRUD Kategori (designer)
    ├── FormKategori.resx             ← CRUD Kategori (resource)
    │
    ├── FormTransaksi.vb              ← Transaksi/Proses Data (code-behind)
    ├── FormTransaksi.Designer.vb     ← Transaksi (designer)
    ├── FormTransaksi.resx            ← Transaksi (resource)
    │
    ├── FormCariPelanggan.vb          ← Cari Pelanggan (code-behind)
    ├── FormCariPelanggan.Designer.vb ← Cari Pelanggan (designer)
    ├── FormCariPelanggan.resx        ← Cari Pelanggan (resource)
    │
    ├── FormCariProduk.vb             ← Cari Produk (code-behind)
    ├── FormCariProduk.Designer.vb    ← Cari Produk (designer)
    ├── FormCariProduk.resx           ← Cari Produk (resource)
    │
    ├── FormCariKategori.vb           ← Cari Kategori (code-behind)
    ├── FormCariKategori.Designer.vb  ← Cari Kategori (designer)
    ├── FormCariKategori.resx         ← Cari Kategori (resource)
    │
    ├── My Project/
    │   ├── Application.Designer.vb
    │   ├── Application.myapp
    │   ├── AssemblyInfo.vb
    │   ├── Resources.Designer.vb
    │   ├── Resources.resx
    │   ├── Settings.Designer.vb
    │   └── Settings.settings
    │
    ├── bin/
    │   └── Debug/                    ← (Folder output, dibuat saat compile)
    │
    └── obj/                          ← (Folder object, dibuat saat compile)
```

## Pola Code CRUD (Mengikuti AplNilaiMhs)

Setiap form CRUD akan mengikuti pola yang **sama persis** dengan referensi:

1. **`Imports MySql.Data.MySqlClient`** — di setiap form
2. **`posisilist()`** — setup kolom ListView
3. **`Isilist()`** — menampilkan data dari database ke ListView
4. **`caridataXXX()`** — pencarian data realtime
5. **`AmbilDataDariListview()`** — klik ListView mengisi textbox
6. **`bersih()`** — kosongkan textbox
7. **`btnSave_Click`** — INSERT INTO (validasi kosong + pesan sukses)
8. **`btnEdit_Click`** — UPDATE WHERE (validasi + pesan sukses)
9. **`btnDelete_Click`** — DELETE FROM (konfirmasi OK/Cancel)
10. **`btnRefresh_Click`** — panggil bersih() + Isilist()
11. **`btnExit_Click`** — Me.Close()
12. **Form_Load** — KoneksiKeDatabase() + posisilist() + Isilist()

### Fitur Tambahan di FormPelanggan (mapping dari FormMahasiswa)
- **RadioButton** untuk Jenis Kelamin (Laki-Laki / Perempuan)
- **CheckBox** untuk Membership (Gold, Silver, Bronze, Regular, VIP)
- **DateTimePicker** untuk Tanggal Lahir + perhitungan Usia otomatis

### Fitur Tambahan di FormTransaksi (mapping dari FormNilai)
- **ComboBox** untuk Tahun Transaksi dan Metode Bayar
- **Button "Cari"** untuk mencari Pelanggan, Produk, Kategori (popup form)
- **Button "Proses"** untuk menghitung Total Harga dan Status

## Proposed Changes

### 1. Database Setup
#### [NEW] SQL Script `db_scmfashion.sql`
- Buat database `db_scmfashion`
- Buat 4 tabel: `tblpelanggan`, `tblproduk`, `tblkategori`, `tbltransaksi`
- Buat VIEW `querytransaksi`
- Insert sample data

---

### 2. Solution & Project Files
#### [NEW] SCMFashion.sln
#### [NEW] SCMFashion/SCMFashion.vbproj
#### [NEW] SCMFashion/SCMFashion.vbproj.user
#### [NEW] SCMFashion/app.config

---

### 3. Module Koneksi
#### [NEW] SCMFashion/BukaKoneksi.vb
- Sama persis pola referensi, database = `db_scmfashion`

---

### 4. Form Menu Utama
#### [NEW] SCMFashion/F_MenuUtama.vb + .Designer.vb + .resx
- Menu: Input Data → Pelanggan, Produk, Kategori
- Menu: Proses Data → Transaksi Penjualan
- Menu: Cari Data → Pelanggan, Produk, Kategori
- Menu: Keluar

---

### 5. Form CRUD Pelanggan
#### [NEW] SCMFashion/FormPelanggan.vb + .Designer.vb + .resx
- CRUD lengkap dengan RadioButton (jenkel), CheckBox (membership), DateTimePicker

---

### 6. Form CRUD Produk
#### [NEW] SCMFashion/FormProduk.vb + .Designer.vb + .resx
- CRUD sederhana 4 field (kd_produk, nm_produk, harga, stok)

---

### 7. Form CRUD Kategori
#### [NEW] SCMFashion/FormKategori.vb + .Designer.vb + .resx
- CRUD sederhana 3 field (kd_kategori, nm_kategori, deskripsi)

---

### 8. Form Transaksi
#### [NEW] SCMFashion/FormTransaksi.vb + .Designer.vb + .resx
- ComboBox, proses perhitungan harga, lookup pencarian

---

### 9. Form Pencarian (3 form)
#### [NEW] SCMFashion/FormCariPelanggan.vb + .Designer.vb + .resx
#### [NEW] SCMFashion/FormCariProduk.vb + .Designer.vb + .resx
#### [NEW] SCMFashion/FormCariKategori.vb + .Designer.vb + .resx

---

### 10. My Project Folder
#### [NEW] My Project/Application.Designer.vb
#### [NEW] My Project/Application.myapp
#### [NEW] My Project/AssemblyInfo.vb
#### [NEW] My Project/Resources.Designer.vb
#### [NEW] My Project/Resources.resx
#### [NEW] My Project/Settings.Designer.vb
#### [NEW] My Project/Settings.settings

---

### 11. Folder bin & obj
- Dibuat sebagai folder kosong (diisi saat compile di Visual Studio)

## Open Questions

> [!IMPORTANT]
> 1. **Nama Database**: Saya gunakan `db_scmfashion` — apakah sudah sesuai keinginan?
> 2. **Laporan/Report**: Di AplNilaiMhs ada Crystal Report (LHS). Apakah ingin ditambahkan laporan juga, atau cukup CRUD + Transaksi saja?
> 3. **Login Form**: Apakah perlu ditambahkan form login sebelum masuk ke Menu Utama?

## Verification Plan

### Manual Verification
1. Buka `SCMFashion.sln` di Visual Studio
2. Import `db_scmfashion.sql` ke PHPMyAdmin
3. Build dan run project
4. Test semua operasi CRUD di setiap form
5. Test pencarian data
6. Test proses transaksi
