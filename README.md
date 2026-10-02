# Digital Forensics Investigation

Repository ini berisi dokumentasi proses investigasi forensik digital yang dilakukan oleh **Kelompok 4** terhadap barang bukti digital dari kelompok target dalam kegiatan **Forensik Digital**.

Investigasi dilakukan untuk mengidentifikasi artefak digital yang relevan, menemukan keyword atau flag yang disembunyikan, serta mendokumentasikan proses dan hasil pemeriksaan dari setiap target.

---

## Investigator

**Kelompok 4**

Repository ini digunakan sebagai dokumentasi hasil investigasi dari seluruh kelompok target yang menjadi bagian dari pembagian ronde Kelompok 4.

---

## Investigation Targets

| No. | Target | Status |
|---|---|---|
| 1 | Kelompok 5 | In Progress |
| 2 | Kelompok 1 | In Progress |
| 3 | Kelompok 2 | Solved |
| 4 | Kelompok 3 | In Progress |

> Status investigasi akan diperbarui seiring proses pemeriksaan setiap barang bukti selesai dilakukan.

---

## Tools

Tool yang digunakan dalam proses investigasi:

- **Autopsy 4.23.1** - forensic examination dan analysis

Tool tambahan akan dicantumkan pada dokumentasi masing-masing investigasi apabila diperlukan.

---

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
        └── README.md
```

Setiap folder kelompok berisi dokumentasi investigasi secara terpisah agar proses pemeriksaan dan hasil dari masing-masing target dapat ditelusuri dengan mudah.

---

# Investigation Cases

## Kelompok 1

**Status:** In Progress

Dokumentasi investigasi:

[→ Buka Investigasi Kelompok 1](./investigations/kelompok%201/)

---

## Kelompok 2

**Status:** Solved

Investigasi terhadap Kelompok 2 berhasil menemukan file `KONMED 5.png` yang memiliki ketidaksesuaian antara ekstensi `.png` dan MIME type `text/plain`.

Pemeriksaan lebih lanjut terhadap file tersebut berhasil menemukan flag yang disembunyikan pada barang bukti.

[→ Buka Investigasi Kelompok 2](./investigations/kelompok%202/)

---

## Kelompok 3

**Status:** In Progress

Dokumentasi investigasi:

[→ Buka Investigasi Kelompok 3](./investigations/kelompok%203/)

---

## Kelompok 5

**Status:** In Progress

Dokumentasi investigasi:

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

---

# Investigation Status

| Target | Status | Documentation |
|---|---:|---|
| Kelompok 5 | In Progress | — |
| Kelompok 1 | In Progress | — |
| Kelompok 2 | Solved | [View](./investigations/kelompok%202/) |
| Kelompok 3 | In Progress | — |

---

## Documentation

Dokumentasi lengkap dari setiap investigasi dapat diakses melalui folder:

[**→ Investigations**](./investigations/)

Setiap target memiliki README masing-masing yang memuat tahapan pemeriksaan, dokumentasi screenshot, hasil temuan, dan kesimpulan investigasi.

---