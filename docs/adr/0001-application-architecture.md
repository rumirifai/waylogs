# ADR-0001: Arsitektur aplikasi MVP

## Status

Accepted

## Konteks

WayLogs adalah tracker pribadi berbasis web responsif yang pada MVP menyediakan pencatatan lamaran manual, tahapan, balasan, ringkasan, dan pustaka berkas. Produk dimulai untuk pemilik dan beberapa kenalan, tanpa impor spreadsheet, integrasi email, atau AI pada MVP.

PRD memilih Next.js App Router dan TypeScript, Supabase untuk PostgreSQL/Auth/Storage, serta Vercel untuk aplikasi dan domain. UI memakai Tailwind CSS, shadcn/ui, dan Lucide; forms memakai React Hook Form dan Zod. Playwright direncanakan untuk pengujian browser pada tahap selanjutnya.

## Pendorong keputusan

- Mendukung satu aplikasi web responsif dengan backend yang cukup untuk operasi sensitif.
- Memusatkan data relasional, autentikasi, dan berkas pada platform yang dipilih PRD.
- Menjaga kompleksitas operasional tetap sesuai MVP kecil.
- Memastikan fitur sensitif dapat divalidasi di server dan data tetap terlindungi oleh policy database/storage.

## Opsi yang dipertimbangkan

PRD menetapkan stack awal, tetapi tidak merekam evaluasi komparatif formal. Karena itu, alternatif berikut dicatat sebagai kelas pendekatan yang ditolak untuk MVP, bukan klaim bahwa benchmark atau evaluasi terperinci telah dilakukan:

1. **Stack yang dipilih: Next.js + Supabase + Vercel**: satu aplikasi Next.js, layanan Supabase terkelola, serta backend Next.js untuk alur yang memerlukan validasi dan orkestrasi.
2. **Backend dan database dikelola sendiri**: memberi kontrol lebih besar, tetapi menambah kebutuhan deployment, operasi database, autentikasi, dan storage yang tidak dibutuhkan untuk skala awal.
3. **Frontend statis dengan backend/API terpisah**: memisahkan layanan dengan jelas, tetapi menambah komponen dan deployment untuk produk MVP yang masih kecil.

## Keputusan

Gunakan Next.js App Router dan TypeScript pada Vercel sebagai aplikasi web WayLogs. Gunakan Supabase PostgreSQL, Auth, dan Storage sebagai backend data dan berkas. Gunakan Server Actions atau Route Handlers Next.js untuk operasi yang memerlukan validasi/orkestrasi server; CRUD sederhana boleh menggunakan Supabase client dengan RLS aktif. Stack UI/forms mengikuti komponen yang sudah ditetapkan PRD.

## Alasan

Keputusan ini mengikuti stack MVP yang dipilih di PRD dan memberi satu jalur implementasi untuk UI serta operasi server, sambil menggunakan layanan terkelola untuk database, auth, dan storage. Batas tanggung jawab akses pengguna dan unggahan dirinci pada ADR-0002 dan ADR-0003.

## Konsekuensi

### Positif

- Satu aplikasi web dapat menyediakan UI dan backend untuk operasi yang butuh orkestrasi.
- PostgreSQL cocok untuk relasi lamaran, riwayat tahap, balasan, pengguna, dan metadata berkas.
- Supabase menyediakan komponen database, auth, dan storage yang dipilih untuk MVP.
- Beban mengelola infrastruktur database dan aplikasi lebih rendah dibanding self-hosting.

### Negatif dan risiko

- Aplikasi bergantung pada platform Vercel dan Supabase serta batas/fitur tier yang digunakan.
- Konfigurasi keamanan Supabase tetap harus benar; layanan terkelola tidak otomatis mengamankan policy aplikasi.
- Alur yang menyentuh PostgreSQL dan Storage tidak mendapat transaksi atomik lintas layanan.
- Konfigurasi deployment, backup, dan metode auth perlu ditetapkan/diterapkan sesuai ADR terkait dan kemampuan tier provider.

## Catatan implementasi

- Jangan menaruh secret/service-role key di bundle browser.
- Validasi pada browser hanya untuk UX; validasi yang menentukan akses, kuota, dan validitas unggahan dilakukan server-side.
- RLS dan Storage policies tetap wajib untuk operasi browser.
- Cocokkan fitur backup dengan tier Supabase yang benar-benar digunakan; jangan menganggap objek Storage tercakup tanpa verifikasi.
- Playwright merupakan rencana pengujian, bukan klaim bahwa pengujian sudah tersedia.

## ADR terkait

- [ADR-0002: Autentikasi dan isolasi data pengguna](0002-user-data-isolation.md)
- [ADR-0003: Konsistensi unggahan dan penghapusan berkas](0003-upload-consistency.md)

## Referensi

- `docs/PRD.md`, bagian 4. Technical Specifications dan bagian 5. MVP.
- [`../architecture.md`](../architecture.md)
