# Entities

## Overview

Dokumen ini berisi daftar entity yang diperlukan untuk merancang basis data Sistem Pengelolaan Data Penjualan.

### Users

- id
- name
- email
- password_hash
- role_id
- is_active
- created_at
- updated_at

---

### Roles

- id
- name
- created_at
- updated_at

---

### Customers

- id
- name
- phone
- email
- address
- created_at
- updated_at

---

### Categories

- id
- name
- description
- created_at
- updated_at

---

### Suppliers

- id
- name
- phone
- address
- created_at
- updated_at

---

### Products

- id
- category_id
- supplier_id
- code
- name
- description
- price
- stock
- is_active
- created_at
- updated_at

---

### Sales

- id
- transaction_no
- customer_id
- user_id
- status
- grand_total
- created_at
- updated_at

---

### Sale Details

- id
- product_id
- sale_id
- qty
- unit_price
- subtotal
- created_at
- updated_at

---

### Payments

- id
- sale_id
- method
- paid_amount
- payment_date
- created_at
- updated_at

---

### Documents

- id
- sale_id
- title
- created_by
- created_at
- updated_at

---

### Document Versions

- id 
- document_id
- version_no
- file_name
- path
- extension
- uploaded_by
- created_at

---

### Document Role Permissions

- id
- role_id
- can_view
- can_upload
- can_download
- created_at
- updated_at

---

### Integration Mappings

- id
- source_column
- target_table
- target_column
- is_required
- created_at
- updated_at

### Integration Logs

- id
- imported_by
- file_name
- success_count
- failed_count
- status
- error_message
- created_at
- updated_at

---

### Integration Log Details

- id
- integration_log_id
- row_number
- product_code
- status
- error_message
- raw_data
- created_at
- updated_at

---

### Data Quality Logs

- id
- table_name
- field_name
- record_id
- old_value
- new_value
- problem_description
- root_cause
- priority
- status
- corrected_by
- corrected_at
- created_at
- updated_at


## Key Constraints

### Unique Constraints

- users.email unique
- roles.name unique
- categories.name unique
- customers.phone unique when not null
- products.code unique
- sales.transaction_no unique
- payments.sale_id unique
- document_role_permissions.role_id unique
- document_versions unique (document_id, version_no)

### Check Constraints

- products.price >= 0
- products.stock >= 0
- sales.grand_total >= 0
- sale_details.qty > 0
- sale_details.unit_price >= 0
- sale_details.subtotal >= 0
- payments.paid_amount >= 0
- sales.status in (DRAFT, PAID, CANCELLED)
- integration_logs.success_count >= 0
- integration_logs.failed_count >= 0
- integration_logs.status in (PENDING, SUCCESS, PARTIAL, FAILED)
- integration_log_details.row_number > 0
- integration_log_details.status in (SUCCESS, FAILED)
- data_quality_logs.priority in (LOW, MEDIUM, HIGH)
- data_quality_logs.status in (OPEN, CORRECTED, IGNORED)
- document_versions.extension in (PDF, DOCX, TXT)

### Derived Values

- sale_details.subtotal derived from qty * unit_price
- sales.grand_total maintained from total sale_details.subtotal


