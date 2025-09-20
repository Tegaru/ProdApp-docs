# Dokumen SRS (Software Requirements Specification) - ProdApp

**Versi:** 1.0
**Tanggal:** 20 September 2025
**Penulis:** [Tegar Miftaqur Rohim dan Tim]

---

## 1. Pendahuluan

*   **1.1. Tujuan Dokumen**
    Dokumen ini merinci persyaratan fungsional dan non-fungsional untuk aplikasi mobile "SimpleDo". Tujuannya adalah untuk menjadi panduan teknis yang jelas bagi tim pengembangan, penguji, dan pemangku kepentingan lainnya, memastikan pemahaman yang seragam tentang apa yang akan dibangun.
*   **1.2. Lingkup Produk**
    SimpleDo adalah aplikasi mobile *to-do list* dan perencana produktivitas ringan yang dirancang untuk platform iOS dan Android. Aplikasi ini akan membantu pengguna dalam manajemen tugas harian, pengingat, dan kategorisasi tugas dengan antarmuka yang intuitif dan minimalis.
*   **1.3. Definisi, Akronim, dan Singkatan**
    *   **PRD:** Product Requirement Document
    *   **SRS:** Software Requirements Specification
    *   **MVP:** Minimum Viable Product
    *   **UI:** User Interface
    *   **UX:** User Experience
    *   **API:** Application Programming Interface (untuk kebutuhan di masa depan, saat ini tidak ada API eksternal)
    *   **Drift:** Database SQLite untuk Flutter
    *   **Riverpod:** State management framework untuk Flutter
    *   **GoRouter:** Routing package untuk Flutter
    *   **CRUD:** Create, Read, Update, Delete
*   **1.4. Referensi**
    *   Dokumen PRD - ProdApp, Versi 1.0, 20 September 2025.
    *   Skema ERD - ProdApp, Versi 1.0, 20 September 2025.

---

## 2. Deskripsi Umum

*   **2.1. Perspektif Produk**
    SimpleDo adalah aplikasi *standalone* mobile yang beroperasi secara *offline-first* dengan penyimpanan data lokal. Saat ini tidak ada integrasi dengan sistem eksternal lain.
*   **2.2. Fungsi Produk**
    Aplikasi ini akan memungkinkan pengguna untuk:
    *   Membuat, melihat, mengedit, dan menghapus tugas.
    *   Menandai tugas sebagai selesai.
    *   Mengatur kategori untuk tugas.
    *   Menetapkan pengingat waktu spesifik untuk tugas.
    *   Menerima notifikasi lokal.
    *   Melihat tugas dalam tampilan daftar dan kalender dasar.
    *   Mengatur tema aplikasi (Light/Dark Mode).
*   **2.3. Karakteristik Pengguna**
    *   **Pengguna Umum:** Mencari aplikasi *to-do list* yang sederhana, cepat, dan mudah digunakan. Tidak memerlukan akun atau fitur kolaborasi.
    *   **Pengguna PRO (Iterasi Berikutnya):** Menginginkan fitur-fitur lanjutan seperti kustomisasi tema lebih banyak, widget, tugas berulang, prioritas, dll.
*   **2.4. Batasan Umum**
    *   Platform: Android.
    *   Tidak ada fitur autentikasi/akun pengguna di MVP.
    *   Data disimpan secara lokal di perangkat pengguna.
    *   Penggunaan bahasa Indonesia sebagai bahasa utama di MVP.
    *   Tidak ada integrasi API eksternal di MVP.

---

## 3. Persyaratan Fungsional (Functional Requirements)

Persyaratan fungsional diuraikan berdasarkan modul atau fitur utama.

*   **3.1. Manajemen Tugas**
    *   **FR.TM.1.001 - Menambahkan Tugas Baru:**
        *   Deskripsi: Pengguna harus dapat menambahkan tugas baru.
        *   Input: Judul tugas (wajib), tanggal jatuh tempo (opsional), kategori (opsional, default 'Umum').
        *   Output: Tugas baru tersimpan dan muncul di daftar tugas.
        *   Validasi: Judul tugas tidak boleh kosong.
    *   **FR.TM.1.002 - Melihat Daftar Tugas:**
        *   Deskripsi: Sistem harus menampilkan semua tugas yang ada.
        *   Aksi: Menampilkan daftar tugas, dengan kemampuan filter berdasarkan kategori dan status selesai/belum selesai (melalui tab).
        *   Output: Daftar tugas yang terorganisir.
    *   **FR.TM.1.003 - Menandai Tugas Selesai:**
        *   Deskripsi: Pengguna harus dapat menandai tugas sebagai selesai atau belum selesai.
        *   Aksi: Interaksi UI (misal: checkbox) untuk mengubah status `is_completed` tugas.
        *   Output: Status tugas diperbarui di database dan UI.
    *   **FR.TM.1.004 - Mengedit Tugas:**
        *   Deskripsi: Pengguna harus dapat mengubah detail tugas yang sudah ada.
        *   Input: Judul, tanggal jatuh tempo, kategori tugas yang dipilih.
        *   Output: Detail tugas diperbarui di database dan UI.
    *   **FR.TM.1.005 - Menghapus Tugas:**
        *   Deskripsi: Pengguna harus dapat menghapus tugas yang sudah ada.
        *   Aksi: Konfirmasi penghapusan untuk mencegah penghapusan yang tidak disengaja.
        *   Output: Tugas dihapus dari database dan UI.
*   **3.2. Manajemen Kategori**
    *   **FR.CAT.1.001 - Menetapkan Kategori ke Tugas:**
        *   Deskripsi: Pengguna harus dapat memilih kategori yang sudah ada saat membuat atau mengedit tugas.
        *   Input: Pilihan kategori dari daftar yang tersedia.
        *   Output: Tugas terkait dengan kategori yang dipilih.
    *   **FR.CAT.1.002 - Melihat Daftar Kategori:**
        *   Deskripsi: Sistem harus menampilkan daftar kategori yang tersedia.
        *   Aksi: Dapat diakses melalui Drawer Menu atau tab filter.
        *   Output: Daftar kategori yang dapat dipilih.
*   **3.3. Pengingat dan Notifikasi**
    *   **FR.REM.1.001 - Menetapkan Waktu Pengingat:**
        *   Deskripsi: Pengguna harus dapat menetapkan waktu spesifik untuk pengingat tugas.
        *   Input: Tanggal dan waktu dari *date/time picker*.
        *   Output: Pengingat tersimpan di database dan dijadwalkan secara lokal.
    *   **FR.REM.1.002 - Menerima Notifikasi Lokal:**
        *   Deskripsi: Sistem harus mengirimkan notifikasi push ke perangkat pengguna pada waktu pengingat yang ditentukan.
        *   Output: Notifikasi muncul di bar notifikasi perangkat.
        *   Persyaratan: Pengguna harus memberikan izin notifikasi kepada aplikasi.
*   **3.4. Kalender**
    *   **FR.CAL.1.001 - Melihat Tugas pada Kalender:**
        *   Deskripsi: Sistem harus menampilkan tampilan kalender bulanan dasar yang mengindikasikan tanggal-tanggal dengan tugas terjadwal.
        *   Aksi: Mengklik tanggal di kalender harus menampilkan daftar tugas untuk tanggal tersebut.
        *   Output: Tampilan kalender dengan indikator tugas.
*   **3.5. Pengaturan**
    *   **FR.SET.1.001 - Mengatur Preferensi Notifikasi:**
        *   Deskripsi: Pengguna harus dapat mengaktifkan/menonaktifkan notifikasi secara global, serta mengatur suara dan getar.
        *   Input: Toggle switch untuk notifikasi, suara, getar.
        *   Output: Preferensi disimpan secara lokal dan diterapkan pada notifikasi yang akan datang.
    *   **FR.SET.1.002 - Mengatur Tema Aplikasi:**
        *   Deskripsi: Pengguna harus dapat memilih antara Light Mode atau Dark Mode.
        *   Input: Pilihan "Light" atau "Dark" (atau "System Default").
        *   Output: Tema UI aplikasi berubah sesuai pilihan.

---

## 4. Persyaratan Non-Fungsional (Non-Functional Requirements)

*   **4.1. Kinerja (Performance)**
    *   **NFR.PER.1.001 - Kecepatan Respon UI:** Aplikasi harus merespon interaksi pengguna (misalnya, menambahkan tugas, menandai selesai) dalam waktu kurang dari 500 ms.
    *   **NFR.PER.1.002 - Waktu Loading:** Waktu *startup* aplikasi tidak boleh lebih dari 3 detik pada perangkat kelas menengah.
    *   **NFR.PER.1.003 - Penggunaan Memori:** Aplikasi harus memiliki jejak memori yang rendah untuk memastikan kinerja yang lancar di berbagai perangkat.
    *   **NFR.PER.1.004 - Kinerja Database:** Operasi CRUD pada database lokal (Drift) harus diselesaikan dalam waktu kurang dari 200 ms untuk hingga 1000 tugas.
*   **4.2. Keamanan (Security)**
    *   **NFR.SEC.1.001 - Perlindungan Data Lokal:** Data pengguna yang disimpan secara lokal harus dilindungi dari akses tidak sah oleh aplikasi lain, sesuai dengan standar keamanan platform iOS dan Android.
    *   **NFR.SEC.1.002 - Keamanan Komponen Pihak Ketiga:** Semua *dependency* dan *package* pihak ketiga harus diperiksa secara berkala untuk kerentanan keamanan yang diketahui.
*   **4.3. Keandalan (Reliability)**
    *   **NFR.REL.1.001 - Penanganan Crash:** Aplikasi harus dapat menangani error dan *crash* dengan anggun, meminimalkan kehilangan data dan menyediakan *feedback* yang informatif jika terjadi masalah.
    *   **NFR.REL.1.002 - Konsistensi Data:** Data tugas dan kategori harus tetap konsisten setelah operasi CRUD, bahkan setelah aplikasi ditutup atau perangkat mati.
    *   **NFR.REL.1.003 - Notifikasi Tepat Waktu:** Notifikasi pengingat harus dikirimkan pada waktu yang ditentukan dengan tingkat akurasi tinggi (deviasi maksimum 1-2 menit).
*   **4.4. Kemampuan Pemeliharaan (Maintainability)**
    *   **NFR.MNT.1.001 - Arsitektur Bersih:** Aplikasi harus dibangun mengikuti prinsip-prinsip Clean Architecture untuk memastikan kode yang modular, mudah dipahami, dan mudah diubah.
    *   **NFR.MNT.1.002 - Dokumentasi Kode:** Kode sumber harus didokumentasikan dengan baik (komentar, *docstrings*) untuk memfasilitasi pemahaman dan pemeliharaan oleh tim di masa mendatang.
    *   **NFR.MNT.1.003 - Testing:** Komponen utama (terutama Domain dan Data Layer) harus memiliki *unit test* dan *integration test* yang memadai.
*   **4.5. Portabilitas (Portability)**
    *   **NFR.POR.1.001 - Kompatibilitas Platform:** Aplikasi harus berfungsi dengan baik di berbagai versi iOS dan Android (misalnya, Android 7.0+ dan iOS 12+).
    *   **NFR.POR.1.002 - Ukuran Layar:** UI aplikasi harus beradaptasi secara responsif dengan berbagai ukuran dan orientasi layar perangkat mobile.
*   **4.6. Kegunaan (Usability)**
    *   **NFR.USA.1.001 - Desain Minimalis:** Antarmuka pengguna harus bersih, tidak berantakan, dan fokus pada fungsi inti.
    *   **NFR.USA.1.002 - Navigasi Intuitif:** Pengguna harus dapat memahami dan menavigasi aplikasi dengan mudah tanpa pelatihan.
    *   **NFR.USA.1.003 - Konsistensi UI:** Elemen UI dan pola interaksi harus konsisten di seluruh aplikasi.
    *   **NFR.USA.1.004 - Aksesibilitas:** Aplikasi harus mempertimbangkan dasar-dasar aksesibilitas (misalnya, ukuran font yang dapat dibaca, kontras warna yang memadai, dukungan *screen reader* minimal).

---

## 5. Arsitektur Sistem

*   **5.1. Komponen Arsitektur**
    *   **Presentation Layer (Flutter):**
        *   UI Framework: Flutter
        *   State Management: Riverpod Generator
        *   Routing: GoRouter
        *   Widget: Materi desain yang konsisten.
    *   **Domain Layer:**
        *   Business Logic: Use Cases (interactors)
        *   Data Models: Entities (menggunakan Freezed untuk immutability)
        *   Functional Programming: fpdart (untuk penanganan error dengan `Either`)
        *   Dependencies: Repositories (interfaces)
    *   **Data Layer:**
        *   Database: Drift (SQLite) untuk penyimpanan lokal.
        *   Repository Implementations: Mengimplementasikan interface repository dari Domain Layer.
        *   Data Sources: Mengelola interaksi langsung dengan database.
        *   Logging: `logger` package untuk debugging.
*   **5.2. Aliran Data (Conceptual)**
    *   UI memanggil Use Case di Domain Layer.
    *   Use Case mengkoordinasikan dengan Repository Interface di Domain Layer.
    *   Repository Implementation di Data Layer mengakses Data Source (Drift).
    *   Data Source berinteraksi dengan Database lokal.
    *   Hasil dikembalikan melalui jalur yang sama ke UI.
*   **5.3. Struktur Database (Lihat ERD Terlampir)**
    *   Merujuk pada Skema ERD yang telah dibuat sebelumnya, mencakup tabel `Categories`, `Tasks`, dan `Reminders` beserta kolom dan hubungan antar mereka.

---

## 6. Model Data

*   **6.1. Entitas `Category`**
    *   `id` (int): Primary Key, Auto Increment
    *   `name` (String): Nama kategori, unik
    *   `color` (String?): Warna hex untuk representasi UI
*   **6.2. Entitas `Task`**
    *   `id` (int): Primary Key, Auto Increment
    *   `title` (String): Judul tugas
    *   `description` (String?): Deskripsi detail tugas (Future)
    *   `due_date` (DateTime?): Tanggal jatuh tempo
    *   `is_completed` (bool): Status selesai (default: false)
    *   `is_starred` (bool): Ditandai sebagai penting (default: false) (Future)
    *   `priority` (String?): Prioritas ('High', 'Medium', 'Low') (Future)
    *   `categoryId` (int?): Foreign Key ke Category.id
    *   `createdAt` (DateTime): Waktu pembuatan
    *   `updatedAt` (DateTime): Waktu terakhir diubah
    *   `isRecurring` (bool): Apakah tugas berulang (Future)
    *   `recurringPattern` (String?): Pola pengulangan (misal: 'daily', 'weekly') (Future)
*   **6.3. Entitas `Reminder`**
    *   `id` (int): Primary Key, Auto Increment
    *   `taskId` (int): Foreign Key ke Task.id
    *   `reminderTime` (DateTime): Waktu pengingat spesifik
    *   `isActive` (bool): Status pengingat aktif/nonaktif (default: true)

---

## 7. Lampiran

*   Wireframes/Mockup (akan ditambahkan)
*   Diagram Arsitektur Clean Code (akan ditambahkan)
*   User Stories (akan ditambahkan)
