# ROADMAP PENGUJIAN OTORISASI

**Judul penelitian:** Evaluasi Kontrol Otorisasi pada Tingkat Fungsi dan Objek pada Aplikasi Web Multi-Role Menggunakan OWASP Web Security Testing Guide: Studi Kasus Self Order System Management  
**Objek penelitian:** Self Order System Management  
**Fokus pengujian:** Function-level authorization, object-level authorization, BFLA, BOLA, dan Broken Access Control  
**Metode utama:** Manual security testing berbasis test case dan evidence request-response  
**Dokumen acuan utama:** Proposal Metodologi Penelitian versi final strict v4 revisi final

---

## 0. Tujuan ROADMAP.md

Dokumen ini menjadi panduan kerja teknis untuk menjalankan pengujian otorisasi pada aplikasi **Self Order System Management** sesuai proposal Metodologi Penelitian. Proposal sudah menjelaskan dasar teori, metode, dan instrumen penelitian. ROADMAP ini menerjemahkan proposal tersebut menjadi langkah kerja praktis agar pengujian benar-benar bisa dilakukan secara sistematis, valid, etis, dan dapat direplikasi.

ROADMAP ini menjawab pertanyaan berikut:

1. Apa saja yang harus disiapkan sebelum pengujian?
2. Stack teknologi dan alat apa yang digunakan?
3. Bagaimana cara membekukan objek penelitian?
4. Akun, role, endpoint, dan data dummy apa yang diperlukan?
5. Bagaimana menjalankan 20 test case authorization?
6. Evidence apa yang harus disimpan?
7. Bagaimana menentukan hasil allow, deny, vulnerable, compliant, logic error, atau inconclusive?
8. Apa hasil akhir yang diharapkan dari pengujian?
9. Bagaimana menyusun hasil pengujian agar siap masuk ke laporan/proposal lanjutan?

---

## 1. Prinsip Utama Pengujian

Pengujian harus mengikuti prinsip berikut.

1. **Legal dan etis**  
   Pengujian hanya dilakukan pada aplikasi Self Order System Management yang berjalan di lingkungan lokal/laboratorium dan berada dalam kendali peneliti.

2. **Data dummy**  
   Seluruh data pengguna, pesanan, transaksi, token, QR, dan akun uji harus berupa data dummy.

3. **Tidak menyerang sistem publik**  
   Tidak ada pengujian pada aplikasi produksi, sistem pihak ketiga, server publik, atau data pengguna nyata.

4. **Manual security testing**  
   Pengujian utama dilakukan secara manual berdasarkan test case. Tools seperti Postman, curl, browser, atau Burp Suite Community hanya menjadi alat bantu untuk mengirim dan memeriksa request.

5. **Bukan automated scanning**  
   Automated scanner bukan metode utama karena broken access control membutuhkan konteks role, endpoint, ownership, session, dan baseline kebijakan akses.

6. **Baseline dulu, testing kemudian**  
   Expected access harus ditentukan sebelum test case dijalankan. Peneliti tidak boleh mengubah baseline setelah melihat hasil pengujian.

7. **Evidence wajib lengkap**  
   Setiap test case harus memiliki bukti request, response, status HTTP, actual access, dan catatan analisis.

8. **Re-test untuk temuan vulnerable**  
   Jika suatu test case menghasilkan status vulnerable, test case tersebut harus diulang minimal satu kali menggunakan token atau konteks sesi baru.

9. **Hasil tidak boleh dipaksa**  
   Jika semua test case compliant, penelitian tetap valid. Tujuan penelitian bukan memaksa adanya celah, tetapi mengevaluasi kesesuaian otorisasi.

---

## 2. Legend Status ROADMAP

| Status | Arti |
|---|---|
| `[todo]` | Belum dikerjakan |
| `[ongoing]` | Sedang dikerjakan |
| `[ready]` | Siap dijalankan |
| `[tested]` | Sudah diuji, tetapi belum dianalisis penuh |
| `[verified]` | Sudah diuji, dianalisis, dan evidence lengkap |
| `[blocked]` | Terhambat karena dependency, bug environment, atau data belum siap |
| `[deferred]` | Ditunda karena di luar scope penelitian |

Aturan penggunaan status:

- Jangan menandai `[verified]` hanya karena endpoint dapat dipanggil.
- Test case menjadi `[verified]` hanya jika expected access, actual access, evidence, klasifikasi, dan catatan analisis sudah lengkap.
- Jika hasil response tidak bisa disimpulkan, gunakan status analisis `inconclusive`, bukan memaksa menjadi vulnerable atau compliant.

---

## 3. Snapshot Roadmap Pengujian

```text
[todo]      Finalisasi repository dan branch pengujian
[todo]      Freeze commit hash objek penelitian
[todo]      Freeze baseline kebijakan akses
[todo]      Siapkan dataset dummy
[todo]      Siapkan akun PUBLIC-A, PUBLIC-B, CASHIER, OWNER
[todo]      Siapkan token/session uji
[todo]      Verifikasi aplikasi berjalan lokal
[todo]      Verifikasi database lokal dan seed data
[todo]      Verifikasi endpoint baseline dari Lampiran B
[todo]      Jalankan dry run 3-5 test case
[todo]      Revisi kecil instrumen jika dry run gagal karena format teknis
[todo]      Jalankan seluruh 20 test case
[todo]      Simpan evidence per test case
[todo]      Re-test semua temuan vulnerable
[todo]      Klasifikasikan hasil compliant/vulnerable/logic error/inconclusive
[todo]      Hitung ACR dan ACFR
[todo]      Tentukan severity temuan vulnerable
[todo]      Susun rekomendasi mitigasi
[todo]      Buat ringkasan hasil pengujian
[todo]      Siapkan lampiran evidence untuk laporan
```

---

## 4. Stack Teknologi dan Tools

### 4.1 Stack Aplikasi Testbed

Stack final harus dicatat saat pembekuan objek penelitian. Berdasarkan proposal, aplikasi Self Order System Management memiliki karakteristik berikut.

| Komponen | Stack/Status | Catatan |
|---|---|---|
| Frontend | React | Digunakan untuk akses normal dan validasi alur UI |
| Backend | REST API | Target utama pengujian authorization |
| Runtime backend | Sesuai repository final | Contoh: Node.js/Express jika memang sesuai implementasi repository |
| Database | Database lokal | Digunakan untuk dataset dummy dan verifikasi perubahan entitas server-side |
| Arsitektur | Frontend + backend API | Endpoint backend dapat dipanggil langsung untuk pengujian manual |
| Deployment pengujian | Lokal/laboratorium | Tidak menggunakan sistem publik/produksi |

Catatan penting:

- Jangan menulis stack final secara mengarang. Isi berdasarkan repository yang benar-benar digunakan saat freeze.
- Jika backend bukan Node.js/Express, ganti dengan runtime sebenarnya.
- Jika menggunakan Docker, catat nama service, image, port, dan file konfigurasi.

### 4.2 Tools Utama

| Tool | Fungsi | Status |
|---|---|---|
| Git | Mencatat branch dan commit hash objek penelitian | `[todo]` |
| Browser | Menguji alur normal melalui frontend | `[todo]` |
| Browser DevTools | Memeriksa request, response, cookie, storage, dan network log | `[todo]` |
| Postman | Mengirim request manual sesuai test case | `[todo]` |
| curl | Alternatif request manual dan bukti yang mudah direplikasi | `[todo]` |
| Burp Suite Community | Intercepting proxy untuk inspeksi dan modifikasi request | `[todo]` |
| Database client | Verifikasi data dan modifikasi entitas pada sisi server | `[todo]` |
| Log viewer/terminal | Memantau error backend dan log request | `[todo]` |
| Spreadsheet/Markdown | Mencatat hasil test case dan evidence | `[todo]` |
| Screenshot tool | Dokumentasi bukti visual jika diperlukan | `[todo]` |

### 4.3 Tools yang Tidak Menjadi Metode Utama

| Tool/Jenis | Status | Alasan |
|---|---|---|
| Automated vulnerability scanner | `[deferred]` | Broken access control membutuhkan konteks role, ownership, dan baseline |
| SQL injection tools | `[out-of-scope]` | Tidak sesuai batasan masalah |
| XSS scanner | `[out-of-scope]` | Tidak sesuai batasan masalah |
| Brute force tools | `[out-of-scope]` | Tidak sesuai batasan masalah |
| Exploit framework | `[out-of-scope]` | Penelitian bukan red teaming umum |

---

## 5. Struktur Folder Evidence

Gunakan struktur folder berikut agar data pengujian rapi dan mudah diperiksa.

```text
metopen-authorization-testing/
├── README.md
├── ROADMAP.md
├── 00-freeze/
│   ├── freeze-object.md
│   ├── baseline-policy.md
│   ├── endpoint-inventory.md
│   └── dataset-dummy.md
├── 01-accounts/
│   ├── test-accounts.md
│   └── token-redaction-rules.md
├── 02-test-cases/
│   ├── test-case-master.md
│   ├── TC-01.md
│   ├── TC-02.md
│   └── ...
├── 03-evidence/
│   ├── TC-01/
│   │   ├── request.txt
│   │   ├── response.txt
│   │   ├── screenshot.png
│   │   └── analysis.md
│   ├── TC-02/
│   └── ...
├── 04-retest/
│   ├── vulnerable-retest-log.md
│   └── session-token-retest-notes.md
├── 05-analysis/
│   ├── result-table.md
│   ├── metrics-acr-acfr.md
│   ├── severity-table.md
│   └── findings-summary.md
└── 06-report-assets/
    ├── tables-final.md
    ├── figures-final.md
    └── recommendation.md
```

Aturan penting:

- Jangan menyimpan token asli secara utuh.
- Token, cookie, password, dan secret harus disamarkan.
- Gunakan format `[REDACTED]` untuk bagian sensitif.

---

## 6. Pembekuan Objek Penelitian

Pembekuan objek dilakukan sebelum pengujian utama. Tujuannya agar hasil penelitian berlaku pada versi aplikasi yang jelas dan dapat direplikasi.

### 6.1 Checklist Freeze Object

| Komponen | Isian | Status |
|---|---|---|
| Nama sistem | Self Order System Management | `[ready]` |
| Lingkungan | Lokal/laboratorium | `[ready]` |
| Repository URL lokal | `[isi]` | `[todo]` |
| Branch | `[isi]` | `[todo]` |
| Commit hash | `[isi]` | `[todo]` |
| Tanggal freeze | `[isi]` | `[todo]` |
| Versi frontend | `[isi]` | `[todo]` |
| Versi backend/runtime | `[isi]` | `[todo]` |
| Versi database/migration | `[isi]` | `[todo]` |
| Dataset dummy | `[isi]` | `[todo]` |
| Port frontend | `[isi]` | `[todo]` |
| Port backend | `[isi]` | `[todo]` |
| Nama database lokal | `[isi]` | `[todo]` |

### 6.2 Aturan Freeze

1. Setelah commit hash dibekukan, jangan mengubah endpoint selama pengujian utama.
2. Jika ada bug teknis environment, perbaikan harus dicatat sebagai versi freeze baru.
3. Jangan mengubah baseline setelah melihat hasil vulnerable.
4. Jika baseline perlu direvisi, pisahkan hasil sebelum dan sesudah revisi.
5. Hasil ACR/ACFR hanya berlaku untuk versi commit yang diuji.

---

## 7. Baseline Kebijakan Akses

Baseline adalah sumber kebenaran untuk expected access. Tanpa baseline, hasil pengujian tidak dapat diklasifikasikan secara objektif.

### 7.1 Sumber Baseline

Urutan sumber baseline:

1. Dokumentasi kebutuhan atau spesifikasi sistem.
2. Definisi route dan middleware backend.
3. Dokumentasi proyek.
4. Validasi dengan pengembang/pemilik sistem.
5. Perilaku frontend sebagai indikator pendukung, bukan sumber utama.

### 7.2 Matriks Hak Akses Ringkas

| Endpoint/Fungsi | PUBLIC | CASHIER | OWNER | Catatan |
|---|---|---|---|---|
| `POST /api/public/qr/validate` | Allow | N/A | N/A | Validasi QR publik |
| `GET /api/public/menu` | Allow | N/A | N/A | Menu publik |
| `POST /api/public/orders` | Allow dengan token valid | N/A | N/A | Public order |
| `GET /api/public/orders/{id}` | Allow hanya untuk sesi pemilik | N/A | N/A | Object-level tracking |
| `GET /api/auth/me` | Deny | Allow | Allow | Profil internal |
| `GET /api/internal/orders` | Deny | Allow | Allow | Order internal |
| `PATCH /api/internal/orders/{id}/accept` | Deny | Allow | Allow | Accept order |
| `POST /api/internal/orders/{id}/payments` | Deny | Allow | Allow | Pembayaran |
| `GET /api/internal/transactions/{id}` | Deny | Allow | Allow | Transaksi internal |
| `/api/internal/users*` | Deny | Deny | Allow | Khusus OWNER |
| `/api/internal/reports*` | Deny | Deny | Allow | Khusus OWNER |
| `/api/internal/tables*` | Deny | Deny | Allow | Khusus OWNER |
| `/api/internal/qr-tokens*` | Deny | Deny | Allow | Khusus OWNER |
| `GET /api/internal/audit-logs` | Deny | Deny | Allow | Audit log khusus OWNER |

Keterangan:

- **Allow** berarti akses diizinkan sesuai baseline.
- **Deny** berarti akses harus ditolak.
- **N/A** berarti bukan jalur penggunaan yang diuji dalam penelitian karena tidak relevan dengan skenario otorisasi.

---

## 8. Identitas dan Akun Uji

### 8.1 Role yang Diuji

| Role | Fungsi | Tujuan Pengujian |
|---|---|---|
| PUBLIC | Pengguna publik tanpa login internal | Menguji endpoint publik dan larangan akses endpoint internal |
| PUBLIC-A | Identitas publik dengan session/token order A | Menguji kepemilikan objek publik |
| PUBLIC-B | Identitas publik dengan session/token order B | Pembanding object-level authorization |
| CASHIER | Pengguna internal operasional | Menguji akses kasir dan larangan akses fungsi OWNER |
| OWNER | Pengguna internal tertinggi | Menguji akses fungsi khusus OWNER |

### 8.2 Akun Dummy Minimal

| Identitas | Role | Data Dummy yang Dibutuhkan | Status |
|---|---|---|---|
| publicA | PUBLIC-A | QR/session token valid untuk orderA | `[todo]` |
| publicB | PUBLIC-B | QR/session token valid untuk orderB | `[todo]` |
| cashier01 | CASHIER | Akun internal kasir aktif | `[todo]` |
| owner01 | OWNER | Akun owner aktif | `[todo]` |

### 8.3 Aturan Token dan Session

1. Token asli tidak ditulis utuh di laporan.
2. Gunakan format redaksi: `Bearer eyJ... [REDACTED]`.
3. Untuk re-test vulnerable, gunakan token atau sesi baru.
4. Jangan mencampur token PUBLIC-A dan PUBLIC-B kecuali memang menjadi bagian test case object-level.
5. Catat waktu pembuatan token/session agar dapat melacak kemungkinan expired token.

---

## 9. Dataset Dummy

Dataset dummy harus cukup untuk menjalankan semua test case.

| Data | Jumlah Minimal | Tujuan |
|---|---:|---|
| Akun OWNER | 1 | Menguji endpoint khusus OWNER |
| Akun CASHIER | 1 | Menguji endpoint operasional internal |
| Session/token PUBLIC-A | 1 | Menguji akses order milik PUBLIC-A |
| Session/token PUBLIC-B | 1 | Menguji akses order milik PUBLIC-B |
| Order A | 1 | Objek milik PUBLIC-A |
| Order B | 1 | Objek milik PUBLIC-B |
| Transaksi dummy | 1 | Menguji endpoint transaksi internal |
| QR token valid | 1 | Menguji pembuatan order valid |
| QR token invalid/expired | 1 | Menguji penolakan order invalid |
| Data user internal | 1-2 | Menguji endpoint user management |
| Data laporan | 1 | Menguji endpoint reports |
| Data audit log | 1 | Menguji endpoint audit logs |

Aturan dataset:

- Dataset harus dicatat sebelum pengujian.
- Jangan mengubah dataset inti saat pengujian utama kecuali test case memang membutuhkan perubahan state.
- Jika data berubah akibat test case, catat perubahan tersebut di evidence.

---

## 10. Definisi Hasil Pengujian

### 10.1 Actual Access

| Status Actual Access | Definisi |
|---|---|
| Allow | Sistem memberikan data, menjalankan fungsi, atau menghasilkan efek di sisi server yang seharusnya terproteksi. |
| Deny | Sistem mencegah akses melalui 401, 403, 404 aman, redirect aman, atau respons lain yang tidak membuka data/fungsi/efek server-side yang tidak sah. |
| Inconclusive | Respons teknis seperti 500, kesalahan konfigurasi, kegagalan koneksi, atau hasil re-test tidak konsisten sehingga tidak cukup untuk menyimpulkan keputusan otorisasi. |

### 10.2 Klasifikasi Hasil

| Expected Access | Actual Access | Klasifikasi | Makna |
|---|---|---|---|
| Allow | Allow | Compliant | Sistem mengizinkan akses yang memang seharusnya diizinkan. |
| Deny | Deny | Compliant | Sistem menolak akses yang memang seharusnya ditolak. |
| Deny | Allow | Vulnerable | Terjadi indikasi Broken Access Control. |
| Allow | Deny | Logic Error | Sistem menolak akses yang seharusnya diizinkan. |
| Allow/Deny | Inconclusive | Inconclusive | Hasil tidak cukup untuk menyimpulkan keputusan otorisasi. |

### 10.3 Severity

Severity hanya diberikan pada temuan vulnerable. Kriteria severity difokuskan pada dampak teknis di sisi server.

| Severity | Kriteria Dampak Teknis |
|---|---|
| Rendah | Disclosure terbatas pada data non-sensitif, tidak menyebabkan modifikasi entitas, dan tidak membuka fungsi internal kritis. |
| Sedang | Disclosure data internal non-publik atau akses fungsi internal terbatas, tetapi tidak menyebabkan perubahan kritis pada basis data atau fungsi khusus OWNER. |
| Tinggi | Akses tidak sah memungkinkan modifikasi, penghapusan, atau pembuatan data; membuka fungsi khusus OWNER; atau mengakses objek milik pihak lain yang berdampak signifikan terhadap kerahasiaan atau integritas. |

---

## 11. Metrik Operasional

ROADMAP ini memakai dua metrik operasional sesuai proposal. Metrik ini tidak diklaim sebagai standar industri baku, tetapi digunakan sebagai alat ukur penelitian agar hasil pengujian dapat dirangkum secara konsisten.

### 11.1 Authorization Compliance Rate (ACR)

ACR dihitung dengan cara membagi jumlah test case berstatus compliant dengan total seluruh test case valid, kemudian dikalikan 100%.

Test case valid adalah test case yang menghasilkan klasifikasi:

- compliant,
- vulnerable,
- logic error.

Test case berstatus inconclusive tidak dimasukkan ke dalam perhitungan ACR.

### 11.2 Access Control Failure Rate (ACFR)

ACFR dihitung dengan cara membagi jumlah test case berstatus vulnerable dengan total test case valid yang memiliki expected access berupa deny, kemudian dikalikan 100%.

Test case berstatus inconclusive tidak dimasukkan ke dalam penyebut ACFR.

### 11.3 Format Pelaporan Metrik

Gunakan format seperti berikut.

```text
Total test case              : 20
Compliant                    : [isi]
Vulnerable                   : [isi]
Logic error                  : [isi]
Inconclusive                 : [isi]
Test case valid              : [isi]
Expected deny valid          : [isi]
ACR                          : [x dari y] = [persentase]
ACFR                         : [x dari y] = [persentase]
```

Catatan:

- Selalu tampilkan pecahan mentah, bukan hanya persentase.
- Contoh: `2 dari 14 expected deny valid menghasilkan actual allow`.

---

## 12. Rancangan 20 Test Case

### 12.1 Master Test Case

| ID | Kategori | Actor | Endpoint/Skenario | Expected |
|---|---|---|---|---|
| TC-01 | Function-level (BFLA) | PUBLIC | `GET /api/auth/me` | Deny |
| TC-02 | Function-level (BFLA) | PUBLIC | `GET /api/internal/orders` | Deny |
| TC-03 | Function-level (BFLA) | PUBLIC | `POST /api/internal/orders/{id}/payments` | Deny |
| TC-04 | Function-level (BFLA) | PUBLIC | `GET /api/internal/reports` | Deny |
| TC-05 | Function-level (BFLA) | CASHIER | `GET /api/internal/users` | Deny |
| TC-06 | Function-level (BFLA) | CASHIER | `POST /api/internal/users` | Deny |
| TC-07 | Function-level (BFLA) | CASHIER | `GET /api/internal/reports` | Deny |
| TC-08 | Function-level (BFLA) | CASHIER | `GET /api/internal/qr-tokens` | Deny |
| TC-09 | Function-level (BFLA) | OWNER | `GET /api/internal/users` | Allow |
| TC-10 | Function-level (BFLA) | OWNER | `GET /api/internal/reports` | Allow |
| TC-11 | Function-level (BFLA) | CASHIER | `GET /api/internal/orders` | Allow |
| TC-12 | Function-level (BFLA) | CASHIER | `PATCH /api/internal/orders/{id}/accept` | Allow |
| TC-13 | Object-level (BOLA) | PUBLIC-A | `GET /api/public/orders/{orderA}` | Allow |
| TC-14 | Object-level (BOLA) | PUBLIC-A | `GET /api/public/orders/{orderB}` | Deny |
| TC-15 | Object-level (BOLA) | PUBLIC tanpa sesi valid | `GET /api/public/orders/{orderA}` | Deny |
| TC-16 | Object-level (BOLA) | PUBLIC | `POST /api/public/orders` dengan QR invalid | Deny |
| TC-17 | Object-level (BOLA) | PUBLIC | `POST /api/public/orders` dengan QR valid | Allow |
| TC-18 | Function-level (BFLA) | PUBLIC | `GET /api/internal/transactions/{id}` | Deny |
| TC-19 | Function-level (BFLA) | PUBLIC | `PATCH /api/internal/orders/{id}/accept` | Deny |
| TC-20 | Function-level (BFLA) | OWNER | `GET /api/internal/audit-logs` | Allow |

Catatan: Pada endpoint internal yang memiliki parameter `{id}`, seperti `/api/internal/orders/{id}/payments`, `/api/internal/transactions/{id}`, dan `/api/internal/orders/{id}/accept`, pengujian dengan aktor PUBLIC difokuskan pada aspek function-level authorization, yaitu apakah aktor yang tidak memiliki hak internal dapat memanggil fungsi internal tersebut. Keberadaan parameter `{id}` tetap dicatat sebagai konteks objek, tetapi kategori utama test case tetap ditetapkan sebagai Function-level (BFLA) karena akses seharusnya sudah ditolak pada tingkat fungsi sebelum evaluasi kepemilikan objek dilakukan.

### 12.2 Expected Hasil Umum

Hasil ideal jika implementasi otorisasi benar:

- Semua test case dengan expected `Allow` menghasilkan actual access `Allow`.
- Semua test case dengan expected `Deny` menghasilkan actual access `Deny`.
- Tidak ada test case vulnerable.
- Tidak ada test case inconclusive.
- ACR = 100%.
- ACFR = 0%.

Namun, penelitian tetap valid jika ditemukan:

- vulnerable,
- logic error,
- inconclusive.

Semua hasil harus didokumentasikan apa adanya.

---

## 13. Detail Eksekusi Test Case

Format detail berikut dapat dipakai untuk membuat file evidence masing-masing test case.

### TC-01 - PUBLIC mengakses profil internal

| Komponen | Isi |
|---|---|
| ID | TC-01 |
| Kategori | Function-level (BFLA) |
| Actor | PUBLIC |
| Endpoint | `GET /api/auth/me` |
| Expected | Deny |
| Tujuan | Memastikan endpoint profil internal tidak dapat diakses tanpa autentikasi internal. |
| Pra-kondisi | Tidak menggunakan token login. |
| Evidence | Request, response, status HTTP, potongan body. |
| Hasil yang diharapkan | 401/403/redirect aman atau respons lain yang tidak membuka data profil. |

### TC-02 - PUBLIC mengakses daftar order internal

| Komponen | Isi |
|---|---|
| ID | TC-02 |
| Kategori | Function-level (BFLA) |
| Actor | PUBLIC |
| Endpoint | `GET /api/internal/orders` |
| Expected | Deny |
| Tujuan | Memastikan endpoint order internal tidak dapat diakses oleh pengguna publik. |
| Pra-kondisi | Tidak menggunakan token internal. |
| Hasil yang diharapkan | Sistem menolak akses dan tidak mengembalikan data order internal. |

### TC-03 - PUBLIC memanggil pembayaran internal

| Komponen | Isi |
|---|---|
| ID | TC-03 |
| Kategori | Function-level (BFLA) |
| Actor | PUBLIC |
| Endpoint | `POST /api/internal/orders/{id}/payments` |
| Expected | Deny |
| Tujuan | Memastikan fungsi pembayaran internal tidak dapat dipanggil oleh aktor publik. |
| Pra-kondisi | Gunakan `{id}` order dummy; tidak menggunakan token internal. |
| Hasil yang diharapkan | Sistem menolak akses; tidak ada transaksi/pembayaran baru di database. |

### TC-04 - PUBLIC mengakses laporan internal

| Komponen | Isi |
|---|---|
| ID | TC-04 |
| Kategori | Function-level (BFLA) |
| Actor | PUBLIC |
| Endpoint | `GET /api/internal/reports` |
| Expected | Deny |
| Tujuan | Memastikan laporan internal tidak dapat diakses oleh publik. |
| Hasil yang diharapkan | Sistem menolak akses dan tidak membuka data laporan. |

### TC-05 - CASHIER mengakses manajemen user

| Komponen | Isi |
|---|---|
| ID | TC-05 |
| Kategori | Function-level (BFLA) |
| Actor | CASHIER |
| Endpoint | `GET /api/internal/users` |
| Expected | Deny |
| Tujuan | Memastikan CASHIER tidak dapat membaca daftar user internal jika fitur tersebut khusus OWNER. |
| Pra-kondisi | Login sebagai CASHIER. |
| Hasil yang diharapkan | 403/404 aman atau respons deny tanpa data user. |

### TC-06 - CASHIER membuat user baru

| Komponen | Isi |
|---|---|
| ID | TC-06 |
| Kategori | Function-level (BFLA) |
| Actor | CASHIER |
| Endpoint | `POST /api/internal/users` |
| Expected | Deny |
| Tujuan | Memastikan CASHIER tidak dapat membuat user internal baru. |
| Pra-kondisi | Login sebagai CASHIER; gunakan payload dummy. |
| Hasil yang diharapkan | Sistem menolak request; tidak ada user baru di database. |

### TC-07 - CASHIER mengakses laporan OWNER

| Komponen | Isi |
|---|---|
| ID | TC-07 |
| Kategori | Function-level (BFLA) |
| Actor | CASHIER |
| Endpoint | `GET /api/internal/reports` |
| Expected | Deny |
| Tujuan | Memastikan laporan khusus OWNER tidak dapat dibaca CASHIER. |
| Hasil yang diharapkan | Sistem menolak akses dan tidak membuka data laporan. |

### TC-08 - CASHIER mengakses QR token management

| Komponen | Isi |
|---|---|
| ID | TC-08 |
| Kategori | Function-level (BFLA) |
| Actor | CASHIER |
| Endpoint | `GET /api/internal/qr-tokens` |
| Expected | Deny |
| Tujuan | Memastikan QR token management hanya dapat diakses OWNER. |
| Hasil yang diharapkan | Sistem menolak akses. |

### TC-09 - OWNER mengakses manajemen user

| Komponen | Isi |
|---|---|
| ID | TC-09 |
| Kategori | Function-level (BFLA) |
| Actor | OWNER |
| Endpoint | `GET /api/internal/users` |
| Expected | Allow |
| Tujuan | Memastikan OWNER dapat mengakses fitur manajemen user. |
| Hasil yang diharapkan | Sistem mengembalikan data atau halaman/response yang sesuai. |

### TC-10 - OWNER mengakses laporan

| Komponen | Isi |
|---|---|
| ID | TC-10 |
| Kategori | Function-level (BFLA) |
| Actor | OWNER |
| Endpoint | `GET /api/internal/reports` |
| Expected | Allow |
| Tujuan | Memastikan OWNER dapat mengakses laporan. |
| Hasil yang diharapkan | Sistem mengembalikan data laporan sesuai baseline. |

### TC-11 - CASHIER mengakses order internal

| Komponen | Isi |
|---|---|
| ID | TC-11 |
| Kategori | Function-level (BFLA) |
| Actor | CASHIER |
| Endpoint | `GET /api/internal/orders` |
| Expected | Allow |
| Tujuan | Memastikan CASHIER dapat menjalankan fungsi operasional order internal. |
| Hasil yang diharapkan | Sistem mengembalikan daftar order internal sesuai hak kasir. |

### TC-12 - CASHIER menerima order

| Komponen | Isi |
|---|---|
| ID | TC-12 |
| Kategori | Function-level (BFLA) |
| Actor | CASHIER |
| Endpoint | `PATCH /api/internal/orders/{id}/accept` |
| Expected | Allow |
| Tujuan | Memastikan CASHIER dapat menerima order sesuai fungsi operasionalnya. |
| Pra-kondisi | Order dummy berada pada status yang dapat diterima. |
| Hasil yang diharapkan | Status order berubah sesuai aturan aplikasi. |

### TC-13 - PUBLIC-A melihat order miliknya

| Komponen | Isi |
|---|---|
| ID | TC-13 |
| Kategori | Object-level (BOLA) |
| Actor | PUBLIC-A |
| Endpoint | `GET /api/public/orders/{orderA}` |
| Expected | Allow |
| Tujuan | Memastikan PUBLIC-A dapat melihat order miliknya sendiri. |
| Pra-kondisi | PUBLIC-A memiliki session/token valid untuk orderA. |
| Hasil yang diharapkan | Sistem menampilkan data orderA. |

### TC-14 - PUBLIC-A mencoba melihat order milik PUBLIC-B

| Komponen | Isi |
|---|---|
| ID | TC-14 |
| Kategori | Object-level (BOLA) |
| Actor | PUBLIC-A |
| Endpoint | `GET /api/public/orders/{orderB}` |
| Expected | Deny |
| Tujuan | Memastikan PUBLIC-A tidak dapat mengakses order milik PUBLIC-B. |
| Pra-kondisi | PUBLIC-A memakai session/token miliknya sendiri, tetapi parameter objek diganti menjadi orderB. |
| Hasil yang diharapkan | Sistem menolak akses dan tidak membuka data orderB. |

### TC-15 - PUBLIC tanpa sesi valid melihat order

| Komponen | Isi |
|---|---|
| ID | TC-15 |
| Kategori | Object-level (BOLA) |
| Actor | PUBLIC tanpa sesi valid |
| Endpoint | `GET /api/public/orders/{orderA}` |
| Expected | Deny |
| Tujuan | Memastikan order publik tidak dapat dilihat tanpa konteks sesi/token valid. |
| Hasil yang diharapkan | Sistem menolak akses. |

### TC-16 - PUBLIC membuat order dengan QR invalid

| Komponen | Isi |
|---|---|
| ID | TC-16 |
| Kategori | Object-level (BOLA) |
| Actor | PUBLIC |
| Endpoint | `POST /api/public/orders` dengan QR invalid |
| Expected | Deny |
| Tujuan | Memastikan order tidak dapat dibuat menggunakan QR invalid/expired. |
| Hasil yang diharapkan | Sistem menolak request dan tidak membuat order baru. |

### TC-17 - PUBLIC membuat order dengan QR valid

| Komponen | Isi |
|---|---|
| ID | TC-17 |
| Kategori | Object-level (BOLA) |
| Actor | PUBLIC |
| Endpoint | `POST /api/public/orders` dengan QR valid |
| Expected | Allow |
| Tujuan | Memastikan alur order publik yang sah dapat berjalan. |
| Hasil yang diharapkan | Sistem membuat order baru sesuai aturan aplikasi. |

### TC-18 - PUBLIC mengakses transaksi internal

| Komponen | Isi |
|---|---|
| ID | TC-18 |
| Kategori | Function-level (BFLA) |
| Actor | PUBLIC |
| Endpoint | `GET /api/internal/transactions/{id}` |
| Expected | Deny |
| Tujuan | Memastikan transaksi internal tidak dapat diakses publik. |
| Hasil yang diharapkan | Sistem menolak akses dan tidak membuka data transaksi. |

### TC-19 - PUBLIC menerima order melalui endpoint internal

| Komponen | Isi |
|---|---|
| ID | TC-19 |
| Kategori | Function-level (BFLA) |
| Actor | PUBLIC |
| Endpoint | `PATCH /api/internal/orders/{id}/accept` |
| Expected | Deny |
| Tujuan | Memastikan publik tidak dapat memanggil fungsi internal accept order. |
| Hasil yang diharapkan | Sistem menolak akses dan status order tidak berubah. |

### TC-20 - OWNER mengakses audit log

| Komponen | Isi |
|---|---|
| ID | TC-20 |
| Kategori | Function-level (BFLA) |
| Actor | OWNER |
| Endpoint | `GET /api/internal/audit-logs` |
| Expected | Allow |
| Tujuan | Memastikan OWNER dapat mengakses audit log sesuai baseline. |
| Hasil yang diharapkan | Sistem mengembalikan audit log atau response sesuai implementasi. |

---

## 14. Template Evidence Pengujian

Gunakan template berikut untuk setiap test case.

````markdown
# Evidence Test Case

## Metadata

| Komponen | Isian |
|---|---|
| ID Test Case | TC-xx |
| Tanggal/Waktu |  |
| Versi Objek/Commit |  |
| Role/Identitas |  |
| Endpoint |  |
| Method |  |
| Object Context |  |
| Expected Access | Allow / Deny |

## Request

```http
METHOD /path HTTP/1.1
Host: localhost:[port]
Authorization: Bearer [REDACTED]
Content-Type: application/json

{ }
```

## Response

```http
HTTP/1.1 [status]
Content-Type: application/json

{ }
```

## Verifikasi Server-Side

| Komponen | Hasil |
|---|---|
| Ada data terbuka? | Ya / Tidak |
| Ada fungsi dijalankan? | Ya / Tidak |
| Ada modifikasi database? | Ya / Tidak / Tidak relevan |
| Catatan database/log |  |

## Analisis

| Komponen | Hasil |
|---|---|
| Actual Access | Allow / Deny / Inconclusive |
| Klasifikasi | Compliant / Vulnerable / Logic Error / Inconclusive |
| Severity | Rendah / Sedang / Tinggi / Tidak Berlaku |
| Perlu Re-test? | Ya / Tidak |
| Hasil Re-test | Konsisten / Tidak Konsisten / Tidak Berlaku |
| Catatan |  |
````

---

## 15. Alur Eksekusi Pengujian

### Fase 0 - Persiapan Administratif

Checklist:

- `[todo]` Pastikan proposal final sudah disetujui sebagai dasar penelitian.
- `[todo]` Pastikan scope tidak berubah dari authorization testing.
- `[todo]` Pastikan objek penelitian adalah Self Order System Management.
- `[todo]` Pastikan tidak ada sistem publik atau data nyata yang digunakan.

Output fase:

- Scope final.
- Objek final.
- Persetujuan internal dari diri sendiri/pembimbing untuk mulai uji lab.

### Fase 1 - Setup Environment Lokal

Checklist:

- `[todo]` Clone/open repository final.
- `[todo]` Install dependency frontend.
- `[todo]` Install dependency backend.
- `[todo]` Siapkan database lokal.
- `[todo]` Jalankan migration/seed.
- `[todo]` Jalankan backend.
- `[todo]` Jalankan frontend.
- `[todo]` Pastikan endpoint health check atau login dapat diakses.
- `[todo]` Catat port frontend/backend.

Output fase:

- Aplikasi berjalan lokal.
- Database lokal siap.
- Log backend dapat dipantau.

### Fase 2 - Freeze Objek dan Baseline

Checklist:

- `[todo]` Catat branch.
- `[todo]` Catat commit hash.
- `[todo]` Catat tanggal freeze.
- `[todo]` Catat versi runtime.
- `[todo]` Catat versi database/migration.
- `[todo]` Catat dataset dummy.
- `[todo]` Simpan `freeze-object.md`.
- `[todo]` Simpan `baseline-policy.md`.

Output fase:

- Dokumen freeze lengkap.
- Baseline tidak berubah selama pengujian utama.

### Fase 3 - Menyiapkan Akun dan Data Dummy

Checklist:

- `[todo]` Buat akun OWNER.
- `[todo]` Buat akun CASHIER.
- `[todo]` Buat session/token PUBLIC-A.
- `[todo]` Buat session/token PUBLIC-B.
- `[todo]` Buat orderA untuk PUBLIC-A.
- `[todo]` Buat orderB untuk PUBLIC-B.
- `[todo]` Buat transaksi dummy.
- `[todo]` Buat QR valid.
- `[todo]` Buat QR invalid/expired.
- `[todo]` Buat data user dummy.
- `[todo]` Buat data laporan dummy.
- `[todo]` Buat audit log dummy jika endpoint audit log tersedia.

Output fase:

- Data dummy lengkap untuk semua test case.

### Fase 4 - Dry Run

Checklist:

- `[todo]` Jalankan TC-01.
- `[todo]` Jalankan TC-05.
- `[todo]` Jalankan TC-09.
- `[todo]` Jalankan TC-13.
- `[todo]` Jalankan TC-14.
- `[todo]` Pastikan format evidence bisa digunakan.
- `[todo]` Pastikan token/session tidak tertukar.
- `[todo]` Pastikan database client dapat memverifikasi perubahan data.

Output fase:

- Format evidence final.
- Instrumen siap dipakai untuk 20 test case.

Catatan:

- Dry run bukan hasil utama penelitian.
- Jika dry run menemukan masalah pada format endpoint, perbaiki instrumen sebelum pengujian utama.

### Fase 5 - Pengujian Utama Function-Level Authorization

Test case utama:

- TC-01 sampai TC-12.
- TC-18 sampai TC-20.

Checklist:

- `[todo]` Jalankan semua function-level test case.
- `[todo]` Simpan request dan response.
- `[todo]` Catat status HTTP.
- `[todo]` Verifikasi apakah data/fungsi terbuka.
- `[todo]` Verifikasi perubahan database untuk request yang dapat mengubah state.
- `[todo]` Tandai compliant/vulnerable/logic error/inconclusive.

Output fase:

- Evidence function-level authorization.
- Tabel hasil BFLA.

### Fase 6 - Pengujian Utama Object-Level Authorization

Test case utama:

- TC-13 sampai TC-17.

Checklist:

- `[todo]` Jalankan semua object-level test case.
- `[todo]` Gunakan session/token PUBLIC-A dan PUBLIC-B secara terpisah.
- `[todo]` Pastikan object context dicatat.
- `[todo]` Jangan mencampur orderA dan orderB tanpa catatan.
- `[todo]` Simpan request dan response.
- `[todo]` Tandai compliant/vulnerable/logic error/inconclusive.

Output fase:

- Evidence object-level authorization.
- Tabel hasil BOLA.

### Fase 7 - Re-test Temuan Vulnerable

Checklist:

- `[todo]` Kumpulkan semua test case berstatus vulnerable.
- `[todo]` Buat token/session baru jika relevan.
- `[todo]` Jalankan ulang setiap test case vulnerable minimal satu kali.
- `[todo]` Bandingkan hasil utama dan hasil re-test.
- `[todo]` Jika konsisten, tetapkan vulnerable final.
- `[todo]` Jika tidak konsisten, ubah menjadi inconclusive atau analisis sebagai state/session non-deterministic.

Output fase:

- Retest log.
- Temuan final yang lebih valid.

### Fase 8 - Analisis Hasil

Checklist:

- `[todo]` Gabungkan seluruh hasil test case.
- `[todo]` Hitung jumlah compliant.
- `[todo]` Hitung jumlah vulnerable.
- `[todo]` Hitung jumlah logic error.
- `[todo]` Hitung jumlah inconclusive.
- `[todo]` Hitung ACR.
- `[todo]` Hitung ACFR.
- `[todo]` Tentukan severity untuk vulnerable.
- `[todo]` Pisahkan hasil BFLA dan BOLA jika diperlukan.

Output fase:

- Tabel hasil utama.
- Metrik ACR dan ACFR.
- Tabel severity.

### Fase 9 - Rekomendasi Mitigasi

Checklist:

- `[todo]` Untuk setiap vulnerable, tentukan akar masalah sementara.
- `[todo]` Susun rekomendasi mitigasi server-side.
- `[todo]` Hindari rekomendasi yang hanya menyembunyikan menu frontend.
- `[todo]` Kaitkan rekomendasi dengan endpoint dan role terkait.
- `[todo]` Tandai prioritas perbaikan berdasarkan severity.

Contoh rekomendasi:

| Temuan | Rekomendasi |
|---|---|
| PUBLIC dapat mengakses endpoint internal | Tambahkan middleware autentikasi dan role check pada route internal. |
| CASHIER dapat mengakses fungsi OWNER | Perketat policy role-permission pada endpoint owner-only. |
| PUBLIC-A dapat melihat orderB | Validasi ownership/session token sebelum mengembalikan data order. |
| Request denied padahal expected allow | Periksa logic middleware, route guard, atau mapping permission. |

Output fase:

- Daftar rekomendasi mitigasi.

### Fase 10 - Penyusunan Hasil untuk Laporan

Checklist:

- `[todo]` Susun tabel hasil akhir.
- `[todo]` Pilih evidence yang layak masuk lampiran.
- `[todo]` Redaksi token/cookie/credential.
- `[todo]` Tulis ringkasan ACR dan ACFR.
- `[todo]` Tulis ringkasan temuan BFLA.
- `[todo]` Tulis ringkasan temuan BOLA.
- `[todo]` Tulis keterbatasan hasil.
- `[todo]` Siapkan bahan presentasi jika diminta dosen.

Output fase:

- Hasil siap dimasukkan ke laporan penelitian.

---

## 16. Expected Output Akhir

Setelah seluruh roadmap dijalankan, hasil yang diharapkan bukan sekadar “aplikasi aman” atau “aplikasi punya celah”. Hasil yang diharapkan adalah data evaluatif yang lengkap.

### 16.1 Output Teknis

| Output | Format |
|---|---|
| Freeze object | `freeze-object.md` |
| Baseline policy | `baseline-policy.md` |
| Dataset dummy | `dataset-dummy.md` |
| Daftar akun uji | `test-accounts.md` |
| Evidence per test case | Folder `03-evidence/TC-xx/` |
| Retest log | `vulnerable-retest-log.md` |
| Tabel hasil | `result-table.md` |
| Metrik ACR/ACFR | `metrics-acr-acfr.md` |
| Rekomendasi mitigasi | `recommendation.md` |

### 16.2 Output Akademik

| Output | Fungsi |
|---|---|
| Tabel expected vs actual | Bukti utama analisis |
| Nilai ACR | Mengukur tingkat kepatuhan authorization |
| Nilai ACFR | Mengukur tingkat kegagalan access control pada expected deny |
| Daftar vulnerable | Menunjukkan indikasi Broken Access Control |
| Daftar logic error | Menunjukkan ketidaksesuaian fungsi non-security |
| Daftar inconclusive | Menunjukkan hasil yang tidak cukup valid untuk disimpulkan |
| Severity temuan | Menilai dampak teknis |
| Rekomendasi mitigasi | Menjawab tujuan penelitian |

### 16.3 Hasil Ideal

Hasil ideal jika aplikasi menerapkan otorisasi dengan benar:

```text
Total test case valid: 20
Compliant            : 20
Vulnerable           : 0
Logic error          : 0
Inconclusive         : 0
ACR                  : 20 dari 20 = 100%
ACFR                 : 0 dari total expected deny valid = 0%
```

### 16.4 Hasil Jika Ditemukan Vulnerable

Jika ditemukan vulnerable, hasil tetap valid asalkan:

- evidence lengkap,
- re-test konsisten,
- severity ditentukan,
- rekomendasi mitigasi diberikan,
- tidak ada eksploitasi di luar scope.

Contoh format ringkas:

```text
TC-14 menghasilkan vulnerable karena PUBLIC-A dapat mengakses orderB.
Expected access: Deny.
Actual access  : Allow.
Severity       : Tinggi.
Re-test        : Konsisten.
Mitigasi       : Validasi session token dan ownership order sebelum response dikirim.
```

---

## 17. Checklist Validitas Pengujian

### 17.1 Validitas Internal

- `[todo]` Baseline dibekukan sebelum pengujian.
- `[todo]` Commit hash dicatat.
- `[todo]` Dataset dummy dicatat.
- `[todo]` Token/session tidak tertukar.
- `[todo]` Test case vulnerable di-retest.
- `[todo]` Hasil re-test tidak konsisten diberi status inconclusive.
- `[todo]` Evidence tersimpan rapi.

### 17.2 Validitas Konstruksi

- `[todo]` Function-level test case benar-benar menguji akses fungsi/endpoint.
- `[todo]` Object-level test case benar-benar menguji kepemilikan objek/session/token.
- `[todo]` BFLA dan BOLA tidak dicampur tanpa catatan.
- `[todo]` ACR dan ACFR dijelaskan sebagai metrik operasional penelitian.

### 17.3 Validitas Eksternal

- `[todo]` Hasil tidak digeneralisasi ke semua aplikasi web.
- `[todo]` Hasil hanya berlaku untuk versi SOS yang diuji.
- `[todo]` Keterbatasan studi kasus tunggal dicatat.

### 17.4 Reliabilitas Pengujian Manual

- `[todo]` Langkah test case ditulis jelas.
- `[todo]` Request dapat diulang.
- `[todo]` Response disimpan.
- `[todo]` Token disamarkan tetapi konteks tetap jelas.
- `[todo]` Temuan vulnerable diuji ulang.

---

## 18. Checklist Etika

- `[todo]` Pengujian hanya di lokal/laboratorium.
- `[todo]` Tidak ada sistem publik yang diuji.
- `[todo]` Tidak ada data pengguna nyata.
- `[todo]` Tidak ada credential asli di laporan.
- `[todo]` Token/cookie disamarkan.
- `[todo]` Evidence tidak dipublikasikan secara sembarangan.
- `[todo]` Detail eksploitasi tidak disebar di luar konteks akademik.
- `[todo]` Hasil digunakan untuk evaluasi dan rekomendasi perbaikan.

---

## 19. Risk Register Pengujian

| Risiko | Dampak | Mitigasi |
|---|---|---|
| Endpoint berubah saat pengujian | Hasil tidak valid | Freeze commit dan baseline |
| Token expired | False deny/inconclusive | Catat waktu token dan buat token baru saat re-test |
| Data dummy berubah | Hasil tidak konsisten | Catat dataset awal dan perubahan data |
| Salah role saat request | Salah klasifikasi | Gunakan file akun uji dan header check |
| Server error 500 | Hasil ambigu | Klasifikasikan inconclusive jika tidak bisa menyimpulkan authorization |
| Burp/Postman salah konfigurasi | Request tidak valid | Lakukan dry run dan simpan request mentah |
| Evidence tidak lengkap | Sulit dipertanggungjawabkan | Gunakan template evidence wajib |
| Pengujian melebar ke XSS/SQLi | Scope rusak | Patuhi batasan authorization testing |

---

## 20. Roadmap 12 Minggu

| Minggu | Fokus | Checklist | Luaran |
|---|---|---|---|
| 1 | Finalisasi scope | Kunci objek, scope, branch, dan tools | Scope final |
| 2 | Freeze objek | Catat commit, runtime, database, dataset | Freeze document |
| 3 | Inventarisasi endpoint | Cocokkan route backend dengan baseline | Endpoint inventory |
| 4 | Siapkan data uji | Buat akun, order, QR, transaksi, audit log | Dataset dummy |
| 5 | Finalisasi matriks | Pastikan baseline dan test case sinkron | Matriks final |
| 6 | Dry run | Jalankan 3-5 test case | Instrumen tervalidasi |
| 7 | Pengujian BFLA | Jalankan TC function-level | Evidence BFLA |
| 8 | Pengujian BOLA | Jalankan TC object-level | Evidence BOLA |
| 9 | Re-test | Ulangi semua vulnerable | Retest log |
| 10 | Analisis metrik | Hitung ACR/ACFR dan severity | Tabel hasil |
| 11 | Rekomendasi | Susun mitigasi per temuan | Rekomendasi teknis |
| 12 | Finalisasi | Rapikan evidence, tabel, dan laporan | Lampiran siap |

---

## 21. Definition of Done

Pengujian dianggap selesai jika semua syarat berikut terpenuhi.

- `[todo]` Objek penelitian sudah dibekukan.
- `[todo]` Baseline sudah dibekukan.
- `[todo]` Dataset dummy lengkap.
- `[todo]` 20 test case sudah dijalankan.
- `[todo]` Semua test case punya evidence.
- `[todo]` Semua actual access sudah diklasifikasikan.
- `[todo]` Semua vulnerable sudah di-retest.
- `[todo]` Semua inconclusive punya alasan teknis.
- `[todo]` ACR dan ACFR sudah dihitung.
- `[todo]` Severity sudah ditentukan untuk vulnerable.
- `[todo]` Rekomendasi mitigasi sudah ditulis.
- `[todo]` Token, cookie, credential, dan secret sudah disamarkan.
- `[todo]` Hasil siap dimasukkan ke laporan atau dipresentasikan.

---

## 22. Pertanyaan Validasi Sebelum Mulai Testing

Jawab pertanyaan berikut sebelum pengujian utama dimulai.

1. Commit hash mana yang diuji?
2. Apakah semua endpoint di test case benar-benar tersedia?
3. Apakah role PUBLIC, CASHIER, dan OWNER sudah siap?
4. Apakah PUBLIC-A dan PUBLIC-B punya order berbeda?
5. Apakah QR valid dan QR invalid sudah tersedia?
6. Apakah database client bisa memverifikasi perubahan data?
7. Apakah Burp/Postman/curl sudah dapat mengirim request ke backend lokal?
8. Apakah baseline sudah dikunci?
9. Apakah semua expected access sudah disepakati sebelum pengujian?
10. Apakah template evidence sudah siap?

Jika salah satu jawaban masih “belum”, jangan mulai pengujian utama.

---

## 23. Ringkasan Inti

ROADMAP ini memandu pengujian otorisasi Self Order System Management dari tahap persiapan hingga analisis hasil. Penelitian tidak bertujuan mencari celah secara bebas, melainkan mengevaluasi kesesuaian antara baseline kebijakan akses dan actual access pada 20 test case yang telah ditentukan.

Inti pengujian:

```text
Baseline hak akses
        ↓
Matriks hak akses
        ↓
20 test case authorization
        ↓
Request-response manual
        ↓
Expected access vs actual access
        ↓
Compliant / Vulnerable / Logic Error / Inconclusive
        ↓
ACR, ACFR, severity, dan rekomendasi mitigasi
```

Jika roadmap ini diikuti dengan disiplin, hasil pengujian akan valid, rapi, etis, dan siap dipertanggungjawabkan dalam konteks Metodologi Penelitian.
