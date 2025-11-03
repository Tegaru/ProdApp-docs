# 📘 Software Design Document (SDD) — ProdApp

**Versi:** 1.0  
**Tanggal:** 20 Oktober 2025  
**Author:** Tegar Miftaqur Rohim dan Tim   

---

### **1.Pendahuluan**

### 1.1 Tujuan Dokumen
Dokumen Desain Perangkat Lunak (SDD) ini menyediakan deskripsi teknis tentang desain arsitektur dan komponen perangkat lunak untuk aplikasi mobile "ProdApp". Dokumen ini bertujuan untuk memandu tim pengembangan dalam implementasi sistem, memastikan semua persyaratan fungsional dan non-fungsional dari dokumen SRS terpenuhi, serta menjaga konsistensi arsitektur.

### 1.2 Lingkup Sistem
ProdApp adalah aplikasi to-do list dan perencana produktivitas mobile yang dikembangkan menggunakan Flutter. Desain ini mencakup struktur arsitektur Clean Code, modul-modul utama (Manajemen Tugas, Kategori, Pengingat, Kalender, Pengaturan), serta detail tentang bagaimana lapisan-lapisan dan komponen-komponen berinteraksi untuk mencapai fungsionalitas yang ditentukan dalam PRD dan SRS.

Aplikasi dikembangkan untuk platform Android menggunakan Flutter dengan penyimpanan lokal (offline-first).

### 1.3 Definisi dan Singkatan

| Istilah | Definisi |
|--------|---------|
| MVP | Minimum Viable Product |
| CRUD | Create, Read, Update, Delete |
| Drift | ORM SQLite untuk Flutter |
| Riverpod | Framework state management |
| GoRouter | Navigation handler |
| Freezed | Generator immutable class |
| fpdart | Functional programming (Either-based error handling) |

### 1.4 Referensi
- 01_PRD_ProdApp.md
- 02_ERD_ProdApp.md
- 03_SRS_ProdApp.md

---

### 2.1 Tinjauan Arsitektur ✔ Clean Architecture
### **2.Desain Arsitektur Sistem**

+---------------------------------------------------------+
| Presentation Layer |
| (Flutter Widgets, Riverpod Providers, GoRouter) |
+---------------------------------------------------------+
                               |
                               V
+---------------------------------------------------------+
| Domain Layer |
| (Entities, UseCases, Abstract Repositories) |
+---------------------------------------------------------+
                              |
                              V
+---------------------------------------------------------+
| Data Layer |
| (Repository Impl, Drift DB, Notification Scheduler) |
+---------------------------------------------------------+

## 2.2 Deskripsi Lapisan

### 📍 Presentation Layer
- Komponen: Screens, Widgets, Providers (Riverpod)
- Navigasi: GoRouter
- Tidak mengandung logika bisnis
- Menampilkan data yang diproses Domain Layer

### 🧠 Domain Layer
- Komponen: Entities, Use Cases, Interfaces Repository
- Menjadi pusat logika bisnis
- Tidak tergantung pada UI maupun database

### 🗄️ Data Layer
- Mengelola data dari Drift Database
- Bertanggung jawab pada notifikasi lokal
- Implementasi dari kontrak repository Domain Layer

---

### **3.Desain Komponen Rinci**

### 3.1 Presentation Layer
📌 Halaman Utama:
- HomeScreen → daftar tugas
- Add/Edit Task Screen → formulir input tugas
- CategoryScreen → pengaturan kategori
- CalendarScreen → kalender tugas
- SettingsScreen → tema & izin notifikasi

📌 State Management
- Menggunakan `StateNotifier` / `AsyncNotifier` Riverpod

📌 Routing
| Route | Page |
|------|------|
| `/` | HomeScreen |
| `/task/add` | AddTaskScreen |
| `/categories` | CategoryScreen |
| `/calendar` | CalendarScreen |
| `/settings` | SettingsScreen |

---

### 3.2 Domain Layer

#### Entities (Freezed)
- TaskEntity
- CategoryEntity
- ReminderEntity

#### Use Cases
| Use Case | Fungsi |
|---------|--------|
| AddTask | CRUD menambah tugas |
| UpdateTask | Edit tugas |
| ToggleTaskStatus | Ubah selesai/belum |
| DeleteTask | Menghapus |
| GetTasksByDate | Filter tanggal |
| SetReminder | Atur pengingat |

---

### **4. Desain Data (Database)**

*   **Implementasi:** Skema database yang telah dirancang akan diimplementasikan menggunakan class-class `Table` di Drift.
*   **Relasi:** Relasi `FOREIGN KEY` antara `Transactions` dan `Categories` akan didefinisikan dalam model tabel Drift.
*   **Aturan Integritas:** Aturan `ON DELETE RESTRICT` akan diimplementasikan pada `FOREIGN KEY` untuk mencegah penghapusan kategori yang sedang digunakan.
*   **Migrasi:** Drift akan menangani skema migrasi. Setiap perubahan pada struktur tabel setelah rilis awal akan memerlukan skema migrasi baru.

### **5. Strategi Penanganan Error (Error Handling)**

*   **Konsep:** Menggunakan `fpdart` untuk menghindari `try-catch` blocks yang berlebihan dan `Exception` yang tidak tertangani.
*   **Alur:**
    1.  **Data Layer:** Method di Repository akan mengembalikan `Future<Either<Failure, T>>`. Jika query database gagal, ia akan mengembalikan `Left(DatabaseFailure("Pesan Error"))`. Jika berhasil, ia akan mengembalikan `Right(data)`.
    2.  **Domain Layer:** Use cases akan meneruskan `Either` ini ke atas.
    3.  **Presentation Layer:** Provider Riverpod akan menerima `Either`. UI (Widget) akan melakukan `pattern matching` pada hasilnya:
        *   Jika `Right`, tampilkan data.
        *   Jika `Left`, tampilkan pesan error kepada pengguna (misalnya, menggunakan `SnackBar` atau widget error).
*   **Tipe `Failure`:** Akan dibuat class `Failure` dasar dan beberapa turunan spesifik:
    *   `abstract class Failure {}`
    *   `class DatabaseFailure extends Failure { final String message; }`
    *   `class NetworkFailure extends Failure { final String message; }` (untuk masa depan).

### **6.Struktur Proyek**
lib/
└── src/
    ├── core/                          # Kode umum & utilitas aplikasi
    │   ├── constants/                 # Konstanta global (mis: teks, keys)
    │   ├── error/                     # Failure handling fpdart
    │   ├── themes/                    # Light & Dark theme App
    │   ├── utils/                     # Helper, formatter
    │   └── di/                        # Dependency Injection provider
    │
    ├── data/                          # Data Layer (paling luar)
    │   ├── datasources/               # Drift Database, DAO, Notifikasi
    │   ├── models/                    # DTO & tabel mapping
    │   └── repositories/              # Implementasi Repositori Domain
    │
    ├── domain/                        # Business Logic Layer
    │   ├── entities/                  # Model inti aplikasi (immutable)
    │   ├── repositories/              # Kontrak Abstract Repository
    │   └── usecases/                  # Aturan bisnis (AddTask, etc.)
    │
    └── presentation/                  # UI Layer
        ├── features/                  # Modular per fitur
        │   ├── task/                  # Fitur To-Do List
        │   │   ├── screens/           # Tampilan halaman Task
        │   │   ├── widgets/           # Komponen reusable Task
        │   │   └── providers/         # Riverpod provider Task
        │   │
        │   ├── category/
        │   │   ├── screens/
        │   │   ├── widgets/
        │   │   └── providers/
        │   │
        │   ├── reminder/
        │   │   ├── providers/
        │   │   └── widgets/
        │   │
        │   ├── calendar/
        │   │   ├── screens/
        │   │   ├── widgets/
        │   │   └── providers/
        │   │
        │   └── settings/
        │       ├── screens/
        │       ├── widgets/
        │       └── providers/
        │
        ├── global_widgets/            # Widget yang banyak dipakai
        └── routing/                   # GoRouter setup & navigation
            └── app_router.dart


📌 **Dokumen SDD ini akan digunakan sebagai pedoman resmi dalam implementasi ProdApp.**  
"""
