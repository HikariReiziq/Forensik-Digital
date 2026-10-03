# Digital Forensics Investigation

Repository ini berisi dokumentasi proses investigasi forensik digital yang dilakukan oleh **Kelompok 4** terhadap barang bukti digital dari kelompok target dalam kegiatan **Forensik Digital**.

Investigasi dilakukan untuk mengidentifikasi artefak digital, menemukan keyword atau flag yang disembunyikan, serta mendokumentasikan proses dan hasil pemeriksaan dari setiap target.

# Anggota Kelompok 4

| No. | Nama Lengkap |
|---|---|
| 1. | Imam Mahmud Dalil Fauzan|
| 2. | M. Hikari Reiziq Rakhmadinta |
| 3. | Ivan Syarifuddin |
| 4. | Nayyara Ashila |
| 5. | Rafika Az Zahra Kusumastuti |

# Summary Status Investigasi

| Target | Status | Documentation |
|---|---:|---|
| Kelompok 5 | Solved | [View](./investigations/kelompok%205/) |
| Kelompok 1 | In Progress | — |
| Kelompok 2 | Solved | [View](./investigations/kelompok%202/) |
| Kelompok 3 | In Progress | — |


## Tools

Tool yang kami gunakan dalam proses investigasi:

- **Autopsy 4.23.1** - forensic examination dan analysis
- **Strings** - extract strings dari file
- **xxd** - extract hex dump dari file

Tool tambahan akan dicantumkan pada dokumentasi masing-masing investigasi apabila diperlukan.

# Repository Structure

```text
Forensik-Digital/
│
├── README.md
│
└── investigations/
    │
    ├── kelompok 1/
    │   └── README.md
    │
    ├── kelompok 2/
    │   ├── README.md
    │   └── documentations/
    │       ├── 01-create-case.png
    │       ├── 02-data-source.png
    │       ├── 03-configure-ingest.png
    │       ├── 04-file-types.png
    │       ├── 05-mime-type.png
    │       ├── 06-konmed-5.png
    │       └── 07-flag.png
    │
    ├── kelompok 3/
    │   └── README.md
    │
    └── kelompok 5/
        ├── README.md
        └── src/
            ├── tool_strings.jpeg
            └── tool_xxd.jpeg
```

Setiap folder kelompok berisi dokumentasi investigasi secara terpisah agar proses pemeriksaan dan hasil dari masing-masing target dapat ditelusuri dengan mudah.

# Investigation Cases

## Barang Bukti Kelompok 1

**Status:** In Progress

Dokumentasi investigasi:

[→ Buka Investigasi Kelompok 1](./investigations/kelompok%201/)

---

## Barang Bukti Kelompok 2

**Status:** Solved

Investigasi terhadap Kelompok 2 berhasil menemukan file `KONMED 5.png` yang memiliki ketidaksesuaian antara ekstensi `.png` dan MIME type `text/plain`.

Pemeriksaan lebih lanjut terhadap file tersebut berhasil menemukan flag yang disembunyikan pada barang bukti.

[→ Buka Investigasi Kelompok 2](./investigations/kelompok%202/)

---

## Barang Bukti Kelompok 3

**Status:** In Progress

Dokumentasi investigasi:

[→ Buka Investigasi Kelompok 3](./investigations/kelompok%203/)

---

## Barang Bukti Kelompok 5

**Status:** Solved

Pencarian Flag pada file `opung_archive.jpg` telah ditemukan menggunakan 2 tools, yaitu tools `strings` dan tools `xxd`.

[→ Buka Investigasi Kelompok 5](./investigations/kelompok%205/)

---

# Evidence Handling

Integritas barang bukti dijaga selama proses investigasi.

Barang bukti asli tidak dimasukkan ke dalam repository ini. Proses analisis dilakukan menggunakan salinan kerja (*working copy*) sehingga barang bukti asli tetap dipertahankan dan tidak mengalami perubahan selama pemeriksaan.

Repository ini hanya berisi dokumentasi investigasi, seperti:

- laporan dan penjelasan proses pemeriksaan;
- screenshot tahapan analisis;
- hasil temuan;
- serta dokumentasi pendukung lainnya.


## Documentation

Dokumentasi lengkap dari setiap investigasi dapat diakses melalui folder:

[**→ Investigations**](./investigations/)

Setiap target memiliki README masing-masing yang memuat tahapan pemeriksaan, dokumentasi screenshot, hasil temuan, dan kesimpulan investigasi.

---