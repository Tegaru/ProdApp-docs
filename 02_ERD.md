# Tagihanku - ERD (Entity-Relationship Diagram)

**Versi:** 1.0
**Tanggal:** 8 November 2025
**Penulis:** [Tegar Miftaqur Rohim dan Tim]

---

## Gambaran Umum

ERD ini menggambarkan struktur database relasional untuk aplikasi Tagihanku, dengan fokus pada entitas utama: `Categories`, `Bills`, dan `Reminders`, serta hubungan di antara mereka. Pada MVP awal, aplikasi tidak memerlukan entitas `Users` karena data disimpan secara lokal di perangkat.

---

## Entitas:

### 1. Categories
Entitas ini digunakan untuk mengelompokkan tagihan.
- **id** (INTEGER, PRIMARY KEY, AUTOINCREMENT)
- **name** (TEXT, NOT NULL) - Nama kategori, harus unik (misal: "Rumah Tangga", "Langganan", "Cicilan").
- **color** (TEXT, NULLABLE) - Opsional, untuk representasi warna kategori di UI (misal: hex code '#RRGGBB').

### 2. Bills
Entitas inti yang menyimpan detail setiap tagihan.
- **id** (INTEGER, PRIMARY KEY, AUTOINCREMENT)
- **name** (TEXT, NOT NULL) - Nama atau judul tagihan (misal: "Listrik PLN", "Internet IndiHome").
- **amount** (INTEGER, NOT NULL) - Jumlah tagihan yang harus dibayar (disimpan sebagai integer, misal dalam satuan sen atau rupiah terkecil untuk menghindari isu floating point).
- **due_date** (INTEGER, NOT NULL) - Tanggal jatuh tempo tagihan (disimpan sebagai Unix timestamp).
- **status** (TEXT, NOT NULL, DEFAULT 'unpaid') - Status tagihan (misal: 'unpaid', 'paid', 'overdue').
- **category_id** (INTEGER, NULLABLE) - Foreign Key ke `Categories.id`. Jika kategori dihapus, tagihan yang terkait akan memiliki `category_id` NULL.
- **is_recurring** (INTEGER, NOT NULL, DEFAULT 0) - Menunjukkan apakah tagihan ini berulang (0=tidak, 1=ya).
- **recurring_pattern** (TEXT, NULLABLE) - Pola pengulangan jika `is_recurring` = 1 (misal: 'daily', 'weekly', 'monthly', 'yearly').
- **created_at** (INTEGER, NOT NULL) - Timestamp saat tagihan dibuat.
- **updated_at** (INTEGER, NOT NULL) - Timestamp saat tagihan terakhir diubah.

### 3. Reminders
Entitas untuk menyimpan jadwal notifikasi untuk setiap tagihan.
- **id** (INTEGER, PRIMARY KEY, AUTOINCREMENT)
- **bill_id** (INTEGER, NOT NULL) - Foreign Key ke `Bills.id`. Jika tagihan dihapus, pengingat terkait juga akan dihapus.
- **reminder_time** (INTEGER, NOT NULL) - Timestamp spesifik kapan pengingat harus muncul.
- **is_active** (INTEGER, NOT NULL, DEFAULT 1) - Status pengingat (0=tidak aktif, 1=aktif).

---

## Hubungan (Relationships):

- **Categories (1) : (Many) Bills**
    - **Kardinalitas:** Satu kategori dapat memiliki banyak tagihan. Setiap tagihan dapat dikaitkan dengan paling banyak satu kategori (atau tidak sama sekali).
    - **Kunci Asing:** `Bills.category_id` mereferensikan `Categories.id`.
    - **Aksi Saat Hapus:** `ON DELETE SET NULL` (jika kategori dihapus, `category_id` pada tagihan terkait akan diatur menjadi NULL).

- **Bills (1) : (Many) Reminders**
    - **Kardinalitas:** Satu tagihan dapat memiliki banyak pengingat (misal: pengingat H-7 dan H-1). Setiap pengingat hanya terikat pada satu tagihan.
    - **Kunci Asing:** `Reminders.bill_id` mereferensikan `Bills.id`.
    - **Aksi Saat Hapus:** `ON DELETE CASCADE` (jika sebuah tagihan dihapus, semua pengingat yang terkait dengan tagihan tersebut akan otomatis ikut terhapus).

---

## Diagram Visual (Konseptual):
```+----------------+      +---------------+      +----------------+
|   Categories   |      |     Bills     |      |    Reminders   |
+----------------+      +---------------+      +----------------+
| PK id          |<----(FK)| PK id         | (FK)---->| PK id          |
|    name        | 1 ------ M |    name        | 1 ------ M | FK bill_id     |
|    color       |      |    amount      |      |    reminder_time |
+----------------+      |    due_date    |      |    is_active   |
                      |    status      |      +----------------+
                      | FK category_id|
                      |    is_recurring|
                      |    rec_pattern |
                      |    created_at  |
                      |    updated_at  |
                      +---------------+


**Keterangan:**
*   `PK`: Primary Key
*   `FK`: Foreign Key
*   `1`: Satu
*   `M`: Banyak

---
