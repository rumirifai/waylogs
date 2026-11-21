# Gambaran Arsitektur WayLogs

**Status:** Rancangan MVP  
**Sumber:** `docs/PRD.md` dan ADR di `docs/adr/`

## Ringkasan

WayLogs adalah aplikasi web responsif untuk mengelola lamaran kerja, tahapan proses, balasan, ringkasan progres, dan berkas terkait. Next.js App Router menyediakan antarmuka serta backend aplikasi di Vercel. Supabase menyediakan PostgreSQL, autentikasi, dan object storage.

```text
Browser
  ├── UI Next.js
  ├── Supabase client untuk operasi CRUD sederhana
  │     └── Supabase Auth session + RLS/Storage policies
  └── Request unggahan/mutasi sensitif
        └── Next.js Server Actions / Route Handlers di Vercel
              ├── autentikasi dan pemeriksaan kepemilikan
              ├── validasi berkas, kuota, dan orkestrasi finalisasi
              └── Supabase PostgreSQL dan Storage
```

Diagram tersebut adalah rancangan batas komponen, bukan jaminan bahwa konfigurasi keamanan atau backup telah diterapkan. Detail keputusan dan kontrol wajib dicatat dalam ADR terkait.

## Komponen

| Komponen | Tanggung jawab |
|---|---|
| Next.js App Router + TypeScript | UI tracker, forms, halaman ringkasan, serta Server Actions/Route Handlers untuk operasi yang memerlukan orkestrasi server. |
| Vercel | Menjalankan dan men-deploy aplikasi Next.js serta konfigurasi domain. |
| Supabase Auth | Sesi dan identitas pengguna, dengan metode email/password dan Google Auth yang dipilih untuk MVP. |
| Supabase PostgreSQL | Data lamaran, konfigurasi tahapan, riwayat perpindahan, hasil akhir, balasan, tindak lanjut, metadata berkas, dan relasi kepemilikan. |
| Supabase Storage | Objek berkas pengguna, termasuk area sementara dan objek final. Akses harus dibatasi oleh policy pemilik. |
| React Hook Form + Zod | Pengelolaan form dan validasi data aplikasi. Validasi browser tidak menggantikan validasi backend. |
| Tailwind CSS + shadcn/ui + Lucide | Styling dan komponen UI sesuai stack PRD. |
| Playwright | Pengujian alur browser penting pada tahap berikutnya; waktu implementasinya belum diputuskan. |

## Batas akses dan keamanan

- Data tracker dan berkas bersifat privat per akun, dengan `user_id` sebagai dasar kepemilikan.
- Akses browser ke Supabase hanya menggunakan kunci publik/publishable dan sesi pengguna. Tabel exposed wajib memiliki RLS, dan objek Storage memerlukan policies kepemilikan yang sesuai.
- Service-role/secret key hanya berada di lingkungan server dan tidak boleh dikirim ke browser.
- Operasi CRUD sederhana dapat melalui Supabase client dengan RLS. Operasi validasi dan orkestrasi unggahan/penghapusan ditangani backend Next.js, dengan pemeriksaan autentikasi serta kepemilikan. RLS tetap menjadi lapisan perlindungan database.

## Alur unggahan tingkat tinggi

1. Pengguna memilih file; file baru dikirim ke area sementara ketika pengguna menyimpan formulir.
2. Backend memeriksa kepemilikan, allowlist format, signature, ukuran per berkas, dan kuota pengguna.
3. Setelah seluruh file lolos validasi dan unggahan, backend menyimpan metadata/relasi serta memfinalisasi objek dengan operasi yang aman diulang.
4. Jika proses gagal, perubahan lamaran tidak disimpan; unggahan sementara dibersihkan atau dicoba ulang pembersihannya. Proses terjadwal menangani objek sementara yang terbengkalai dan orphan.
5. Penghapusan lamaran/berkas membersihkan relasi database dan objek Storage melalui alur server-side yang dapat dicoba ulang.

Detail protokol finalisasi, retensi objek sementara, dan konsekuensi kegagalan lintas PostgreSQL/Storage ada di ADR konsistensi unggahan.

## Konvensi dokumentasi

Dokumen ini memberi gambaran komponen. Keputusan dan trade-off arsitektur normatif ada di `docs/adr/README.md` dan ADR yang terindeks di sana. Jika overview berbeda dengan ADR yang diterima, ADR menjadi sumber keputusan dan overview perlu diperbarui.
