# Entity Relationships

## Overview

Dokumen ini menjelaskan hubungan antar entity, kardinalitas, foreign key, serta aturan integritas referensial pada Sistem Pengelolaan Data Penjualan.

---

## Relationship List

### 1. Roles - Users

- Cardinality: One to Many
- Description: Satu role dapat dimiliki banyak user. Satu user hanya memiliki satu role.
- Foreign Key: users.role_id -> roles.id
- On Delete: RESTRICT
- On Update: CASCADE
- Reason: Role tidak boleh dihapus jika masih digunakan user.

---

### 2. Customers - Sales

- Cardinality: One to Many
- Description: Satu customer dapat memiliki banyak transaksi. Satu transaksi hanya milik satu customer.
- Foreign Key: sales.customer_id -> customers.id
- On Delete: RESTRICT
- On Update: CASCADE
- Reason: Riwayat transaksi harus tetap terhubung dengan customer.

---

### 3. Users - Sales

- Cardinality: One to Many
- Description: Satu user/kasir dapat membuat banyak transaksi. Satu transaksi dibuat oleh satu user.
- Foreign Key: sales.user_id -> users.id
- On Delete: RESTRICT
- On Update: CASCADE
- Reason: Riwayat transaksi harus menyimpan pembuat transaksi.

---

### 4. Categories - Products

- Cardinality: One to Many
- Description: Satu kategori memiliki banyak produk. Satu produk hanya memiliki satu kategori.
- Foreign Key: products.category_id -> categories.id
- On Delete: RESTRICT
- On Update: CASCADE
- Reason: Produk harus tetap memiliki kategori valid.

---

### 5. Suppliers - Products

- Cardinality: One to Many
- Description: Satu supplier memasok banyak produk. Satu produk memiliki satu supplier.
- Foreign Key: products.supplier_id -> suppliers.id
- On Delete: RESTRICT
- On Update: CASCADE
- Reason: Produk harus tetap memiliki supplier valid.

---

### 6. Sales - Sale Details

- Cardinality: One to Many
- Description: Satu transaksi memiliki banyak detail. Satu detail hanya milik satu transaksi.
- Foreign Key: sale_details.sale_id -> sales.id
- On Delete: CASCADE
- On Update: CASCADE
- Reason: Jika transaksi draft dibatalkan/dihapus, detail ikut terhapus.

---

### 7. Products - Sale Details

- Cardinality: One to Many
- Description: Satu produk dapat muncul di banyak detail transaksi. Satu detail hanya mengacu ke satu produk.
- Foreign Key: sale_details.product_id -> products.id
- On Delete: RESTRICT
- On Update: CASCADE
- Reason: Produk yang sudah digunakan dalam transaksi tidak boleh dihapus.

---

### 8. Sales - Payments

- Cardinality: One to One
- Description: Satu transaksi memiliki maksimal satu pembayaran. Satu pembayaran hanya milik satu transaksi.
- Foreign Key: payments.sale_id -> sales.id
- Constraint: UNIQUE(payments.sale_id)
- On Delete: RESTRICT
- On Update: CASCADE
- Reason: Pembayaran merupakan bukti transaksi dan tidak boleh hilang dari riwayat.

---

### 9. Sales - Documents

- Cardinality: One to Many
- Description: Satu transaksi dapat memiliki banyak dokumen. Satu dokumen terkait ke satu transaksi.
- Foreign Key: documents.sale_id -> sales.id
- On Delete: RESTRICT
- On Update: CASCADE
- Reason: Dokumen transaksi harus tetap terhubung dengan transaksi.

---

### 10. Users - Documents

- Cardinality: One to Many
- Description: Satu user dapat membuat banyak dokumen. Satu dokumen dibuat oleh satu user.
- Foreign Key: documents.created_by -> users.id
- On Delete: RESTRICT
- On Update: CASCADE
- Reason: Metadata dokumen harus menyimpan pembuat dokumen.

---

### 11. Documents - Document Versions

- Cardinality: One to Many
- Description: Satu dokumen dapat memiliki banyak versi. Satu versi hanya milik satu dokumen.
- Foreign Key: document_versions.document_id -> documents.id
- Constraint: UNIQUE(document_id, version_no)
- On Delete: RESTRICT
- On Update: CASCADE
- Reason: Riwayat versi dokumen harus tetap tersimpan sebagai bukti.

---

### 12. Users - Document Versions

- Cardinality: One to Many
- Description: Satu user dapat mengunggah banyak versi dokumen. Satu versi diunggah oleh satu user.
- Foreign Key: document_versions.uploaded_by -> users.id
- On Delete: RESTRICT
- On Update: CASCADE
- Reason: Riwayat versi harus menyimpan user pengunggah.

---

### 13. Roles - Document Role Permissions

- Cardinality: One to One
- Description: Satu role memiliki satu konfigurasi permission dokumen.
- Foreign Key: document_role_permissions.role_id -> roles.id
- Constraint: UNIQUE(role_id)
- On Delete: CASCADE
- On Update: CASCADE
- Reason: Hak akses dokumen mengikuti role.

---

### 14. Users - Integration Logs

- Cardinality: One to Many
- Description: Satu user dapat melakukan banyak proses import. Satu log import dibuat oleh satu user.
- Foreign Key: integration_logs.imported_by -> users.id
- On Delete: RESTRICT
- On Update: CASCADE
- Reason: Setiap proses import harus memiliki penanggung jawab.

---

### 15. Integration Logs - Integration Log Details

- Cardinality: One to Many
- Description: Satu proses import memiliki banyak detail baris. Satu detail hanya milik satu proses import.
- Foreign Key: integration_log_details.integration_log_id -> integration_logs.id
- On Delete: CASCADE
- On Update: CASCADE
- Reason: Detail log tidak berdiri sendiri tanpa log utama.

---

### 16. Users - Data Quality Logs

- Cardinality: One to Many
- Description: Satu user dapat melakukan banyak koreksi kualitas data. Satu log koreksi dilakukan oleh satu user.
- Foreign Key: data_quality_logs.corrected_by -> users.id
- On Delete: SET NULL
- On Update: CASCADE
- Reason: Log kualitas data tetap disimpan walaupun user sudah tidak aktif.

---

## Relationship Summary

| No | Parent | Child | Cardinality | Foreign Key |
|----|--------|-------|-------------|-------------|
| 1 | roles | users | 1:N | users.role_id |
| 2 | customers | sales | 1:N | sales.customer_id |
| 3 | users | sales | 1:N | sales.user_id |
| 4 | categories | products | 1:N | products.category_id |
| 5 | suppliers | products | 1:N | products.supplier_id |
| 6 | sales | sale_details | 1:N | sale_details.sale_id |
| 7 | products | sale_details | 1:N | sale_details.product_id |
| 8 | sales | payments | 1:1 | payments.sale_id |
| 9 | sales | documents | 1:N | documents.sale_id |
| 10 | users | documents | 1:N | documents.created_by |
| 11 | documents | document_versions | 1:N | document_versions.document_id |
| 12 | users | document_versions | 1:N | document_versions.uploaded_by |
| 13 | roles | document_role_permissions | 1:1 | document_role_permissions.role_id |
| 14 | users | integration_logs | 1:N | integration_logs.imported_by |
| 15 | integration_logs | integration_log_details | 1:N | integration_log_details.integration_log_id |
| 16 | users | data_quality_logs | 1:N | data_quality_logs.corrected_by |