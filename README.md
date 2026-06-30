# Kalender Indonesia untuk Pebble

*Kalender Indonesia for Pebble*

Kalender Indonesia adalah aplikasi kalender dan peristiwa untuk Pebble smartwatch. Repositori ini menyimpan data kalender publik, halaman pengaturan, aset web, contoh pengelolaan data, dan struktur integrasi Timeline.

*Kalender Indonesia is a calendar and events app for Pebble smartwatches. This repository stores public calendar data, configuration pages, web assets, a data-management example, and the Timeline integration structure.*

---

## A. Struktur Repositori

*Repository Structure*

```text
kalender-indonesia-pebble/
├── README.md
├── data/
│   ├── kalender.json
│   ├── keterangan.json
│   ├── examples/
│   │   └── international.xlsx
│   └── catalogs/
│       ├── libur-nasional.json
│       ├── libur-daerah.json
│       ├── hijriah.json
│       ├── akademik.json
│       ├── umum.json
│       └── international.json
├── settings/
│   ├── indeks.html
│   ├── aplikasi.html
│   └── assets/
│       ├── banner-besar.jpg
│       └── banner-kecil.jpg
└── timeline/
    ├── README.md
    └── template-pin.json
```

Keterangan:

- `data/kalender.json`: indeks tetap yang mencantumkan file data yang dibaca aplikasi
- `data/catalogs/*.json`: data peristiwa menurut kategori
- `data/keterangan.json`: keterangan panjang berdasarkan ID peristiwa
- `data/examples/international.xlsx`: contoh pengelolaan kategori Internasional dengan Excel
- `settings/indeks.html`: halaman pembuka pengaturan
- `settings/aplikasi.html`: halaman lengkap pengaturan
- `settings/assets/`: gambar yang digunakan halaman pengaturan
- `timeline/`: dokumentasi Timeline dan contoh format pin

*Notes:*

- *`data/kalender.json`: a stable index listing the data files read by the app*
- *`data/catalogs/*.json`: event data grouped by category*
- *`data/keterangan.json`: long descriptions linked by event ID*
- *`data/examples/international.xlsx`: an Excel example for managing the International category*
- *`settings/indeks.html`: the settings entry page*
- *`settings/aplikasi.html`: the complete settings page*
- *`settings/assets/`: images used by the settings page*
- *`timeline/`: Timeline documentation and a pin-format example*

---

## B. Indeks Data Utama

*Main Data Index*

`data/kalender.json` menjadi satu alamat tetap yang dibaca aplikasi. File ini mencantumkan seluruh file kategori dan file keterangan.

*`data/kalender.json` is the stable data address read by the app. It lists all category files and the description file.*

```json
{
  "skema": 3,
  "berkas": [
    "catalogs/libur-nasional.json",
    "catalogs/libur-daerah.json",
    "catalogs/hijriah.json",
    "catalogs/akademik.json",
    "catalogs/umum.json",
    "catalogs/international.json"
  ],
  "keterangan": "keterangan.json"
}
```

Field `skema` menandai kontrak struktur data. Nomornya berubah hanya jika nama field wajib atau bentuk data berubah, bukan ketika tanggal, judul, subtipe, atau keterangan diperbarui.

*The `skema` field identifies the data-structure contract. Its number changes only when required field names or the data shape change, not when dates, titles, subtypes, or descriptions are updated.*

---

## C. Kategori Peristiwa

*Event Categories*

| Tipe | Kategori | Nilai `kategori` | File | *Category* |
|---:|---|---|---|---|
| 1 | Libur Nasional | `libur-nasional` | `libur-nasional.json` | *National Holiday* |
| 2 | Libur Daerah | `libur-daerah` | `libur-daerah.json` | *Regional Holiday* |
| 3 | Hijriah | `hijriah` | `hijriah.json` | *Hijri* |
| 4 | Akademik | `akademik` | `akademik.json` | *Academic* |
| 5 | Umum | `umum` | `umum.json` | *General* |
| 6 | Internasional | `international` | `international.json` | *International* |
| 7 | Pribadi | — | — | *Personal* |

Tipe `7` tidak disimpan dalam repositori publik. Peristiwa pribadi dikelola melalui halaman pengaturan pada perangkat pengguna.

*Type `7` is not stored in the public repository. Personal events are managed through the settings page on the user's device.*

---

## D. Struktur File Kategori

*Category File Structure*

Setiap file kategori hanya memuat nilai `kategori` dan array `peristiwa`. Metadata versi tidak disimpan dalam file kategori karena perubahan isi tercatat dalam riwayat commit.

*Each category file contains only the `kategori` value and the `peristiwa` array. Version metadata is not stored in category files because content changes are recorded in commit history.*

```json
{
  "kategori": "international",
  "peristiwa": []
}
```

*Array `peristiwa` diisi menggunakan format pada Bagian E.*

*The `peristiwa` array is filled using the format described in Section E.*

Field `skema` tidak diulang pada setiap file kategori karena hanya disimpan pada `data/kalender.json`.

*The `skema` field is not repeated in category files because it is stored only in `data/kalender.json`.*

---

## E. Format Data Peristiwa

*Event Data Format*

### Arti field

*Field Meanings*

| Field | Wajib | Arti | *Meaning* |
|---|---|---|---|
| `th` | Ya | Tahun; `0` untuk berulang setiap tahun | *Year; use `0` for annual recurrence* |
| `bm` | Ya | Bulan mulai | *Start month* |
| `bs` | Ya | Bulan selesai | *End month* |
| `hm` | Ya | Hari mulai | *Start day* |
| `hs` | Ya | Hari selesai | *End day* |
| `tp` | Ya | Tipe peristiwa | *Event type* |
| `sub` | Tidak | Subtipe untuk pengelompokan, misalnya `pbb`, `ksa`, `ugm`, atau `itb` | *Optional subtype for grouping, such as `pbb`, `ksa`, `ugm`, or `itb`* |
| `id` | Ya | ID unik dan stabil | *Stable unique ID* |
| `jd` | Ya | Judul pendek, maksimal 39 karakter | *Short title, maximum 39 characters* |

Urutan field baku:

```text
th, bm, bs, hm, hs, tp, sub, id, jd
```

Field `sub` bersifat opsional. Jika tidak digunakan, field tersebut tidak perlu ditulis.

*The `sub` field is optional. When it is not used, the field may be omitted.*

### Peristiwa untuk satu tahun

*Event for a specific year*

```json
{"th":2026,"bm":6,"bs":6,"hm":1,"hs":1,"tp":1,"id":4001,"jd":"Hari Lahir Pancasila"}
```

### Peristiwa berulang setiap tahun dengan subtipe

*Annually recurring event with a subtype*

```json
{"th":0,"bm":1,"bs":1,"hm":24,"hs":24,"tp":6,"sub":"pbb","id":24002,"jd":"International Day of Education"}
```

Aturan:

- `th:2026` berarti peristiwa hanya berlaku pada tahun 2026
- `th:0` atau tahun kosong pada Excel berarti peristiwa berulang setiap tahun
- bulan mulai dan hari mulai wajib berisi angka valid serta tidak boleh `0`
- bulan selesai kosong atau `0` pada Excel mengikuti bulan mulai
- hari selesai kosong atau `0` pada Excel mengikuti hari mulai
- jika waktu selesai tidak diisi, peristiwa dianggap berlangsung satu hari
- `sub` ditulis dengan huruf kecil, tanpa spasi, dan digunakan secara konsisten
- `id` tidak berubah hanya karena tanggal, subtipe, atau judul diperbarui
- `jd` maksimal 39 karakter
- objek terakhir dalam array `peristiwa` tidak diakhiri koma

*Rules:*

- *`th:2026` means the event applies only to 2026*
- *`th:0`, or an empty year in Excel, means the event repeats every year*
- *the start month and start day must contain valid numbers and cannot be `0`*
- *an empty or `0` end month in Excel uses the start month*
- *an empty or `0` end day in Excel uses the start day*
- *when the end values are omitted, the event is treated as a one-day event*
- *`sub` uses lowercase text without spaces and must be used consistently*
- *an `id` does not change only because its date, subtype, or title is updated*
- *`jd` is limited to 39 characters*
- *the final object in the `peristiwa` array must not end with a comma*

---

## F. Aturan ID Peristiwa

*Event ID Rules*

ID tidak disusun dari tipe, bulan, dan hari karena tanggal tentatif dapat berubah dan beberapa peristiwa dapat berada pada tanggal yang sama.

*IDs are not composed from the type, month, and day because tentative dates can change and multiple events can share the same date.*

Gunakan rumus:

```text
id = (tp × 4000) + nomor urut kategori
```

| Tipe | Kategori | Rentang ID |
|---:|---|---:|
| 1 | Libur Nasional | 4001–7999 |
| 2 | Libur Daerah | 8001–11999 |
| 3 | Hijriah | 12001–15999 |
| 4 | Akademik | 16001–19999 |
| 5 | Umum | 20001–23999 |
| 6 | Internasional | 24001–27999 |

Aturan nomor urut:

- mulai dari `1` pada setiap kategori
- gunakan nomor baru yang belum pernah dipakai
- jangan mengubah ID lama ketika menyisipkan peristiwa
- jangan menggunakan ulang ID yang sudah dihapus
- rentang `28000–29999` disimpan sebagai cadangan
- ID peristiwa publik tetap berada di bawah `30000`

*Sequence-number rules:*

- *start from `1` in each category*
- *use a new number that has never been assigned*
- *do not renumber existing IDs when inserting an event*
- *do not reuse a deleted ID*
- *the `28000–29999` range is reserved*
- *public event IDs remain below `30000`*

---

## G. Mengelola Data dengan Excel

*Managing Data with Excel*

Satu sheet Excel digunakan untuk satu kategori dengan urutan tipe `1` sampai `6`. Baris `1` digunakan sebagai judul kolom dan data dimulai dari baris `2`.

*One Excel sheet is used for each category in type order from `1` to `6`. Row `1` contains the headings and data begins on row `2`.*

Agar dapat digabung otomatis melalui Power Query, ubah setiap rentang kategori menjadi Excel Table dan gunakan nama berikut:

| Kategori | Nama Excel Table |
|---|---|
| Libur Nasional | `tblLiburNasional` |
| Libur Daerah | `tblLiburDaerah` |
| Hijriah | `tblHijriah` |
| Akademik | `tblAkademik` |
| Umum | `tblUmum` |
| Internasional | `tblInternational` |

*Convert each category range into an Excel Table using the names listed above so Power Query can combine the description rows automatically.*

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
| G | `sub` | Subtipe | Opsional; gunakan huruf kecil tanpa spasi |
| H | — | Nomor Urut ID | Angka 1–3999 dan tidak boleh dipakai ulang |
| I | `id` | ID | Dibentuk otomatis |
| J | `jd` | Judul Pendek | Maksimal 39 karakter |
| K | `ket` | Keterangan | Opsional |
| L | — | JSON Peristiwa | Dibentuk otomatis |
| M | — | JSON Keterangan | Dibentuk otomatis jika kolom K terisi |

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

### Formula JSON peristiwa pada L2

*Event JSON Formula in L2*

Versi pemisah titik koma:

```excel
=IF(OR(B2="";B2=0;D2="";D2=0;F2="";I2="";J2="");"ERROR: Data wajib belum lengkap";IF(OR(B2<1;B2>12;D2<1;D2>31;AND(C2<>"";C2<>0;OR(C2<1;C2>12));AND(E2<>"";E2<>0;OR(E2<1;E2>31)));"ERROR: Bulan atau hari tidak valid";IF(LEN(J2)>39;"ERROR: Judul lebih dari 39 karakter";"{""th"":"&IF(A2="";0;A2)&",""bm"":"&B2&",""bs"":"&IF(OR(C2="";C2=0);B2;C2)&",""hm"":"&D2&",""hs"":"&IF(OR(E2="";E2=0);D2;E2)&",""tp"":"&F2&IF(G2="";"";",""sub"":"""&SUBSTITUTE(SUBSTITUTE(LOWER(G2);CHAR(92);CHAR(92)&CHAR(92));CHAR(34);CHAR(92)&CHAR(34))&"""")&",""id"":"&I2&",""jd"":"""&SUBSTITUTE(SUBSTITUTE(J2;CHAR(92);CHAR(92)&CHAR(92));CHAR(34);CHAR(92)&CHAR(34))&"""}"&IF(COUNTIF(I3:I$1000;"<>")>0;",";""))))
```

Versi pemisah koma:

```excel
=IF(OR(B2="",B2=0,D2="",D2=0,F2="",I2="",J2=""),"ERROR: Data wajib belum lengkap",IF(OR(B2<1,B2>12,D2<1,D2>31,AND(C2<>"",C2<>0,OR(C2<1,C2>12)),AND(E2<>"",E2<>0,OR(E2<1,E2>31))),"ERROR: Bulan atau hari tidak valid",IF(LEN(J2)>39,"ERROR: Judul lebih dari 39 karakter","{""th"":"&IF(A2="",0,A2)&",""bm"":"&B2&",""bs"":"&IF(OR(C2="",C2=0),B2,C2)&",""hm"":"&D2&",""hs"":"&IF(OR(E2="",E2=0),D2,E2)&",""tp"":"&F2&IF(G2="","",",""sub"":"""&SUBSTITUTE(SUBSTITUTE(LOWER(G2),CHAR(92),CHAR(92)&CHAR(92)),CHAR(34),CHAR(92)&CHAR(34))&"""")&",""id"":"&I2&",""jd"":"""&SUBSTITUTE(SUBSTITUTE(J2,CHAR(92),CHAR(92)&CHAR(92)),CHAR(34),CHAR(92)&CHAR(34))&"""}"&IF(COUNTIF(I3:I$1000,"<>")>0,",",""))))
```

Formula tersebut:

- mengubah tahun kosong menjadi `th:0`
- melengkapi waktu selesai yang kosong dari waktu mulai
- menolak bulan atau hari mulai yang kosong atau `0`
- menambahkan `sub` hanya jika kolom G terisi
- mengubah nilai `sub` menjadi huruf kecil
- memeriksa judul maksimal 39 karakter
- mengamankan tanda `\` dan tanda kutip
- menambahkan koma jika masih ada data setelah baris aktif

*The formula converts an empty year to `th:0`, fills missing end values from the start values, rejects an empty or zero start month or day, adds `sub` only when column G is filled, converts the subtype to lowercase, validates the title length, escapes special characters, and adds a comma when more records follow.*

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

Salin formula I2, L2, dan M2 sampai baris terakhir. Formula memeriksa data sampai baris `1000`; ubah batas tersebut jika diperlukan.

*Copy the I2, L2, and M2 formulas to the final row. The formulas check records through row `1000`; increase the limit when necessary.*

---

## H. Menyalin JSON Peristiwa

*Copying Event JSON*

1. Buka sheet kategori yang akan diperbarui
2. Isi data mulai dari baris `2`
3. Salin formula I2, L2, dan M2 ke seluruh baris data
4. Pastikan kolom L menampilkan JSON dan bukan pesan error
5. Salin hasil kolom L
6. Buka file JSON kategori yang sesuai pada GitHub (contoh international.json)
7. Pilih **Edit this file**
8. Tempel hasil di antara tanda `[` dan `]` pada bagian `peristiwa`
9. Pastikan objek terakhir tidak memiliki koma
10. Pilih **Commit changes** atau ajukan pull request jika tidak memiliki akses tulis

*Enter data from row `2`, fill the formulas down, copy column L, edit the matching category JSON file on GitHub, paste the generated rows into the `peristiwa` array, verify the final comma, and commit the change or submit a pull request.*

```text
{
  "kategori": "international",
  "peristiwa": [
    TEMPEL-HASIL-KOLOM-L-DI-SINI
  ]
}
```

Contoh:

*Example:*

```json
{
  "kategori": "international",
  "peristiwa": [
    {"th":0,"bm":1,"bs":1,"hm":4,"hs":4,"tp":6,"sub":"pbb","id":24001,"jd":"World Braille Day"},
    {"th":0,"bm":1,"bs":1,"hm":24,"hs":24,"tp":6,"sub":"pbb","id":24002,"jd":"International Day of Education"}
  ]
}
```

---

## I. Menyatukan Keterangan dengan Power Query

*Merging Descriptions with Power Query*

Power Query menggabungkan kolom `JSON Keterangan` dari seluruh Excel Table ke satu sheet `Keterangan`. Hasil dapat diperbarui kembali melalui **Refresh All** setelah data kategori berubah.

*Power Query combines the `JSON Keterangan` column from all Excel Tables into one `Keterangan` sheet. The result can be updated through **Refresh All** after category data changes.*

Langkah:

1. Pastikan setiap sheet kategori sudah menjadi Excel Table dengan nama pada Bagian G
2. Pilih **Data → Get Data → From Other Sources → Blank Query**
3. Buka **Advanced Editor**
4. Ganti seluruh isi dengan kode berikut
5. Pilih **Close & Load To...**
6. Muat sebagai tabel pada sheet baru bernama `Keterangan`
7. Salin isi kolom `Hasil` tanpa header ke `data/keterangan.json`

*Ensure that every category sheet is an Excel Table, create a Blank Query, paste the code below into Advanced Editor, load the result into a `Keterangan` sheet, and copy the `Hasil` values without the header into `data/keterangan.json`.*

```powerquery
let
    Urutan = {
        "tblLiburNasional",
        "tblLiburDaerah",
        "tblHijriah",
        "tblAkademik",
        "tblUmum",
        "tblInternational"
    },
    Sumber = Excel.CurrentWorkbook(),
    PilihTabel = Table.SelectRows(
        Sumber,
        each List.Contains(Urutan, [Name])
    ),
    TambahUrutan = Table.AddColumn(
        PilihTabel,
        "Urutan",
        each List.PositionOf(Urutan, [Name]),
        Int64.Type
    ),
    Urutkan = Table.Sort(
        TambahUrutan,
        {{"Urutan", Order.Ascending}}
    ),
    AmbilKolom = List.Transform(
        Urutkan[Content],
        each Table.SelectColumns(_, {"JSON Keterangan"})
    ),
    Gabungkan = Table.Combine(AmbilKolom),
    HapusKosong = Table.SelectRows(
        Gabungkan,
        each [JSON Keterangan] <> null
            and Text.Trim(Text.From([JSON Keterangan])) <> ""
    ),
    HapusKomaLama = Table.TransformColumns(
        HapusKosong,
        {{"JSON Keterangan", each Text.TrimEnd(Text.From(_), {","}), type text}}
    ),
    TambahIndeks = Table.AddIndexColumn(
        HapusKomaLama,
        "Index",
        0,
        1,
        Int64.Type
    ),
    JumlahBaris = Table.RowCount(TambahIndeks),
    TambahKoma = Table.AddColumn(
        TambahIndeks,
        "Hasil",
        each [JSON Keterangan]
            & (if [Index] < JumlahBaris - 1 then "," else ""),
        type text
    ),
    HasilAkhir = Table.SelectColumns(
        TambahKoma,
        {"Hasil"}
    )
in
    HasilAkhir
```

Lokasi penempelan:

*Paste Location:*

```text
{
  "keterangan": {
    TEMPEL-HASIL-POWER-QUERY-DI-SINI
  }
}
```

Jika suatu peristiwa tidak memiliki keterangan, ID tersebut tidak perlu dimasukkan ke `keterangan.json`.

*When an event has no description, its ID does not need to be included in `keterangan.json`.*

---

## J. Pemeliharaan File

*File Maintenance*

Nama file aktif tidak memuat nomor versi. Setiap pembaruan dilakukan dengan mengedit isi file yang sama.

*Active filenames do not contain version numbers. Every update edits the content of the same file.*

Benar:

```text
data/catalogs/umum.json
data/keterangan.json
settings/aplikasi.html
```

Salah:

```text
data/catalogs/umum-v2.json
data/catalogs/umum-final.json
data/keterangan-revisi-2027.json
settings/aplikasi-v12-final.html
```

Riwayat perubahan tersedia melalui commit Git. Jangan membuat file baru hanya untuk mengganti versi dari file aktif yang sama.

*Change history is available through Git commits. Do not create a new file only to replace the version of the same active file.*

---

## K. Timeline

*Timeline*

Folder `timeline/` masih berada dalam tahap pengembangan. Data Timeline dirancang menggunakan sumber peristiwa yang sama dari JSON kategori agar tidak memerlukan pemeliharaan daftar kalender kedua.

*The `timeline/` folder is still under development. Timeline data is designed to use the same category JSON source, avoiding maintenance of a second calendar list.*


---

## L. Dukung Pengembang

*Support the Developer*

Pengembangan Kalender Indonesia untuk Pebble dilakukan secara mandiri. Dukungan dapat diberikan melalui platform berikut:

- [Ko-fi](https://ko-fi.com/ramabary) for international support
- [Saweria](https://saweria.co/ramabary) untuk dukungan dari Indonesia

Dukungan bersifat sukarela dan membantu pengembangan fitur, pemeliharaan data, serta pengujian pada perangkat Pebble.

Terima kasih atas segala dukungan terhadap aplikasi ini! :)

*Kalender Indonesia for Pebble is independently developed. Voluntary support helps fund feature development, data maintenance, and testing on Pebble devices.*

*Thank you for supporting this application! :)*
