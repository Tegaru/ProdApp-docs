# Dokumen PRD (Product Requirement Document) - Tagihanku

**Versi:** 1.0
**Tanggal:** 8 November 2025
**Penulis:** [Tegar Miftaqur Rohim dan Tim]

---

## 1. Pendahuluan (Introduction)

**Tagihanku** adalah aplikasi pencatat dan pengingat tagihan mobile yang dirancang untuk membantu pengguna mengelola dan melacak semua tagihan mereka secara efisien. Dengan fokus pada kesederhanaan dan kinerja ringan, Tagihanku bertujuan untuk menjadi alat yang intuitif dan mudah digunakan bagi siapa saja yang ingin menghindari denda keterlambatan dan mengelola keuangan pribadi dengan lebih baik. Aplikasi ini akan dikembangkan menggunakan Flutter dengan arsitektur Clean Code untuk memastikan skalabilitas, kemudahan pemeliharaan, dan pengalaman pengguna yang optimal.

---

## 2. Tujuan Produk (Product Goals)

- Menyediakan platform pencatatan tagihan yang sederhana, cepat, dan mudah digunakan.
- Membantu pengguna mengelola semua tagihan di satu tempat, mulai dari tagihan bulanan hingga cicilan.
- Mengurangi risiko lupa membayar tagihan melalui sistem pengingat yang andal.
- Memberikan gambaran umum tentang pengeluaran rutin pengguna.
- Menawarkan pengalaman pengguna yang bersih, minimalis, dan intuitif.
- Memastikan kinerja aplikasi yang ringan dan responsif di berbagai perangkat.

---

## 3. Target Pengguna (Target Audience)

- Individu dan kepala keluarga yang ingin melacak tagihan bulanan (listrik, air, internet, dll.).
- Profesional muda, pekerja lepas, dan siapa saja yang memiliki banyak langganan atau cicilan.
- Pengguna yang mengutamakan kecepatan dan kemudahan dalam mengelola keuangan pribadi.

---

## 4. Lingkup Fitur (Feature Scope)

Berikut adalah daftar fitur utama yang akan diimplementasikan:

- **Manajemen Tagihan:**
    - **Menambahkan Tagihan Baru:** Pengguna dapat memasukkan nama tagihan, jumlah tagihan, tanggal jatuh tempo, dan memilih kategori (misal: Rumah Tangga). Pengguna juga bisa mengatur siklus tagihan (misal: Bulanan, Sekali Bayar).
    - **Melihat Daftar Tagihan:** Menampilkan semua tagihan yang akan datang, diurutkan berdasarkan tanggal jatuh tempo terdekat. Memberikan indikator status (Belum Dibayar, Sudah Dibayar, Lewat Jatuh Tempo).
    - **Menandai Tagihan Lunas:** Pengguna dapat mengubah status tagihan menjadi "Lunas".
    - **Mengedit Tagihan:** Mengubah detail tagihan (nama, jumlah, tanggal jatuh tempo, kategori).
    - **Menghapus Tagihan:** Menghapus data tagihan.
- **Kategori Tagihan:** Fitur untuk mengelompokkan tagihan (e.g., Rumah Tangga, Langganan, Cicilan, Asuransi).
- **Pengingat:**
    - **Menetapkan Waktu Pengingat:** Pengguna dapat mengatur pengingat beberapa hari sebelum tanggal jatuh tempo.
    - **Notifikasi Lokal:** Aplikasi akan mengirimkan notifikasi push ke perangkat pengguna pada waktu yang ditentukan.
- **UI/UX Dasar:**
    - **Halaman Utama (Dashboard):** Tampilan utama yang merangkum total tagihan bulan ini, yang sudah dibayar, dan sisa tagihan. Di bawahnya terdapat daftar tagihan terdekat.
    - **Form Penambahan/Edit Tagihan:** Modal atau halaman terpisah untuk input detail tagihan.
    - **Halaman Pengaturan Dasar:** Mencakup pengaturan notifikasi (on/off, suara) dan tema (Light/Dark Mode).
    - **Drawer Menu Sederhana:** Navigasi ke Pengaturan, Kategori, dan "Tentang Aplikasi".
- **Kalender:**
    - Tampilan kalender bulanan dasar yang menunjukkan tanggal dengan tagihan jatuh tempo.

---

## 5. Arsitektur Teknis (Technical Architecture)

Aplikasi akan dibangun menggunakan pendekatan Arsitektur Clean Code untuk memisahkan *concerns* dan memastikan modularitas.

- **Lapisan Presentasi (Presentation Layer):**
    - Dibangun dengan Flutter untuk UI dan interaksi pengguna.
    - Menggunakan Riverpod Generator untuk state management yang reaktif dan type-safe.
    - GoRouter untuk navigasi antar halaman.
- **Lapisan Domain (Domain Layer):**
    - Berisi business logic inti aplikasi.
    - Definisi entities (model data) dan use cases (interactor).
    - Model data akan menggunakan Freezed untuk immutability.
    - Menggunakan fpdart untuk functional programming (misal: Either untuk penanganan error).
- **Lapisan Data (Data Layer):**
    - Mengimplementasikan repositories dan data sources.
    - Drift (SQLite) akan digunakan sebagai database lokal untuk penyimpanan data persisten.
    - Penggunaan logger untuk tujuan debugging dan monitoring.

---

## 6. User Interface (UI) dan User Experience (UX)

- **Desain Minimalis:** Bersih, sederhana, fokus pada keterbacaan informasi tagihan (jumlah, tanggal jatuh tempo).
- **Intuitif:** Alur pengguna yang mudah dipahami dan dinavigasi.
- **Konsisten:** Elemen desain yang konsisten di seluruh aplikasi.
- **Responsif:** Tata letak yang beradaptasi dengan berbagai ukuran layar perangkat.
- **Tema:** Dukungan Light dan Dark Mode.
- **Notifikasi:** Notifikasi yang jelas dan tepat waktu.

---

## 7. Metrik Keberhasilan (Success Metrics)

- **Adopsi Pengguna:** Jumlah unduhan dan pengguna aktif harian/mingguan.
- **Retensi Pengguna:** Tingkat retensi pengguna dalam 7, 30, dan 90 hari.
- **Engagement:** Frekuensi penambahan tagihan, persentase tagihan yang ditandai lunas, dan tingkat penggunaan pengingat.
- **Rating Aplikasi:** Rata-rata rating di toko aplikasi.
- **Kinerja:** Kecepatan loading, kelancaran aplikasi, dan penggunaan memori.

---

## 8. Batasan (Constraints)

- **Platform:** Android.
- **Monetisasi:** Versi awal gratis dengan fitur inti; model freemium dengan fitur PRO (misal: laporan bulanan, sinkronisasi cloud) bisa dipertimbangkan di masa depan.
- **Bahasa:** Bahasa Indonesia sebagai bahasa utama (untuk rilis awal).
- **Autentikasi:** Tidak ada fitur login/akun pengguna untuk MVP awal (data tersimpan lokal).

---

## 9. Rencana Rilis (Release Plan)

- **MVP (Alpha Release):** Targetkan fitur inti manajemen tagihan, pengingat dasar, dan UI/UX minimalis. Pengujian internal.
- **Beta Release:** Perbaikan bug dan feedback dari kelompok pengguna terbatas.
- **Public Release:** Rilis di Google Play Store.
- **Iterasi Selanjutnya:** Pengembangan fitur lanjutan (misal: laporan pengeluaran, fitur PRO) berdasarkan feedback pengguna.

---

## 10. Lampiran (Appendices)

- Wireframes atau mockup desain (akan ditambahkan setelah PRD ini disetujui).
- Diagram arsitektur Clean Code.
- Definisi model data awal (Drift schema).
