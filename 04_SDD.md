# 📘 Software Design Document (SDD) — Tagihanku

**Versi:** 1.0  
**Tanggal:** 8 November 2025  
**Author:** Tegar Miftaqur Rohim dan Tim   

---

### **1. Pendahuluan**

#### 1.1 Tujuan Dokumen
Dokumen Desain Perangkat Lunak (SDD) ini menyediakan deskripsi teknis tentang desain arsitektur dan komponen perangkat lunak untuk aplikasi mobile "Tagihanku". Dokumen ini bertujuan untuk memandu tim pengembangan dalam implementasi sistem, memastikan semua persyaratan fungsional dan non-fungsional dari dokumen SRS terpenuhi, serta menjaga konsistensi arsitektur.

#### 1.2 Lingkup Sistem
Tagihanku adalah aplikasi pencatat dan pengingat tagihan mobile yang dikembangkan menggunakan Flutter. Desain ini mencakup struktur arsitektur Clean Code, modul-modul utama (Manajemen Tagihan, Kategori, Pengingat, Kalender, Pengaturan), serta detail tentang bagaimana lapisan-lapisan dan komponen-komponen berinteraksi untuk mencapai fungsionalitas yang ditentukan dalam PRD dan SRS.

Aplikasi dikembangkan untuk platform Android menggunakan Flutter dengan penyimpanan lokal (offline-first).

#### 1.3 Definisi dan Singkatan

| Istilah | Definisi |
|--------|---------|
| MVP | Minimum Viable Product |
| CRUD | Create, Read, Update, Delete |
| Drift | ORM SQLite untuk Flutter |
| Riverpod | Framework state management |
| GoRouter | Navigation handler |
| Freezed | Generator immutable class |
| fpdart | Functional programming (Either-based error handling) |

#### 1.4 Referensi
- 01_PRD_Tagihanku.md
- 02_ERD_Tagihanku.md
- 03_SRS_Tagihanku.md

---

### **2. Desain Arsitektur Sistem**

#### 2.1 Tinjauan Arsitektur: Clean Architecture
Arsitektur yang dipilih adalah Clean Architecture untuk memisahkan logika bisnis dari detail implementasi (UI, database), sehingga aplikasi menjadi lebih modular, mudah diuji, dan dipelihara.

### **2.Desain Arsitektur Sistem**
```
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
```

#### 2.2 Deskripsi Lapisan

-   **📍 Presentation Layer:** Bertanggung jawab untuk semua yang terkait dengan UI. Lapisan ini tidak mengandung logika bisnis. Ia hanya menampilkan data yang diberikan oleh Domain Layer dan mengirimkan input pengguna ke Domain Layer.
-   **🧠 Domain Layer:** Merupakan inti dari aplikasi. Berisi entitas, aturan bisnis (use cases), dan antarmuka (kontrak) untuk repository. Lapisan ini tidak bergantung pada lapisan lain.
-   **🗄️ Data Layer:** Bertanggung jawab untuk mengelola sumber data, baik itu database lokal (Drift) maupun layanan eksternal (API, di masa depan). Lapisan ini mengimplementasikan kontrak repository yang didefinisikan di Domain Layer.

---

### **3. Desain Komponen Rinci**

#### 3.1 Presentation Layer

-   **📌 Halaman (Screens):**
    -   `HomeScreen`: Menampilkan ringkasan dan daftar tagihan yang akan datang.
    -   `AddEditBillScreen`: Formulir untuk menambah atau mengedit tagihan.
    -   `CategoryScreen`: Pengelolaan kategori (menambah, mengedit, menghapus).
    -   `CalendarScreen`: Tampilan kalender yang menandai tanggal jatuh tempo tagihan.
    -   `SettingsScreen`: Pengaturan tema aplikasi dan izin notifikasi.

-   **📌 State Management:**
    -   Menggunakan `StateNotifier` atau `AsyncNotifier` dari Riverpod untuk mengelola state setiap halaman.

-   **📌 Routing (GoRouter):**

| Route | Page |
|---|---|
| `/` | HomeScreen |
| `/bill/add` | AddEditBillScreen (mode tambah) |
| `/bill/edit/:id` | AddEditBillScreen (mode edit) |
| `/categories` | CategoryScreen |
| `/calendar` | CalendarScreen |
| `/settings` | SettingsScreen |

#### 3.2 Domain Layer

-   **Entities (dibuat dengan Freezed):**
    -   `BillEntity`: Merepresentasikan objek tagihan.
    -   `CategoryEntity`: Merepresentasikan objek kategori.
    -   `ReminderEntity`: Merepresentasikan objek pengingat.

-   **Use Cases:**

| Use Case | Fungsi |
|---|---|
| `AddBill` | Menambah tagihan baru. |
| `UpdateBill` | Mengedit detail tagihan. |
| `UpdateBillStatus` | Mengubah status tagihan (misal: menjadi 'Lunas'). |
| `DeleteBill` | Menghapus tagihan. |
| `GetBillsByDueDate` | Mendapatkan daftar tagihan berdasarkan tanggal jatuh tempo. |
| `SetReminderForBill` | Mengatur jadwal pengingat untuk sebuah tagihan. |

---

### **4. Desain Data (Database)**

-   **Implementasi:** Skema database yang telah dirancang dalam ERD akan diimplementasikan menggunakan class `Table` di Drift.
-   **Relasi:** Relasi `FOREIGN KEY` antara `Bills` dan `Categories` akan didefinisikan dalam model tabel Drift.
-   **Aturan Integritas:** Aturan `ON DELETE SET NULL` akan diimplementasikan pada `FOREIGN KEY` `category_id` di tabel `Bills` untuk memastikan jika sebuah kategori dihapus, tagihan yang terkait tidak ikut terhapus.
-   **Migrasi:** Drift akan menangani skema migrasi. Setiap perubahan pada struktur tabel setelah rilis awal akan memerlukan skema migrasi baru.

---

### **5. Strategi Penanganan Error (Error Handling)**

-   **Konsep:** Menggunakan `fpdart` dengan tipe `Either` untuk menangani hasil operasi yang bisa gagal (seperti query database) secara fungsional. Ini menghindari `try-catch` blocks yang berlebihan dan `Exception` yang tidak tertangani.
-   **Alur:**
    1.  **Data Layer:** Method di Repository akan mengembalikan `Future<Either<Failure, T>>`. Jika query gagal, ia mengembalikan `Left(DatabaseFailure("Pesan Error"))`. Jika berhasil, ia mengembalikan `Right(data)`.
    2.  **Domain Layer:** Use cases akan meneruskan `Either` ini ke atas tanpa modifikasi.
    3.  **Presentation Layer:** Provider Riverpod akan menerima `Either`. UI (Widget) akan melakukan `pattern matching` pada hasilnya:
        -   Jika `Right`, tampilkan data.
        -   Jika `Left`, tampilkan pesan error kepada pengguna (misal, menggunakan `SnackBar` atau widget khusus error).
-   **Tipe `Failure`:** Akan dibuat class `Failure` abstrak dan beberapa turunan spesifik:
    -   `abstract class Failure {}`
    -   `class DatabaseFailure extends Failure { final String message; }`

---

### **6. Struktur Proyek**

```
lib/
└── src/
    ├── core/                          # Kode umum & utilitas aplikasi
    │   ├── constants/                 # Konstanta global (mis: teks, keys)
    │   ├── error/                     # Failure handling fpdart
    │   ├── themes/                    # Light & Dark theme App
    │   ├── utils/                     # Helper, formatter
    │   └── di/                        # Dependency Injection (Riverpod providers)
    │
    ├── data/                          # Data Layer (lapisan terluar)
    │   ├── datasources/               # Drift Database, DAO, Local Notification Service
    │   ├── models/                    # Model tabel & mapping data
    │   └── repositories/              # Implementasi dari kontrak Repository Domain
    │
    ├── domain/                        # Business Logic Layer (inti aplikasi)
    │   ├── entities/                  # Model inti aplikasi (immutable, via Freezed)
    │   ├── repositories/              # Kontrak/Interface Abstract Repository
    │   └── usecases/                  # Aturan bisnis (mis: AddBill, GetBills, etc.)
    │
    └── presentation/                  # UI Layer (semua yang terkait tampilan)
        ├── features/                  # Dibagi per modul fitur
        │   ├── bill/                  # Fitur Manajemen Tagihan (inti)
        │   │   ├── screens/           # Tampilan halaman daftar, tambah/edit tagihan
        │   │   ├── widgets/           # Komponen UI khusus untuk fitur tagihan
        │   │   └── providers/         # Riverpod provider untuk state tagihan
        │   │
        │   ├── category/              # Fitur Manajemen Kategori
        │   │   ├── screens/           # Halaman untuk mengelola kategori
        │   │   ├── widgets/           # Komponen UI khusus kategori
        │   │   └── providers/         # Riverpod provider untuk state kategori
        │   │
        │   ├── calendar/              # Fitur Kalender
        │   │   ├── screens/           # Halaman utama kalender
        │   │   ├── widgets/           # Komponen UI kalender
        │   │   └── providers/         # Riverpod provider untuk state kalender
        │   │
        │   └── settings/              # Fitur Pengaturan
        │       ├── screens/           # Halaman pengaturan aplikasi
        │       ├── widgets/           # Komponen UI pengaturan
        │       └── providers/         # Riverpod provider untuk state pengaturan
        │
        ├── global_widgets/            # Widget yang bisa dipakai di banyak fitur
        └── routing/                   # Konfigurasi GoRouter & navigasi
            └── app_router.dart
```

📌 **Dokumen SDD ini akan digunakan sebagai pedoman resmi dalam implementasi aplikasi Tagihanku.**
"""
