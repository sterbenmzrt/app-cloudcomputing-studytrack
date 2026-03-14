# 📚 StudyTrack — Firebase Auth + Realtime Database

Aplikasi manajemen tugas kuliah berbasis web menggunakan **Firebase Authentication** dan **Firebase Realtime Database**.

Dibuat untuk tugas mata kuliah **Cloud Computing 2024/2025**.

---

## ✨ Fitur

- 🔐 **Register & Login** — Email/Password dan Google Sign-In
- 🔄 **CRUD Real-time** — tambah, lihat, edit, hapus tugas langsung sync ke Firebase
- 👤 **Data per-user** — setiap akun hanya bisa mengakses datanya sendiri
- 🔍 **Filter & Search** — cari berdasarkan judul, mata kuliah, status, prioritas
- 📊 **Statistik** — ringkasan total, prioritas tinggi, pending, selesai

---

## 🚀 Cara Menjalankan

### 1. Clone repo ini
```bash
git clone https://github.com/username/studytrack.git
cd studytrack
```

### 2. Buat file konfigurasi Firebase
```bash
cp firebase-config.example.js firebase-config.js
```
Buka `firebase-config.js` dan isi dengan config Firebase project kamu.

### 3. Setup Firebase Console

#### Authentication
1. [console.firebase.google.com](https://console.firebase.google.com) → pilih project
2. **Build → Authentication → Get started**
3. Tab **Sign-in method** → aktifkan **Email/Password** → Save
4. Klik **Add new provider** → **Google** → Enable → isi support email → Save

#### Realtime Database
1. **Build → Realtime Database → Create Database**
2. Region: **asia-southeast1 (Singapore)** → Next
3. Pilih **Start in test mode** → Enable

#### Security Rules (rekomendasi)
Ganti rules default dengan yang lebih aman — hanya user yang login bisa akses datanya sendiri:
```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "$uid === auth.uid",
        ".write": "$uid === auth.uid"
      }
    }
  }
}
```

### 4. Buka aplikasi

> ⚠️ **Penting:** Google Sign-In membutuhkan protokol `http://` atau `https://`. Membuka file langsung dari explorer (`file://`) akan menyebabkan error.

**Opsi A — Live Server (rekomendasi untuk development):**
1. Install extension **Live Server** di VS Code
2. Klik kanan `index.html` → **Open with Live Server**
3. Browser akan terbuka otomatis di `http://127.0.0.1:5500`

**Opsi B — Firebase Hosting (untuk production):**
```bash
npm install -g firebase-tools
firebase login
firebase init hosting   # pilih folder: . (current directory)
firebase deploy
```

**Opsi C — HTTP server sederhana:**
```bash
# Python
python -m http.server 8080

# Node.js
npx serve .
```
Kemudian buka `http://localhost:8080` di browser.

> 💡 Login dengan **Email/Password** tetap bisa digunakan meskipun dibuka via `file://`, hanya **Google Sign-In** yang membutuhkan web server.

---

## 📁 Struktur File

```
studytrack/
├── index.html                  ← Aplikasi utama (HTML + CSS + JS)
├── firebase-config.js          ← 🔒 Config Firebase (di .gitignore)
├── firebase-config.example.js  ← Template config (aman di-commit)
├── .gitignore
└── README.md
```

> **Kenapa `firebase-config.js` tidak ada di repo?**
> File tersebut berisi API key dan sudah ditambahkan ke `.gitignore` sebagai best practice.
> Salin dari `firebase-config.example.js` dan isi dengan config project kamu sendiri.

---

## 🗄️ Struktur Data Firebase

```
users/
  {uid}/
    tasks/
      {taskId}/
        judul:       "Tugas Besar Cloud Computing"
        matkul:      "Cloud Computing"
        deskripsi:   "Implementasi BaaS menggunakan Firebase"
        tenggat:     "2025-04-15"
        prioritas:   "Tinggi"
        status:      "Sedang Dikerjakan"
        createdAt:   "2025-03-09T08:00:00.000Z"
        updatedAt:   "2025-03-09T10:30:00.000Z"
```

---

## ❓ Troubleshooting

### Error: `auth/operation-not-supported-in-this-environment`
**Penyebab:** Halaman dibuka langsung dari file explorer (URL dimulai dengan `file://`). Firebase Google OAuth membutuhkan origin `http://` atau `https://`.

**Solusi:** Jalankan aplikasi melalui web server lokal (lihat bagian [Buka aplikasi](#4-buka-aplikasi) di atas).

### Error: `auth/unauthorized-domain`
**Penyebab:** Domain tempat aplikasi di-host belum didaftarkan di Firebase.

**Solusi:**
1. Buka [Firebase Console](https://console.firebase.google.com) → project kamu
2. **Authentication → Settings → Authorized domains**
3. Klik **Add domain** → tambahkan domain kamu (misal: `localhost`, `127.0.0.1`, atau domain hosting)

### Error: `auth/popup-blocked`
**Penyebab:** Browser memblokir pop-up untuk Google Sign-In.

**Solusi:** Aplikasi akan otomatis mencoba metode redirect sebagai fallback. Jika tetap gagal, izinkan pop-up untuk situs ini di pengaturan browser.

---

## 🛠️ Teknologi

| Teknologi | Keterangan |
|---|---|
| HTML5 + CSS3 | Frontend, tanpa framework |
| Vanilla JavaScript | Logika aplikasi |
| Firebase Auth v10 | Autentikasi pengguna |
| Firebase Realtime Database v10 | Penyimpanan data real-time |
| Google Fonts | Lora + DM Sans |
