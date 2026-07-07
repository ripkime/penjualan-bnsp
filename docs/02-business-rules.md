# Business Rules

## Overview

Dokumen ini berisi aturan bisnis yang menjadi dasar dalam perancangan basis data Sistem Pengelolaan Data Penjualan. Seluruh entitas, relasi, constraint, serta proses bisnis harus mengikuti aturan-aturan yang dijelaskan pada dokumen ini.

---

# User Management

## BR-001

Setiap pengguna harus memiliki satu role.

---

## BR-002

Email pengguna harus unik.

---

## BR-003

Password harus disimpan dalam bentuk hash.

---

# Customer

## BR-004

Satu customer dapat memiliki banyak transaksi penjualan.

---

## BR-005

Nomor telepon customer harus unik apabila tidak kosong.

---

# Category

## BR-006

Satu kategori dapat memiliki banyak produk.

---

# Supplier

## BR-007

Satu supplier dapat memasok banyak produk.

---

# Product

## BR-008

Setiap produk harus memiliki satu kategori.

---

## BR-009

Setiap produk harus memiliki satu supplier.

---

## BR-010

Kode produk harus unik.

---

## BR-011

Harga produk tidak boleh negatif.

---

## BR-012

Stok produk tidak boleh negatif.

---

## BR-013

Produk tidak boleh dihapus apabila masih digunakan dalam transaksi penjualan.

---

# Sales

## BR-014

Satu transaksi penjualan hanya dimiliki oleh satu customer.

---

## BR-015

Transaksi penjualan dengan status PAID harus memiliki minimal satu detail transaksi.

---

## BR-016

Nomor transaksi harus unik.

---

## BR-017

Total transaksi dihitung dari seluruh detail penjualan.

---

## BR-018

Status penjualan terdiri dari DRAFT, PAID, CANCELLED.

---

## BR-019

Stok hanya berkurang ketika transaksi dikonfirmasi/PAID.

---

# Sale Details

## BR-020

Satu detail transaksi hanya mengacu pada satu produk.

---

## BR-021

Jumlah pembelian minimal satu.

---

## BR-022

Harga jual disimpan pada saat transaksi.

---

# Payment

## BR-023

Setiap transaksi memiliki maksimal satu pembayaran.

---

## BR-024

Jumlah pembayaran harus sama dengan total transaksi.

---

## BR-025

Pembayaran tidak dapat dilakukan apabila transaksi belum memiliki detail.

---

# Inventory

## BR-026

Stok otomatis berkurang ketika transaksi dikonfirmasi/PAID dalam database transaction.

---

## BR-027

Transaksi tidak boleh diproses apabila stok tidak mencukupi.

---

## BR-028

Rollback dilakukan apabila terjadi kegagalan pada proses transaksi.

---

# Data Integration

## BR-029

Import hanya menerima file CSV.

---

## BR-030

Produk dengan kode yang sama tidak boleh dibuat dua kali.

---

## BR-031

Seluruh proses import dicatat pada integration_logs.

---

## BR-032

Produk CSV dengan kode yang sudah ada akan ditolak dan dicatat sebagai gagal.

---

# Data Quality

## BR-033

Nama produk tidak boleh kosong.

---

## BR-034

Harga tidak boleh negatif.

---

## BR-035

Stok tidak boleh negatif.

---

## BR-036

Data yang tidak valid dicatat pada data_quality_logs.

---

## BR-037

Setiap masalah kualitas data memiliki root cause.

---

## BR-038

Setiap masalah kualitas data memiliki priority: LOW, MEDIUM, HIGH.

---

## BR-039

Setiap koreksi data harus dicatat pada data_quality_logs.

---

# Document Management

## BR-040

Dokumen hanya boleh bertipe PDF, DOCX, atau TXT.

---

## BR-041

Setiap dokumen memiliki versi.

---

## BR-042

Riwayat perubahan dokumen harus disimpan.

---

## BR-043

Hak akses dokumen mengikuti role pengguna.

---

## BR-044

Upload dokumen dengan title dan transaksi yang sama membuat versi baru pada document_versions.

---

## BR-045

Versi dokumen tidak boleh berkurang.

---

# Security

## BR-046

Pengguna harus login sebelum mengakses sistem.

---

## BR-047

Role menentukan hak akses pengguna.

---

## BR-048

Session yang tidak valid harus ditolak.

---

# Audit

## BR-049

Seluruh transaksi penting harus memiliki timestamp.

---

## BR-050

Data created_at dan updated_at harus dicatat secara otomatis.

---

## BR-051

Saat transaksi dikonfirmasi, data produk yang dibeli harus dikunci menggunakan row-level locking atau SELECT FOR UPDATE.

---

## BR-052

Produk yang sudah pernah digunakan dalam transaksi tidak dihapus secara fisik, tetapi dinonaktifkan menggunakan is_active.

