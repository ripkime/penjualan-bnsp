# Entities

## Overview

Dokumen ini berisi daftar entity yang diperlukan untuk merancang basis data Sistem Pengelolaan Data Penjualan.

### Users

- id
- name
- email
- password
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
- status
- paid_amount
- payment_date
- created_at
- updated_at

---

### Documents

- id
- sale_id
- uploaded_by
- file_name
- path
- extension
- created_at
- updated_at

---

### Integration Logs

- id
- file_name
- succeeded
- failed
- status
- error_message
- created_at
- updated_at

---

### Data Quality Logs

- id
- table_name
- field_name
- old_value
- new_value
- problem_description
- created_at
- updated_at




