# 🚀 LeadSight: AI-Powered Banking Lead Scoring Dashboard

![Status](https://img.shields.io/badge/Status-Production%20Ready-success?style=for-the-badge)
![Tech Stack](https://img.shields.io/badge/Stack-MERN%20%2B%20Python%20ML-blue?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)

**LeadSight** adalah platform dashboard analitik berbasis AI yang dirancang untuk merevolusi strategi telemarketing perbankan. Aplikasi ini memprediksi nasabah mana yang memiliki probabilitas tertinggi untuk membuka deposito berjangka, memungkinkan tim sales bekerja lebih cerdas dengan memprioritaskan prospek berkualitas tinggi.

---

## 📋 Daftar Isi
1. [Tutorial Penggunaan (User Flow)](#-tutorial-penggunaan-berdasarkan-user-flow)
2. [Spesifikasi & Arsitektur](#-spesifikasi--arsitektur-proyek)
3. [Dokumentasi Frontend](#-leadsight--frontend)
4. [Dokumentasi Backend](#-leadsight--backend)
5. [Dokumentasi Machine Learning](#-leadsight--machine-learning)
6. [Instalasi Lokal](#-instalasi--setup-lokal)

---

## 💻 Tutorial Penggunaan (Berdasarkan User Flow)

Berikut adalah panduan langkah demi langkah penggunaan aplikasi LeadSight, mulai dari login hingga pengelolaan data nasabah, sesuai dengan alur sistem.

### 1. Kredensial Login (Mode Demo)
Gunakan akun berikut untuk masuk ke dalam sistem:

| Role | Username | Password | Fitur yang Dapat Diakses |
| :--- | :--- | :--- | :--- |
| **Admin** | `admin.leadsight` | `leadsight123` | Akses Penuh: Dashboard Statistik, Insight AI, Semua Data Nasabah, Input Data Baru. |
| **Sales** | `sales.user` | `leadsight123` | Akses Terbatas: Fokus pada menu Promotion (Daftar Leads) dan pengelolaan Notes. |

### 2. Alur Penggunaan Aplikasi

#### A. Memulai & Login
1.  Buka aplikasi. Halaman pertama yang muncul adalah **Login**.
2.  Masukkan Username dan Password (gunakan kredensial demo di atas).
3.  Klik tombol Login.
    * *(Opsional: Jika lupa password, klik "Forgot Password" untuk alur pemulihan).*
4.  Setelah berhasil login, Anda akan diarahkan ke menu utama.

#### B. Menu Utama (Memilih Navigasi)
Setelah login, Anda dapat memilih tiga menu utama:

**Jalur 1: Dashboard (Melihat Analitik)**
1.  Pilih menu **Dashboard**.
2.  Di sini Anda dapat **Melihat** berbagai visualisasi makro:
    * **Tren Chart:** Melihat performa kampanye dari waktu ke waktu.
    * **Customer Chart:** Melihat distribusi demografi nasabah secara umum.
    * **Campaign Chart:** Melihat statistik keberhasilan kampanye.
3.  Dari widget Customer Table di dashboard, Anda bisa mengklik nasabah tertentu untuk melihat **Customer Demografi** detailnya dan mendapatkan **AI Insight** spesifik untuk nasabah tersebut.

**Jalur 2: Promotion (Fokus Sales & Follow-up)**
1.  Pilih menu **Promotion**. Ini adalah area kerja utama untuk sales.
2.  Anda akan melihat **Customer Table** yang berisi daftar leads lengkap dengan Skor Prioritas.
3.  Klik pada salah satu nama nasabah. Halaman akan berpindah ke detail **Customer Demografi**.
4.  Di halaman detail, Anda akan melihat **AI Insight** (rekomendasi tindakan).
5.  Di bagian ini, Anda dapat mengelola catatan interaksi (Notes):
    * **Add Notes:** Menambahkan hasil panggilan baru (misal: "Nasabah tertarik, hubungi besok").
    * **Edit Notes:** Mengubah catatan yang sudah ada.
    * **Remove Notes:** Menghapus catatan yang tidak relevan.

**Jalur 3: Customer Entry (Input Data Baru)**
(Biasanya hanya untuk role Admin/Data Entry)
1.  Pilih menu **Customer Entry**.
2.  Isi formulir data diri nasabah baru.
3.  Klik **Save New Customer**. Data akan tersimpan ke database dan akan diproses oleh AI untuk mendapatkan skor di kemudian hari.

---

## 🏗 Spesifikasi & Arsitektur Proyek

LeadSight menggunakan arsitektur modern yang memisahkan tanggung jawab antara antarmuka, logika bisnis, data, dan kecerdasan buatan.

### Diagram Alur Data & Logika Prioritas

1.  **Frontend** mengirim data nasabah ke **Backend**.
2.  **Backend** meneruskan data ke **ML Engine**.
3.  **ML Engine** menganalisis dan mengembalikan **Skor Probabilitas Mentah** (misal: 0.75, 0.30).
4.  ⭐ **Logika Prioritas di Database:** Backend menyimpan skor tersebut ke Database. Database kemudian menentukan **Label Prioritas** berdasarkan aturan berikut:
    * Jika Skor Probabilitas > 50% (0.50) → Label: **Priority**
    * Jika Skor Probabilitas ≤ 50% (0.50) → Label: **Non-Priority**
5.  Data yang sudah berlabel dikirim kembali ke Frontend untuk ditampilkan.

---

## 🎨 LeadSight — Frontend

Bagian antarmuka pengguna yang interaktif, responsif, dan kaya animasi.

### Tech Stack Frontend
* **Bundler:** Vite (React)
* **Styling:** TailwindCSS
* **UI Components & Charts:** shadcn/ui (Menggunakan Recharts di dalamnya untuk grafik)
* **Animations:** Framer Motion (Untuk animasi button, navbar, dan transisi halaman)
* **State Management:** React Context API & Custom Hooks
* **Data Fetching:** Axios

### Struktur Folder
```bash
src/
├── api/                    # API client & endpoints (Axios instances)
│   └── api.js
├── components/
│   ├── ui/                 # Reusable UI components (Button, Input, Card dari shadcn)
│   ├── dashboard/          # Dashboard widgets & charts components
│   ├── common/             # Shared components (Table, Searchbar, Pagination, LoadingSpinner)
│   └── layout/             # Layout utama, Navbar, Sidebar (dengan Framer Motion)
├── contexts/               # React Context providers (AuthContext, ThemeContext)
├── hooks/                  # Custom hooks (useCustomers, useStats, useAuth)
├── pages/                  # Page components (Login, Dashboard, Promotion, CustomerEntry)
├── lib/                    # Utils dan helper functions (cn untuk tailwind merge)
├── styles/                 # Global CSS dan Tailwind directives
├── App.jsx                 # Main routing setup
└── main.jsx                # Entry point
