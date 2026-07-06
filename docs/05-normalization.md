# Normalization

## Overview

Dokumen ini menjelaskan proses normalisasi basis data Sistem Pengelolaan Data Penjualan sampai Third Normal Form (3NF).

---

## Unnormalized Form (UNF)

Pada sistem lama, data penjualan masih dapat tercatat dalam satu dokumen/file dengan struktur campuran.

Contoh bentuk tidak normal:

```text
TRX-001, Budi, 08123, Produk: [Kopi, Gula], Qty: [2, 1], Harga: [10000, 15000], Payment: CASH
```

Contoh data setelah dipecah menjadi baris:

| transaction_no | customer_name | customer_phone | product_code | product_name | category_name | supplier_name | qty | unit_price | subtotal | payment_method |
|----------------|---------------|----------------|--------------|--------------|---------------|---------------|-----|------------|----------|----------------|
| TRX-001 | Budi | 08123 | PRD-001 | Kopi | Minuman | Supplier A | 2 | 10000 | 20000 | CASH |
| TRX-001 | Budi | 08123 | PRD-002 | Gula | Sembako | Supplier B | 1 | 15000 | 15000 | CASH |

Masalah:
- Data customer berulang.
- Data produk berulang.
- Data kategori dan supplier berulang.
- Produk dan qty dapat berbentuk repeating group.
- Perubahan data dapat menyebabkan inkonsistensi.

---

## First Normal Form (1NF)

Syarat 1NF:
- Setiap kolom bernilai atomik.
- Tidak ada repeating group dalam satu kolom.
- Setiap baris memiliki identifier.

Penerapan:
- Repeating group produk, qty, dan harga dipisah menjadi baris detail transaksi.
- Setiap tabel memiliki primary key.
- Kolom seperti name, phone, price, dan qty disimpan sebagai nilai tunggal.

Tabel hasil 1NF:
- sales
- sale_details
- products
- customers

---

## Second Normal Form (2NF)

Syarat 2NF:
- Sudah memenuhi 1NF.
- Semua atribut non-key bergantung penuh pada primary key.
- Tidak ada partial dependency.

Penerapan:
- Data customer dipisah ke tabel customers.
- Data produk dipisah ke tabel products.
- Data kategori dipisah ke tabel categories.
- Data supplier dipisah ke tabel suppliers.
- Header transaksi dipisah ke sales.
- Item transaksi dipisah ke sale_details.

Penjelasan:
Pada rancangan transaksi awal, detail penjualan dapat dianggap bergantung pada kombinasi transaction_no dan product_code. Atribut seperti product_name, category_name, dan supplier_name tidak disimpan pada sale_details karena atribut tersebut bergantung pada product_code, bukan pada keseluruhan transaksi.

Contoh:
- customer_name tidak disimpan di sales, tetapi di customers.
- product_name tidak disimpan di sale_details, tetapi di products.
- sale_details hanya menyimpan product_id, qty, unit_price, subtotal.

---

## Third Normal Form (3NF)

Syarat 3NF:
- Sudah memenuhi 2NF.
- Tidak ada transitive dependency.
- Atribut non-key tidak bergantung pada atribut non-key lain.

Penerapan:
- role_name tidak disimpan di users, tetapi di roles.
- customer_phone tidak disimpan di sales, tetapi di customers.
- category_name tidak disimpan di products, tetapi di categories.
- supplier_name tidak disimpan di products, tetapi di suppliers.
- payment_method tidak disimpan di sales, tetapi di payments.
- document version dipisah ke document_versions.
- import detail dipisah ke integration_log_details.

Contoh:
- users.role_id -> roles.id, bukan users.role_name.
- products.category_id -> categories.id, bukan products.category_name.
- products.supplier_id -> suppliers.id, bukan products.supplier_name.

---

## Derived Data Handling

Beberapa data merupakan hasil perhitungan:

| Field | Source | Handling |
|-------|--------|----------|
| sale_details.subtotal | qty * unit_price | Derived/generated value |
| sales.grand_total | SUM(sale_details.subtotal) | Maintained by procedure/trigger |
| integration_logs.success_count | Count detail status success | Summary for reporting |
| integration_logs.failed_count | Count detail status failed | Summary for reporting |

Alasan penyimpanan:
- Mempercepat laporan.
- Menyimpan snapshot transaksi.
- Nilai dijaga oleh procedure atau trigger agar tetap konsisten.

---

## Normalization Result

| Problem in Old Data | Normalized Solution |
|---------------------|---------------------|
| Customer data repeated in transactions | customers table |
| Product data repeated in transactions | products table |
| Category name repeated in products | categories table |
| Supplier name repeated in products | suppliers table |
| Multiple products in one transaction | sale_details table |
| Payment data mixed with sales | payments table |
| Document files overwritten | document_versions table |
| CSV import result mixed with product data | integration_logs and integration_log_details |
| Data quality issue not tracked | data_quality_logs table |

---

## Final Tables in 3NF

- roles
- users
- customers
- categories
- suppliers
- products
- sales
- sale_details
- payments
- documents
- document_versions
- document_role_permissions
- integration_mappings
- integration_logs
- integration_log_details
- data_quality_logs

---

## Conclusion

Struktur basis data telah dinormalisasi sampai 3NF dengan memisahkan data master, transaksi, pembayaran, dokumen, integrasi, dan log kualitas data. Data turunan tetap digunakan secara terbatas untuk kebutuhan snapshot dan performa laporan, dengan pengendalian melalui procedure atau trigger.
