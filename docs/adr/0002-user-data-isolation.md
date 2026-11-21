# ADR-0002: Autentikasi dan isolasi data pengguna

## Status

Accepted

## Konteks

WayLogs menyimpan data tracker dan dokumen pribadi milik pengguna. Pengguna MVP adalah pemilik aplikasi dan beberapa kenalan yang dikenal, tanpa kebutuhan referral. PRD menetapkan Supabase Auth serta akses privat per pemilik, tetapi tidak merinci metode login, RLS, Storage policies, atau pembagian operasi browser/backend.

Pemilik produk memilih email/password dan Google Auth. Isolasi data memakai identitas pengguna terautentikasi dan `user_id`. Supabase memerlukan konfigurasi policy: autentikasi saja tidak membatasi baris yang dapat diakses, sehingga RLS dan Storage policies perlu diterapkan secara eksplisit.

## Pendorong keputusan

- Pengguna hanya boleh melihat dan mengubah data serta berkas miliknya.
- Alur login tetap sederhana untuk pengguna awal.
- Operasi browser tidak boleh membuka akses lintas akun.
- Rahasia backend tidak pernah dikirim ke browser.
- Pemeriksaan kepemilikan berlaku untuk operasi server maupun browser.

## Opsi yang dipertimbangkan

1. **Supabase Auth dengan email/password dan Google Auth; RLS/Storage policies berdasarkan pemilik**: sesuai pilihan produk dan menyediakan identitas sesi yang menjadi dasar policy.
2. **Autentikasi khusus buatan aplikasi**: tidak dipilih karena menambah tanggung jawab pengelolaan kredensial dan sesi di luar platform auth yang telah dipilih.
3. **Mengandalkan pemeriksaan UI atau hanya menyembunyikan data di frontend**: ditolak karena tidak memberi kontrol akses pada database atau object storage.

## Keputusan

1. Gunakan Supabase Auth dengan email/password dan Google Auth. Akses awal ditujukan untuk pemilik dan kenalan yang diizinkan; produk tidak membuat referral sebagai fitur.
2. Setiap entitas privat memiliki kepemilikan yang dapat ditentukan dari pengguna terautentikasi (`user_id` atau relasi kepemilikan setara). Setiap tabel di schema yang terekspos wajib mengaktifkan RLS dan policy harus membatasi baris kepada pemilik.
3. Policy UPDATE wajib membatasi baris lama dan memastikan nilai kepemilikan setelah update tetap milik pengguna yang sama. Policy INSERT juga memastikan pemilik yang disimpan sama dengan identitas sesi.
4. Objek Storage menggunakan struktur/path yang memungkinkan pemilik diverifikasi, dan policies Storage membatasi baca, tulis, serta hapus ke pemilik yang sama.
5. CRUD sederhana boleh langsung dari browser menggunakan Supabase client dan sesi pengguna, dengan RLS/Storage policies aktif. Operasi yang memerlukan validasi atau orkestrasi khusus, terutama unggahan/finalisasi dan penghapusan berkas, dijalankan melalui Next.js backend.
6. Kunci publishable/public boleh berada di browser; service-role/secret key hanya di server. Server harus memverifikasi sesi dan kepemilikan sebelum operasi yang memakai hak istimewa. RLS tetap menjadi pertahanan tambahan, bukan digantikan oleh pemeriksaan server.

## Alasan

Supabase Auth menyediakan identitas sesi yang dapat dipakai policy. RLS membatasi data pada lapisan PostgreSQL sekalipun request datang dari browser; Storage policies melengkapi kontrol yang sama untuk objek. Backend Next.js menampung alur yang memerlukan orkestrasi tanpa mengekspos rahasia. Pola ini menyesuaikan kemampuan pengguna awal sekaligus melindungi batas antar akun.

## Konsekuensi

### Positif

- Data dan berkas dibatasi di lapisan layanan, bukan hanya oleh UI.
- Operasi CRUD sederhana tidak harus melewati endpoint kustom untuk setiap permintaan.
- Operasi sensitif dapat divalidasi dan diorkestrasi di server.

### Negatif dan risiko

- Setiap tabel dan bucket memerlukan policy yang benar dan pengujian lintas pengguna.
- Kesalahan policy, join/relasi yang tidak memeriksa kepemilikan, atau penggunaan service-role yang tidak tepat dapat membuka data.
- Google Auth memerlukan konfigurasi OAuth pada Supabase dan penyedia identitas; metode login memerlukan pengujian alur callback dan sesi.
- Kebutuhan pembatasan pendaftaran untuk pengguna awal harus diwujudkan pada konfigurasi Auth/deployment, bukan melalui asumsi bahwa aplikasi referral tersedia.

## Catatan implementasi

- Aktifkan RLS pada setiap tabel di schema exposed, termasuk tabel baru.
- Policy harus memeriksa kepemilikan, bukan hanya role `authenticated`.
- Jangan memakai metadata profil yang dapat diubah pengguna sebagai klaim otorisasi.
- Jangan menaruh service-role/secret key dalam variabel `NEXT_PUBLIC_*` atau kode client.
- Uji setidaknya dua akun: akun A tidak dapat membaca, mengubah, menghapus, atau mengakses objek milik akun B.
- Periksa grant/eksposur Data API selain RLS; aksesibilitas tabel dan visibilitas baris merupakan kontrol berbeda.

## ADR terkait

- [ADR-0001: Arsitektur aplikasi MVP](0001-application-architecture.md)
- [ADR-0003: Konsistensi unggahan dan penghapusan berkas](0003-upload-consistency.md)

## Referensi

- `docs/PRD.md`, bagian 4. Architecture Overview dan Security & Privacy.
- [Supabase: Row Level Security](https://supabase.com/docs/guides/database/postgres/row-level-security)
- [Supabase: Storage access control](https://supabase.com/docs/guides/storage/security/access-control)
- [Supabase: Securing the Data API](https://supabase.com/docs/guides/api/securing-your-api)
