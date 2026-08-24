# Evaluasi Kontrol Otorisasi pada Aplikasi Web Multi-Role

Repository ini berisi proposal dan roadmap untuk tugas mata kuliah Metodologi Penelitian. Fokusnya adalah merancang evaluasi *function-level* dan *object-level authorization* pada aplikasi **Self Order System Management** dengan acuan OWASP Web Security Testing Guide (WSTG).

Repository ini merupakan artefak perencanaan penelitian, bukan aplikasi keamanan, scanner, atau laporan hasil pengujian. Source testbed dan evidence eksekusi belum tersedia di sini.

## Ruang Lingkup

Rancangan penelitian membatasi evaluasi pada:

- pemisahan hak akses fungsi untuk peran PUBLIC, CASHIER, dan OWNER;
- kepemilikan resource antara identitas uji PUBLIC-A dan PUBLIC-B;
- perbandingan *expected access* dengan *actual access*;
- klasifikasi hasil sebagai `compliant`, `vulnerable`, `logic error`, atau `inconclusive`;
- pengujian manual pada lingkungan lokal/laboratorium dengan data fiktif.

SQL injection, XSS, brute force, social engineering, eksploitasi jaringan, dan pengujian sistem publik berada di luar ruang lingkup dokumen ini.

## Isi Repository

- [Proposal penelitian](Proposal_Metopen_Evaluasi_Otorisasi_SOS_final_strict_v4_revisi_final.md) — latar belakang, tinjauan pustaka, metode, instrumen, dan rancangan test case.
- [Roadmap pengujian](ROADMAP.md) — langkah operasional untuk pembekuan objek, penyusunan baseline, pelaksanaan test case, penyimpanan evidence, dan analisis.
- Dokumen `.docx` — salinan proposal untuk kebutuhan penyerahan akademik.

Nama file proposal mengikuti versi dokumen tugas yang sudah ada. Penamaan tersebut dipertahankan agar hubungan antara versi Markdown dan dokumen penyerahan tetap jelas.

## Metode yang Direncanakan

Alur kerja penelitian yang dirancang adalah:

1. membekukan branch, commit, konfigurasi, dan dataset testbed;
2. menyusun matriks hak akses sebagai baseline;
3. menyiapkan akun dan data dummy untuk setiap peran;
4. menjalankan 20 test case authorization secara manual;
5. menyimpan request, response, status HTTP, dan catatan analisis dengan secret yang sudah disamarkan;
6. mengulang temuan yang terindikasi rentan dengan sesi baru;
7. menyusun hasil dan rekomendasi berdasarkan evidence.

Angka test case tersebut adalah bagian dari rancangan, bukan klaim bahwa pengujian sudah selesai.

## Cara Menggunakan Dokumen

Tidak ada proses build atau dependency aplikasi pada repository ini. Dokumen dapat dibaca langsung melalui renderer Markdown GitHub. Jika roadmap akan dipakai untuk pengujian, isi seluruh placeholder freeze dan baseline terlebih dahulu, lalu gunakan hanya pada testbed lokal yang dimiliki atau diizinkan.

## Catatan Keamanan dan Etika

Pengujian dirancang hanya untuk lingkungan lokal/laboratorium yang berada dalam kendali peneliti. Token, cookie, password, identifier objek, dan bukti request-response harus menggunakan data dummy atau disamarkan sebelum dimasukkan ke repository.

## Yang Saya Pelajari

Project mata kuliah ini melatih penyusunan ruang lingkup security testing, pembedaan authentication dan authorization, pemodelan hak akses berbasis peran dan kepemilikan objek, serta pencatatan evidence yang dapat ditinjau ulang.

## Status

**Proposal dan roadmap tersedia; eksekusi penelitian belum dapat diverifikasi dari repository ini.** Evidence hasil yang aman dipublikasikan dapat ditambahkan setelah pengujian benar-benar dilakukan.
