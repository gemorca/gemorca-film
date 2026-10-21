<div align="center">

<img src="assets/img/icon-512.png" alt="Logo Gemorca Film" width="120">

# 🎬 Gemorca Film

**Tonton film pilihan langsung dari browser, di HP maupun laptop.**

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![PWA](https://img.shields.io/badge/PWA-Installable-5A0FC8?style=for-the-badge&logo=pwa&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)

</div>

---

## 📖 Tentang Proyek

**Gemorca Film** adalah web streaming film berbasis *Progressive Web App* (PWA). Pengunjung dapat menelusuri katalog, mencari berdasarkan judul atau genre, lalu menonton langsung lewat pemutar bawaan tanpa berpindah halaman. Aplikasi juga bisa dipasang ke layar utama ponsel dan terasa seperti aplikasi asli.

Katalog film dikelola lewat **Google Sheets** sebagai database, sehingga penambahan dan perubahan film bisa dilakukan dari panel admin tanpa menyentuh kode.

## ✨ Fitur Utama

### Untuk Pengunjung
- **Hero slideshow**: film pilihan tampil bergantian secara acak dengan latar yang berganti halus.
- **Pencarian cerdas**: kolom cari dengan saran otomatis berdasarkan judul dan genre.
- **Filter genre dan pengurutan**: pilih genre lewat *chip*, urutkan menurut tahun terbaru, tahun terlama, judul A–Z, atau paling banyak ditonton.
- **Pemutar video kustom**: kontrol sendiri untuk putar/jeda, seek, volume, bisu, dan layar penuh, dengan dukungan pintasan keyboard.
- **Sumber video**: mendukung tautan **YouTube** dan **Google Drive**.
- **Film serupa**: rekomendasi otomatis di dalam jendela pemutar.
- **Favorit**: simpan film kesukaan, tersimpan di perangkat pengunjung.
- **Bagikan film**: setiap film punya tautan sendiri (`#film-...`) yang bisa dibagikan, termasuk lewat menu *share* bawaan perangkat.
- **Segera hadir**: film yang belum rilis tampil dengan hitung mundur.
- **Pengumuman dan mode pemeliharaan**: banner pengumuman serta halaman pemeliharaan dengan hitung mundur perkiraan selesai.
- **PWA**: bisa dipasang ke layar utama dan tetap memuat cepat berkat *service worker*.
- **Tampilan responsif** dengan *skeleton loading* agar halaman terasa cepat.

### Untuk Admin
- **Login dengan PIN** yang diverifikasi di sisi server.
- **Kelola film**: tambah, ubah, dan hapus film, lengkap dengan pencarian, filter "Segera Hadir", dan pagination.
- **Input genre berbentuk tag** dengan saran otomatis.
- **Pratinjau poster** sebelum disimpan.
- **Pengaturan situs**: aktifkan mode pemeliharaan, atur pesan, perkiraan selesai, dan pengumuman untuk pengunjung.
- **Refresh data**: bersihkan cache dan muat ulang katalog dengan satu tombol.

## 🗂️ Struktur Proyek

```
.
├── index.html            # Halaman utama dan markup panel admin
├── manifest.json         # Konfigurasi PWA
├── service-worker.js     # Cache aplikasi dan dukungan offline
├── api/
│   └── login.js          # Serverless function verifikasi PIN admin (Vercel)
├── css/
│   └── style.css         # Seluruh gaya tampilan
├── js/
│   ├── script.js         # Katalog, pencarian, filter, pemutar, favorit, PWA
│   ├── status.js         # Mode pemeliharaan dan pengumuman
│   └── admin.js          # Panel admin
└── assets/
    └── img/
        ├── icon-192.png
        ├── icon-512.png
        └── developer.jpg
```

## 🧩 Cara Kerja

```
Pengunjung  ──►  index.html + script.js  ──►  Google Sheets (data film)
                        │
                        ├──►  Google Apps Script (status situs dan jumlah tayangan)
                        │
Admin  ──►  admin.js  ──►  /api/login (cek PIN)  ──►  Apps Script (simpan perubahan)
```

- **Data film** dibaca dari Google Sheets dan di-*cache* sementara di browser agar cepat.
- **Status situs dan statistik tayangan** dilayani oleh Google Apps Script.
- **PIN admin** tidak disimpan di kode. Nilainya dibaca dari *environment variable* di server.

## 🚀 Menjalankan dan Deploy

Proyek ini dirancang untuk **Vercel**.

1. *Clone* repositori ini.

   ```bash
   git clone https://github.com/gemorca/<nama-repo>.git
   cd <nama-repo>
   ```

2. Import repositori ke [Vercel](https://vercel.com/).

3. Tambahkan *environment variable* berikut di pengaturan proyek Vercel:

   | Nama | Fungsi |
   |---|---|
   | `ADMIN_PIN` | PIN rahasia untuk masuk ke panel admin |

4. Deploy. Folder `api/` otomatis menjadi serverless function.

**Pengembangan lokal:** gunakan Vercel CLI agar endpoint `/api/login` ikut berjalan.

```bash
npm i -g vercel
vercel dev
```

> Membuka `index.html` langsung di browser tetap menampilkan katalog, tetapi login admin tidak akan berfungsi karena membutuhkan endpoint `/api/login`.

## ⚙️ Kustomisasi

| Yang ingin diubah | Lokasi |
|---|---|
| Sumber data film (Google Sheets) | konstanta `SPREADSHEET_ID` di `js/script.js` |
| Lama cache data katalog | `SHEET_CACHE_TTL_MS` di `js/script.js` |
| Jumlah dan interval slide hero | `HERO_MAX_SLIDES` dan `HERO_SLIDE_INTERVAL_MS` di `js/script.js` |
| Jumlah film serupa | `RELATED_MAX` di `js/script.js` |
| Jumlah item per halaman admin | `ITEMS_PER_PAGE` di `js/admin.js` |
| Nama, warna, dan ikon aplikasi | `manifest.json` serta variabel CSS di `css/style.css` |
| Versi cache PWA | `CACHE_NAME` di `service-worker.js` (naikkan setiap rilis baru) |

## 🛠️ Teknologi

- **HTML5**, **CSS3**, dan **JavaScript** (Vanilla, tanpa framework)
- **Progressive Web App**: Web App Manifest dan Service Worker
- **Vercel Serverless Functions** untuk verifikasi login
- **Google Sheets** dan **Google Apps Script** sebagai database dan backend ringan
- [SweetAlert2](https://sweetalert2.github.io/) untuk dialog dan notifikasi di panel admin
- [Google Fonts](https://fonts.google.com/) (Bricolage Grotesque dan DM Sans)
- YouTube IFrame API untuk pemutar video

## 👤 Kredit

Seluruh proyek ini, mulai dari desain, kode, hingga konten, dibuat sepenuhnya oleh:

**Moh. Agil S. Abdul**
[GitHub](https://github.com/gemorca) · [Instagram](https://instagram.com/mohammadagill)

> Pustaka dan layanan pihak ketiga yang disebutkan di bagian Teknologi tetap menjadi milik masing-masing pembuatnya dan digunakan sesuai ketentuan mereka.

## 📄 Lisensi

Hak Cipta © 2026 **Moh. Agil S. Abdul**. Seluruh hak dilindungi (*All Rights Reserved*).

Kode sumber, desain, dan logo dalam repositori ini dibuat dan dimiliki sepenuhnya oleh pemilik hak cipta. Anda bebas memakai, mengubah, dan membagikannya dengan tetap mencantumkan pemberitahuan hak cipta di atas.

---

<div align="center">

**Gemorca Film** · Dibuat sendiri oleh Moh. Agil S. Abdul.

</div>
