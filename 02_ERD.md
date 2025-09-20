# ProdApp - ERD (Entity-Relationship Diagram)

**Versi:** 1.0
**Tanggal:** 9 20 September 2025
**Penulis:** [Tegar Miftaqur Rohim dan Tim]

---

## Gambaran Umum

ERD ini menggambarkan struktur database relasional untuk aplikasi ProdApp, fokus pada entitas utama: `Categories`, `Tasks`, dan `Reminders`, serta hubungan di antara mereka. Pada MVP awal, aplikasi tidak memerlukan entitas `Users` karena data disimpan secara lokal.

---

## Entitas:

### 1. Categories
*   **id** (INTEGER, PRIMARY KEY, AUTOINCREMENT)
*   **name** (TEXT, NOT NULL) - Nama kategori, harus unik.
*   **color** (TEXT, NULLABLE) - Opsional, untuk representasi warna kategori di UI (misal: hex code '#RRGGBB').

### 2. Tasks
*   **id** (INTEGER, PRIMARY KEY, AUTOINCREMENT)
*   **title** (TEXT, NOT NULL) - Judul tugas.
*   **description** (TEXT, NULLABLE) - Detail tambahan tugas (Fitur iterasi berikutnya).
*   **due_date** (INTEGER, NULLABLE) - Tanggal jatuh tempo tugas (disimpan sebagai Unix timestamp).
*   **is_completed** (INTEGER, NOT NULL, DEFAULT 0) - Status tugas (0=belum selesai, 1=selesai).
*   **is_starred** (INTEGER, NOT NULL, DEFAULT 0) - Menandai tugas sebagai penting (0=tidak, 1=ya) (Fitur iterasi berikutnya).
*   **priority** (TEXT, NULLABLE) - Level prioritas (e.g., 'High', 'Medium', 'Low') (Fitur iterasi berikutnya).
*   **category_id** (INTEGER, NULLABLE) - Foreign Key ke `Categories.id`. Jika kategori dihapus, tugas yang terkait akan memiliki `category_id` NULL.
*   **created_at** (INTEGER, NOT NULL) - Timestamp saat tugas dibuat.
*   **updated_at** (INTEGER, NOT NULL) - Timestamp saat tugas terakhir diubah.
*   **is_recurring** (INTEGER, NOT NULL, DEFAULT 0) - Menunjukkan apakah tugas berulang (0=tidak, 1=ya) (Fitur iterasi berikutnya).
*   **recurring_pattern** (TEXT, NULLABLE) - Pola pengulangan (e.g., 'daily', 'weekly', 'monthly') (Fitur iterasi berikutnya).

### 3. Reminders
*   **id** (INTEGER, PRIMARY KEY, AUTOINCREMENT)
*   **task_id** (INTEGER, NOT NULL) - Foreign Key ke `Tasks.id`. Jika tugas dihapus, pengingat terkait juga akan dihapus.
*   **reminder_time** (INTEGER, NOT NULL) - Timestamp spesifik untuk pengingat.
*   **is_active** (INTEGER, NOT NULL, DEFAULT 1) - Status pengingat (0=tidak aktif, 1=aktif).

---

## Hubungan (Relationships):

*   **Categories (1) : (Many) Tasks**
    *   **Kardinalitas:** Satu kategori dapat memiliki banyak tugas. Setiap tugas dapat dikaitkan dengan paling banyak satu kategori (atau tidak sama sekali).
    *   **Kunci Asing:** `Tasks.category_id` mereferensikan `Categories.id`.
    *   **Aksi Saat Hapus:** `ON DELETE SET NULL` (jika kategori dihapus, `category_id` pada tugas terkait akan diatur menjadi NULL).

*   **Tasks (1) : (Many) Reminders**
    *   **Kardinalitas:** Satu tugas dapat memiliki banyak pengingat. Setiap pengingat dikaitkan dengan satu tugas.
    *   **Kunci Asing:** `Reminders.task_id` mereferensikan `Tasks.id`.
    *   **Aksi Saat Hapus:** `ON DELETE CASCADE` (jika tugas dihapus, semua pengingat yang terkait dengan tugas tersebut akan otomatis dihapus).

---

## Diagram Visual (Konseptual):
+----------------+ +---------------+ +----------------+
| Categories | | Tasks | | Reminders |
+----------------+ +---------------+ +----------------+
| PK id | <----(FK)| PK id | (FK)---->| PK id |
| name | 1 ------ M | title | 1 ------ M | FK task_id |
| color | | description | | reminder_time |
+----------------+ | due_date | | is_active |
| is_completed | +----------------+
| is_starred |
| priority |
| FK category_id|
| created_at |
| updated_at |
| is_recurring |
| recurring_pattern |
+---------------+


**Keterangan:**
*   `PK`: Primary Key
*   `FK`: Foreign Key
*   `1`: Satu
*   `M`: Banyak

---
