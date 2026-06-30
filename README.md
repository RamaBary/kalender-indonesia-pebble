# Kalender Indonesia untuk Pebble

*Kalender Indonesia for Pebble*

Kalender Indonesia adalah aplikasi kalender dan peristiwa untuk Pebble smartwatch. Repositori ini menyimpan data kalender publik, halaman pengaturan, aset web, dan struktur integrasi Timeline.

*Kalender Indonesia is a calendar and events app for Pebble smartwatches. This repository stores public calendar data, configuration pages, web assets, and the Timeline integration structure.*

---

## Struktur Repositori

*Repository Structure*

```text
kalender-indonesia-pebble/
├── README.md
├── data/
│   ├── kalender.json
│   ├── keterangan.json
│   └── katalog/
│       ├── libur-nasional.json
│       ├── libur-daerah.json
│       ├── hijriah.json
│       ├── akademik.json
│       ├── umum.json
│       └── internasional.json
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

Keterangan:

- `data/kalender.json`: indeks tetap yang mencantumkan file data yang dibaca aplikasi
- `data/katalog/*.json`: data peristiwa menurut kategori
- `data/keterangan.json`: keterangan panjang berdasarkan ID peristiwa
- `settings/index.html`: halaman pembuka pengaturan
- `settings/app.html`: halaman lengkap pengaturan
- `settings/assets/`: gambar yang digunakan halaman pengaturan
- `timeline/`: dokumentasi Timeline dan contoh format pin

*Notes:*

- *`data/kalender.json`: a stable index listing the data files read by the app*
- *`data/katalog/*.json`: event data grouped by category*
- *`data/keterangan.json`: long descriptions linked by event ID*
- *`settings/index.html`: the settings entry page*
- *`settings/app.html`: the complete settings page*
- *`settings/assets/`: images used by the settings page*
- *`timeline/`: Timeline documentation and a pin-format example*

---

## Kategori Peristiwa

*Event Categories*

| Tipe | Kategori | *Category* |
|---:|---|---|
| 1 | Libur Nasional | *National Holiday* |
| 2 | Libur Daerah | *Regional Holiday* |
| 3 | Peristiwa Hijriah | *Hijri Event* |
| 4 | Peristiwa Akademik | *Academic Event* |
| 5 | Peristiwa Umum | *General Observance* |
| 6 | Peristiwa Internasional | *International Event* |
| 7 | Peristiwa Pribadi | *Personal Event* |

Tipe `7` tidak disimpan dalam repositori publik. Peristiwa pribadi dikelola melalui halaman pengaturan pada perangkat pengguna.

*Type `7` is not stored in the public repository. Personal events are managed through the settings page on the user's device.*

---

## Format Data Peristiwa

*Event Data Format*

### Peristiwa untuk satu tahun

*Event for a specific year*

```json
{"th":2026,"bm":6,"bs":6,"hm":1,"hs":1,"tp":1,"id":4001,"jd":"Hari Lahir Pancasila"}
```

### Peristiwa berulang setiap tahun

*Annually recurring event*

```json
{"th":0,"bm":6,"bs":6,"hm":1,"hs":1,"tp":5,"id":20001,"jd":"Hari Lahir Pancasila"}
```

Aturan tahun dan tanggal:

- `th:2026` berarti peristiwa hanya berlaku pada tahun 2026
- `th:0` atau tahun kosong berarti peristiwa berulang setiap tahun
- bulan mulai dan hari mulai wajib berisi angka yang valid serta tidak boleh `0`
- bulan selesai kosong atau `0` berarti sama dengan bulan mulai
- hari selesai kosong atau `0` berarti sama dengan hari mulai
- jika bulan dan hari selesai tidak diisi, peristiwa dianggap berlangsung satu hari

*Year and date rules:*

- *`th:2026` means the event applies only to 2026*
- *`th:0` or an empty year means the event repeats every year*
- *the start month and start day must contain valid numbers and cannot be `0`*
- *an empty or `0` end month uses the start month*
- *an empty or `0` end day uses the start day*
- *when the end month and end day are omitted, the event is treated as a one-day event*

### Arti field

*Field Meanings*

| Field | Arti | *Meaning* |
|---|---|---|
| `th` | Tahun; `0` atau kosong untuk berulang setiap tahun | *Year; use `0` or leave blank for annual recurrence* |
| `bm` | Bulan mulai | *Start month* |
| `bs` | Bulan selesai | *End month* |
| `hm` | Hari mulai | *Start day* |
| `hs` | Hari selesai | *End day* |
| `tp` | Tipe peristiwa | *Event type* |
| `id` | ID unik dan stabil | *Stable unique ID* |
| `jd` | Judul pendek, maksimal 39 karakter | *Short title, maximum 39 characters* |

Aturan penting:

- urutan field baku adalah `th`, `bm`, `bs`, `hm`, `hs`, `tp`, `id`, lalu `jd`
- `bm`, `bs`, `hm`, dan `hs` menggunakan angka
- untuk peristiwa satu hari, nilai mulai dan selesai dibuat sama
- `id` tidak boleh berubah hanya karena tanggal atau judul diperbarui
- `jd` maksimal 39 karakter
- baris terakhir dalam array `peristiwa` tidak boleh diakhiri koma

*Important rules:*

- *the standard field order is `th`, `bm`, `bs`, `hm`, `hs`, `tp`, `id`, and then `jd`*
- *`bm`, `bs`, `hm`, and `hs` use numeric values*
- *for a one-day event, the start and end values must be the same*
- *an `id` must not change only because the date or title is updated*
- *`jd` is limited to 39 characters*
- *the last item in the `peristiwa` array must not end with a comma*

---

## Struktur File Kategori

*Category File Structure*

Setiap file kategori hanya memuat nama kategori dan daftar peristiwa. Metadata versi tidak disimpan dalam file kategori karena perubahan isi dicatat melalui riwayat commit.

*Each category file contains only the category name and its event list. Version metadata is not stored in category files because content changes are recorded in the commit history.*

```json
{
  "kategori": "internasional",
  "peristiwa": [
    {"th":0,"bm":1,"bs":1,"hm":4,"hs":4,"tp":6,"id":24001,"jd":"World Braille Day"},
    {"th":0,"bm":1,"bs":1,"hm":24,"hs":24,"tp":6,"id":24002,"jd":"International Day of Education"}
  ]
}
```

Field `skema` tidak diulang pada setiap file kategori. Struktur global data ditentukan oleh `data/kalender.json`, sehingga perubahan format cukup dikelola dari satu indeks utama.

*The `skema` field is not repeated in every category file. The global data structure is defined by `data/kalender.json`, allowing format changes to be managed from one main index.*

---

## Aturan ID Peristiwa

*Event ID Rules*

ID tidak disusun dari tipe, bulan, dan hari. Tanggal peristiwa tentatif dapat berubah setiap tahun, beberapa peristiwa dapat berada pada tanggal yang sama, dan susunan tanggal dapat menghasilkan ID yang bertabrakan.

*IDs are not composed from the type, month, and day. Tentative event dates can change every year, multiple events can share the same date, and date combinations can produce collisions.*

Gunakan rumus:

```text
id = (tp × 4000) + nomor urut kategori
```

| Tipe | Kategori | Rentang ID |
|---:|---|---:|
| 1 | Libur Nasional | 4001–7999 |
| 2 | Libur Daerah | 8001–11999 |
| 3 | Peristiwa Hijriah | 12001–15999 |
| 4 | Peristiwa Akademik | 16001–19999 |
| 5 | Peristiwa Umum | 20001–23999 |
| 6 | Peristiwa Internasional | 24001–27999 |

Aturan nomor urut:

- mulai dari `1` pada setiap kategori
- gunakan nomor baru yang belum pernah dipakai
- jika menyisipkan peristiwa di tengah tabel, jangan mengubah ID lama
- jangan menggunakan ulang ID yang sudah dihapus
- rentang `28000–29999` disimpan sebagai cadangan
- ID peristiwa publik tetap berada di bawah `30000`

*Sequence-number rules:*

- *start from `1` in each category*
- *use a new number that has never been assigned*
- *when inserting an event in the middle of the table, do not renumber existing IDs*
- *do not reuse a deleted ID*
- *the `28000–29999` range is reserved*
- *public event IDs remain below `30000`*

---

## Mengelola Data dengan Excel

*Managing Data with Excel*

Satu sheet Excel digunakan untuk satu kategori peristiwa dengan urutan:

1. Libur Nasional
2. Libur Daerah
3. Peristiwa Hijriah
4. Peristiwa Akademik
5. Peristiwa Umum
6. Peristiwa Internasional

Baris `1` digunakan sebagai judul kolom dan pengisian data dimulai dari baris `2`.

*One Excel sheet is used for each event category in type order. Row `1` contains the column headings and data entry begins on row `2`.*

Subtipe hanya digunakan di Excel untuk mengelompokkan dan memfilter data, misalnya `pbb`, `ksa`, `ugm`, atau `itb`. Subtipe tidak dimasukkan ke JSON, tidak dikirim ke watch, dan tidak memengaruhi skema data.

*The subtype is used only in Excel to group and filter records, such as `pbb`, `ksa`, `ugm`, or `itb`. It is not included in JSON, sent to the watch, or used by the data schema.*

### Susunan kolom

*Column Layout*

| Kolom | Field | Nama | Isi |
|---|---|---|---|
| A | `th` | Tahun | Kosong atau `0` untuk berulang; isi tahun sebenarnya untuk satu tahun |
| B | `bm` | Bulan Mulai | Angka 1–12; wajib diisi dan tidak boleh `0` |
| C | `bs` | Bulan Selesai | Kosong atau `0` untuk mengikuti bulan mulai |
| D | `hm` | Hari Mulai | Angka 1–31; wajib diisi dan tidak boleh `0` |
| E | `hs` | Hari Selesai | Kosong atau `0` untuk mengikuti hari mulai |
| F | `tp` | Tipe | Angka 1–6 |
| G | — | Subtipe | Opsional; hanya untuk pengelompokan dan filter Excel |
| H | — | Nomor Urut ID | Angka 1–3999 dan tidak boleh dipakai ulang |
| I | `id` | ID | Dibentuk otomatis dari tipe dan nomor urut |
| J | `jd` | Judul Pendek | Maksimal 39 karakter |
| K | `ket` | Keterangan | Opsional dan boleh dikosongkan |
| L | — | JSON Peristiwa | Dibentuk otomatis |
| M | — | JSON Keterangan | Dibentuk otomatis jika kolom K terisi |

*Columns A–K contain the maintained source data. Columns I, L, and M use formulas to generate the stable ID, event JSON, and description JSON. Column G is an Excel-only grouping field.*

### Formula ID pada I2

*ID Formula in I2*

Versi pemisah titik koma:

```excel
=IF(OR(F2="";H2="");"";F2*4000+H2)
```

Versi pemisah koma:

```excel
=IF(OR(F2="",H2=""),"",F2*4000+H2)
```

Salin formula I2 ke bawah sampai baris data terakhir.

*Copy the I2 formula down to the last data row.*

### Formula JSON peristiwa pada L2

*Event JSON Formula in L2*

Versi pemisah titik koma:

```excel
=IF(OR(B2="";B2=0;D2="";D2=0;F2="";I2="";J2="");"ERROR: Data wajib belum lengkap";IF(OR(B2<1;B2>12;D2<1;D2>31;AND(C2<>"";C2<>0;OR(C2<1;C2>12));AND(E2<>"";E2<>0;OR(E2<1;E2>31)));"ERROR: Bulan atau hari tidak valid";IF(LEN(J2)>39;"ERROR: Judul lebih dari 39 karakter";"{""th"":"&IF(A2="";0;A2)&",""bm"":"&B2&",""bs"":"&IF(OR(C2="";C2=0);B2;C2)&",""hm"":"&D2&",""hs"":"&IF(OR(E2="";E2=0);D2;E2)&",""tp"":"&F2&",""id"":"&I2&",""jd"":"""&SUBSTITUTE(SUBSTITUTE(J2;CHAR(92);CHAR(92)&CHAR(92));CHAR(34);CHAR(92)&CHAR(34))&"""}"&IF(COUNTIF(I3:I$1000;"<>")>0;",";""))))
```

Versi pemisah koma:

```excel
=IF(OR(B2="",B2=0,D2="",D2=0,F2="",I2="",J2=""),"ERROR: Data wajib belum lengkap",IF(OR(B2<1,B2>12,D2<1,D2>31,AND(C2<>"",C2<>0,OR(C2<1,C2>12)),AND(E2<>"",E2<>0,OR(E2<1,E2>31))),"ERROR: Bulan atau hari tidak valid",IF(LEN(J2)>39,"ERROR: Judul lebih dari 39 karakter","{""th"":"&IF(A2="",0,A2)&",""bm"":"&B2&",""bs"":"&IF(OR(C2="",C2=0),B2,C2)&",""hm"":"&D2&",""hs"":"&IF(OR(E2="",E2=0),D2,E2)&",""tp"":"&F2&",""id"":"&I2&",""jd"":"""&SUBSTITUTE(SUBSTITUTE(J2,CHAR(92),CHAR(92)&CHAR(92)),CHAR(34),CHAR(92)&CHAR(34))&"""}"&IF(COUNTIF(I3:I$1000,"<>")>0,",",""))))
```

Formula tersebut:

- tidak memasukkan kolom Subtipe ke JSON
- mengubah tahun kosong menjadi `th:0`
- menolak bulan mulai atau hari mulai yang kosong atau `0`
- menggunakan bulan mulai jika bulan selesai kosong atau `0`
- menggunakan hari mulai jika hari selesai kosong atau `0`
- memeriksa rentang dasar bulan dan hari
- memeriksa batas judul 39 karakter
- mengamankan tanda `\` dan tanda kutip dalam judul
- menambahkan koma jika masih ada data di bawah baris aktif

*The formula does not include the Subtype column in JSON. It converts an empty year to `th:0`, rejects an empty or zero start month or day, defaults missing end values to the start values, checks basic month and day ranges, validates the title length, escapes special characters, and adds a comma when more records follow.*

### Formula JSON keterangan pada M2

*Description JSON Formula in M2*

Versi pemisah titik koma:

```excel
=IF(OR(I2="";K2="");"";""""&I2&""":{""ket"":"""&SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(K2;CHAR(92);CHAR(92)&CHAR(92));CHAR(34);CHAR(92)&CHAR(34));CHAR(13);"");CHAR(10);CHAR(92)&"n")&"""}"&IF(COUNTIF(K3:K$1000;"<>")>0;",";""))
```

Versi pemisah koma:

```excel
=IF(OR(I2="",K2=""),"",""""&I2&""":{""ket"":"""&SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(K2,CHAR(92),CHAR(92)&CHAR(92)),CHAR(34),CHAR(92)&CHAR(34)),CHAR(13),""),CHAR(10),CHAR(92)&"n")&"""}"&IF(COUNTIF(K3:K$1000,"<>")>0,",",""))
```

Formula tersebut:

- menghasilkan baris hanya jika keterangan tersedia
- menggunakan ID yang sama dengan peristiwa
- mengamankan tanda `\`, tanda kutip, dan pindah baris
- tidak membutuhkan tabel keterangan terpisah pada setiap sheet

*The formula generates a row only when a description is available, uses the same event ID, escapes special characters and line breaks, and does not require a separate description table on each sheet.*

Formula memeriksa data sampai baris `1000`. Sesuaikan batas `$1000` jika jumlah data melebihi batas tersebut.

*The formulas check records through row `1000`. Increase the `$1000` limit when a category exceeds that row.*

---

## Menyalin JSON Peristiwa

*Copying Event JSON*

1. Buka sheet kategori yang akan diperbarui
2. Isi data mulai dari baris `2`
3. Salin formula I2, L2, dan M2 ke seluruh baris data
4. Gunakan kolom G untuk memfilter atau mengelompokkan Subtipe jika diperlukan
5. Pastikan kolom L menampilkan JSON dan bukan pesan error
6. Salin hasil kolom L
7. Buka file JSON kategori yang sesuai pada GitHub
8. Pilih **Edit this file**
9. Tempel hasil kolom L di antara tanda `[` dan `]` pada bagian `peristiwa`
10. Periksa agar objek terakhir tidak memiliki koma
11. Pilih **Commit changes** atau ajukan pull request jika tidak memiliki akses tulis

*Open the required category sheet, enter data from row `2`, fill the formulas down, optionally filter records by Subtype in column G, copy column L, edit the matching category JSON file on GitHub, paste the generated rows into the `peristiwa` array, verify the final comma, and commit the change or submit a pull request.*

### Lokasi penempelan

*Paste Location*

```text
{
  "kategori": "internasional",
  "peristiwa": [
    TEMPEL HASIL KOLOM L DI SINI
  ]
}
```

Contoh setelah ditempel:

*Example After Pasting:*

```json
{
  "kategori": "internasional",
  "peristiwa": [
    {"th":0,"bm":1,"bs":1,"hm":4,"hs":4,"tp":6,"id":24001,"jd":"World Braille Day"},
    {"th":0,"bm":1,"bs":1,"hm":24,"hs":24,"tp":6,"id":24002,"jd":"International Day of Education"}
  ]
}
```

---

## Menyatukan Keterangan dari Semua Kategori

*Merging Descriptions from All Categories*

Buat satu sheet tambahan bernama `Keterangan`. Sheet ini hanya digunakan untuk menyatukan hasil kolom M dari seluruh sheet kategori.

*Create one additional sheet named `Keterangan`. This sheet is used only to combine the values generated in column M from every category sheet.*

Urutan penggabungan:

1. Libur Nasional
2. Libur Daerah
3. Peristiwa Hijriah
4. Peristiwa Akademik
5. Peristiwa Umum
6. Peristiwa Internasional

Langkah:

1. Pada setiap sheet kategori, salin sel kolom M yang tidak kosong
2. Tempel sebagai nilai secara berurutan mulai dari `A2` pada sheet `Keterangan`
3. Jangan menambahkan baris kosong di antara kategori
4. Periksa agar tidak terdapat ID ganda
5. Pastikan hanya baris terakhir yang tidak memiliki koma
6. Salin seluruh hasil dari sheet `Keterangan`
7. Buka `data/keterangan.json` pada GitHub
8. Tempel hasilnya ke dalam objek `keterangan`
9. Pilih **Commit changes** atau ajukan pull request jika tidak memiliki akses tulis

*Copy every non-empty value from column M of each category sheet, paste the values in type order into the `Keterangan` sheet, check for duplicate IDs and the final comma, then paste the combined result into the `keterangan` object in `data/keterangan.json`.*

### Lokasi penempelan

*Paste Location*

```text
{
  "keterangan": {
    TEMPEL HASIL SHEET KETERANGAN DI SINI
  }
}
```

Contoh setelah ditempel:

*Example After Pasting:*

```json
{
  "keterangan": {
    "4001":{"ket":"Keterangan untuk peristiwa libur nasional."},
    "24001":{"ket":"World Braille Day raises awareness of the importance of Braille."}
  }
}
```

Jika kolom K pada sheet kategori kosong, kolom M juga kosong dan ID tersebut tidak perlu dimasukkan ke `keterangan.json`. Aplikasi cukup menampilkan judul pendek.

*When column K is empty, column M also remains empty and the ID does not need to be included in `keterangan.json`. The app displays only the short title.*

---

## Indeks Data Utama

*Main Data Index*

`data/kalender.json` menjadi satu alamat tetap yang dibaca aplikasi. File ini mencantumkan seluruh file kategori dan file keterangan.

*`data/kalender.json` is the stable data address read by the app. It lists all category files and the description file.*

Contoh struktur:

```json
{
  "skema": 3,
  "berkas": [
    "katalog/libur-nasional.json",
    "katalog/libur-daerah.json",
    "katalog/hijriah.json",
    "katalog/akademik.json",
    "katalog/umum.json",
    "katalog/internasional.json"
  ],
  "keterangan": "keterangan.json"
}
```

Field `skema` hanya disimpan pada indeks utama untuk menandai kontrak struktur data. Nomor tersebut berubah hanya jika nama field atau bentuk data berubah, bukan ketika tanggal, judul, atau keterangan diperbarui.

*The `skema` field is stored only in the main index to identify the data-structure contract. It changes only when field names or the data shape change, not when dates, titles, or descriptions are updated.*

---

## Pemeliharaan File

*File Maintenance*

Nama file aktif tidak memuat nomor versi. Setiap pembaruan dilakukan dengan mengedit isi file yang sama.

*Active filenames do not contain version numbers. Every update edits the content of the same file.*

Contoh:

```text
data/katalog/umum.json
data/keterangan.json
settings/app.html
settings/index.html
```

Riwayat perubahan tersedia melalui commit Git.

*The change history is available through Git commits.*

Jangan mengunggah file baru hanya untuk mengganti versi dari file aktif yang sama.

*Do not upload a new file only to replace the version of the same active file.*

---

## Halaman Pengaturan

*Settings Page*

Halaman pengaturan menggunakan file berikut:

```text
settings/index.html
settings/app.html
settings/assets/
```

*The settings page uses the files listed above.*

---

## Timeline

Folder `timeline/` memuat dokumentasi Timeline dan contoh struktur pin. Data Timeline menggunakan sumber peristiwa yang sama dari JSON kategori sehingga tidak memerlukan pemeliharaan daftar kalender kedua.

*The `timeline/` folder contains Timeline documentation and a pin-structure example. Timeline data uses the same category JSON source, avoiding maintenance of a second calendar list.*

---

## Status Pengembangan

*Development Status*

Aplikasi masih berada pada tahap pengembangan dan belum menjadi rilis publik.

*The app is still under development and has not yet reached its public release.*
