# Kalender Indonesia for Pebble

## Indonesia

Kalender Indonesia adalah aplikasi kalender dan peristiwa untuk Pebble smartwatch. Repositori publik ini menyimpan data kalender, halaman pengaturan, aset web, dan persiapan integrasi Timeline.

Kode sumber watchapp dikembangkan pada repositori privat terpisah.

## English

Kalender Indonesia is a calendar and events app for Pebble smartwatches. This public repository stores calendar data, configuration pages, web assets, and preparation files for future Timeline integration.

The watchapp source code is maintained in a separate private repository.

---

## Struktur Repositori / Repository Structure

```text
kalender-indonesia-pebble/
├── README.md
├── data/
│   ├── kalender.json
│   ├── deskripsi.json
│   └── katalog/
│       ├── akademik.json
│       ├── hijriah.json
│       ├── internasional.json
│       ├── libur-daerah.json
│       ├── libur-nasional.json
│       └── umum.json
├── settings/
│   ├── index.html
│   ├── app.html
│   └── assets/
│       ├── banner-large.jpg
│       └── banner-small.jpg
└── timeline/
    ├── README.md
    └── template-pin.json
```

Keterangan singkat:

- `data/kalender.json`: indeks tetap yang mencantumkan file data yang dibaca aplikasi
- `data/katalog/*.json`: data peristiwa menurut kategori
- `data/deskripsi.json`: keterangan panjang berdasarkan ID peristiwa
- `settings/index.html`: shell halaman pengaturan
- `settings/app.html`: isi lengkap halaman pengaturan
- `settings/assets/`: gambar untuk halaman pengaturan
- `timeline/`: persiapan struktur data Timeline; belum menjadi fitur aktif

---

## Kategori Peristiwa / Event Categories

| Kode | Indonesia | English |
|---:|---|---|
| 1 | Libur Nasional | National Holiday |
| 2 | Libur Daerah | Regional Holiday |
| 3 | Peristiwa Hijriah | Hijri Event |
| 4 | Peristiwa Akademik | Academic Event |
| 5 | Peristiwa Umum | General Observance |
| 6 | Peristiwa Internasional | International Event |
| 7 | Peristiwa Pribadi | Personal Event |

Tipe `7` tidak disimpan di repositori publik. Peristiwa pribadi disimpan melalui halaman pengaturan pada perangkat pengguna.

Type `7` is not stored in this public repository. Personal events are managed through the settings page on the user's device.

---

## Format Data Peristiwa / Event Data Format

### Peristiwa berdasarkan tahun / Year-specific event

```json
{"th":2026,"bl":6,"hr":1,"bs":6,"sd":1,"tp":1,"id":601,"jd":"Hari Lahir Pancasila"}
```

### Peristiwa berulang setiap tahun / Annual recurring event

```json
{"ut":1,"bl":6,"hr":1,"bs":6,"sd":1,"tp":5,"id":601,"jd":"Hari Lahir Pancasila"}
```

Arti field:

| Field | Arti / Meaning |
|---|---|
| `th` | Tahun peristiwa / Event year |
| `ut` | Ulang tahunan; gunakan `1` / Annual recurrence; use `1` |
| `bl` | Bulan mulai / Start month |
| `hr` | Hari mulai / Start day |
| `bs` | Bulan selesai / End month |
| `sd` | Hari selesai / End day |
| `tp` | Tipe peristiwa / Event type |
| `id` | ID unik global / Global unique ID |
| `jd` | Judul pendek, maksimal 39 karakter / Short title, maximum 39 characters |

Aturan penting:

- Gunakan `th` untuk peristiwa yang hanya berlaku pada tahun tertentu
- Gunakan `ut:1` untuk peristiwa tetap yang berulang setiap tahun
- Jangan memakai `th` dan `ut` pada objek yang sama
- `id` harus unik dan tidak berubah hanya karena judul diperbaiki
- `bs` dan `sd` sama dengan `bl` dan `hr` untuk peristiwa satu hari
- Jangan menambahkan peristiwa pribadi ke repositori publik
- Baris terakhir di dalam array `peristiwa` tidak boleh diakhiri koma

---

## Mengelola Data dengan Excel / Managing Data with Excel

Excel dipakai untuk menyusun satu objek JSON per baris. Tidak diperlukan CSV atau converter tambahan.

Excel is used to generate one JSON object per row. No CSV file or additional converter is required.

### Susunan kolom / Column Layout

| Kolom | Nama | Isi |
|---|---|---|
| A | Tahun | Tahun peristiwa; kosongkan untuk peristiwa tahunan |
| B | Bulan Mulai | Angka 1–12 |
| C | Hari Mulai | Angka 1–31 |
| D | Bulan Selesai | Angka 1–12 |
| E | Hari Selesai | Angka 1–31 |
| F | Tipe | Angka 1–6 |
| G | ID | ID unik global |
| H | Judul Pendek | Maksimal 39 karakter |
| I | Ulang Tahunan | Isi `1` untuk peristiwa tetap; selain itu kosong |
| J | Hasil JSON | Formula pembentuk objek JSON |

Jangan menyisipkan baris kosong di tengah data karena formula menggunakan ID pada baris berikutnya untuk menentukan koma.

Do not insert blank rows between records because the formula uses the next row's ID to determine whether a comma is needed.

### Formula gabungan untuk kolom J / Combined Formula for Column J

Formula berikut memakai pemisah argumen titik koma. Jika Excel Anda memakai koma sebagai pemisah argumen, ganti seluruh `;` menjadi `,`.

```excel
=IF(G2="";"";IF(I2=1;"{""ut"":1,""bl"":"&B2&",""hr"":"&C2&",""bs"":"&D2&",""sd"":"&E2&",""tp"":"&F2&",""id"":"&G2&",""jd"":"""&SUBSTITUTE(H2;CHAR(34);CHAR(92)&CHAR(34))&"""}";"{""th"":"&A2&",""bl"":"&B2&",""hr"":"&C2&",""bs"":"&D2&",""sd"":"&E2&",""tp"":"&F2&",""id"":"&G2&",""jd"":"""&SUBSTITUTE(H2;CHAR(34);CHAR(92)&CHAR(34))&"""}")&IF(G3<>"";",";""))
```

Perilaku formula:

- Jika kolom `I` berisi `1`, hasil memakai field `ut`
- Jika kolom `I` kosong, hasil memakai field `th`
- Tanda kutip di dalam judul akan diubah menjadi bentuk aman untuk JSON
- Koma otomatis ditambahkan selama baris berikutnya masih memiliki ID
- Baris terakhir tidak memperoleh koma

### Prosedur pengisian / Editing Procedure

1. Buka file Excel pengelolaan data
2. Isi kolom A–I sesuai kategori peristiwa
3. Salin formula pada kolom J sampai baris terakhir
4. Periksa tanggal, tipe, ID, dan panjang judul
5. Salin hasil pada kolom J
6. Buka file JSON kategori yang sesuai di VS Code
7. Tempel hasil ke dalam array `peristiwa`
8. Pastikan tidak ada koma setelah objek terakhir
9. Simpan file
10. Periksa tampilan perubahan pada Source Control
11. Commit dan push setelah JSON dinyatakan valid

### Contoh wadah file kategori / Category File Container Example

```json
{
  "skema": 3,
  "kategori": "umum",
  "versi": "2026-r1",
  "peristiwa": [
    {"ut":1,"bl":6,"hr":1,"bs":6,"sd":1,"tp":5,"id":601,"jd":"Hari Lahir Pancasila"},
    {"ut":1,"bl":8,"hr":17,"bs":8,"sd":17,"tp":5,"id":817,"jd":"Hari Kemerdekaan"}
  ]
}
```

---

## Mengelola Keterangan Panjang dengan Excel / Managing Long Descriptions with Excel

Keterangan panjang disimpan terpisah dalam:

```text
data/deskripsi.json
```

Gunakan susunan kolom:

| Kolom | Nama |
|---|---|
| A | ID |
| B | Keterangan |
| C | Hasil JSON |

Formula kolom C:

```excel
=IF(A2="";"";""""&A2&""":{""teks"":"""&SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(B2;CHAR(92);CHAR(92)&CHAR(92));CHAR(34);CHAR(92)&CHAR(34));CHAR(10);CHAR(92)&"n")&"""}"&IF(A3<>"";",";""))
```

Formula tersebut:

- mengubah tanda `\` menjadi bentuk aman
- mengubah tanda kutip menjadi bentuk aman
- mengubah pindah baris menjadi `\n`
- menambahkan koma kecuali pada data terakhir

Contoh hasil:

```json
{
  "601":{"teks":"Hari Lahir Pancasila diperingati setiap 1 Juni."},
  "817":{"teks":"Hari Kemerdekaan Republik Indonesia diperingati setiap 17 Agustus."}
}
```

Jika suatu peristiwa tidak memiliki keterangan panjang, ID tersebut tidak perlu dimasukkan ke `deskripsi.json`. Aplikasi nantinya cukup menampilkan judul pendek.

---

## Pembagian Data Tetap dan Tentatif / Fixed and Tentative Data

Data tetap menggunakan:

```json
"ut":1
```

Data tentatif atau tahunan menggunakan:

```json
"th":2026
```

Contoh data tentatif:

- libur nasional berdasarkan keputusan tahunan
- libur daerah
- kalender akademik
- peristiwa Hijriah

Contoh data tetap:

- peringatan umum nonlibur
- peringatan internasional yang tanggalnya tetap

PebbleKit JS nantinya hanya mengirim peristiwa tahun aktif dan peristiwa `ut:1` ke watch. Data tahun lain tetap berada di repositori dan tidak disimpan seluruhnya pada memori watch.

---

## Pemeliharaan File / File Maintenance

Nama file aktif tidak memuat nomor versi. Setiap perbaikan dilakukan dengan mengedit isi file yang sama.

Contoh:

```text
data/katalog/umum.json
settings/app.html
settings/index.html
```

Nomor versi dicatat melalui:

- pesan commit
- tag Git
- metadata `versi` di dalam JSON jika relevan

Jangan mengunggah file baru hanya untuk mengganti versi file yang sama.

---

## Settings Page

Halaman pengaturan aktif terdiri dari:

```text
settings/index.html
settings/app.html
settings/assets/
```

`index.html` berfungsi sebagai shell ringan. `app.html` memuat halaman pengaturan lengkap.

---

## Timeline

Folder `timeline/` hanya disiapkan untuk pengembangan berikutnya. Data Timeline nantinya dapat dibentuk dari JSON kategori yang sama sehingga tidak memerlukan pemeliharaan daftar peristiwa kedua.

Timeline integration is reserved for a later version. Timeline data can later be generated from the same category JSON files, avoiding duplicate event maintenance.

---

## Status Pengembangan / Development Status

Versi saat ini masih merupakan tahap pengembangan internal dan belum menjadi rilis publik.

The current version is an internal development build and is not yet a public release.