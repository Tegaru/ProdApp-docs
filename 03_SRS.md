# Dokumen SRS (Software Requirements Specification) - Tagihanku

**Versi:** 1.0
**Tanggal:** 8 November 2025
**Penulis:** [Tegar Miftaqur Rohim dan Tim]

---

## 1. Pendahuluan

*   **1.1. Tujuan Dokumen**
    Dokumen ini merinci persyaratan fungsional dan non-fungsional untuk aplikasi mobile "Tagihanku". Tujuannya adalah untuk menjadi panduan teknis yang jelas bagi tim pengembangan, penguji, dan pemangku kepentingan lainnya, memastikan pemahaman yang seragam tentang apa yang akan dibangun.
*   **1.2. Lingkup Produk**
    Tagihanku adalah aplikasi mobile pencatat dan pengingat tagihan ringan yang dirancang untuk platform Android. Aplikasi ini akan membantu pengguna dalam manajemen tagihan bulanan dan cicilan, mengatur pengingat jatuh tempo, dan mengkategorikan pengeluaran rutin dengan antarmuka yang intuitif dan minimalis.
*   **1.3. Definisi, Akronim, dan Singkatan**
    *   **PRD:** Product Requirement Document
    *   **SRS:** Software Requirements Specification
    *   **ERD:** Entity-Relationship Diagram
    *   **MVP:** Minimum Viable Product
    *   **UI:** User Interface
    *   **UX:** User Experience
    *   **Drift:** Database SQLite untuk Flutter
    *   **Riverpod:** State management framework untuk Flutter
    *   **GoRouter:** Routing package untuk Flutter
    *   **CRUD:** Create, Read, Update, Delete
*   **1.4. Referensi**
    *   Dokumen PRD - Tagihanku, Versi 1.0, 8 November 2025.
    *   Skema ERD - Tagihanku, Versi 1.0, 8 November 2025.

---

## 2. Deskripsi Umum

*   **2.1. Perspektif Produk**
    Tagihanku adalah aplikasi *standalone* mobile yang beroperasi secara *offline-first* dengan penyimpanan data lokal. Saat ini tidak ada integrasi dengan sistem eksternal lain.
*   **2.2. Fungsi Produk**
    Aplikasi ini akan memungkinkan pengguna untuk:
    *   Membuat, melihat, mengedit, dan menghapus data tagihan.
    *   Menandai tagihan sebagai lunas.
    *   Mengatur kategori untuk tagihan (misal: Rumah Tangga, Langganan).
    *   Menetapkan pengingat sebelum tanggal jatuh tempo.
    *   Menerima notifikasi lokal untuk pengingat tagihan.
    *   Melihat tagihan dalam tampilan daftar dan kalender dasar.
    *   Mengatur tema aplikasi (Light/Dark Mode).
*   **2.3. Karakteristik Pengguna**
    *   **Pengguna Umum:** Individu atau kepala keluarga yang ingin melacak tagihan bulanan dan cicilan secara terpusat. Tidak memerlukan akun atau fitur pembayaran.
    *   **Pengguna PRO (Iterasi Berikutnya):** Menginginkan fitur-fitur lanjutan seperti laporan pengeluaran bulanan, sinkronisasi cloud, dan tagihan berulang yang lebih kompleks.
*   **2.4. Batasan Umum**
    *   Platform: Android.
    *   Tidak ada fitur autentikasi/akun pengguna di MVP.
    *   Data disimpan secara lokal di perangkat pengguna.
    *   Penggunaan bahasa Indonesia sebagai bahasa utama di MVP.
    *   Tidak ada integrasi API eksternal di MVP.

---

## 3. Persyaratan Fungsional (Functional Requirements)

*   **3.1. Manajemen Tagihan**
    *   **FR.BIL.1.001 - Menambahkan Tagihan Baru:**
        *   **Deskripsi:** Pengguna harus dapat menambahkan tagihan baru.
        *   **Input:** Nama tagihan (wajib), jumlah tagihan (wajib), tanggal jatuh tempo (wajib), kategori (opsional), siklus tagihan (opsional, default 'Sekali Bayar').
        *   **Output:** Tagihan baru tersimpan dan muncul di daftar tagihan.
        *   **Validasi:** Nama tagihan dan jumlah tidak boleh kosong. Jumlah harus berupa angka.
    *   **FR.BIL.1.002 - Melihat Daftar Tagihan:**
        *   **Deskripsi:** Sistem harus menampilkan semua tagihan yang ada, diurutkan berdasarkan tanggal jatuh tempo terdekat.
        *   **Aksi:** Menampilkan daftar tagihan dengan indikator status (Belum Dibayar, Lunas, Lewat Jatuh Tempo).
        *   **Output:** Daftar tagihan yang terorganisir.
    *   **FR.BIL.1.003 - Menandai Tagihan Lunas:**
        *   **Deskripsi:** Pengguna harus dapat mengubah status tagihan menjadi 'Lunas'.
        *   **Aksi:** Interaksi UI (misal: tombol atau swipe) untuk mengubah status `status` tagihan.
        *   **Output:** Status tagihan diperbarui di database dan UI.
    *   **FR.BIL.1.004 - Mengedit Tagihan:**
        *   **Deskripsi:** Pengguna harus dapat mengubah detail tagihan yang sudah ada.
        *   **Input:** Nama, jumlah, tanggal jatuh tempo, kategori tagihan yang dipilih.
        *   **Output:** Detail tagihan diperbarui di database dan UI.
    *   **FR.BIL.1.005 - Menghapus Tagihan:**
        *   **Deskripsi:** Pengguna harus dapat menghapus tagihan.
        *   **Aksi:** Konfirmasi penghapusan untuk mencegah penghapusan yang tidak disengaja.
        *   **Output:** Tagihan dihapus dari database dan UI.
*   **3.2. Manajemen Kategori**
    *   **FR.CAT.1.001 - Menetapkan Kategori ke Tagihan:**
        *   **Deskripsi:** Pengguna harus dapat memilih kategori saat membuat atau mengedit tagihan.
        *   **Input:** Pilihan kategori dari daftar yang tersedia.
        *   **Output:** Tagihan terkait dengan kategori yang dipilih.
    *   **FR.CAT.1.002 - Melihat Daftar Kategori:**
        *   **Deskripsi:** Sistem harus menampilkan daftar kategori yang tersedia untuk dipilih.
        *   **Aksi:** Dapat diakses melalui Drawer Menu atau saat mengisi form tagihan.
        *   **Output:** Daftar kategori yang dapat dipilih.
*   **3.3. Pengingat dan Notifikasi**
    *   **FR.REM.1.001 - Menetapkan Waktu Pengingat:**
        *   **Deskripsi:** Pengguna harus dapat mengatur pengingat (misal: 1 hari sebelum, 3 hari sebelum) untuk sebuah tagihan.
        *   **Input:** Pilihan waktu pengingat.
        *   **Output:** Pengingat tersimpan di database dan dijadwalkan secara lokal.
    *   **FR.REM.1.002 - Menerima Notifikasi Lokal:**
        *   **Deskripsi:** Sistem harus mengirimkan notifikasi push pada waktu pengingat yang ditentukan.
        *   **Output:** Notifikasi muncul di bar notifikasi perangkat.
        *   **Persyaratan:** Pengguna harus memberikan izin notifikasi kepada aplikasi.
*   **3.4. Kalender**
    *   **FR.CAL.1.001 - Melihat Tagihan pada Kalender:**
        *   **Deskripsi:** Sistem harus menampilkan kalender bulanan yang mengindikasikan tanggal-tanggal jatuh tempo tagihan.
        *   **Aksi:** Mengklik tanggal di kalender akan menampilkan daftar tagihan yang jatuh tempo pada tanggal tersebut.
        *   **Output:** Tampilan kalender dengan indikator tagihan.
*   **3.5. Pengaturan**
    *   **FR.SET.1.001 - Mengatur Preferensi Notifikasi:**
        *   **Deskripsi:** Pengguna dapat mengaktifkan/menonaktifkan notifikasi, serta mengatur suara dan getar.
        *   **Input:** Toggle switch untuk notifikasi, suara, getar.
        *   **Output:** Preferensi disimpan dan diterapkan pada notifikasi.
    *   **FR.SET.1.002 - Mengatur Tema Aplikasi:**
        *   **Deskripsi:** Pengguna dapat memilih antara Light Mode atau Dark Mode.
        *   **Input:** Pilihan "Light", "Dark", atau "System Default".
        *   **Output:** Tema UI aplikasi berubah sesuai pilihan.

---

## 4. Persyaratan Non-Fungsional (Non-Functional Requirements)

*   **4.1. Kinerja (Performance)**
    *   **NFR.PER.1.001 - Kecepatan Respon UI:** Aplikasi harus merespon interaksi pengguna dalam waktu kurang dari 500 ms.
    *   **NFR.PER.1.002 - Waktu Loading:** Waktu *startup* aplikasi tidak boleh lebih dari 3 detik.
    *   **NFR.PER.1.003 - Penggunaan Memori:** Aplikasi harus memiliki jejak memori yang rendah.
    *   **NFR.PER.1.004 - Kinerja Database:** Operasi CRUD pada database lokal (Drift) harus diselesaikan dalam waktu kurang dari 200 ms untuk hingga 1000 tagihan.
*   **4.2. Keamanan (Security)**
    *   **NFR.SEC.1.001 - Perlindungan Data Lokal:** Data pengguna yang disimpan lokal harus dilindungi dari akses tidak sah oleh aplikasi lain.
*   **4.3. Keandalan (Reliability)**
    *   **NFR.REL.1.001 - Penanganan Crash:** Aplikasi harus dapat menangani error dan *crash* dengan baik, meminimalkan kehilangan data.
    *   **NFR.REL.1.002 - Konsistensi Data:** Data tagihan dan kategori harus tetap konsisten setelah operasi CRUD.
    *   **NFR.REL.1.003 - Notifikasi Tepat Waktu:** Notifikasi pengingat harus dikirimkan pada waktu yang ditentukan dengan akurasi tinggi.
*   **4.4. Kemampuan Pemeliharaan (Maintainability)**
    *   **NFR.MNT.1.001 - Arsitektur Bersih:** Aplikasi harus dibangun mengikuti prinsip-prinsip Clean Architecture.
    *   **NFR.MNT.1.002 - Dokumentasi Kode:** Kode sumber harus didokumentasikan dengan baik.
    *   **NFR.MNT.1.003 - Testing:** Komponen utama (Domain dan Data Layer) harus memiliki *unit test* dan *integration test*.
*   **4.5. Portabilitas (Portability)**
    *   **NFR.POR.1.001 - Kompatibilitas Platform:** Aplikasi harus berfungsi dengan baik di berbagai versi Android (misalnya, Android 7.0+).
    *   **NFR.POR.1.002 - Ukuran Layar:** UI aplikasi harus responsif dengan berbagai ukuran layar.
*   **4.6. Kegunaan (Usability)**
    *   **NFR.USA.1.001 - Desain Minimalis:** Antarmuka pengguna harus bersih dan fokus pada fungsi inti.
    *   **NFR.USA.1.002 - Navigasi Intuitif:** Pengguna harus dapat menavigasi aplikasi dengan mudah.
    *   **NFR.USA.1.003 - Konsistensi UI:** Elemen UI harus konsisten di seluruh aplikasi.
    *   **NFR.USA.1.004 - Aksesibilitas:** Aplikasi harus mempertimbangkan dasar-dasar aksesibilitas.

---

## 5. Arsitektur Sistem

*   **5.1. Komponen Arsitektur**
    *   **Presentation Layer (Flutter):** UI Framework, Riverpod Generator, GoRouter.
    *   **Domain Layer:** Use Cases, Entities (Freezed), Repository Interfaces, fpdart.
    *   **Data Layer:** Drift (SQLite), Repository Implementations, Data Sources, `logger`.
*   **5.2. Aliran Data (Conceptual)**
    *   UI memanggil Use Case -> Use Case menggunakan Repository Interface -> Repository Implementation mengakses Data Source (Drift) -> Data Source berinteraksi dengan Database -> Hasil dikembalikan ke UI.
*   **5.3. Struktur Database (Lihat ERD Terlampir)**
    *   Merujuk pada Skema ERD yang telah dibuat, mencakup tabel `Categories`, `Bills`, dan `Reminders` beserta kolom dan hubungan antar mereka.

---

## 6. Model Data

*   **6.1. Entitas `Category`**
    *   `id` (int): Primary Key, Auto Increment
    *   `name` (String): Nama kategori, unik
    *   `color` (String?): Warna hex untuk representasi UI
*   **6.2. Entitas `Bill`**
    *   `id` (int): Primary Key, Auto Increment
    *   `name` (String): Nama tagihan
    *   `amount` (int): Jumlah tagihan
    *   `due_date` (DateTime): Tanggal jatuh tempo
    *   `status` (String): Status tagihan ('unpaid', 'paid', 'overdue')
    *   `categoryId` (int?): Foreign Key ke Category.id
    *   `isRecurring` (bool): Apakah tagihan berulang (default: false)
    *   `recurringPattern` (String?): Pola pengulangan (misal: 'monthly')
    *   `createdAt` (DateTime): Waktu pembuatan
    *   `updatedAt` (DateTime): Waktu terakhir diubah
*   **6.3. Entitas `Reminder`**
    *   `id` (int): Primary Key, Auto Increment
    *   `billId` (int): Foreign Key ke Bill.id
    *   `reminderTime` (DateTime): Waktu pengingat spesifik
    *   `isActive` (bool): Status pengingat aktif/nonaktif (default: true)

---

## 7. Lampiran

*   Wireframes/Mockup (akan ditambahkan)
*   Diagram Arsitektur Clean Code (akan ditambahkan)
*   User Stories (akan ditambahkan)
