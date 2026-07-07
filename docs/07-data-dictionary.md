# Data Dictionary

## Overview

Dokumen ini menjelaskan struktur tabel, kolom, tipe data, key, constraint, dan deskripsi pada database Sistem Pengelolaan Data Penjualan.

---

## roles

| Column     | Type        | Key    | Nullable | Default        | Description                    |
| ---------- | ----------- | ------ | -------- | -------------- | ------------------------------ |
| id         | BIGSERIAL   | PK     | No       | auto increment | Role ID                        |
| name       | VARCHAR(50) | UNIQUE | No       | -              | Nama role: Admin, Kasir, Owner |
| created_at | TIMESTAMPTZ | -      | No       | now()          | Waktu data dibuat              |
| updated_at | TIMESTAMPTZ | -      | No       | now()          | Waktu data diperbarui          |

Constraints:
- `roles.name` unique

---

## users

| Column        | Type         | Key    | Nullable | Default        | Description                 |
| ------------- | ------------ | ------ | -------- | -------------- | --------------------------- |
| id            | BIGSERIAL    | PK     | No       | auto increment | User ID                     |
| name          | VARCHAR(100) | -      | No       | -              | Nama pengguna               |
| email         | VARCHAR(100) | UNIQUE | No       | -              | Email login pengguna        |
| password_hash | TEXT         | -      | No       | -              | Password yang sudah di-hash |
| role_id       | BIGINT       | FK     | No       | -              | Referensi ke roles.id       |
| is_active     | BOOLEAN      | -      | No       | true           | Status aktif pengguna       |
| created_at    | TIMESTAMPTZ  | -      | No       | now()          | Waktu data dibuat           |
| updated_at    | TIMESTAMPTZ  | -      | No       | now()          | Waktu data diperbarui       |

Foreign Key:
- `role_id -> roles.id`

Constraints:
- `users.email` unique

---

## customers

| Column     | Type         | Key    | Nullable | Default        | Description             |
| ---------- | ------------ | ------ | -------- | -------------- | ----------------------- |
| id         | BIGSERIAL    | PK     | No       | auto increment | Customer ID             |
| name       | VARCHAR(100) | -      | No       | -              | Nama pelanggan          |
| phone      | VARCHAR(20)  | UNIQUE | Yes      | -              | Nomor telepon pelanggan |
| email      | VARCHAR(100) | -      | Yes      | -              | Email pelanggan         |
| address    | TEXT         | -      | Yes      | -              | Alamat pelanggan        |
| created_at | TIMESTAMPTZ  | -      | No       | now()          | Waktu data dibuat       |
| updated_at | TIMESTAMPTZ  | -      | No       | now()          | Waktu data diperbarui   |

Constraints:
- `customers.phone` unique when not null

---

## categories

| Column      | Type         | Key    | Nullable | Default        | Description           |
| ----------- | ------------ | ------ | -------- | -------------- | --------------------- |
| id          | BIGSERIAL    | PK     | No       | auto increment | Category ID           |
| name        | VARCHAR(100) | UNIQUE | No       | -              | Nama kategori         |
| description | TEXT         | -      | Yes      | -              | Deskripsi kategori    |
| created_at  | TIMESTAMPTZ  | -      | No       | now()          | Waktu data dibuat     |
| updated_at  | TIMESTAMPTZ  | -      | No       | now()          | Waktu data diperbarui |

Constraints:
- `categories.name` unique

---

## suppliers

| Column     | Type         | Key | Nullable | Default        | Description            |
| ---------- | ------------ | --- | -------- | -------------- | ---------------------- |
| id         | BIGSERIAL    | PK  | No       | auto increment | Supplier ID            |
| name       | VARCHAR(100) | -   | No       | -              | Nama supplier          |
| phone      | VARCHAR(20)  | -   | Yes      | -              | Nomor telepon supplier |
| address    | TEXT         | -   | Yes      | -              | Alamat supplier        |
| created_at | TIMESTAMPTZ  | -   | No       | now()          | Waktu data dibuat      |
| updated_at | TIMESTAMPTZ  | -   | No       | now()          | Waktu data diperbarui  |

---

## products

| Column      | Type          | Key    | Nullable | Default        | Description                |
| ----------- | ------------- | ------ | -------- | -------------- | -------------------------- |
| id          | BIGSERIAL     | PK     | No       | auto increment | Product ID                 |
| category_id | BIGINT        | FK     | No       | -              | Referensi ke categories.id |
| supplier_id | BIGINT        | FK     | No       | -              | Referensi ke suppliers.id  |
| code        | VARCHAR(50)   | UNIQUE | No       | -              | Kode unik produk           |
| name        | VARCHAR(150)  | -      | No       | -              | Nama produk                |
| description | TEXT          | -      | Yes      | -              | Deskripsi produk           |
| price       | NUMERIC(12,2) | CHECK  | No       | 0              | Harga produk               |
| stock       | INTEGER       | CHECK  | No       | 0              | Jumlah stok produk         |
| is_active   | BOOLEAN       | -      | No       | true           | Status aktif produk        |
| created_at  | TIMESTAMPTZ   | -      | No       | now()          | Waktu data dibuat          |
| updated_at  | TIMESTAMPTZ   | -      | No       | now()          | Waktu data diperbarui      |

Foreign Key:
- `category_id -> categories.id`
- `supplier_id -> suppliers.id`

Constraints:
- `products.code` unique
- `products.price >= 0`
- `products.stock >= 0`

---

## sales

| Column         | Type          | Key    | Nullable | Default        | Description                                           |
| -------------- | ------------- | ------ | -------- | -------------- | ----------------------------------------------------- |
| id             | BIGSERIAL     | PK     | No       | auto increment | Sales ID                                              |
| transaction_no | VARCHAR(50)   | UNIQUE | No       | -              | Nomor unik transaksi                                  |
| customer_id    | BIGINT        | FK     | No       | -              | Referensi ke customers.id                             |
| user_id        | BIGINT        | FK     | No       | -              | Referensi ke users.id sebagai kasir/pembuat transaksi |
| status         | VARCHAR(20)   | CHECK  | No       | DRAFT          | Status transaksi                                      |
| grand_total    | NUMERIC(12,2) | CHECK  | No       | 0              | Total transaksi                                       |
| created_at     | TIMESTAMPTZ   | -      | No       | now()          | Waktu data dibuat                                     |
| updated_at     | TIMESTAMPTZ   | -      | No       | now()          | Waktu data diperbarui                                 |

Foreign Key:
- `customer_id -> customers.id`
- `user_id -> users.id`

Constraints:
- `sales.transaction_no` unique
- `sales.status in ('DRAFT', 'PAID', 'CANCELLED')`
- `sales.grand_total >= 0`

Notes:
- `grand_total` dihitung dari total `sale_details.subtotal` dan dijaga oleh procedure/trigger.

---

## sale_details

| Column     | Type          | Key   | Nullable | Default        | Description                 |
| ---------- | ------------- | ----- | -------- | -------------- | --------------------------- |
| id         | BIGSERIAL     | PK    | No       | auto increment | Sale detail ID              |
| product_id | BIGINT        | FK    | No       | -              | Referensi ke products.id    |
| sale_id    | BIGINT        | FK    | No       | -              | Referensi ke sales.id       |
| qty        | INTEGER       | CHECK | No       | -              | Jumlah produk dibeli        |
| unit_price | NUMERIC(12,2) | CHECK | No       | -              | Harga produk saat transaksi |
| subtotal   | NUMERIC(12,2) | CHECK | No       | -              | Hasil qty * unit_price      |
| created_at | TIMESTAMPTZ   | -     | No       | now()          | Waktu data dibuat           |
| updated_at | TIMESTAMPTZ   | -     | No       | now()          | Waktu data diperbarui       |

Foreign Key:
- `product_id -> products.id`
- `sale_id -> sales.id`

Constraints:
- `sale_details.qty > 0`
- `sale_details.unit_price >= 0`
- `sale_details.subtotal >= 0`

Notes:
- `unit_price` disimpan sebagai snapshot karena harga produk dapat berubah.
- `subtotal` merupakan nilai turunan dari `qty * unit_price`.

---

## payments

| Column       | Type          | Key        | Nullable | Default        | Description           |
| ------------ | ------------- | ---------- | -------- | -------------- | --------------------- |
| id           | BIGSERIAL     | PK         | No       | auto increment | Payment ID            |
| sale_id      | BIGINT        | FK, UNIQUE | No       | -              | Referensi ke sales.id |
| method       | VARCHAR(20)   | -          | No       | -              | Metode pembayaran     |
| paid_amount  | NUMERIC(12,2) | CHECK      | No       | -              | Jumlah pembayaran     |
| payment_date | TIMESTAMPTZ   | -          | No       | now()          | Tanggal pembayaran    |
| created_at   | TIMESTAMPTZ   | -          | No       | now()          | Waktu data dibuat     |
| updated_at   | TIMESTAMPTZ   | -          | No       | now()          | Waktu data diperbarui |

Foreign Key:
- `sale_id -> sales.id`

Constraints:
- `payments.sale_id` unique
- `payments.paid_amount >= 0`

Notes:
- `paid_amount` harus sama dengan `sales.grand_total` saat transaksi dibayar.

---

## documents

| Column     | Type         | Key | Nullable | Default        | Description                   |
| ---------- | ------------ | --- | -------- | -------------- | ----------------------------- |
| id         | BIGSERIAL    | PK  | No       | auto increment | Document ID                   |
| sale_id    | BIGINT       | FK  | No       | -              | Referensi ke sales.id         |
| title      | VARCHAR(150) | -   | No       | -              | Judul dokumen transaksi       |
| created_by | BIGINT       | FK  | No       | -              | User pembuat metadata dokumen |
| created_at | TIMESTAMPTZ  | -   | No       | now()          | Waktu data dibuat             |
| updated_at | TIMESTAMPTZ  | -   | No       | now()          | Waktu data diperbarui         |

Foreign Key:
- `sale_id -> sales.id`
- `created_by -> users.id`

---

## document_versions

| Column      | Type         | Key    | Nullable | Default        | Description                   |
| ----------- | ------------ | ------ | -------- | -------------- | ----------------------------- |
| id          | BIGSERIAL    | PK     | No       | auto increment | Document version ID           |
| document_id | BIGINT       | FK     | No       | -              | Referensi ke documents.id     |
| version_no  | INTEGER      | UNIQUE | No       | -              | Nomor versi dokumen           |
| file_name   | VARCHAR(255) | -      | No       | -              | Nama file dokumen             |
| path        | TEXT         | -      | No       | -              | Lokasi penyimpanan file       |
| extension   | VARCHAR(10)  | CHECK  | No       | -              | Ekstensi file                 |
| uploaded_by | BIGINT       | FK     | No       | -              | User pengunggah versi dokumen |
| created_at  | TIMESTAMPTZ  | -      | No       | now()          | Waktu versi dibuat            |

Foreign Key:
- `document_id -> documents.id`
- `uploaded_by -> users.id`

Constraints:
- `document_versions unique(document_id, version_no)`
- `document_versions.extension in ('PDF', 'DOCX', 'TXT')`

---

## document_role_permissions

| Column       | Type        | Key        | Nullable | Default        | Description                 |
| ------------ | ----------- | ---------- | -------- | -------------- | --------------------------- |
| id           | BIGSERIAL   | PK         | No       | auto increment | Document role permission ID |
| role_id      | BIGINT      | FK, UNIQUE | No       | -              | Referensi ke roles.id       |
| can_view     | BOOLEAN     | -          | No       | false          | Izin melihat dokumen        |
| can_upload   | BOOLEAN     | -          | No       | false          | Izin mengunggah dokumen     |
| can_download | BOOLEAN     | -          | No       | false          | Izin mengunduh dokumen      |
| created_at   | TIMESTAMPTZ | -          | No       | now()          | Waktu data dibuat           |
| updated_at   | TIMESTAMPTZ | -          | No       | now()          | Waktu data diperbarui       |

Foreign Key:
- `role_id -> roles.id`

Constraints:
- `document_role_permissions.role_id` unique

---

## integration_mappings

| Column        | Type         | Key | Nullable | Default        | Description            |
| ------------- | ------------ | --- | -------- | -------------- | ---------------------- |
| id            | BIGSERIAL    | PK  | No       | auto increment | Integration mapping ID |
| source_column | VARCHAR(100) | -   | No       | -              | Nama kolom pada CSV    |
| target_table  | VARCHAR(100) | -   | No       | -              | Nama tabel tujuan      |
| target_column | VARCHAR(100) | -   | No       | -              | Nama kolom tujuan      |
| is_required   | BOOLEAN      | -   | No       | false          | Penanda kolom wajib    |
| created_at    | TIMESTAMPTZ  | -   | No       | now()          | Waktu data dibuat      |
| updated_at    | TIMESTAMPTZ  | -   | No       | now()          | Waktu data diperbarui  |

Notes:
- Tabel ini tidak memiliki foreign key karena digunakan sebagai konfigurasi pemetaan CSV.

---

## integration_logs

| Column        | Type         | Key   | Nullable | Default        | Description                  |
| ------------- | ------------ | ----- | -------- | -------------- | ---------------------------- |
| id            | BIGSERIAL    | PK    | No       | auto increment | Integration log ID           |
| imported_by   | BIGINT       | FK    | No       | -              | User yang menjalankan import |
| file_name     | VARCHAR(255) | -     | No       | -              | Nama file CSV                |
| success_count | INTEGER      | CHECK | No       | 0              | Jumlah baris berhasil        |
| failed_count  | INTEGER      | CHECK | No       | 0              | Jumlah baris gagal           |
| status        | VARCHAR(20)  | CHECK | No       | PENDING        | Status proses import         |
| error_message | TEXT         | -     | Yes      | -              | Pesan error umum             |
| created_at    | TIMESTAMPTZ  | -     | No       | now()          | Waktu data dibuat            |
| updated_at    | TIMESTAMPTZ  | -     | No       | now()          | Waktu data diperbarui        |

Foreign Key:
- `imported_by -> users.id`

Constraints:
- `integration_logs.success_count >= 0`
- `integration_logs.failed_count >= 0`
- `integration_logs.status in ('PENDING', 'SUCCESS', 'PARTIAL', 'FAILED')`

---

## integration_log_details

| Column             | Type        | Key   | Nullable | Default        | Description                      |
| ------------------ | ----------- | ----- | -------- | -------------- | -------------------------------- |
| id                 | BIGSERIAL   | PK    | No       | auto increment | Integration log detail ID        |
| integration_log_id | BIGINT      | FK    | No       | -              | Referensi ke integration_logs.id |
| row_number         | INTEGER     | CHECK | No       | -              | Nomor baris pada CSV             |
| product_code       | VARCHAR(50) | -     | Yes      | -              | Kode produk pada baris CSV       |
| status             | VARCHAR(20) | CHECK | No       | -              | Status hasil import baris        |
| error_message      | TEXT        | -     | Yes      | -              | Pesan error baris                |
| raw_data           | JSONB       | -     | Yes      | -              | Data mentah baris CSV            |
| created_at         | TIMESTAMPTZ | -     | No       | now()          | Waktu data dibuat                |
| updated_at         | TIMESTAMPTZ | -     | No       | now()          | Waktu data diperbarui            |

Foreign Key:
- `integration_log_id -> integration_logs.id`

Constraints:
- `integration_log_details.row_number > 0`
- `integration_log_details.status in ('SUCCESS', 'FAILED')`

---

## data_quality_logs

| Column              | Type         | Key   | Nullable | Default        | Description                     |
| ------------------- | ------------ | ----- | -------- | -------------- | ------------------------------- |
| id                  | BIGSERIAL    | PK    | No       | auto increment | Data quality log ID             |
| table_name          | VARCHAR(100) | -     | No       | -              | Nama tabel yang diperiksa       |
| field_name          | VARCHAR(100) | -     | No       | -              | Nama kolom yang bermasalah      |
| record_id           | BIGINT       | -     | Yes      | -              | ID record yang bermasalah       |
| old_value           | TEXT         | -     | Yes      | -              | Nilai lama                      |
| new_value           | TEXT         | -     | Yes      | -              | Nilai baru setelah koreksi      |
| problem_description | TEXT         | -     | No       | -              | Deskripsi masalah kualitas data |
| root_cause          | TEXT         | -     | Yes      | -              | Akar masalah                    |
| priority            | VARCHAR(20)  | CHECK | No       | MEDIUM         | Prioritas masalah               |
| status              | VARCHAR(20)  | CHECK | No       | OPEN           | Status penanganan               |
| corrected_by        | BIGINT       | FK    | Yes      | -              | User yang melakukan koreksi     |
| corrected_at        | TIMESTAMPTZ  | -     | Yes      | -              | Waktu koreksi                   |
| created_at          | TIMESTAMPTZ  | -     | No       | now()          | Waktu data dibuat               |
| updated_at          | TIMESTAMPTZ  | -     | No       | now()          | Waktu data diperbarui           |

Foreign Key:
- `corrected_by -> users.id`

Constraints:
- `data_quality_logs.priority in ('LOW', 'MEDIUM', 'HIGH')`
- `data_quality_logs.status in ('OPEN', 'CORRECTED', 'IGNORED')`

---

## Notes

- Semua `updated_at` diperbarui otomatis melalui trigger.
- Semua password disimpan dalam bentuk hash pada `users.password_hash`.
- Nilai `sale_details.subtotal` dan `sales.grand_total` adalah nilai turunan yang dijaga oleh procedure/trigger.
- Tipe data pada dokumen ini mengikuti PostgreSQL.
