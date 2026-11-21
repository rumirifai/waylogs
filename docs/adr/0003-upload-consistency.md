# ADR-0003: Konsistensi unggahan dan penghapusan berkas

## Status

Accepted

## Konteks

WayLogs menyimpan CV, portofolio, dan dokumen pribadi di Supabase Storage serta metadata/relasinya di PostgreSQL. Object storage dan database tidak berbagi transaksi atomik. PRD mensyaratkan bahwa relasi berkas dan perubahan lamaran hanya disimpan setelah seluruh file berhasil diunggah dan divalidasi; kegagalan tidak boleh meninggalkan perubahan lamaran parsial. PRD juga mensyaratkan validasi backend dan pembersihan unggahan sementara/orphan.

Pengguna boleh menghapus lamaran dan berkas. Penghapusan harus membersihkan relasi serta objek terkait. Proses terdistribusi ini dapat mengalami kegagalan parsial dan harus dapat dicoba ulang.

## Pendorong keputusan

- Menghindari catatan lamaran yang menunjuk ke file yang belum tervalidasi/final.
- Menangani kegagalan jaringan, pembatalan, dan kegagalan metadata tanpa kehilangan jejak pemulihan.
- Menegakkan tipe, signature, ukuran, kuota, dan kepemilikan di backend.
- Menangani cleanup agar unggahan terbengkalai/orphan tidak menetap tanpa batas.
- Membuat finalisasi serta penghapusan aman diulang.

## Opsi yang dipertimbangkan

1. **Unggah langsung ke lokasi final sebelum metadata disimpan**: lebih sederhana, tetapi kegagalan berikutnya dapat meninggalkan objek yang tampak final dan sulit dibedakan dari file aktif.
2. **Area sementara, lalu validasi dan finalisasi yang dapat diulang**: membutuhkan state dan cleanup, tetapi membuat batas antara file belum committed dan file aktif eksplisit.
3. **Satu transaksi atomik untuk database dan Storage**: tidak dipilih karena kedua layanan tidak menyediakan satu transaksi atomik bersama.

## Keputusan

Gunakan alur sementara dengan finalisasi terkontrol:

1. Saat pengguna memilih Simpan, unggah file ke area sementara yang terikat pada pengguna dan operasi unggah. Pemilihan file saja tidak mengubah lamaran atau pustaka final.
2. Backend memverifikasi sesi dan kepemilikan, ekstensi allowlist (PDF, DOCX, PPTX, DOC, PPT), signature/tipe aktual, batas 20 MB per file, dan kuota 500 MB per pengguna. Peringatan kuota ditampilkan mulai 80% (400 MB); upload yang melampaui kuota ditolak. DOCM/PPTM dan ZIP tidak diizinkan.
3. Jangan mengandalkan `Content-Type` browser. Backend melakukan validasi aktual dan mencegah path/object key dari pengguna menentukan lokasi di luar ruang pemiliknya.
4. Simpan state operasi unggah yang memungkinkan retry/idempotency. Metadata dan relasi lamaran hanya menjadi final setelah semua file yang diperlukan selesai diunggah dan lolos validasi. Pengiriman ulang operasi finalisasi tidak boleh membuat relasi/objek duplikat.
5. Jika ada unggahan/validasi yang gagal atau pengguna membatalkan, jangan simpan perubahan lamaran. Berikan hasil kesalahan per file dengan pilihan retry atau hapus file gagal. Bersihkan objek sementara segera jika memungkinkan; proses retry menangani kegagalan cleanup.
6. Jalankan cleanup terjadwal untuk objek sementara yang belum difinalisasi setelah 24 jam. Masa tenggang ini adalah default awal yang dapat dikonfigurasi; proses cleanup harus aman diulang. Jalankan pula sweep berkala untuk menemukan objek yatim dan metadata yang tidak konsisten.
7. Koreksi file yang telah dikaitkan dilakukan dengan mengganti referensi ke file pengganti sesuai PRD; tampilkan waktu pembaruan tanpa menyimpan versi koreksi sebelumnya.
8. Penghapusan lamaran/berkas dilakukan melalui backend: hapus/putuskan relasi metadata sesuai aturan domain dan hapus objek Storage terkait. Lacak status cleanup yang belum selesai dan retry sampai konsisten. Penghapusan satu lamaran tidak menghapus file pustaka yang masih dipakai lamaran lain; hapus objek hanya ketika tidak ada referensi aktif atau pengguna menghapus file pustaka secara eksplisit.
9. Backup mengikuti kemampuan dan retensi provider yang digunakan. Penghapusan dari sistem aktif tidak menjanjikan penghapusan langsung dari salinan backup; batas retensi provider harus didokumentasikan. Sebelum peluncuran, verifikasi apakah backup tier Supabase mencakup database dan objek Storage. Gunakan backup platform untuk database, dan tambahkan salinan terpisah untuk berkas bila Storage tidak tercakup dan berkas perlu dapat dipulihkan.

## Alasan

Pemisahan area sementara dan final mencegah objek yang belum siap dianggap sebagai data aktif. State yang idempotent dan cleanup retry mengakui bahwa PostgreSQL dan Storage tidak dapat diperbarui atomik. Validasi server dan pemeriksaan kepemilikan memenuhi persyaratan keamanan; penghapusan berbasis referensi menghindari terhapusnya berkas yang masih dipakai lamaran lain.

## Konsekuensi

### Positif

- Kegagalan upload tidak menyimpan perubahan lamaran parsial.
- Finalisasi dan cleanup dapat dicoba ulang setelah timeout atau kegagalan sementara.
- Format, ukuran, kuota, dan kepemilikan diperiksa pada server.
- File bersama tetap dapat ditautkan ke beberapa lamaran tanpa penghapusan prematur.

### Negatif dan risiko

- Dibutuhkan state operasi, endpoint/backend orchestration, cleanup terjadwal, dan observabilitas kegagalan.
- Cleanup terjadwal mungkin memerlukan fasilitas scheduler yang sesuai dengan deployment; mekanisme aktual perlu dipilih saat implementasi.
- Backup dan penghapusan backup mengikuti kapabilitas serta retensi tier/provider; objek Storage perlu diverifikasi terpisah.
- Orphan dapat tetap ada sementara sampai retry/sweep berhasil.

## Catatan implementasi

- Gunakan ID operasi unggah yang unik dan validasi transisi status yang sah.
- Periksa kuota secara konsisten ketika ada upload bersamaan agar beberapa request tidak melewati batas total.
- Pastikan cleanup tidak menghapus objek yang telah difinalisasi atau masih direferensikan.
- Catat kegagalan finalisasi/penghapusan agar retry dan sweep dapat menemukan pekerjaan tertunda.
- Jangan membuat klaim penghapusan backup seketika; tampilkan kebijakan retensi provider yang berlaku.
- Verifikasi fitur backup pada tier Supabase sebelum memutuskan kebutuhan salinan Storage tambahan.

## ADR terkait

- [ADR-0001: Arsitektur aplikasi MVP](0001-application-architecture.md)
- [ADR-0002: Autentikasi dan isolasi data pengguna](0002-user-data-isolation.md)

## Referensi

- `docs/PRD.md`, User Story 5, Security & Privacy, dan Technical Risks.
- [Supabase Storage](https://supabase.com/docs/guides/storage)
