# Requirement Analysis

# Project Information

| Item | Description |
|------|-------------|
| Project | Sistem Pengelolaan Data Penjualan |
| Purpose | Membangun sistem basis data untuk mengelola proses penjualan toko secara terintegrasi. |
| Database | PostgreSQL |
| Programming Language | Go |
| Database Driver | pgx |
| Version | 1.0.0 |

---

## Background 

Saat ini proses pengelolaan data pada toko masih dilakukan menggunakan file terpisah dan pencatatan manual. Kondisi tersebut menyebabkan berbagai permasalahan seperti duplikasi data, inkonsistensi stok, kesulitan dalam pelacakan transaksi, proses pembuatan laporan yang lambat, serta belum adanya pengaturan hak akses pengguna. 

Oleh karena itu diperlukan sebuah sistem basis data yang mampu mengintegrasikan seluruh data penjualan agar proses bisnis menjadi lebih cepat, akurat, aman, dan mudah dipelihara.

---

## Problems

Permasalahan yang ditemukan pada sistem lama antara lain:
1. Data pelanggan masih dapat tercatat lebih dari satu kali.
2. Data produk dapat mengalami duplikasi.
3. Stok barang sering tidak sesuai dengan transaksi.
4. Riwayat transaksi sulit ditelusuri.
5. Laporan penjualan membutuhkan waktu yang lama.
6. Data produk lama belum terintegrasi.
7. Masih terdapat data kosong, tidak valid atau tidak konsisten.
8. Dokumen penjualan belum memiliki pengelolaan yang baik.
9. Seluruh pengguna memiliki hak akses yang sama.
10. Belum tersedia sistem yang mudah dipasang dan digunakan.

---

## Project Objectives

Sistem yang akan dibangun bertujuan untuk:

- Mengelola data pelanggan.
- Mengelola data kategori produk.
- Mengelola data pemasok.
- Mengelola data produk.
- Mengelola transaksi penjualan.
- Mengelola pembayaran.
- Mengelola stok produk.
- Mengintegrasikan data produk dari file CSV.
- Melakukan pemeriksaan kualitas data.
- Mengelola dokumen penjualan.
- Menerapkan hak akses pengguna.
- Menyediakan instalasi database yang mudah.
  
---

## Stakeholders

### Admin

Hak Akses:

- Mengelola seluruh data master.
- Mengelola pengguna.
- Mengelola dokumen.
- Mengelola integrasi data.
- Mengelola kualitas data.
- Melihat seluruh laporan.

---

### Kasir

Hak Akses:

- Mengelola transaksi penjualan.
- Mengelola pembayaran.
- Melihat data produk.
- Melihat data pelanggan.
- Mengunggah dokumen transaksi.

---

### Owner

Hak Akses:

- Melihat dashboard.
- Melihat laporan penjualan.
- Melihat laporan stok.
- Melihat laporan kualitas data.

---

## Functional Requirements

Sistem harus mampu menyediakan fitur berikut:

### Master Data

- CRUD Customer.
- CRUD Category.
- CRUD Supplier.
- CRUD Product.
- CRUD User.

### Transaction

- Membuat transaksi penjualan.
- Menambahkan detail transaksi.
- Melakukan pembayaran.
- Mengurangi stok secara otomatis.

## Data Integration

- Import produk dari file CSV.
- Validasi data CSV.
- Menyimpan log integrasi.

## Data Quality

- Pemeriksaan kualitas data.
- Deteksi data tidak valid.
- Koreksi data.
- Penyimpanan log kualitas data.

## Document Management

- Upload dokumen.
- Menyimpan metadata dokumen.
- Versioning dokumen.
- Pengaturan hak akses dokumen.

## Reporting

- Laporan penjualan.
- Laporan stok.
- Laporan pelanggan.
- Laporan produk.
- Laporan integrasi.
- Laporan kualitas data.

---

## Non functional requirements

### Performance 

- Query harus berjalan secara efisien.
- Menggunakan indeks pada kolom yang sering digunakan.

### Security

- Login menggunakan autentikasi.
- Password disimpan dalam bentuk hash.
- Pengguna hanya dapat mengakses fitur sesuai role.

### Reliability

- Menggunakan transaksi database (COMMIT and ROLLBACK).
- Menjaga konsistensi data menggunakan constraint.

### Maintainability
- Struktur database terdokumentasi.
- Menggunakan foreign key.
- Menggunakan normalisasi minimal sampai Third Normal Form (3NF).

---

## Business Proccess Overview

Alur proses bisnis utama:

1. Admin mengelola data master.
2. Kasir melakukan transaksi penjualan.
3. Sistem menyimpan transaksi.
4. Sistem mengurangi stok produk secara otomatis.
5. Kasir melakukan pembayaran.
6. Sistem menyimpan dokumen transaksi.
7. Owner melihat laporan.

---

## Expected Outputs

Sistem yang dibangun diharapkan menghasilkan:

- Basis data yang terstruktur.
- Data yang konsisten.
- Laporan penjualan yang cepat.
- Pengelolaan stok yang akurat.
- Integrasi data yang mudah.
- Manajemen dokumen yang terorganisir.
- Hak akses pengguna yang aman.

---

## Technology Stack

| Component | Technology |
|-----------|------------|
| Language | Go |
| Database | PostgreSQL |
| Driver | pgx |
| SQL | PostgreSQL SQL |
| Authentication | Session Based Authentication |
| CSV Import | encoding/csv |
| File Storage | Local Storage |




