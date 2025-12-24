# Analisis Repositori: ikafsrds-digital-home

**URL Repositori**: <https://github.com/awplk/ikafsrds-digital-home>
**Clone Path**: `ikafsrds-digital-home/`

## Ringkasan Proyek

Ini adalah proyek **Frontend Web** yang dibangun menggunakan **React** dan **Vite**, dikembangkan atau di-generate menggunakan platform **Lovable** (lovable.dev). Aplikasi ini berfungsi sebagai "Digital Home" untuk Ikatan Alumni Fakultas Seni Rupa & Desain (IKA FSRD).

## Teknologi Utama (Tech Stack)

- **Framework**: Vite + React
- **Bahasa**: TypeScript
- **Styling**: Tailwind CSS
- **UI Components**: shadcn-ui + Radix UI
- **Routing**: react-router-dom
- **State Management/Data Fetching**: @tanstack/react-query
- **Form Handling**: react-hook-form + zod
- **Charts**: recharts
- **Icons**: Lucide React

## Struktur Halaman (Routes)

Berdasarkan isi folder `src/pages`, aplikasi memiliki fitur-fitur berikut:

- **Beranda** (`Index.tsx`)
- **Tentang Kami** (`TentangKamiPage.tsx`) - Profil organisasi.
- **Program** (`Program.tsx`, `ProgramDetail.tsx`) - Daftar dan detail kegiatan/program.
- **Karya Alumni** (`KaryaAlumniPage.tsx`) - Galeri atau portofolio karya alumni.
- **Jurnal/Artikel** (`JurnalPage.tsx`, `ArticleDetail.tsx`) - Blog atau berita.
- **Donasi** (`DonasiPage.tsx`) - Halaman untuk donasi.

## Cara Menjalankan

1. Masuk ke direktori: `cd ikafsrds-digital-home`
2. Install dependencies: `npm install`
3. Jalankan server development: `npm run dev`

## Catatan Tambahan

- Proyek ini menggunakan `bun.lockb` dan `package-lock.json`, yang menunjukkan mungkin pernah dijalankan dengan Bun dan NPM.
- Konfigurasi ESLint dan TypeScript sudah tersedia untuk maintainability code.
