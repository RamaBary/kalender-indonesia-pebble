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

Keterangan:

- `data/kalender.json`: indeks tetap yang mencantumkan file data yang dibaca aplikasi
- `data/katalog/*.json`: data peristiwa menurut kategori
- `data/keterangan.json`: keterangan panjang berdasarkan ID peristiwa
- `settings/index.html`: halaman shell ringan yang dibuka lebih dahulu, menampilkan proses pemuatan, lalu memuat `app.html`
- `settings/app.html`: halaman lengkap yang berisi tampilan, pengaturan, dan logika halaman konfigurasi
- `settings/assets/`: gambar yang digunakan halaman pengaturan
- `timeline/`: dokumentasi integrasi Timeline dan contoh format pin

*Notes:*

- *`data/kalender.json`: a stable index listing the data files read by the app*
- *`data/katalog/*.json`: event data grouped by category*
- *`data/keterangan.json`: long descriptions linked by event ID*
- *`settings/index.html`: a lightweight shell loaded first to show the loading state and then load `app.html`*
- *`settings/app.html`: the complete settings page containing its interface, configuration, and page logic*
- *`settings/assets/`: images used by the settings page*
- *`timeline/`: Timeline integration documentation and a pin-format example*

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

Aturan tahun:

- `th:2026` berarti peristiwa hanya berlaku pada tahun 2026
- `th:0` berarti peristiwa berulang setiap tahun
- field `ut` tidak digunakan
- field `th` tidak boleh dikosongkan
- peristiwa tidak berulang wajib memakai tahun sebenarnya

*Year rules:*

- *`th:2026` means the event applies only to 2026*
- *`th:0` means the event repeats every year*
- *the `ut` field is not used*
- *the `th` field must not be left blank*
- *a non-recurring event must use its actual year*

### Arti field

*Field meanings*

| Field | Kelompok | Arti | *Meaning* |
|---|---|---|---|
| `th` | Waktu | Tahun; `0` untuk berulang setiap tahun | *Year; use `0` for annual recurrence* |
| `bm` | Waktu | Bulan mulai | *Start month* |
| `bs` | Waktu | Bulan selesai | *End month* |
| `hm` | Waktu | Hari mulai | *Start day* |
| `hs` | Waktu | Hari selesai | *End day* |
| `tp` | Klasifikasi | Tipe peristiwa | *Event type* |
| `id` | Identitas | ID unik dan stabil | *Stable unique ID* |
| `jd` | Tampilan | Judul pendek, maksimal 39 karakter | *Short title, maximum 39 characters* |

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

## Aturan ID Peristiwa

*Event ID Rules*

ID tidak disusun dari tipe, bulan, dan hari. Tanggal peristiwa tentatif dapat berubah setiap tahun, beberapa peristiwa dapat berada pada tanggal yang sama, dan susunan tanpa angka tetap dapat menghasilkan ID yang bertabrakan.

*IDs are not composed from the type, month, and day. Tentative event dates can change each year, multiple events can share the same date, and variable-length date combinations can produce collisions.*

Gunakan rumus:

```text
id = (tp × 4000) + nomor urut kategori
```

Rentang awal:

| Tipe | Rentang ID |
|---:|---:|
| 1 | 4001–7999 |
| 2 | 8001–11999 |
| 3 | 12001–15999 |
| 4 | 16001–19999 |
| 5 | 20001–23999 |
| 6 | 24001–27999 |

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

Satu sheet Excel digunakan untuk satu kategori peristiwa, misalnya Akademik, Hijriah, Internasional, Libur Daerah, Libur Nasional, atau Umum. Baris `1` digunakan sebagai judul kolom dan pengisian data dimulai dari baris `2`.

*One Excel sheet is used for one event category, such as Academic, Hijri, International, Regional Holiday, National Holiday, or General. Row `1` contains the column headings and data entry begins on row `2`.*

### Susunan kolom

*Column layout*

| Kolom | Field | Nama | Isi |
|---|---|---|---|
| A | `th` | Tahun | Isi `0` untuk berulang atau tahun sebenarnya |
| B | `bm` | Bulan Mulai | Angka 1–12 |
| C | `bs` | Bulan Selesai | Angka 1–12 |
| D | `hm` | Hari Mulai | Angka 1–31 |
| E | `hs` | Hari Selesai | Angka 1–31 |
| F | `tp` | Tipe | Angka 1–6 |
| G | — | Nomor Urut ID | Angka 1–3999, tidak boleh dipakai ulang |
| H | `id` | ID | Dibentuk otomatis dari tipe dan nomor urut |
| I | `jd` | Judul Pendek | Maksimal 39 karakter |
| J | `ket` | Keterangan | Opsional; boleh dikosongkan |
| K | — | JSON Peristiwa | Dibentuk otomatis |
| L | — | JSON Keterangan | Dibentuk otomatis jika kolom J terisi |

*Columns A–J are the maintained source data. Columns H, K, and L use formulas to generate the stable ID, event JSON, and description JSON.*

### Formula ID pada H2

*ID formula in H2*

Versi pemisah titik koma:

```excel
=IF(OR(F2="";G2="");"";F2*4000+G2)
```

Versi pemisah koma:

```excel
=IF(OR(F2="",G2=""),"",F2*4000+G2)
```

Salin formula H2 ke bawah sampai baris data terakhir.

*Copy the H2 formula down to the last data row.*

### Formula JSON peristiwa pada K2

*Event JSON formula in K2*

Versi pemisah titik koma:

```excel
=IF(OR(A2="";B2="";C2="";D2="";E2="";F2="";H2="";I2="");"";IF(LEN(I2)>39;"ERROR: Judul lebih dari 39 karakter";"{""th"":"&A2&",""bm"":"&B2&",""bs"":"&C2&",""hm"":"&D2&",""hs"":"&E2&",""tp"":"&F2&",""id"":"&H2&",""jd"":"""&SUBSTITUTE(SUBSTITUTE(I2;CHAR(92);CHAR(92)&CHAR(92));CHAR(34);CHAR(92)&CHAR(34))&"""}"&IF(COUNTIF(H3:H$1000;"<>")>0;",";"")))
```

Versi pemisah koma:

```excel
=IF(OR(A2="",B2="",C2="",D2="",E2="",F2="",H2="",I2=""),"",IF(LEN(I2)>39,"ERROR: Judul lebih dari 39 karakter","{""th"":"&A2&",""bm"":"&B2&",""bs"":"&C2&",""hm"":"&D2&",""hs"":"&E2&",""tp"":"&F2&",""id"":"&H2&",""jd"":"""&SUBSTITUTE(SUBSTITUTE(I2,CHAR(92),CHAR(92)&CHAR(92)),CHAR(34),CHAR(92)&CHAR(34))&"""}"&IF(COUNTIF(H3:H$1000,"<>")>0,",","")))
```

Formula tersebut:

- menolak baris dengan field wajib yang kosong
- menerima `th:0` sebagai peristiwa tahunan
- memeriksa batas judul 39 karakter
- mengamankan tanda `\` dan tanda kutip dalam judul
- menambahkan koma jika masih ada data di bawah baris aktif

*The formula rejects incomplete rows, accepts `th:0` as an annual event, checks the 39-character title limit, escapes backslashes and quotation marks, and adds a comma when more records follow.*

### Formula JSON keterangan pada L2

*Description JSON formula in L2*

Versi pemisah titik koma:

```excel
=IF(OR(H2="";J2="");"";""""&H2&""":{""ket"":"""&SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(J2;CHAR(92);CHAR(92)&CHAR(92));CHAR(34);CHAR(92)&CHAR(34));CHAR(13);"");CHAR(10);CHAR(92)&"n")&"""}"&IF(COUNTIF(J3:J$1000;"<>")>0;",";""))
```

Versi pemisah koma:

```excel
=IF(OR(H2="",J2=""),"",""""&H2&""":{""ket"":"""&SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(J2,CHAR(92),CHAR(92)&CHAR(92)),CHAR(34),CHAR(92)&CHAR(34)),CHAR(13),""),CHAR(10),CHAR(92)&"n")&"""}"&IF(COUNTIF(J3:J$1000,"<>")>0,",",""))
```

Formula tersebut:

- menghasilkan baris hanya jika keterangan tersedia
- menggunakan ID yang sama dengan peristiwa
- mengamankan tanda `\`, tanda kutip, dan pindah baris
- tidak membutuhkan sheet terpisah untuk keterangan

*The formula generates a row only when a description is available, uses the same event ID, escapes special characters and line breaks, and does not require a separate description sheet.*

Formula di atas memeriksa data sampai baris `1000`. Jika satu kategori melebihi baris tersebut, sesuaikan batas `$1000`.

*The formulas check records through row `1000`. Increase the `$1000` limit if a category grows beyond that row.*

---

## Prosedur Pengisian dan Pembaruan

*Data Entry and Update Procedure*

1. Buka sheet kategori yang akan diperbarui
2. Isi data mulai dari baris `2`
3. Isi `th` dengan `0` untuk peristiwa berulang atau dengan tahun sebenarnya untuk peristiwa satu tahun
4. Isi nomor urut ID baru pada kolom G
5. Salin formula H2, K2, dan L2 ke seluruh baris data
6. Periksa apakah kolom K menampilkan JSON dan bukan pesan error
7. Salin hasil kolom K
8. Buka file JSON kategori yang sesuai
9. Tempel hasil kolom K di antara tanda `[` dan `]` pada bagian `peristiwa`
10. Jika terdapat keterangan, salin hasil kolom L
11. Buka `data/keterangan.json` dan tempel hasil kolom L ke dalam objek `keterangan`
12. Simpan perubahan
13. Pada halaman GitHub, periksa tab perubahan, lalu pilih **Commit changes**

*Open the required category sheet, enter data from row `2`, fill the formulas down, copy the generated event JSON into the relevant category file, copy optional description JSON into `data/keterangan.json`, save the changes, review them on GitHub, and select **Commit changes**.*

### Lokasi penempelan JSON peristiwa

*Event JSON paste location*

```text
{
  "skema": 3,
  "kategori": "internasional",
  "versi": "1",
  "peristiwa": [
    TEMPEL HASIL KOLOM K DI SINI
  ]
}
```

Contoh setelah ditempel:

*Example after pasting:*

```json
{
  "skema": 3,
  "kategori": "internasional",
  "versi": "1",
  "peristiwa": [
    {"th":0,"bm":1,"bs":1,"hm":4,"hs":4,"tp":6,"id":24001,"jd":"World Braille Day"},
    {"th":0,"bm":1,"bs":1,"hm":24,"hs":24,"tp":6,"id":24002,"jd":"International Day of Education"}
  ]
}
```

### Lokasi penempelan JSON keterangan

*Description JSON paste location*

```text
{
  "skema": 1,
  "keterangan": {
    TEMPEL HASIL KOLOM L DI SINI
  }
}
```

Contoh setelah ditempel:

*Example after pasting:*

```json
{
  "skema": 1,
  "keterangan": {
    "24001":{"ket":"World Braille Day raises awareness of the importance of Braille."},
    "24002":{"ket":"International Day of Education recognizes the role of education in peace and development."}
  }
}
```

Jika kolom J kosong, kolom L juga kosong dan ID tersebut tidak perlu dimasukkan ke `keterangan.json`. Aplikasi cukup menampilkan judul pendek.

*When column J is empty, column L also remains empty and the ID does not need to be included in `keterangan.json`. The app displays only the short title.*

---

## Pembagian Data Tetap dan Tentatif

*Fixed and Tentative Data*

Peristiwa tetap memakai:

```json
"th":0
```

Peristiwa tentatif atau yang hanya berlaku pada satu tahun memakai tahun sebenarnya:

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
- peringatan internasional dengan tanggal tetap

PebbleKit JS hanya mengirim peristiwa `th:0` dan peristiwa yang tahunnya sesuai dengan tahun aktif. Data untuk tahun lain tetap berada dalam repositori dan tidak dikirim seluruhnya ke memori watch.

*PebbleKit JS sends only events with `th:0` and events matching the active year. Records for other years remain in the repository and are not all transferred to watch memory.*

---

## Pemeliharaan File

*File Maintenance*

Nama file aktif tidak memuat nomor versi. Setiap perbaikan dilakukan dengan mengedit isi file yang sama.

*Active filenames do not contain version numbers. Every update edits the content of the same file.*

Contoh:

```text
data/katalog/umum.json
data/keterangan.json
settings/app.html
settings/index.html
```

Nomor versi dicatat melalui pesan commit, tag Git, dan metadata `versi` di dalam JSON jika diperlukan.

*Version numbers are recorded through commit messages, Git tags, and the JSON `versi` metadata when needed.*

Jangan mengunggah file baru hanya untuk mengganti versi dari file aktif yang sama.

*Do not upload a new file only to replace the version of the same active file.*

---

## Halaman Pengaturan

*Settings Page*

Halaman pengaturan terdiri atas:

```text
settings/index.html
settings/app.html
settings/assets/
```

`settings/index.html` adalah shell ringan. File ini dibuka pertama kali, menampilkan status pemuatan, lalu mengambil dan menampilkan isi `settings/app.html`.

*`settings/index.html` is a lightweight shell. It opens first, displays the loading state, and then retrieves and renders `settings/app.html`.*

`settings/app.html` adalah halaman lengkap. File ini berisi tampilan pengaturan, teks, tombol, modal, penyimpanan lokal, serta logika komunikasi halaman dengan PebbleKit JS.

*`settings/app.html` is the complete page. It contains the settings interface, text, buttons, modals, local storage, and page communication logic with PebbleKit JS.*

Pemisahan ini mempertahankan URL pengaturan yang stabil dan mengurangi kemungkinan halaman tampak kosong ketika isi lengkap sedang dimuat.

*This separation keeps the settings URL stable and reduces the chance of showing a blank page while the full content is loading.*

---

## Timeline

Folder `timeline/` memuat dokumentasi integrasi Timeline dan contoh struktur pin. Data Timeline menggunakan sumber peristiwa yang sama dari JSON kategori sehingga tidak memerlukan pemeliharaan daftar kalender kedua.

*The `timeline/` folder contains Timeline integration documentation and a pin-structure example. Timeline data uses the same category JSON source, avoiding maintenance of a second calendar list.*

---

## Status Pengembangan

*Development Status*

Aplikasi masih berada pada tahap pengembangan internal dan belum menjadi rilis publik.

*The app is still under internal development and has not yet reached its public release.*