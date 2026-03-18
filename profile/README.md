# 🚀 BukainJalan Core Ecosystem

Selamat datang di repositori utama **BukainJalan**, sebuah Super-App Ekonomi Sirkular. 

Semua pengembangan di bawah organisasi ini wajib mematuhi prinsip **Clean Architecture**, **Modular Structure**, dan **Simple to Develop**.

---

## 🏗️ Ekosistem Polyrepo
Sistem kami dipisah menjadi 4 layanan independen untuk memastikan skalabilitas:
1. [Backend (Express.js/PostgreSQL)](https://github.com/BukainJalan/backend)
2. [Mobile App (React Native)](https://github.com/BukainJalan/mobile)
3. [Admin Dashboard (Web)](https://github.com/BukainJalan/admin)
4. [Landing Page (Web)](https://github.com/BukainJalan/landing)

---

## 📋 Project Management Guidelines
Kami menggunakan hierarki 3 Lapis (Epic > Sprint > Task) dengan 6 Pilar utama:
- `[IDN]` **Identity** (Auth, KYC, Profile)
- `[MSN]` **Mission** (Service, Discovery, B2B Market, Gig)
- `[FIN]` **Financial** (Payment, Escrow, Wallet, Tax)
- `[COM]` **Communication** (Chat, Notification)
- `[GAM]` **Gamification** (Review, Points, Leveling)
- `[INT]` **Intelligence** (Analytics, Recommendation)

**Format Tiket (User Story):**
`[ID Pillar][Nama Modul]-Aksi Spesifik` (Contoh: `[IDN-01][Auth]-Register User`)

---

## 💻 Git & Development Workflow
Setiap *developer* wajib mengikuti aturan *branching* dan *commit* berikut agar CI/CD berjalan otomatis.

**1. Aturan Branching:**
Satu *branch* untuk satu tiket. Gunakan format `<tipe>/<ID_Tiket>-<deskripsi-singkat>`.
* Contoh: `feat/IDN-01-register-email`

**2. Aturan Commit (Hybrid Conventional):**
* Contoh: `feat(auth): [IDN-01] create user registration endpoint`
* *Tipe yang diizinkan:* `feat`, `fix`, `chore`, `refactor`.

**3. Otomatisasi Kanban (Pull Request):**
Saat membuat Pull Request, wajib mencantumkan `Closes #ID_Issue` di deskripsi agar tiket di Project Board otomatis pindah ke kolom *Done*.
