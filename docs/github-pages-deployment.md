# Panduan Deployment GitHub Pages dengan Custom Domain

Dokumentasi lengkap bagaimana website IKA FSRD Trisakti di-deploy ke **<https://ikafsrdtrisakti.com>** menggunakan GitHub Pages dan GitHub Actions.

---

## Bagian 1: Teori GitHub Pages

### Apa itu GitHub Pages?

**GitHub Pages** adalah layanan hosting website statis **GRATIS** yang disediakan oleh GitHub. Layanan ini memungkinkan Anda meng-host website langsung dari repository GitHub.

### Fitur Utama GitHub Pages

| Fitur | Keterangan |
|-------|------------|
| **Gratis** | Tidak ada biaya untuk hosting |
| **HTTPS Otomatis** | SSL certificate gratis dari Let's Encrypt |
| **Custom Domain** | Bisa menggunakan domain sendiri |
| **CDN Global** | Konten di-cache di server seluruh dunia |
| **Direct from Repo** | Deploy langsung dari kode di repository |

### Batasan GitHub Pages

- Ukuran repository maksimal **1 GB**
- Bandwidth **100 GB/bulan**
- Hanya untuk **website statis** (HTML, CSS, JS)
- Tidak support backend/database
- Build limit **10 builds/jam**

### Tipe Website yang Cocok

✅ Portfolio pribadi  
✅ Dokumentasi proyek  
✅ Landing page organisasi  
✅ Blog statis (Jekyll, Hugo)  
✅ Single Page Application (React, Vue - sudah di-build)  

---

## Bagian 2: Filosofi GitHub Actions

### Apa itu GitHub Actions?

**GitHub Actions** adalah platform **CI/CD (Continuous Integration/Continuous Deployment)** yang terintegrasi langsung dengan GitHub.

### Filosofi Dasar

```
Code → Push → Automated Build → Automated Test → Automated Deploy
```

GitHub Actions menerapkan filosofi **"Infrastructure as Code"** dimana:

1. **Workflow didefinisikan sebagai file YAML** di dalam repository
2. **Otomatis berjalan** ketika ada event tertentu (push, pull request, schedule)
3. **Reproducible** - siapapun bisa menjalankan workflow yang sama
4. **Version controlled** - history workflow tersimpan bersama kode

### Komponen GitHub Actions

```
┌─────────────────────────────────────────────────┐
│                   WORKFLOW                       │
│  (file .yml di .github/workflows/)              │
│                                                  │
│  ┌─────────────────────────────────────────┐    │
│  │                  JOB                     │    │
│  │  (unit eksekusi, berjalan di runner)    │    │
│  │                                          │    │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐    │    │
│  │  │  STEP   │ │  STEP   │ │  STEP   │    │    │
│  │  │ (aksi)  │ │ (aksi)  │ │ (aksi)  │    │    │
│  │  └─────────┘ └─────────┘ └─────────┘    │    │
│  └─────────────────────────────────────────┘    │
└─────────────────────────────────────────────────┘
```

### Mengapa Menggunakan GitHub Actions untuk Pages?

| Tanpa GitHub Actions | Dengan GitHub Actions |
|---------------------|----------------------|
| Deploy dari root atau /docs | Deploy dari **subfolder manapun** |
| Manual build | **Otomatis build** setiap push |
| Tidak ada preprocessing | Bisa **build Tailwind, minify, dll** |
| Satu sumber saja | **Fleksibel** - bisa gabung multiple sources |

---

## Bagian 3: Langkah Praktis Deployment

### Struktur Project

```
fsrd/
├── .github/
│   └── workflows/
│       └── deploy.yml          ← Workflow file
├── docs/                        ← Dokumentasi
├── ikafsrds-static/            ← Website statis
│   ├── index.html
│   ├── tentang-kami.html
│   ├── program.html
│   ├── karya-alumni.html
│   ├── jurnal.html
│   ├── donasi.html
│   ├── CNAME                   ← Custom domain
│   ├── assets/
│   ├── dist/output.css
│   └── js/main.js
└── README.md
```

---

### Step 1: Buat Workflow File

Buat file `.github/workflows/deploy.yml`:

```yaml
# Deploy static site from ikafsrds-static folder to GitHub Pages
name: Deploy to GitHub Pages

on:
  # Berjalan setiap push ke branch main
  push:
    branches: ["main"]

  # Bisa di-trigger manual dari tab Actions
  workflow_dispatch:

# Permissions untuk deploy ke Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Hanya 1 deployment berjalan dalam satu waktu
concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      # Checkout kode dari repository
      - name: Checkout
        uses: actions/checkout@v4

      # Setup GitHub Pages
      - name: Setup Pages
        uses: actions/configure-pages@v4

      # Upload HANYA folder ikafsrds-static
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: './ikafsrds-static'

      # Deploy ke GitHub Pages
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

---

### Step 2: Push Workflow ke GitHub

```bash
# Commit dan push workflow
git add .github/workflows/deploy.yml
git commit -m "ci: Add GitHub Actions workflow for Pages deployment"
git push origin main
```

---

### Step 3: Aktifkan GitHub Pages

1. Buka **Settings** repository di GitHub
2. Klik **Pages** di sidebar kiri
3. Di bagian **"Build and deployment"**:
   - **Source**: Pilih **"GitHub Actions"**
4. Klik **Save**

> 💡 Workflow akan otomatis berjalan setelah setting ini diaktifkan

---

### Step 4: Konfigurasi Custom Domain

#### 4a. Beli Domain

Beli domain dari registrar manapun (contoh: Domainesia, Niagahoster, dll).

Domain yang digunakan: `ikafsrdtrisakti.com`

#### 4b. Konfigurasi DNS Records

Login ke panel registrar dan tambahkan DNS records:

**A Records** (untuk apex domain):

| Host | Type | Address |
|------|------|---------|
| @ | A | 185.199.108.153 |
| @ | A | 185.199.109.153 |
| @ | A | 185.199.110.153 |
| @ | A | 185.199.111.153 |

**CNAME Record** (untuk www subdomain):

| Host | Type | Target |
|------|------|--------|
| www | CNAME | awplk.github.io |

#### 4c. Tambahkan Custom Domain di GitHub

1. Buka **Settings** → **Pages**
2. Di bagian **"Custom domain"**, masukkan: `ikafsrdtrisakti.com`
3. Klik **Save**
4. Tunggu DNS check selesai (✅ hijau)
5. Centang **"Enforce HTTPS"**

#### 4d. Buat File CNAME di Repo

```bash
# Buat file CNAME
echo "ikafsrdtrisakti.com" > ikafsrds-static/CNAME

# Commit dan push
git add ikafsrds-static/CNAME
git commit -m "Add custom domain CNAME file"
git push origin main
```

---

### Step 5: Verifikasi Deployment

#### Cek Status Workflow

Buka: `https://github.com/awplk/fsrd/actions`

- 🟡 Kuning = Sedang berjalan
- ✅ Hijau = Sukses
- ❌ Merah = Gagal

#### Cek DNS Propagation

Buka: `https://dnschecker.org/#A/ikafsrdtrisakti.com`

Pastikan semua server menunjukkan IP GitHub (185.199.x.x)

#### Test Akses Website

Buka URL berikut:

- <https://ikafsrdtrisakti.com>
- <https://www.ikafsrdtrisakti.com>
- <https://awplk.github.io/fsrd/> (fallback)

---

## Bagian 4: Alur Deployment Otomatis

```
┌─────────────────────────────────────────────────────────────────┐
│                        DEVELOPER                                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
                    ┌─────────────────┐
                    │   git push      │
                    │   origin main   │
                    └─────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      GITHUB REPOSITORY                           │
│                      github.com/awplk/fsrd                       │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼ (trigger)
┌─────────────────────────────────────────────────────────────────┐
│                     GITHUB ACTIONS                               │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ 1. Checkout code                                          │  │
│  │ 2. Setup Pages                                            │  │
│  │ 3. Upload artifact (ikafsrds-static/)                     │  │
│  │ 4. Deploy to GitHub Pages                                 │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      GITHUB PAGES CDN                            │
│                     (Global Distribution)                        │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                         DNS ROUTING                              │
│  ikafsrdtrisakti.com ──▶ 185.199.x.x ──▶ GitHub Pages           │
│  www.ikafsrdtrisakti.com ──▶ awplk.github.io ──▶ GitHub Pages   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                          USER                                    │
│              Akses https://ikafsrdtrisakti.com                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Bagian 5: Troubleshooting

### DNS Check Gagal

**Penyebab**: DNS belum propagate  
**Solusi**: Tunggu 10-30 menit, cek di dnschecker.org

### Website Menampilkan 404

**Penyebab**: Workflow belum jalan atau gagal  
**Solusi**: Cek tab Actions, trigger manual jika perlu

### HTTPS Tidak Tersedia

**Penyebab**: DNS belum fully propagate  
**Solusi**: Tunggu sampai custom domain check hijau, baru centang Enforce HTTPS

### Perubahan Tidak Muncul

**Penyebab**: Cache browser atau CDN  
**Solusi**: Hard refresh (Ctrl+Shift+R) atau clear cache

---

## Referensi

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Configuring Custom Domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)

---

**Terakhir diupdate**: 24 Desember 2024  
**Website**: <https://ikafsrdtrisakti.com>  
**Repository**: <https://github.com/awplk/fsrd>
