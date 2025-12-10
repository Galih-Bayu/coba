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

Berikut adalah panduan langkah demi langkah penggunaan aplikasi LeadSight sesuai dengan alur sistem yang telah dirancang.

### 1. Kredensial Login (Mode Demo)
Gunakan akun berikut untuk masuk ke dalam sistem:

| Role | Username | Password | Fitur yang Dapat Diakses |
| :--- | :--- | :--- | :--- |
| **Demo User** | `user1@gmail.com` | `user1` | Akses ke Dashboard Statistik, Menu Promotion (Leads), Notes, dan Customer Entry. |

### 2. Alur Penggunaan Aplikasi

#### A. Memulai & Login
1.  Buka aplikasi. Halaman pertama yang muncul adalah **Login**.
2.  Masukkan Username dan Password (gunakan kredensial demo di atas).
3.  Klik tombol Login (atau "Forgot Password" jika lupa).
4.  Setelah sukses, Anda akan masuk ke navigasi utama.

#### B. Menu Utama & Navigasi
Terdapat 3 jalur utama yang bisa dipilih user:

**Jalur 1: Dashboard (Analitik)**
* **Aksi:** Klik menu "Dashboard".
* **Fitur:** Anda hanya dapat **Melihat** visualisasi data.
* **Konten:**
    * **Tren Chart:** Grafik performa dari waktu ke waktu (animasi by shadcn).
    * **Customer Chart:** Distribusi demografi nasabah.
    * **Campaign Chart:** Statistik keberhasilan kampanye.
    * **Customer Table (Preview):** Klik salah satu baris untuk melihat detail demografi & AI Insight.

**Jalur 2: Promotion (Sales Operation)**
* **Aksi:** Klik menu "Promotion".
* **Fitur:**
    1.  **Customer Table:** Melihat daftar nasabah lengkap dengan Label Prioritas.
    2.  **Detail Nasabah:** Klik nama nasabah -> Masuk ke halaman Customer Demografi.
    3.  **AI Insight:** Lihat rekomendasi spesifik dari AI.
    4.  **Manajemen Notes:**
        * *Add Notes:* Tambah catatan baru.
        * *Edit Notes:* Ubah catatan.
        * *Remove Notes:* Hapus catatan.

**Jalur 3: Customer Entry (Input Data)**
* **Aksi:** Klik menu "Customer Entry".
* **Fitur:** Form input data nasabah baru.
* **Output:** Klik "Save New Customer" untuk menyimpan ke database.

---

## 🏗 Spesifikasi & Arsitektur Proyek

LeadSight menggunakan arsitektur modern yang memisahkan tanggung jawab antara antarmuka, logika bisnis, data, dan kecerdasan buatan.

### Diagram Logika Prioritas (PENTING)

Sistem menggunakan alur hibrida antara ML dan Database untuk menentukan prioritas nasabah:

1.  **Analisis ML:** Data nasabah dikirim ke Machine Learning. ML hanya mengembalikan **Skor Probabilitas** (angka desimal 0.0 - 1.0).
2.  **Logika Database:** Backend menyimpan skor tersebut. Penentuan label dilakukan oleh Database/Backend dengan logika:
    * `IF (Probability_Score > 0.50)` ➔ Set Label: **PRIORITY** 🔴
    * `IF (Probability_Score <= 0.50)` ➔ Set Label: **NON-PRIORITY** ⚪

---

## 🎨 LeadSight — Frontend

Bagian antarmuka pengguna yang interaktif, responsif, dan kaya animasi.

### Tech Stack Frontend
* **Bundler:** Vite (React Ecosystem)
* **Styling:** TailwindCSS
* **UI Components:** shadcn/ui (Radix UI based)
* **Charting:** Recharts (via shadcn charts) dengan animasi transisi.
* **Micro-Interactions:** Framer Motion (Digunakan untuk animasi Button hover, transisi Navbar, dan kemunculan elemen halaman).
* **Network:** Axios

### Struktur Folder (Source Code)
Struktur direktori disusun rapi untuk skalabilitas:

```bash
src/
├── api/                    # Konfigurasi Axios & Endpoints terpusat
│   └── api.js
├── components/
│   ├── ui/                 # Komponen Reusable (Button, Card, Input - shadcn)
│   ├── dashboard/          # Widget khusus dashboard & chart components
│   ├── common/             # Komponen umum (Table, Searchbar, Pagination)
│   └── layout/             # Struktur utama (Navbar, Sidebar dengan animasi)
├── contexts/               # React Context Providers (Auth, Theme)
├── hooks/                  # Custom Hooks (useCustomers, useStats, useAuth)
├── pages/                  # Halaman Utama (Login, Dashboard, Promotion)
├── lib/                    # Utilities (cn utils, formatters)
├── styles/                 # Global CSS & Tailwind config
├── App.jsx                 # Routing Utama
└── main.jsx                # Entry Point
```

---

## ⚙️ LeadSight — Backend

Server API yang tangguh, aman, dan modular.

### 1. Tech Stack Backend
* **Runtime:** Node.js
* **Framework:** Express.js
* **Validation:** Joi (Validasi input request)
* **Auth:** JWT (JSON Web Token - Access & Refresh Token mechanism)

### 2. Database (Supabase & RPC)
LeadSight menggunakan **PostgreSQL** (via Supabase).
* **RPC (Remote Procedure Calls):** Fungsi analitik berat (seperti menghitung tren harian atau agregasi prioritas) dijalankan langsung di database melalui RPC function, bukan di loop Node.js, untuk performa maksimal.
* **Tabel Utama:** `users`, `authentications`, `customers`, `notes`, `economic_indicators`.

### 3. Environment Variables
Konfigurasi wajib untuk file `.env` (Backend):

```env
NODE_ENV=production
FRONTEND_URL=http://localhost:5173
# Auth Keys
ACCESS_TOKEN_KEY=your_secret_access_key
REFRESH_TOKEN_KEY=your_secret_refresh_key
ACCESS_TOKEN_AGE=3600s
REFRESH_TOKEN_AGE=7d
# Database & AI
SUPABASE_URL=[https://your-project.supabase.co](https://your-project.supabase.co)
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key
GEMINI_API_KEY=your_google_ai_key
```

---

## 🧠 LeadSight — Machine Learning

Otak di balik sistem rekomendasi LeadSight.

### 1. Ringkasan Model
* **Algoritma:** **LightGBM Classifier** (Gradient Boosting).
* **Dataset:** Bank Marketing Dataset (41.188 data).
* **Teknik:** SMOTE (Synthetic Minority Over-sampling) untuk mengatasi data imbalance.

### 2. Output Model
Model ini **tidak** mengeluarkan output berupa teks "Priority". Model hanya mengeluarkan angka probabilitas (misal: `0.78`). Konversi angka ini menjadi label bisnis dilakukan di layer aplikasi/database sesuai ambang batas (threshold) yang ditentukan bisnis (saat ini > 50%).

### 3. Metrik Performa
| Metrik | Nilai |
| :--- | :--- |
| **Accuracy** | **88.72%** |
| **Precision (Yes)**| **54.89%** |
| **Recall (No)** | **95.04%** |

---

## 🛠 Instalasi & Setup Lokal

Ikuti langkah ini untuk menjalankan LeadSight di komputer Anda secara lokal.

### Prasyarat
* Node.js (v16+)
* NPM / Yarn
* Git

### 1. Clone Repository
```bash
git clone [https://github.com/username-anda/leadsight-project.git](https://github.com/username-anda/leadsight-project.git)
cd leadsight-project
```

### 2. Setup Backend
```bash
cd backend
npm install
# Buat file .env sesuai spesifikasi di atas
npm run start
# Server berjalan di http://localhost:5000
```

### 3. Setup Frontend
Buka terminal baru:
```bash
cd frontend
npm install
npm run dev
# Aplikasi berjalan di http://localhost:5173
```

---

## 🧪 Lampiran: Hasil Blackbox Testing

Berikut adalah dokumentasi hasil pengujian fungsional (Blackbox Testing) yang dilakukan untuk memastikan semua fitur berjalan sesuai harapan sebelum rilis (Production).

### 1. Ringkasan Statistik Pengujian

| Keterangan | Jumlah / Hasil | Status |
| :--- | :--- | :--- |
| **Total Skenario Pengujian** | 16 Skenario | - |
| **Skenario Berhasil** | 16 | ✅ |
| **Skenario Gagal** | 0 | ❌ |
| **Persentase Kegagalan** | 0% | - |
| **Persentase Keberhasilan** | **100%** | **PASSED** |

Rincian pengujian per modul dan fitur:

| No | Modul | Skenario Pengujian | Harapan | Hasil |
|:--:|:---|:---|:---|:---:|
| 1 | **Login** | Memasukkan email & password valid | Menampilkan dashboard utama | ✅ |
| 2 | **Login** | Memasukkan password salah | Muncul pesan error “Invalid credentials” | ✅ |
| 3 | **Dashboard** | Berhasil login | Semua grafik, card statistik, dan daftar nasabah tampil | ✅ |
| 4 | **Customer List** | Membuka halaman nasabah list | Tabel nasabah muncul dengan filtering dan sorting | ✅ |
| 5 | **Search** | Mengetik nama pada search bar | Tabel menampilkan hasil pencarian yang sesuai | ✅ |
| 6 | **Filter** | Memilih filter (contoh: prioritas “High”) | Tabel menampilkan data terpilih | ✅ |
| 7 | **Detail** | Klik salah satu nasabah | Muncul detail nasabah lengkap, AI insight, catatan promosi | ✅ |
| 8 | **Input Form** | Mengisi semua field form tambah nasabah | Nasabah baru berhasil disimpan dan tampil di daftar | ✅ |
| 9 | **Validation** | Mengosongkan field wajib pada form | Muncul pesan kesalahan “Field wajib diisi” | ✅ |
| 10 | **Prediction** | Submit form prediksi nasabah baru | Muncul probability score, priority label, dan rekomendasi | ✅ |
| 11 | **Notes** | Menulis catatan promosi baru | Catatan tersimpan dan muncul di riwayat promosi nasabah | ✅ |
| 12 | **Edit Notes** | Mengubah catatan yang sudah dibuat | Catatan ter-update dengan nilai baru | ✅ |
| 13 | **Campaign** | Membuka halaman campaign | Grafik campaign effectiveness tampil seluruhnya | ✅ |
| 14 | **Logout** | Menekan tombol logout | Sistem kembali ke halaman login | ✅ |
| 15 | **Responsive** | Membuka website di layar kecil (mobile) | Tampilan tetap rapi dan menu dapat diakses | ✅ |
| 16 | **Error Handling** | Simulasi server down / gagal fetch | Sistem menampilkan pesan error “Failed to load data” | ✅ |

**LeadSight Project**
*Dikembangkan sebagai bagian dari program Asah Digital led by Dicoding in association with Accenture.*

Copyright © 2024 LeadSight Team.
