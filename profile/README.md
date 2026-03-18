# 🚀 BukainJalan — Engineering & Development Guidelines

> **Single Source of Truth** untuk seluruh ekosistem pengembangan BukainJalan.
> Semua kontributor wajib membaca dan mematuhi panduan ini sebelum menulis satu baris kode pun.

**Prinsip Utama:** `Clean Architecture` · `Modular Structure` · `Readable Code` · `Simple to Develop`

---

## 📑 Daftar Isi

1. [Arsitektur Ekosistem](#-1-arsitektur-ekosistem-polyrepo)
2. [Project Management](#-2-project-management-matrix-6-pilar)
3. [The Lifecycle — End-to-End Workflow](#-3-the-lifecycle--end-to-end-workflow)
   - [Fase 1 — Planning & Ticket Creation](#-fase-1--planning--ticket-creation)
   - [Fase 2 — Local Development & Testing](#-fase-2--local-development--testing)
   - [Fase 3 — Commit, Push & CI](#-fase-3--commit-push--ci)
   - [Fase 4 — Pull Request & Code Review](#-fase-4--pull-request--code-review)
   - [Fase 5 — Staging & Merge](#-fase-5--staging--merge)
   - [Fase 6 — Production Deploy](#-fase-6--production-deploy)
4. [Referensi Cepat](#-referensi-cepat)

---

## 🏗️ 1. Arsitektur Ekosistem (Polyrepo)

Sistem dipisah menjadi **4 repositori independen** untuk memudahkan *maintenance* dan mempercepat *deployment*. Setiap repositori memiliki tanggung jawab yang jelas dan terpisah.

| Repositori | Teknologi | Deskripsi |
|---|---|---|
| [`backend`](https://github.com/BukainJalan/backend) | ElysiaJS · TypeScript · PostgreSQL · Prisma | High-performance API & Core Business Logic |
| [`mobile`](https://github.com/BukainJalan/mobile) | React Native | Cross-platform Mobile App (iOS & Android) |
| [`admin`](https://github.com/BukainJalan/admin) | Next.js · TypeScript (TSX) | Internal Dashboard & Management System |
| [`landing`](https://github.com/BukainJalan/landing) | Vite · TypeScript (TSX) | Marketing Website & Company Profile |

---

## 📋 2. Project Management (Matrix 6 Pilar)

Seluruh proyek dikelola menggunakan **GitHub Projects** terpusat di level *Organization*. Hierarki tiket mengikuti 3 lapisan berikut:

```
Epic (Pilar)  →  Sprint (Modul)  →  Task (Tiket/Issue)
```

### 🏛️ 6 Pilar Utama

| Kode | Pilar | Cakupan |
|---|---|---|
| `[IDN]` | **Identity** | Auth, KYC, Profile |
| `[MSN]` | **Mission** | Service, Discovery, B2B Market, Gig |
| `[FIN]` | **Financial** | Payment, Escrow, Wallet, Tax |
| `[COM]` | **Communication** | Chat, Notification |
| `[GAM]` | **Gamification** | Review, Points, Leveling |
| `[INT]` | **Intelligence** | Analytics, Recommendation |

### 👤 Aktor / Role

`Client` · `Talent` · `Provider` · `Merchant` · `Government` · `Admin`

### 🏷️ Format Judul Tiket

```
[ID Pillar][Nama Modul]-Aksi Spesifik
```

**Contoh:**
```
[IDN-01][Auth]-Register User
[FIN-03][Wallet]-Top Up Balance
[MSN-02][Gig]-Create Job Posting
```

---

## 🔄 3. The Lifecycle — End-to-End Workflow

Berikut adalah **siklus hidup wajib** untuk setiap fitur maupun *bugfix*, dari ide hingga rilis ke *production*.

```
💡 Ide  →  📋 Tiket  →  🌿 Branch  →  💻 Kode  →  🧪 Test (Local)  →  📤 Push  →  🤖 CI  →  🔍 Review  →  🧪 Staging  →  ✅ Merge  →  🚀 Deploy
```

> Setiap tahap adalah **gerbang (gate)**. Jangan lanjut ke tahap berikutnya sebelum tahap saat ini selesai dan lolos.

---

### 📌 Fase 1 — Planning & Ticket Creation

> Tidak boleh ada satu baris kode pun ditulis sebelum tiket terdokumentasi dengan baik.

**Langkah-langkah:**

1. Buat tiket baru di kolom **`Todo`** pada GitHub Projects dengan format judul standar.
2. Ubah tiket menjadi **Issue** dan hubungkan ke repositori yang sesuai (misal: `backend`).
3. Isi deskripsi tiket menggunakan template **Acceptance Criteria** berikut:

```markdown
### 📖 User Story
> **Sebagai** [Role], **Saya ingin** [Aksi], **Sehingga** [Benefit].

### ✅ Acceptance Criteria (AC)
- [ ] Syarat teknis 1
- [ ] Syarat teknis 2
- [ ] Syarat teknis 3

### 📂 Struktur Modular & Teknis (Opsional)
> Diisi jika diperlukan untuk fitur kompleks atau lintas modul.
- **Pilar:** [IDN] Identity
- **Modul:** Auth
- **Feature:** Register
- **Folder Path:** `src/modules/identity/auth/`
- **API Endpoint:** `POST /api/identity/auth/register`
```

**Contoh tiket yang baik:**

```markdown
### 📖 User Story
> **Sebagai** Client, **Saya ingin** mendaftarkan akun dengan email,
> **Sehingga** saya bisa mengakses platform BukainJalan.

### ✅ Acceptance Criteria (AC)
- [ ] Endpoint POST /auth/register tersedia dan mengembalikan status 201
- [ ] Password di-hash menggunakan bcrypt sebelum disimpan ke database
- [ ] Email yang sudah terdaftar mengembalikan error 409 Conflict
- [ ] Response menyertakan JWT token yang valid

### 📂 Struktur Modular & Teknis (Opsional)
- **Pilar:** [IDN] Identity
- **Modul:** Auth
- **Feature:** Register
- **Folder Path:** `src/modules/identity/auth/`
- **API Endpoint:** `POST /api/identity/auth/register`
```

---

### 💻 Fase 2 — Local Development & Testing

**1. Perbarui kode lokal dari `main`:**
```bash
git checkout main
git pull origin main
```

**2. Buat branch baru dari tiket:**

```bash
git checkout -b <tipe>/<ID_Tiket>-<deskripsi-singkat>
```

| Tipe | Kegunaan |
|---|---|
| `feat` | Fitur baru |
| `fix` | Perbaikan bug |
| `chore` | Setup, konfigurasi, atau tugas teknis |
| `refactor` | Merapikan kode tanpa mengubah fungsionalitas |

**Contoh:**
```bash
git checkout -b feat/IDN-01-register-email
git checkout -b fix/FIN-05-wallet-balance-error
git checkout -b refactor/COM-02-cleanup-notification-service
```

**3. Koding secara Modular:**

Pastikan kode baru berada di dalam folder domainnya masing-masing. Jangan melampaui batasan *Acceptance Criteria* yang sudah disepakati.

```
src/
└── modules/
    ├── identity/
    │   └── auth/         ← Kode untuk [IDN] di sini
    ├── financial/
    │   └── wallet/       ← Kode untuk [FIN] di sini
    └── communication/
        └── notification/ ← Kode untuk [COM] di sini
```

**4. 🧪 Jalankan test secara lokal sebelum push:**

> ⚠️ **Wajib lulus di lokal sebelum di-push.** Jangan andalkan CI untuk menemukan bug yang bisa ditemukan lebih awal.

```bash
# Jalankan seluruh test suite
npm run test

# Jalankan test untuk modul spesifik saja
npm run test -- --testPathPattern=auth

# Pastikan tidak ada error TypeScript
npm run type-check

# Pastikan linting bersih
npm run lint
```

**Checklist sebelum push:**
- [ ] Semua unit test lolos (`npm run test`)
- [ ] Tidak ada error TypeScript (`npm run type-check`)
- [ ] Tidak ada pelanggaran linting (`npm run lint`)
- [ ] Tidak ada `console.log` yang tertinggal
- [ ] Kode hanya menyentuh file yang relevan dengan tiket

---

### 📤 Fase 3 — Commit, Push & CI

**Format Commit (Hybrid Conventional Commits):**

```
<tipe>(<modul>): [<ID_Tiket>] <pesan singkat dalam huruf kecil>
```

**Contoh commit yang baik:**
```bash
git commit -m "feat(auth): [IDN-01] create register endpoint and hash password"
git commit -m "fix(wallet): [FIN-05] handle negative balance on top-up"
git commit -m "refactor(notification): [COM-02] extract push service to separate class"
git commit -m "chore(deps): [IDN-03] add bcrypt and jwt packages"
```

**Aturan pesan commit:**
- ✅ Gunakan huruf kecil semua
- ✅ Tulis pesan dalam bahasa Inggris
- ✅ Pesan harus deskriptif dan spesifik
- ❌ Jangan tulis `fix bug`, `update code`, atau `wip`

**Push ke GitHub:**
```bash
git push -u origin feat/IDN-01-register-email
```

**🤖 CI otomatis berjalan setelah push:**

Setelah push, **GitHub Actions** akan otomatis menjalankan pipeline CI pada branch Anda:

```
git push
    │
    └──► 🤖 GitHub Actions (CI Pipeline)
              │
              ├── ✅ Install dependencies
              ├── ✅ Type check
              ├── ✅ Lint
              └── ✅ Run test suite
```

> Pantau status CI di tab **Actions** pada repositori. **Jangan buat PR jika CI masih merah (failed).**

---

### 🔍 Fase 4 — Pull Request & Code Review

> PR hanya boleh dibuat setelah **CI berstatus hijau (passed)** pada branch Anda.

**1. Buat Pull Request dari branch Anda ke `main`.**

**2. Gunakan format judul PR yang jelas:**
```
[IDN-01] feat(auth): create user registration endpoint
```

**3. ⚠️ WAJIB: Isi deskripsi PR dengan template berikut:**

```markdown
## 📝 Deskripsi
Menambahkan endpoint registrasi user baru menggunakan email dan password.
Password di-hash dengan bcrypt sebelum disimpan.

## 🔧 Perubahan
- Tambah `POST /auth/register` endpoint
- Integrasi bcrypt untuk hashing password
- Tambah validasi input dengan Joi

## 🧪 Testing
- [ ] Unit test untuk auth service
- [ ] Integrasi test untuk endpoint

## 🔗 Linked Issue
Closes #1

## ✅ CI Status
> Pastikan badge CI hijau sebelum meminta review.

Closes #1
```

**4. Checklist sebelum meminta review:**
- [ ] CI pipeline berstatus **passed** ✅
- [ ] Tidak ada `console.log` yang tertinggal
- [ ] Kode sudah bersih dan terbaca
- [ ] Hanya menyentuh file yang relevan dengan tiket
- [ ] Semua *Acceptance Criteria* sudah terpenuhi
- [ ] Tidak ada konflik dengan branch `main`

---

### 🧪 Fase 5 — Staging & Merge

**Alur setelah PR disetujui reviewer:**

```
PR Disetujui (Approved)
        │
        └──► ⚙️ GitHub Actions
                  Auto-deploy ke 🧪 Staging Environment
                        │
                        ├── Lakukan verifikasi manual di Staging
                        │   - [ ] Fitur berjalan sesuai Acceptance Criteria
                        │   - [ ] Tidak ada regresi pada fitur lain
                        │   - [ ] UI/UX sesuai ekspektasi (untuk mobile/admin)
                        │
                        └── ✅ Staging OK → Tekan "Merge Pull Request"
```

> ⚠️ **Jangan merge jika hasil verifikasi di Staging belum OK**, meskipun PR sudah disetujui reviewer.

---

### 🚀 Fase 6 — Production Deploy

Setelah merge ke `main` berhasil, dua proses otomatis akan berjalan sekaligus:

```
Merge ke main
     │
     ├──► 📋 GitHub Projects
     │         Tiket otomatis pindah ke kolom "Done" 🎉
     │
     └──► ⚙️ GitHub Actions (CD Pipeline)
               │
               ├── ✅ Final build & test
               ├── 🐳 Build Docker image (jika ada)
               ├── 🚀 Deploy ke Production Server
               └── 🔔 Notifikasi deploy berhasil/gagal
```

> Pantau log deployment di tab **Actions**. Jika deployment gagal, segera koordinasikan dengan tim untuk *rollback*.

**Setelah deploy berhasil, bersihkan branch lokal:**
```bash
git checkout main
git pull origin main
git branch -d feat/IDN-01-register-email
```
Kemudian ulangi kembali dari **Fase 1** untuk tiket berikutnya. 🔁

---

## 📖 Referensi Cepat

### Cheat Sheet: Full Workflow

```bash
# ── FASE 1: Start ──────────────────────────────────────
git checkout main && git pull origin main
git checkout -b feat/IDN-01-nama-fitur

# ... koding ...

# ── FASE 2: Test Lokal ─────────────────────────────────
npm run test
npm run type-check
npm run lint

# ── FASE 3: Commit & Push ──────────────────────────────
git add .
git commit -m "feat(auth): [IDN-01] deskripsi singkat"
git push -u origin feat/IDN-01-nama-fitur

# → Tunggu CI hijau di GitHub Actions, lalu buat PR
# → Setelah PR di-approve, verifikasi di Staging
# → Merge jika Staging OK

# ── SETELAH MERGE: Cleanup ─────────────────────────────
git checkout main && git pull origin main
git branch -d feat/IDN-01-nama-fitur
```

### Cheat Sheet: Format Standar

| Item | Format | Contoh |
|---|---|---|
| **Judul Tiket** | `[PILAR-NO][Modul]-Aksi` | `[IDN-01][Auth]-Register User` |
| **Nama Branch** | `tipe/PILAR-NO-deskripsi` | `feat/IDN-01-register-email` |
| **Pesan Commit** | `tipe(modul): [PILAR-NO] pesan` | `feat(auth): [IDN-01] create register endpoint` |
| **Judul PR** | `[PILAR-NO] tipe(modul): pesan` | `[IDN-01] feat(auth): create register endpoint` |
| **PR Description** | Wajib ada `Closes #ID` | `Closes #1` |

### Tipe Commit yang Diizinkan

| Tipe | Kapan Digunakan |
|---|---|
| `feat` | Menambah fitur baru |
| `fix` | Memperbaiki bug |
| `chore` | Setup, instalasi, konfigurasi |
| `refactor` | Merapikan kode tanpa ubah fungsi |

### Status Gate per Fase

| Fase | Gate — Syarat Wajib Sebelum Lanjut |
|---|---|
| Fase 1 → 2 | Tiket sudah jadi Issue dengan AC lengkap |
| Fase 2 → 3 | Semua test, type-check, dan lint lolos di lokal |
| Fase 3 → 4 | CI pipeline **passed** di GitHub Actions |
| Fase 4 → 5 | PR disetujui minimal 1 reviewer |
| Fase 5 → 6 | Verifikasi manual di Staging **OK** |

---

## 🤝 Kontribusi

Semua anggota tim BukainJalan wajib mengikuti panduan ini tanpa pengecualian. Jika ada pertanyaan atau usulan perubahan, buat diskusi di **GitHub Issues** dengan label `documentation`.

---

<div align="center">

**BukainJalan** — *Platform Pembuka Akses Peluang & Karier*

Dibangun dengan ❤️ oleh Tim BukainJalan

</div>
