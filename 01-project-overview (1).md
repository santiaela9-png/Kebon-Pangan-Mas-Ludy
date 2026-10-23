# 01 — Project Overview

## 1. Latar Belakang dan Permasalahan

**[Fakta, dari laporan]** Proyek ini adalah tugas mata kuliah *Aplikasi Berbasis Service* (Program Studi Teknik Informatika, Fakultas Teknik dan Ilmu Komputer, Universitas Nahdlatul Ulama Indonesia, 2026) berjudul *Sistem Pemesanan Makanan Berbasis Layanan Mikro pada Kebon Pangan Mas Ludy*. Disusun oleh Fatih Ludy Aryasatya (24260047) dan Ela Santia (24260019).

Permasalahan yang diselesaikan:

1. Pengelolaan pesanan secara manual pada usaha kuliner berisiko menimbulkan kesalahan pencatatan dan keterlambatan layanan.
2. Ketersediaan menu sulit dipantau tanpa sistem yang terstruktur.
3. Kebon Pangan Mas Ludy membutuhkan sistem pemesanan yang melayani tiga pihak sekaligus: pelanggan, pemilik, dan admin.
4. Aplikasi yang tidak dipisah per fungsi lebih sulit dikembangkan dan dipelihara.

## 2. Tujuan dan Manfaat

| Tujuan | Manfaat |
|---|---|
| Membangun sistem pemesanan makanan berbasis web dengan arsitektur layanan mikro: 5 layanan independen (User, Menu, Cart, Order, Payment) di belakang satu API Gateway | Sistem lebih mudah dikembangkan dan dipelihara per layanan |
| Menyediakan alur pemesanan lengkap: pencarian menu, keranjang, alamat antar, pemesanan, dan pembayaran | Proses pemesanan lebih terstruktur, risiko salah catat berkurang |
| Mendukung tiga tipe pesanan: makan di tempat, bawa pulang, dan antar | Pelanggan dapat memilih cara menerima pesanan sesuai kebutuhan |
| Menyediakan pengelolaan menu, kategori, stok porsi, pesanan, dan pengguna | Pemilik dan admin dapat memantau ketersediaan menu dan pesanan |
| Memodelkan perancangan dengan diagram arsitektur, use case, dan ERD | Dasar dokumentasi dan analisis tugas |

## 3. Ruang Lingkup

**Dalam lingkup (In scope):**
- Registrasi dan login pengguna dengan tiga peran (Pelanggan, Pemilik, Admin).
- Pengelolaan alamat antar oleh pelanggan.
- Menu: daftar menu, pencarian menu, kategori, dan stok porsi.
- Keranjang pesanan.
- Pemesanan dengan tipe makan di tempat, bawa pulang, atau antar.
- Pembayaran.
- Pengelolaan menu, kategori, pesanan, dan pengguna oleh pemilik/admin.
- Antarmuka web (HTML, CSS, JavaScript) yang mengakses sistem lewat API Gateway.

**Di luar lingkup (Won't, tahap ini) — PROPOSED, perlu dikonfirmasi:** integrasi payment gateway nyata, notifikasi email/push, aplikasi mobile native, pelacakan kurir real-time, laporan keuangan lanjutan, dan penggunaan basis data relasional (data disimpan dalam berkas JSON).

## 4. Pengguna dan Stakeholder

| Peran | Kebutuhan utama |
|---|---|
| Pelanggan | Registrasi dan login, mencari menu, mengelola keranjang dan alamat antar, memesan, membayar |
| Pemilik | Mengelola menu, kategori, dan stok porsi, memantau serta memproses pesanan |
| Administrator | Mengelola pengguna dan pesanan, serta menjaga operasional sistem |
| Pengembang (stakeholder) | Bukti arsitektur layanan mikro dan dokumentasi perancangan |
| Dosen pengampu | Kesesuaian dengan tujuan mata kuliah *Aplikasi Berbasis Service* |

## 5. Gambaran Fitur Utama

Frontend → API Gateway (port 3000) → User Service (3001), Menu Service (3002), Cart Service (3003), Order Service (3004), Payment Service (3005).

Alur inti: *login → telusuri/cari menu → masukkan ke keranjang → buat pesanan (pilih tipe pesanan, alamat bila diantar) → pembayaran → pesanan diproses pemilik/admin*. Komunikasi antara antarmuka dan layanan memakai REST API dengan format JSON. Setiap layanan menyimpan datanya dalam berkas JSON miliknya sendiri:

| Layanan | Port | Tanggung jawab | Penyimpanan |
|---|---|---|---|
| API Gateway | 3000 | Pintu masuk yang mengatur permintaan dari website | — |
| User Service | 3001 | Register, login, alamat antar | `users.json` |
| Menu Service | 3002 | Menu, kategori, stok porsi | `menu.json` |
| Cart Service | 3003 | Keranjang pesanan | `carts.json` |
| Order Service | 3004 | Pesanan dan tipe pesanan | `orders.json` |
| Payment Service | 3005 | Pembayaran | `payments.json` |

Detail di [02](02-requirements.md) dan [03](03-system-architecture.md).

## 6. Batasan Sistem

- Waktu pengerjaan tugas mata kuliah terbatas, sehingga cakupan fitur dibatasi pada alur pemesanan inti.
- Penyimpanan data memakai berkas JSON per layanan, bukan basis data, sehingga tidak dimaksudkan untuk beban atau konkurensi tinggi.
- Antarmuka dibangun dengan HTML, CSS, dan JavaScript tanpa framework (sesuai laporan).
- Setiap layanan memiliki data sendiri; tidak ada basis data bersama antarlayanan.

## 7. Asumsi dan Dependensi

| ID | Asumsi/Dependensi | Status |
|---|---|---|
| AS-01 | Pengguna melakukan registrasi sendiri dan login | Dari laporan |
| AS-02 | Tiga peran pengguna: pelanggan, pemilik, admin | Dari laporan |
| AS-03 | Tipe pesanan: makan di tempat, bawa pulang, antar | Dari laporan |
| AS-04 | Dikerjakan oleh dua orang (Fatih Ludy Aryasatya dan Ela Santia) | Dari laporan |
| AS-05 | Data tiap layanan disimpan dalam berkas JSON masing-masing | Dari laporan |
| AS-06 | Metode pembayaran yang didukung (tunai, transfer, QRIS, dll.) belum dirinci | TBD |
| AS-07 | Stok porsi berkurang otomatis saat pesanan dibuat atau dibayar | TBD |
| AS-08 | Perlu tidaknya perbandingan formal dengan arsitektur monolith dalam laporan | TBD |
| AS-09 | Skala pengguna dan target kinerja (uji beban) belum ditetapkan | TBD |
| AS-10 | Lingkungan deployment (lokal, VPS, atau Docker) belum ditentukan | TBD |
| DEP-01 | Runtime server (mis. Node.js) dan versinya diverifikasi ke dokumentasi resmi lalu dipin saat implementasi | PROPOSED |

## 8. Kriteria Keberhasilan

| ID | Kriteria | Cara ukur |
|---|---|---|
| SC-01 | Seluruh fitur berprioritas Must berjalan sesuai acceptance criteria | Laporan pengujian |
| SC-02 | Alur E2E: registrasi/login → pilih menu → keranjang → pesanan → pembayaran berjalan benar | Tes E2E |
| SC-03 | Kelima layanan dan API Gateway dapat dijalankan bersamaan dan diakses lewat port 3000 | Uji deployment |
| SC-04 | Kegagalan satu layanan tidak menghentikan layanan lain yang tidak bergantung padanya | Tes ketahanan |
| SC-05 | Peran pelanggan, pemilik, dan admin hanya dapat mengakses fitur sesuai haknya | Tes otorisasi |
| SC-06 | Diagram arsitektur, use case, dan ERD tersedia dan konsisten dengan implementasi | Tinjauan dokumen |

## 9. Glosarium

| Istilah | Arti |
|---|---|
| Layanan mikro (microservices) | Arsitektur yang memecah sistem menjadi layanan independen dengan tanggung jawab dan data masing-masing |
| API Gateway | Pintu masuk tunggal (port 3000) yang meneruskan permintaan frontend ke layanan terkait |
| REST API | Gaya komunikasi HTTP antara antarmuka dan layanan dengan format JSON |
| Pelanggan | Pengguna yang memesan makanan |
| Pemilik | Pengelola usaha yang mengatur menu dan memantau pesanan |
| Admin | Pengelola sistem, termasuk pengguna dan pesanan |
| Stok porsi | Jumlah porsi menu yang tersedia untuk dipesan |
| Tipe pesanan | Cara pesanan diterima: makan di tempat, bawa pulang, atau antar |
| Alamat antar | Alamat tujuan pengiriman yang disimpan pelanggan |
| Keranjang (Cart) | Daftar menu sementara sebelum dijadikan pesanan |
| ERD | Entity Relationship Diagram |
| Use case | Diagram yang menggambarkan interaksi pengguna dengan sistem |
