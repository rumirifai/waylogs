# PRD: WayLogs

**Status:** Draft berdasarkan eksplorasi produk  
**Bahasa:** Indonesia  
**Platform awal:** Web responsif  
**Cakupan dokumen:** MVP dan arah lanjutan  
**Catatan:** Bagian berlabel **Belum diputuskan (TBD)** belum disetujui sebagai kebutuhan final.

## 1. Executive Summary

### Problem Statement

Pencari kerja aktif mengirim lamaran melalui banyak kanal, sehingga status, detail lowongan, balasan, dan berkas yang digunakan tersebar. Pengguna kesulitan memusatkan lamaran dan memahami progres pencarian kerjanya.

### Proposed Solution

WayLogs adalah tracker pribadi berbasis web responsif untuk mencatat lamaran dari kanal mana pun, memantau tahapan proses, menghubungkan berkas yang digunakan dengan tiap lamaran, dan meninjau ringkasan progres. MVP menggunakan pencatatan manual, alur status visual, log balasan manual, serta pustaka berkas.

### Success Criteria

Untuk MVP yang digunakan oleh pemilik dan beberapa teman, keberhasilan dinilai secara praktis: pengguna dapat memusatkan lamaran dari berbagai kanal, memperbarui status dan balasan, mengaitkan berkas yang dipakai, serta meninjau progres dari WayLogs. Penetapan KPI kuantitatif dan periode evaluasinya ditunda ke pengembangan selanjutnya.

Kriteria praktis MVP:

- Pengguna dapat mencatat lamaran dan melihatnya bersama lamaran lain dalam satu tracker.
- Pengguna dapat memperbarui tahapan dan mencatat balasan tanpa bergantung pada spreadsheet terpisah.
- Pengguna dapat melihat berkas yang terkait dengan tiap lamaran.
- Ringkasan menampilkan status dan respons berdasarkan data yang dicatat.

## 2. User Experience & Functionality

### User Personas

**Persona utama: pencari kerja aktif**

- Sedang mengelola beberapa lamaran yang masuk melalui kanal berbeda.
- Membutuhkan satu tempat untuk memperbarui status dan melihat progres.
- Dapat berupa mahasiswa atau lulusan baru yang sedang aktif mencari kerja.

**Konteks penting:** Pengguna dapat memiliki CV atau portofolio master, versi kustom untuk beberapa lamaran, dan memakai berkas master untuk lamaran lain.

### User Stories

#### Story 1: Mencatat lamaran

Sebagai pencari kerja aktif, saya ingin mencatat lamaran dari kanal mana pun secara manual agar seluruh proses saya terpusat.

**Acceptance Criteria**

- Pengguna dapat membuat catatan lamaran dengan perusahaan, posisi, tanggal melamar, dan status.
- Catatan dapat menyertakan tautan lowongan, kompensasi, lokasi, sistem kerja, kanal pengiriman, dan catatan.
- Pengguna dapat membuat lamaran tanpa impor dari job board atau spreadsheet.
- Pengguna dapat melihat dan memperbarui catatan lamaran yang telah dibuat.

#### Story 2: Memantau tahapan lamaran

Sebagai pencari kerja aktif, saya ingin melihat lamaran dalam alur status visual agar posisi tiap proses mudah dipahami.

**Acceptance Criteria**

- Status awal yang disediakan adalah Dikirim, Screening, Tes, Wawancara, Penawaran, dan Selesai.
- Pengguna menandai lamaran selesai secara manual dengan memilih hasil Diterima, Ditolak, Ditarik, atau Tanpa kabar.
- WayLogs tidak otomatis mengubah status kemajuan atau menandai lamaran selesai pada MVP.
- Pengguna dapat melompati atau memundurkan tahapan.
- Perubahan status kemajuan dicatat dalam riwayat perjalanan lamaran.
- Pengguna dapat mengubah hasil akhir yang sudah dipilih, misalnya untuk memperbaiki salah input atau mencatat kabar yang datang terlambat.
- Perubahan hasil akhir tidak dicatat dalam riwayat perjalanan lamaran.
- Pengguna dapat mengubah nama dan urutan tahapan.

#### Story 3: Melihat progres pencarian kerja

Sebagai pencari kerja aktif, saya ingin melihat jumlah lamaran per status dan ringkasan respons agar dapat memahami progres tanpa menghitung manual.

**Acceptance Criteria**

- Ringkasan menampilkan jumlah lamaran per status.
- Pengguna dapat memilih periode preset dan rentang tanggal bebas.
- Ringkasan respons menampilkan tingkat respons keseluruhan dan rincian kategori.
- Kategori ringkasan mencakup Lanjut ke tahap, Penolakan, dan Balasan umum.
- Jika ada beberapa jenis balasan untuk satu lamaran, ringkasan menggunakan kategori hasil terakhir; riwayat detail tetap menyimpan balasan sebelumnya.
- Satu lamaran dihitung sebagai satu lamaran yang mendapat respons, berapa pun jumlah balasannya.
- Filter periode memilih cohort berdasarkan tanggal melamar. Semua lamaran dalam rentang langsung masuk denominator, termasuk lamaran yang baru dikirim.
- Lamaran dalam cohort dihitung pernah mendapat respons jika tercatat menerima setidaknya satu balasan kapan pun, termasuk balasan yang datang setelah rentang filter berakhir. Tingkat respons cohort dapat bertambah ketika balasan terlambat dicatat.
- Ringkasan dapat menunjukkan langkah berikutnya dan tanggal tindak lanjut yang dicatat pengguna.

#### Story 4: Mencatat balasan

Sebagai pencari kerja aktif, saya ingin mencatat balasan pada lamaran terkait agar perkembangan komunikasi dan hasil proses tetap memiliki konteks.

**Acceptance Criteria**

- Pengguna dapat menambahkan balasan secara manual pada sebuah lamaran.
- Balasan dapat memiliki kategori Lanjut ke tahap, Penolakan, atau Balasan umum.
- Balasan tersimpan di riwayat lamaran dan dapat memengaruhi kategori hasil terakhir pada ringkasan.
- Integrasi kotak email tidak termasuk MVP.
- Isi balasan, tanggal yang diinput pengguna, dan referensi email bukan persyaratan MVP; ketiganya masuk backlog pengembangan selanjutnya.

#### Story 5: Mengelola berkas lamaran

Sebagai pencari kerja aktif, saya ingin menyimpan berkas dan mengaitkan berkas yang dipakai dengan lamaran tertentu agar saya tahu dokumen mana yang digunakan.

**Acceptance Criteria**

- Pengguna dapat mengelola pustaka berkas, termasuk berkas master dan berkas kustom.
- Berkas yang sama dapat ditautkan ke lebih dari satu lamaran.
- Untuk tiap jenis dokumen pada sebuah lamaran, pengguna mencatat berkas yang digunakan; berkas dengan tujuan berbeda seperti CV dan portofolio merupakan jenis terpisah.
- Setelah lamaran dikirim, berkas yang tercatat dikunci agar catatan mencerminkan berkas yang digunakan.
- Jika catatan berkas salah, pengguna dapat mengoreksinya dengan berkas pengganti. Sistem menampilkan waktu last update, tanpa meminta alasan perubahan atau menyimpan berkas koreksi sebelumnya.
- Format yang diizinkan untuk MVP adalah PDF, DOCX, PPTX, DOC, dan PPT. Format bermakro seperti DOCM/PPTM dan arsip ZIP tidak diizinkan.
- Ukuran maksimum adalah 20 MB per berkas, dengan kuota total 500 MB per pengguna.
- WayLogs menampilkan peringatan saat penggunaan mencapai 80% kuota (400 MB) dan menolak unggahan yang akan membuat pemakaian melebihi 100% kuota (500 MB).
- Kuota 500 MB adalah perkiraan awal untuk pustaka pribadi yang berisi CV/resume, surat lamaran, portofolio, transkrip, sertifikat, surat referensi, sampel kerja, dan tugas seleksi. Perkiraan kebutuhan 100-300 MB bukan statistik industri dan perlu ditinjau dari pemakaian aktual.
- Pengguna dapat memilih atau mengganti berkas dalam formulir tanpa menyimpan perubahan pada lamaran/pustaka. Saat pengguna memilih Simpan, berkas diunggah ke area sementara.
- Relasi berkas dan perubahan lamaran baru disimpan setelah semua berkas berhasil diunggah dan divalidasi.
- Jika proses dibatalkan atau unggahan gagal, perubahan lamaran tidak disimpan. Unggahan sementara dibatalkan atau dibersihkan; pesan kesalahan menjelaskan masalah per berkas dan memberi pilihan untuk mencoba ulang atau menghapus berkas yang gagal.
- Backend memvalidasi ekstensi, tipe/signature berkas, ukuran, kuota, dan hak akses. Validasi tidak hanya mengandalkan tipe yang dikirim browser.
- Berkas di object storage dan catatan di database tidak dapat disimpan dalam satu transaksi atomik. Backend perlu menangani finalisasi yang aman diulang, membersihkan unggahan sementara bila penyimpanan metadata gagal, serta membersihkan berkas yatim yang tertinggal.

#### Story 6: Menggunakan bantuan AI di masa mendatang

Sebagai pencari kerja, saya ingin meminta saran yang relevan dari data lamaran dan berkas saya agar dapat mempersiapkan wawancara dan menyesuaikan dokumen.

**Acceptance Criteria untuk fase lanjutan**

- Bantuan yang dieksplorasi mencakup persiapan wawancara, analisis pencarian kerja, penilaian kesesuaian dokumen dengan lowongan, dan strategi penyesuaian.
- Izin diberikan per jenis bantuan.
- Untuk jenis bantuan yang diaktifkan, AI dapat memakai seluruh data tracker dan berkas yang relevan.
- AI memberikan saran; AI tidak mengirim lamaran atau mengubah tracker secara otomatis.
- Permintaan AI dimulai oleh pengguna, bukan pemrosesan latar belakang.
- Usulan untuk diputuskan sebelum AI dibuat: jelaskan data yang akan dikirim sebelum permintaan, dan sediakan kontrol pencabutan izin per bantuan, misalnya melalui Settings. Pencabutan hanya dapat menghentikan permintaan berikutnya dan tidak menjamin penghapusan data provider.
- Retensi percakapan belum diputuskan. Alternatif yang perlu dievaluasi mencakup konteks sementara tanpa transcript permanen, atau penyimpanan riwayat chat yang dapat dihapus pengguna; penyimpanan hasil ke catatan lamaran juga perlu ditentukan.
- Detail retensi oleh provider, pemrosesan dan penghapusan file di provider, serta model yang digunakan masih **TBD**.
- Fitur AI bukan bagian dari MVP awal.

### Non-Goals

Hal-hal berikut tidak termasuk MVP berdasarkan eksplorasi saat ini:

- Impor spreadsheet.
- Integrasi email atau pencocokan balasan otomatis.
- Pengingat otomatis.
- Kustomisasi status mendalam di luar perubahan nama dan urutan.
- Pengiriman lamaran otomatis.
- AI yang mengubah data tracker atau dokumen secara otomatis.
- Analitik AI sebagai bagian MVP.
- Pemrosesan AI otomatis di latar belakang atau pengiriman seluruh berkas tanpa permintaan pengguna.
- Mengekspos OpenRouter API key ke browser atau menyimpannya di kode frontend.

## 3. AI System Requirements (If Applicable)

AI merupakan arah produk setelah MVP, bukan persyaratan peluncuran MVP.

### Tool Requirements

- Penyedia AI final belum dipilih. Kandidat backlog adalah OpenRouter API dengan model gratis untuk uji coba dan penggunaan pribadi berfrekuensi rendah; ini bukan keputusan final.
- Usulan penggunaan satu OpenRouter API key khusus WayLogs milik pemilik aplikasi, disimpan hanya di backend dan tidak dikirim ke browser. Pemilik akun menanggung pemakaian; aplikasi perlu membatasi request per pengguna. Key khusus membatasi dampak jika perlu dinonaktifkan atau diputar.
- Model gratis dapat berubah ketersediaan, batas request, latensi, kualitas, dan provider endpoint-nya. Jangan mengandalkan ketersediaan atau perilaku model yang konsisten; model/router final masih **TBD**.
- Potensi input mencakup data tracker, deskripsi lowongan, dan dokumen yang diunggah pengguna.
- Izin per jenis bantuan menjadi prinsip akses yang dipilih; untuk bantuan yang diaktifkan, akses mencakup data tracker dan berkas yang relevan.
- Usulan pemrosesan: kirim hanya konteks yang relevan dengan permintaan, misalnya lowongan dan CV/portofolio terkait, bukan seluruh data tanpa seleksi pada setiap permintaan.
- Bentuk ekstraksi teks atau konversi berkas, kebijakan retensi percakapan, penghapusan berkas dari provider, model/provider final, dan pembatasan biaya masih **TBD**.
- Usulan privasi OpenRouter: nonaktifkan logging input/output opsional, batasi routing ke endpoint yang memenuhi kebijakan data (`data_collection: deny` dan/atau ZDR bila tersedia), serta jangan fallback diam-diam ke endpoint dengan kebijakan yang lebih longgar. Ketersediaan model gratis yang memenuhi syarat perlu diuji.
- OpenRouter dan provider model memiliki kebijakan berbeda. OpenRouter menyatakan prompt tidak disimpan olehnya kecuali fitur tertentu diaktifkan, tetapi provider endpoint dapat memiliki aturan retensi dan penggunaan data tersendiri.

Referensi untuk backlog, kebijakan dan batas dapat berubah: [OpenRouter free models](https://openrouter.ai/docs/guides/routing/routers/free-router), [limits](https://openrouter.ai/docs/api_reference/limits), [data collection](https://openrouter.ai/docs/guides/privacy/data-collection), [provider logging](https://openrouter.ai/docs/guides/privacy/provider-logging), dan [provider routing/data policies](https://openrouter.ai/docs/guides/routing/provider-selection).

### Evaluation Strategy

- Sebelum peluncuran AI, evaluasi terpisah perlu dibuat untuk tiap jenis bantuan:
  - **Persiapan wawancara:** relevansi dan keterkaitan saran dengan posisi serta deskripsi lowongan.
  - **Analisis pencarian kerja:** ketepatan perhitungan dan keterlacakan insight ke data pengguna.
  - **Kecocokan dokumen:** apakah temuan merujuk pada isi dokumen dan persyaratan lowongan yang benar.
  - **Strategi penyesuaian:** apakah saran spesifik, dapat ditindaklanjuti, dan tidak mengarang pengalaman pengguna.
- Usulan evaluasi awal: buat kumpulan kecil contoh uji anonim yang mewakili CV dan lowongan, lalu tinjau hasil secara manual sebelum peluncuran dan saat model/prompt berubah.
- Rubrik usulan mencakup kesesuaian dengan bukti sumber, ketepatan pencocokan, kegunaan saran, dan tidak mengarang pengalaman/kualifikasi. Hasil yang mengarang fakta tentang pengguna merupakan kegagalan kritis.
- Pengguna tetap meninjau saran; WayLogs dapat menyediakan umpan balik sederhana seperti membantu/tidak membantu.
- Ukuran dataset, rubrik final, target kelulusan, dan proses evaluasi produksi masih **TBD**.

## 4. Technical Specifications

### Architecture Overview

Stack awal yang dipilih:

1. **Aplikasi web:** Next.js App Router dan TypeScript.
2. **UI:** Tailwind CSS dan shadcn/ui untuk styling/komponen, dengan Lucide untuk ikon.
3. **Backend data:** Supabase untuk PostgreSQL, autentikasi, dan penyimpanan berkas.
4. **Deployment:** Vercel untuk aplikasi Next.js dan domain.
5. **Form:** React Hook Form untuk pengelolaan form/state dan Zod untuk validasi data.
6. **Pengujian browser:** Playwright direncanakan untuk menguji alur penting pada tahap selanjutnya; waktu penerapannya belum ditentukan.

Secara konseptual, aplikasi Next.js menyediakan pencatatan lamaran, status, ringkasan, log balasan, dan pustaka berkas. Supabase menyimpan data dan berkas terkait, sedangkan Vercel menjalankan aplikasi. Fitur AI, jika jadi dibuat, menjadi komponen fase lanjutan.

Detail konfigurasi deployment, metode login/invitasi, kebijakan akses Supabase (RLS dan Storage), backup, dan desain skema penyimpanan masih **TBD**.

### Integration Points

- Supabase: PostgreSQL, Auth, dan Storage.
- Vercel: deployment aplikasi Next.js dan konfigurasi domain.
- Integrasi eksternal AI belum ditentukan; OpenRouter tetap kandidat backlog saja.
- Integrasi email dan sumber lowongan otomatis: bukan bagian MVP.
- Impor data dari spreadsheet: ditunda.

### Security & Privacy

- Data tracker dan berkas bersifat pribadi dan terkait pengguna.
- MVP menggunakan Supabase Auth; metode login dan cara mengundang teman masih **TBD**.
- Akses data dan berkas harus dibatasi per pemilik melalui kebijakan database/Storage; detail RLS dan Storage policies masih **TBD**.
- Berkas CV, portofolio, dan dokumen lamaran dapat berisi informasi pribadi; kebijakan retensi, penghapusan, dan backup masih **TBD**. Batas awal unggahan adalah 20 MB per berkas dan kuota 500 MB per pengguna.
- Unggahan menggunakan allowlist format MVP, validasi tipe/signature di backend, batas ukuran dan kuota, serta pemeriksaan hak akses pengguna.
- Integrasi AI di masa mendatang memerlukan izin per jenis bantuan dan penjelasan data relevan yang dipakai.
- Jika OpenRouter dipilih nanti, API key tidak boleh terekspos pada browser; biaya dan request perlu dibatasi karena key dipakai bersama oleh pengguna WayLogs.
- Kebijakan retensi dan pemrosesan provider AI harus ditinjau sebelum berkas pribadi dikirim. Penghapusan data di WayLogs tidak menjamin penghapusan segera dari provider.
- Kebijakan penyimpanan dan pemrosesan data oleh penyedia AI belum ditentukan.
- Dokumen ini tidak membuat klaim sertifikasi atau kepatuhan.

## 5. Risks & Roadmap

### Phased Rollout

#### MVP

- Web responsif.
- Pencatatan manual lamaran dari berbagai kanal.
- Tahapan status visual dengan perubahan nama/urutan dan riwayat perpindahan.
- Ringkasan jumlah per status dan respons dengan filter periode.
- Pencatatan balasan manual dan langkah berikutnya.
- Pustaka berkas dan pengaitan berkas yang digunakan ke lamaran.
- Unggahan PDF, DOCX, PPTX, DOC, dan PPT dengan batas 20 MB per berkas dan kuota 500 MB per pengguna.
- Unggahan sementara yang hanya difinalisasi setelah seluruh berkas berhasil, pembatalan, validasi, penanganan kegagalan, dan pembersihan berkas sementara.

#### Backlog pengembangan berikutnya (urutan TBD)

Item berikut adalah kemungkinan pengembangan setelah MVP, bukan komitmen versi atau urutan prioritas:

- Pengingat tindak lanjut otomatis.
- Impor spreadsheet.
- Integrasi email atau pencocokan balasan.
- Penambahan isi balasan, tanggal manual, atau referensi email ke log balasan.
- KPI kuantitatif jika penggunaan meluas, berdasarkan pola penggunaan aktual.
- Bantuan AI untuk persiapan wawancara, analisis pencarian kerja, dan kecocokan/penyesuaian dokumen.
- Uji OpenRouter dan model gratis, termasuk ketersediaan, batas request, serta kebijakan data endpoint.
- Penetapan privasi, izin, retensi percakapan/berkas, pengelolaan key, dan evaluasi sebelum AI tersedia.

Tidak ada prioritas atau urutan rilis untuk backlog ini yang sudah disetujui.

### Technical Risks

- **Balasan terlambat:** tingkat respons untuk cohort lamaran dapat meningkat seiring datangnya balasan setelah periode filter; ringkasan perlu menjelaskan bahwa hitungan mencakup balasan kapan pun.
- **Pencatatan manual tidak lengkap:** pengguna bisa lupa memperbarui status atau mencatat balasan, sehingga ringkasan tidak merepresentasikan kondisi sebenarnya.
- **Berkas sensitif:** penyimpanan unggahan meningkatkan kebutuhan kontrol akses, penghapusan, retensi, dan keamanan.
- **Koreksi berkas:** menyimpan pengganti saja dapat mengurangi keterlacakan berkas yang sempat salah tercatat.
- **Konsistensi unggahan:** gangguan layanan atau koneksi dapat meninggalkan objek sementara; finalisasi harus aman diulang dan pembersihan berkas yatim perlu tersedia.
- **Perbedaan alur rekrutmen:** tahapan standar mungkin tidak cocok untuk semua perusahaan; kemampuan melompati tahap dan mengubah nama/urutan membantu, tetapi aturan status akhir masih perlu diuji.
- **AI masa depan:** akses ke tracker dan dokumen dapat menimbulkan risiko privasi, biaya, dan ketidakakuratan; fase ini memerlukan evaluasi dan batas data tersendiri.
- **OpenRouter/model gratis:** ketersediaan, rate limit, latensi, kualitas, dan kebijakan data dapat berbeda antar-model/provider serta berubah. Pembatasan privasi dapat mengurangi pilihan model.
- **Key bersama:** satu API key backend untuk beberapa pengguna dapat menghabiskan kuota atau disalahgunakan; autentikasi, rate limit per pengguna, dan batas akun perlu dirancang sebelum peluncuran.

### Keputusan yang Belum Diputuskan

1. Detail deployment Vercel, metode login/invitasi, kebijakan akses Supabase (RLS dan Storage), backup, dan desain skema.
2. Kebijakan privasi AI, retensi provider dan aplikasi, penghapusan berkas/transkrip AI, serta mekanisme pencabutan izin.
3. Apakah OpenRouter dipilih, model/endpoint yang sesuai kebijakan data, dan batas penggunaan/key.
4. Dataset, rubrik final, dan ambang evaluasi kualitas fitur AI.
5. Urutan prioritas dan waktu rilis item backlog setelah MVP.
