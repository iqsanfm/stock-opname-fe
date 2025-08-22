# SKU Feature Testing Guide

## Overview
Fitur SKU telah diimplementasi untuk memudahkan identifikasi dan tracking item inventory. Setiap item memiliki SKU (Stock Keeping Unit) yang unik.

## Features Implemented

### 1. SKU Input Form
- **Location**: Form "Input Transaksi Harian"
- **Field**: SKU (required field)
- **Format**: Uppercase (otomatis)
- **Validation**: Harus minimal 2 karakter

### 2. Auto-Suggest SKU
- **Trigger**: Ketika user mengetik di field SKU
- **Functionality**: 
  - Dropdown suggestions muncul
  - Format: `SKU - Nama Barang`
  - Auto-fill form fields ketika SKU dipilih

### 3. Auto-Fill dari SKU
- **Behavior**: Ketika SKU valid dipilih/diketik
- **Auto-filled fields**:
  - Nama Barang
  - Jenis/Kategori
  - Merk
  - Harga (untuk transaksi "masuk")

### 4. SKU Validation
- **Unique per item**: SKU sama hanya boleh untuk item yang sama
- **Duplicate detection**: Error jika SKU digunakan untuk item berbeda
- **Case insensitive**: PP001 = pp001 = Pp001

### 5. Display Updates
- **Daily Transactions Table**: Kolom SKU ditambahkan
- **Monthly Report Table**: Kolom SKU ditambahkan  
- **Stock Opname Table**: Kolom SKU ditambahkan

### 6. Export/Import Support
- **CSV Export**: Semua export include kolom SKU
- **CSV Import**: Support kolom SKU (optional untuk backward compatibility)
- **Sample Files**: Semua sample CSV sudah include SKU

## Testing Scenarios

### Test 1: Basic SKU Input
1. Buka form "Input Transaksi Harian"
2. Isi SKU: `PP001`
3. Isi fields lainnya
4. Submit form
5. **Expected**: Transaksi tersimpan dengan SKU

### Test 2: Auto-Suggest
1. Mulai ketik SKU yang sudah ada (misal: `PP`)
2. **Expected**: Dropdown muncul dengan suggestions
3. Pilih dari dropdown
4. **Expected**: Form auto-fill dengan data item

### Test 3: Auto-Fill
1. Ketik SKU lengkap yang sudah ada: `PP001`
2. Wait 800ms atau tab ke field lain
3. **Expected**: 
   - Nama Barang auto-fill
   - Jenis auto-fill
   - Merk auto-fill
   - Alert hijau: "Item ditemukan"

### Test 4: SKU Validation
1. Input SKU `PP001` untuk item "Pulpen Gel"
2. Submit (should success)
3. Input SKU `PP001` lagi untuk item "Pensil" (berbeda)
4. **Expected**: Error "SKU sudah digunakan untuk item berbeda"

### Test 5: Repeated Transactions
1. Input SKU `PP001` untuk "Pulpen Gel" - stock_awal
2. Input SKU `PP001` untuk "Pulpen Gel" - masuk
3. Input SKU `PP001` untuk "Pulpen Gel" - keluar
4. **Expected**: Semua berhasil (same item, different transactions)

### Test 6: CSV Import
1. Import `sample_import.csv` (yang sudah include SKU)
2. **Expected**: Data ter-import dengan SKU
3. Check table untuk verify SKU columns

### Test 7: Export Functionality
1. Export "Transaksi Harian"
2. Open CSV file
3. **Expected**: File include kolom SKU dengan data

## Sample Data for Testing

### Sample SKUs in CSV files:
- `LT001` - Laptop Dell Inspiron 15
- `AT001` - Kertas A4 80gsm  
- `FN001` - Meja Kantor Executive
- `MS001` - Mouse Wireless
- `PP001` - Pulpen Gel 0.5mm
- `SM001` - Smartphone Samsung Galaxy A54

### Test Cases:
```
Valid SKU inputs:
- LP001, AT001, FN001 (new items)
- PP001 (existing item, same product)

Invalid SKU inputs:
- P (too short)
- PP001 (if used for different product)
```

## Known Issues & Limitations

### Fixed Issues:
- ✅ Alert duplicate pada auto-fill
- ✅ SKU validation terlalu strict
- ✅ Sample CSV tidak include SKU

### Current Limitations:
- SKU format bebas (no enforced pattern)
- Manual SKU entry (no auto-generation)
- No bulk SKU update

## Backend Compatibility

### API Endpoints:
- `GET /api/items/sku/:sku` - Get item by SKU
- `GET /api/items/suggestions?q=query` - SKU autocomplete
- All existing endpoints support SKU field

### Database:
- Item model includes unique SKU field
- Transactions reference items by ID (not SKU)
- SKU stored in uppercase format

## Performance Notes

- Auto-fill has 800ms delay to prevent spam
- Suggestions limited to 20 results
- Local storage caching for repeated sessions
- Backward compatible with non-SKU data

---

**Testing Status**: ✅ Ready for testing
**Last Updated**: August 2025
**Branch**: feature/sku-support