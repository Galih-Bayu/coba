# 🚀 LeadSight: AI-Powered Banking Lead Scoring Dashboard

![Status](https://img.shields.io/badge/Status-Production%20Ready-success?style=for-the-badge)
![Tech Stack](https://img.shields.io/badge/Stack-MERN%20%2B%20Python%20ML-blue?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)

**LeadSight** adalah platform dashboard analitik berbasis AI yang dirancang untuk merevolusi strategi telemarketing perbankan. Aplikasi ini memprediksi nasabah mana yang memiliki probabilitas tertinggi untuk membuka deposito berjangka, memungkinkan tim sales bekerja lebih cerdas dengan memprioritaskan prospek berkualitas tinggi.

---

## 📋 Daftar Isi
1. [Tutorial & Akses Demo](#-tutorial-singkat--akses-demo)
2. [Spesifikasi & Arsitektur](#-spesifikasi--arsitektur-proyek)
3. [Dokumentasi Frontend](#-leadsight--frontend)
4. [Dokumentasi Backend](#-leadsight--backend)
5. [Dokumentasi Machine Learning](#-leadsight--machine-learning)
6. [Instalasi Lokal](#-instalasi--setup-lokal)

---

## 💻 Tutorial Singkat & Akses Demo

Aplikasi ini menampilkan daftar nasabah yang telah dinilai oleh AI. Ikuti panduan berikut untuk mencoba aplikasi:

### 1. Kredensial Login (Mode Demo)
Gunakan akun berikut untuk masuk ke dalam sistem:

| Role | Username | Password | Fitur yang Dapat Diakses |
| :--- | :--- | :--- | :--- |
| **Admin** | `admin.leadsight` | `leadsight123` | Akses Penuh: Dashboard Statistik, User Management, Insight AI, Semua Data Nasabah. |
| **Sales** | `sales.user` | `leadsight123` | Akses Terbatas: Hanya daftar Leads (Nasabah), Input Catatan (Notes), Update Status. |

### 2. Cara Menggunakan Fitur Utama
* **Membaca Skor Prioritas:**
  Pada halaman **Leads**, perhatikan kolom **Priority Label** dan **Probability Score**:
  * 🔴 **High Priority (Score ≥ 0.7):** Nasabah "Panas". Wajib dihubungi segera. Peluang closing tinggi.
  * 🟡 **Medium Priority (0.4 - 0.7):** Nasabah potensial, butuh pendekatan persuasif.
  * ⚪ **Non-Priority (< 0.4):** Peluang kecil. Prioritas terakhir atau gunakan email marketing saja.

* **Pencatatan Follow-up:**
  Klik nama nasabah -> Buka tab **Notes** -> Masukkan hasil panggilan (misal: "Tertarik, minta ditelepon besok siang").

---

## 🏗 Spesifikasi & Arsitektur Proyek

LeadSight menggunakan arsitektur modern yang memisahkan tanggung jawab antara antarmuka, logika bisnis, data, dan kecerdasan buatan.

### Diagram Alur Data
`[User/Frontend]` ↔ `[Backend API (Node.js)]` ↔ `[Database (Supabase)]`
                                      ↕
                           `[ML Engine (Python/LightGBM)]`

### Teknologi Utama
* **Frontend:** React.js, Vite, TailwindCSS, Shadcn UI, Recharts.
* **Backend:** Node.js, Express.js, JWT Authentication.
* **Database:** Supabase (PostgreSQL) dengan RPC Functions.
* **Machine Learning:** Python, Scikit-Learn, LightGBM, SMOTE.
* **Deployment:** Vercel (FE) & Railway (BE).

---

## 🎨 LeadSight — Frontend

Bagian antarmuka pengguna yang interaktif dan responsif.

### Struktur Folder
```bash
src/
├── api/            # Konfigurasi Axios & Endpoints
├── components/
│   ├── ui/         # Komponen atomik (Button, Card, Badge - shadcn)
│   ├── dashboard/  # Widget grafik & statistik
│   ├── common/     # Tabel, Pagination, Sidebar
│   └── layout/     # Layout utama aplikasi
├── contexts/       # AuthContext (JWT Handling) & ThemeContext
├── hooks/          # Custom hooks (useCustomers, useStats)
├── pages/          # Halaman: Login, Dashboard, Leads, Profile
└── lib/            # Utilities (Format Currency, Date Parser)
```

### Fitur Teknis FE
1.  **State Management:** Menggunakan React Context untuk Auth dan Custom Hooks untuk data fetching agar kode lebih bersih.
2.  **Visualisasi Data:** Grafik interaktif menggunakan Recharts dengan wrapper `ChartContainer` agar konsisten dengan tema aplikasi.
3.  **Keamanan Frontend:** Token JWT disimpan di memory (bukan LocalStorage) untuk meminimalisir risiko XSS, dengan mekanisme *silent refresh*.

---

## ⚙️ LeadSight — Backend

Server API yang tangguh, aman, dan modular.

### 1. Tech Stack Backend
* **Node.js & Express:** Dipilih karena performa *non-blocking I/O* dan ekosistem NPM yang kaya.
* **Joi Validation:** Memastikan data input (body/params) valid sebelum diproses controller.
* **JWT:** *Stateless authentication* dengan *Access Token* (pendek) dan *Refresh Token* (panjang).

### 2. Database (Supabase & RPC)
LeadSight menggunakan kekuatan **PostgreSQL** via Supabase. Logika analitik berat tidak dilakukan di Node.js, melainkan langsung di database menggunakan **Remote Procedure Calls (RPC)** untuk kecepatan eksekusi.

**Daftar Tabel Utama:**
`users`, `authentications`, `customers`, `notes`, `customer_sales_notes`, `economic_indicators`.

**Fungsi RPC (Stored Procedures) Utama:**
* `get_customers`: Fetch data dengan server-side pagination & filtering.
* `get_priority_counts`: Menghitung total nasabah Priority vs Non-Priority.
* `get_daily_trends` / `get_monthly_trends`: Analitik performa kampanye waktu ke waktu.
* `get_contact_effectiveness`: Menghitung rasio durasi telepon terhadap keberhasilan.
* `get_deposit_average`: Rata-rata saldo berdasarkan pekerjaan/umur.

### 3. Environment Variables (Railway)
Backend membutuhkan variabel berikut untuk berjalan. Pastikan dikonfigurasi di file `.env` atau dashboard Railway:

```env
# Server
NODE_ENV=production
FRONTEND_URL=https://leadsight-app.vercel.app

# Auth Secrets
ACCESS_TOKEN_KEY=your_super_secret_access_key
REFRESH_TOKEN_KEY=your_super_secret_refresh_key
ACCESS_TOKEN_AGE=3600s
REFRESH_TOKEN_AGE=7d

# Database
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key

# AI Services
GEMINI_API_KEY=your_google_ai_key
```

---

## 🧠 LeadSight — Machine Learning

Otak di balik sistem rekomendasi LeadSight.

### 1. Ringkasan Model
* **Algoritma:** **LightGBM Classifier** (Gradient Boosting).
* **Dataset:** Bank Marketing Dataset (41.188 baris data).
* **Handling Imbalance:** Menggunakan **SMOTE** (Synthetic Minority Over-sampling Technique) untuk menyeimbangkan kelas target.

### 2. Performa Model
Model LightGBM terpilih setelah mengalahkan Random Forest, XGBoost, dan Logistic Regression.

| Metrik | Nilai | Interpretasi Bisnis |
| :--- | :--- | :--- |
| **Accuracy** | **88.72%** | Konsistensi prediksi secara umum sangat baik. |
| **Precision (Yes)**| **54.89%** | Jika AI bilang "Ya", peluang benar adalah ~55%. (Jauh diatas tebakan acak 11%). |
| **Recall (No)** | **95.04%** | Sangat efektif membuang nasabah yang pasti menolak (Hemat waktu). |

### 3. Dampak Bisnis (Business Impact)
* **Efisiensi:** Mengurangi jumlah panggilan gagal hingga **450%**.
* **Konversi:** Meningkatkan conversion rate dari 11% (baseline) menjadi estimasi **~44%** pada segmen prioritas tinggi.
* **Deployment:** Model di-export sebagai `.pkl` file (`best_LGBM_without_duration_model.pkl`) untuk dipanggil oleh Backend.

---

## 🛠 Instalasi & Setup Lokal

Ikuti langkah ini untuk menjalankan LeadSight di komputer Anda secara lokal.

### Prasyarat
* Node.js (v16+)
* NPM / Yarn
* Git

### 1. Clone Repository
```bash
git clone https://github.com/username-anda/leadsight-project.git
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

**LeadSight Project**
*Dikembangkan sebagai bagian dari program Asah Digital led by Dicoding in association with Accenture.*

Copyright © 2024 LeadSight Team.
