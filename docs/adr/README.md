# Architecture Decision Records

Dokumen di direktori ini mencatat keputusan arsitektur WayLogs. Gambaran komponen tersedia di [`../architecture.md`](../architecture.md), sedangkan kebutuhan produk dan acceptance criteria ada di [`../PRD.md`](../PRD.md).

## Daftar keputusan

| ID | Judul | Status |
|---|---|---|
| [ADR-0001](0001-application-architecture.md) | Arsitektur aplikasi MVP | Accepted |
| [ADR-0002](0002-user-data-isolation.md) | Autentikasi dan isolasi data pengguna | Accepted |
| [ADR-0003](0003-upload-consistency.md) | Konsistensi unggahan dan penghapusan berkas | Accepted |

## Status ADR

- **Proposed**: diajukan untuk dibahas, belum menjadi keputusan implementasi.
- **Accepted**: keputusan berlaku untuk pengembangan saat ini.
- **Deprecated**: keputusan tidak lagi disarankan tetapi belum tentu diganti.
- **Superseded**: keputusan digantikan ADR lain.
- **Rejected**: opsi pernah dipertimbangkan dan tidak dipilih.

ADR yang sudah Accepted tidak diubah untuk mengganti keputusan historis. Jika keputusan berubah, buat ADR baru yang merujuk dan menggantikannya.

## Membuat ADR baru

1. Gunakan ID berikutnya dan nama file `NNNN-kata-kunci.md`.
2. Ikuti struktur MADR yang digunakan dokumen saat ini: status, konteks, pendorong, opsi, keputusan, alasan, konsekuensi, implementasi, relasi, dan referensi.
3. Tautkan ADR terkait dan perbarui tabel index ini.
4. Jangan menulis item PRD yang masih TBD sebagai keputusan Accepted tanpa persetujuan.

## Cakupan

ADR 0001-0003 mencakup keputusan inti MVP. Pilihan provider AI, kebijakan retensi AI, evaluasi model, dan fitur backlog tidak diputuskan oleh ADR ini; buat ADR tersendiri jika fase AI diprioritaskan.
