# Business Rules

## Overview

Dokumen ini berisi aturan bisnis yang menjadi dasar dalam perancangan basis data Sistem Pengelolaan Data Penjualan. Seluruh entitas, relasi, constraint, serta proses bisnis harus mengikuti aturan-aturan yang dijelaskan pada dokumen ini.

---

# User Management

## BR-001

Setiap pengguna harus memiliki satu role.

Reason

Hak akses setiap pengguna berbeda sesuai tanggung jawabnya.

---

## BR-002

Email pengguna harus unik.

Reason

Digunakan sebagai identitas login.

---

## BR-003

Password harus disimpan dalam bentuk hash.

Reason

Menjaga keamanan data pengguna.

---

# Customer

## BR-004

Satu customer dapat memiliki banyak transaksi penjualan.

Reason

Customer dapat melakukan pembelian lebih dari satu kali.

---

# Category

## BR-005

Satu kategori dapat memiliki banyak produk.

Reason

Produk dikelompokkan berdasarkan kategori.

---

# Supplier

## BR-006

Satu supplier dapat memasok banyak produk.

Reason

Supplier merupakan sumber pengadaan barang.

---

# Product

## BR-007

Setiap produk harus memiliki satu kategori.

---

## BR-008

Setiap produk harus memiliki satu supplier.

---

## BR-009

Kode produk harus unik.

---

## BR-010

Harga produk tidak boleh negatif.

---

## BR-011

Stok produk tidak boleh negatif.

---

## BR-012

Produk tidak boleh dihapus apabila masih digunakan dalam transaksi penjualan.

Reason

Menjaga integritas data historis.

---

# Sales

## BR-013

Satu transaksi penjualan hanya dimiliki oleh satu customer.

---

## BR-014

Satu transaksi penjualan harus memiliki minimal satu detail transaksi.

---

## BR-015

Nomor transaksi harus unik.

---

## BR-016

Total transaksi dihitung dari seluruh detail penjualan.

---

# Sale Details

## BR-017

Satu detail transaksi hanya mengacu pada satu produk.

---

## BR-018

Jumlah pembelian minimal satu.

---

## BR-019

Harga jual disimpan pada saat transaksi.

Reason

Harga produk dapat berubah sewaktu-waktu.

---

# Payment

## BR-020

Setiap transaksi hanya memiliki satu pembayaran.

---

## BR-021

Jumlah pembayaran harus sama dengan total transaksi.

---

## BR-022

Pembayaran tidak dapat dilakukan apabila transaksi belum memiliki detail.

---

# Inventory

## BR-023

Stok otomatis berkurang ketika transaksi berhasil.

---

## BR-024

Transaksi tidak boleh diproses apabila stok tidak mencukupi.

---

## BR-025

Rollback dilakukan apabila terjadi kegagalan pada proses transaksi.

---

# Data Integration

## BR-026

Import hanya menerima file CSV.

---

## BR-027

Produk dengan kode yang sama tidak boleh dibuat dua kali.

---

## BR-028

Seluruh proses import dicatat pada integration_logs.

---

# Data Quality

## BR-029

Nama produk tidak boleh kosong.

---

## BR-030

Harga tidak boleh negatif.

---

## BR-031

Stok tidak boleh negatif.

---

## BR-032

Data yang tidak valid dicatat pada data_quality_logs.

---

# Document Management

## BR-033

Dokumen hanya boleh bertipe PDF, DOCX, atau TXT.

---

## BR-034

Setiap dokumen memiliki versi.

---

## BR-035

Riwayat perubahan dokumen harus disimpan.

---

## BR-036

Hak akses dokumen mengikuti role pengguna.

---

# Security

## BR-037

Pengguna harus login sebelum mengakses sistem.

---

## BR-038

Role menentukan hak akses pengguna.

---

## BR-039

Session yang tidak valid harus ditolak.

---

# Audit

## BR-040

Seluruh transaksi penting harus memiliki timestamp.

---

## BR-041

Data created_at dan updated_at harus dicatat secara otomatis.