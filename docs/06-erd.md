# Entity Relationship Diagram

## Overview

Dokumen ini menampilkan ERD Sistem Pengelolaan Data Penjualan berdasarkan entity dan relationship yang telah didefinisikan.

---

## ERD Diagram

```mermaid
erDiagram
    ROLES ||--o{ USERS : has
    CUSTOMERS ||--o{ SALES : makes
    USERS ||--o{ SALES : creates
    CATEGORIES ||--o{ PRODUCTS : groups
    SUPPLIERS ||--o{ PRODUCTS : supplies
    SALES ||--o{ SALE_DETAILS : contains
    PRODUCTS ||--o{ SALE_DETAILS : sold_as
    SALES ||--o| PAYMENTS : paid_by
    SALES ||--o{ DOCUMENTS : has
    USERS ||--o{ DOCUMENTS : creates
    DOCUMENTS ||--o{ DOCUMENT_VERSIONS : has
    USERS ||--o{ DOCUMENT_VERSIONS : uploads
    ROLES ||--o| DOCUMENT_ROLE_PERMISSIONS : has
    USERS ||--o{ INTEGRATION_LOGS : imports
    INTEGRATION_LOGS ||--o{ INTEGRATION_LOG_DETAILS : contains
    USERS ||--o{ DATA_QUALITY_LOGS : corrects

    ROLES {
        bigint id PK
        varchar name UK
        datetime created_at
        datetime updated_at
    }

    USERS {
        bigint id PK
        varchar name
        varchar email UK
        text password_hash
        bigint role_id FK
        boolean is_active
        datetime created_at
        datetime updated_at
    }

    CUSTOMERS {
        bigint id PK
        varchar name
        varchar phone UK
        varchar email
        text address
        datetime created_at
        datetime updated_at
    }

    CATEGORIES {
        bigint id PK
        varchar name
        text description
        datetime created_at
        datetime updated_at
    }

    SUPPLIERS {
        bigint id PK
        varchar name
        varchar phone
        text address
        datetime created_at
        datetime updated_at
    }

    PRODUCTS {
        bigint id PK
        bigint category_id FK
        bigint supplier_id FK
        varchar code UK
        varchar name
        text description
        numeric price
        int stock
        boolean is_active
        datetime created_at
        datetime updated_at
    }

    SALES {
        bigint id PK
        varchar transaction_no UK
        bigint customer_id FK
        bigint user_id FK
        varchar status
        numeric grand_total
        datetime created_at
        datetime updated_at
    }

    SALE_DETAILS {
        bigint id PK
        bigint product_id FK
        bigint sale_id FK
        int qty
        numeric unit_price
        numeric subtotal
        datetime created_at
        datetime updated_at
    }

    PAYMENTS {
        bigint id PK
        bigint sale_id FK
        varchar method
        numeric paid_amount
        datetime payment_date
        datetime created_at
        datetime updated_at
    }

    DOCUMENTS {
        bigint id PK
        bigint sale_id FK
        varchar title
        bigint created_by FK
        datetime created_at
        datetime updated_at
    }

    DOCUMENT_VERSIONS {
        bigint id PK
        bigint document_id FK
        int version_no
        varchar file_name
        text path
        varchar extension
        bigint uploaded_by FK
        datetime created_at
    }

    DOCUMENT_ROLE_PERMISSIONS {
        bigint id PK
        bigint role_id FK
        boolean can_view
        boolean can_upload
        boolean can_download
        datetime created_at
        datetime updated_at
    }

    INTEGRATION_MAPPINGS {
        bigint id PK
        varchar source_column
        varchar target_table
        varchar target_column
        boolean is_required
        datetime created_at
        datetime updated_at
    }

    INTEGRATION_LOGS {
        bigint id PK
        bigint imported_by FK
        varchar file_name
        int success_count
        int failed_count
        varchar status
        text error_message
        datetime created_at
        datetime updated_at
    }

    INTEGRATION_LOG_DETAILS {
        bigint id PK
        bigint integration_log_id FK
        int row_number
        varchar product_code
        varchar status
        text error_message
        json raw_data
        datetime created_at
        datetime updated_at
    }

    DATA_QUALITY_LOGS {
        bigint id PK
        varchar table_name
        varchar field_name
        bigint record_id
        text old_value
        text new_value
        text problem_description
        text root_cause
        varchar priority
        varchar status
        bigint corrected_by FK
        datetime corrected_at
        datetime created_at
        datetime updated_at
    }
```

---

## Notes

- `integration_mappings` tidak memiliki foreign key karena berfungsi sebagai konfigurasi pemetaan CSV.
- `sale_details.subtotal` adalah nilai turunan dari `qty * unit_price`.
- `sales.grand_total` adalah nilai turunan dari total `sale_details.subtotal`.
- `payments.sale_id` memiliki unique constraint agar satu transaksi maksimal memiliki satu pembayaran.
- `document_role_permissions.role_id` memiliki unique constraint agar satu role hanya memiliki satu konfigurasi izin dokumen.
- `document_versions` menyimpan riwayat versi dokumen.
- `data_quality_logs.corrected_by` dapat bernilai null jika user tidak tersedia.
