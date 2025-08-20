# Panduan Import CSV - Stock Opname System

## Format CSV yang Diperlukan

File CSV harus memiliki header (baris pertama) dengan kolom-kolom berikut:

```csv
tanggal,sparepart,jenis,merk,tipe_transaksi,jumlah,harga,keterangan
```

### Penjelasan Kolom:

1. **tanggal** - Format: YYYY-MM-DD (contoh: 2024-08-20)
2. **sparepart** - Nama barang (minimal 2 karakter)
3. **jenis** - Jenis/kategori barang (Elektronik, Alat Tulis, Furniture, dll)
4. **merk** - Merk/brand barang (minimal 1 karakter)
5. **tipe_transaksi** - Harus salah satu dari: `stock_awal`, `masuk`, atau `keluar`
6. **jumlah** - Jumlah barang (harus angka positif)
7. **harga** - Harga per unit (untuk tipe `keluar` boleh 0)
8. **keterangan** - Keterangan tambahan (optional)

## Contoh File CSV:

### 1. Sample Basic (sample_import.csv)
File sample dasar dengan berbagai jenis transaksi:
- Stock awal untuk berbagai barang kantor
- Transaksi masuk (pembelian)
- Transaksi keluar (distribusi/penjualan)

### 2. Sample Extended (sample_import_extended.csv)
File sample dengan barang premium:
- Elektronik canggih, furniture mewah
- Produk teknologi tinggi
- Barang berkualitas premium

### 3. Sample Error (sample_import_error.csv)
File sample dengan berbagai error untuk testing validasi:
- Tanggal invalid
- Field yang kosong
- Tipe transaksi salah
- Nilai negatif

## Cara Menggunakan:

1. Buka aplikasi Stock Opname
2. Pergi ke tab "Daily Transactions"
3. Klik tombol **"Import CSV"**
4. Pilih file CSV yang sudah disiapkan
5. Sistem akan memvalidasi data
6. Jika valid, konfirmasi untuk mengimport
7. Data akan ditambahkan ke transaksi harian

## Validasi Yang Dilakukan:

- ✅ Format tanggal (YYYY-MM-DD)
- ✅ Kelengkapan field wajib
- ✅ Panjang minimal nama spare part (2 karakter)
- ✅ Validitas tipe transaksi
- ✅ Nilai jumlah harus positif
- ✅ Nilai harga untuk transaksi masuk/stock_awal
- ✅ Format CSV yang benar

## Tips:

1. **Encoding**: Simpan file CSV dalam encoding UTF-8
2. **Separator**: Gunakan koma (,) sebagai pemisah
3. **Quotes**: Gunakan tanda kutip (") jika data mengandung koma
4. **Backup**: Selalu backup data sebelum import besar
5. **Test**: Gunakan sample kecil dulu untuk testing

## Error yang Mungkin Terjadi:

- **"Header yang hilang"** - Pastikan semua kolom ada
- **"Format tanggal harus YYYY-MM-DD"** - Periksa format tanggal
- **"Tipe transaksi harus salah satu dari..."** - Gunakan nilai yang valid
- **"Jumlah harus berupa angka positif"** - Periksa nilai jumlah

## File Sample Tersedia:

- `sample_import.csv` - Sample dasar untuk pembelajaran (Agustus 2024)
- `sample_import_extended.csv` - Sample advanced dengan part premium (Agustus 2024)
- `sample_today.csv` - Sample untuk hari ini (20 Agustus 2024) - untuk test statistik
- `sample_test_quotes.csv` - Sample dengan quotes dan koma dalam data
- `sample_import_error.csv` - Sample error untuk testing validasi

## Update Statistik Setelah Import:

Setelah import CSV berhasil, statistik akan otomatis terupdate:
- **Transaksi Hari Ini**: Menampilkan jumlah transaksi tanggal hari ini saja
- **Total Barang Masuk**: Total semua barang masuk dari seluruh data (stock_awal + masuk)
- **Total Barang Keluar**: Total semua barang keluar dari seluruh data
- **Nilai Transaksi**: Total nilai dari semua transaksi (menggunakan field total)

## Catatan Harga:

- Untuk transaksi **keluar**: Harga boleh 0 atau sama dengan harga beli
- Untuk transaksi **masuk/stock_awal**: Harga harus > 0
- Sistem akan menghitung total otomatis (jumlah × harga)