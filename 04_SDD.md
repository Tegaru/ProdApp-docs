Dokumen SDD (Software Design Document) - ProdApp

Versi: 1.0
Tanggal: 20 Oktober 2025
Penulis: [Tegar Miftaqur Rohim dan Tim]

1. Pendahuluan

1.1. Tujuan Dokumen
Dokumen Desain Perangkat Lunak (SDD) ini menyediakan deskripsi teknis tentang desain arsitektur dan komponen perangkat lunak untuk aplikasi mobile "ProdApp". Dokumen ini bertujuan untuk memandu tim pengembangan dalam implementasi sistem, memastikan semua persyaratan fungsional dan non-fungsional dari dokumen SRS terpenuhi, serta menjaga konsistensi arsitektur.

1.2. Lingkup Sistem
ProdApp adalah aplikasi to-do list dan perencana produktivitas mobile yang dikembangkan menggunakan Flutter. Desain ini mencakup struktur arsitektur Clean Code, modul-modul utama (Manajemen Tugas, Kategori, Pengingat, Kalender, Pengaturan), serta detail tentang bagaimana lapisan-lapisan dan komponen-komponen berinteraksi untuk mencapai fungsionalitas yang ditentukan dalam PRD dan SRS.

1.3. Definisi, Akronim, dan Singkatan

PRD: Product Requirement Document

SRS: Software Requirements Specification

SDD: Software Design Document

MVP: Minimum Viable Product

UI: User Interface

UX: User Experience

DI: Dependency Injection

CRUD: Create, Read, Update, Delete

Drift: Database SQLite untuk Flutter

Riverpod: State management framework untuk Flutter

GoRouter: Routing package untuk Flutter

Freezed: Code generator untuk data class yang immutable

fpdart: Functional programming utility untuk Dart (error handling)

1.4. Referensi

01_PRD_Product_Requirement_Document.md

02_ERD_Entity_Relationship_Diagram.md

03_SRS_Software_Requirements_Specification.md

2. Gambaran Umum Arsitektur Sistem

ProdApp akan mengadopsi Clean Architecture untuk memastikan pemisahan concerns yang jelas, kemudahan pengujian, dan skalabilitas. Arsitektur ini membagi aplikasi menjadi beberapa lapisan utama: Presentation, Domain, dan Data.
