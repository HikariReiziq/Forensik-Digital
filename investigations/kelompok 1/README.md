# Investigasi Forensik Digital - Kelompok 1

terdapat file README.md
<img width="542" height="466" alt="image" src="https://github.com/user-attachments/assets/c5500c62-dfa6-47b6-b276-cadf31763ee0" />

berisi panduan pengerjaan
<img width="848" height="438" alt="image" src="https://github.com/user-attachments/assets/f60ab3af-0c68-46a0-835f-51d3f9a6c840" />


ditemukan flag
'FLAG{anti_forensics_reimage_cover_story_2026}'
<img width="393" height="33" alt="image" src="https://github.com/user-attachments/assets/1acd6359-b74c-4fda-8966-290ece733015" />
---

## 1. Ringkasan Soal

Barang bukti Kelompok 1 berupa SSD/NVMe bersistem file `ext4` (label `CONFIDENTIAL`, skema partisi GPT) dengan skenario sebagai berikut: Billy (Developer) menyerahkan drive ke IT, lalu Bob (System Administrator) ditugaskan lewat Tiket #IT-8842 untuk melakukan wipe dan re-imaging. Bob diduga memakai proses re-imaging sebagai alibi untuk menyembunyikan data rahasia perusahaan.

**Hasil:** payload tersembunyi ditemukan di *unallocated space* (sektor 100), di luar partisi aktif. Payload dienkripsi XOR satu byte (`0x5A`) dan header zip-nya sengaja dirusak (`DE AD BE EF`). Setelah didekripsi dan header diperbaiki, ditemukan berkas `secret_evidence.txt` berisi flag.

**Flag:** `FLAG{anti_forensics_reimage_cover_story_2026}`

📸 **SS-01: Foto barang bukti / drive Kelompok 1 yang terhubung ke laptop**
<!-- ![SS-01 Barang bukti](screenshots/01-barang-bukti.png) -->

---

## 2. Alat yang Digunakan

| Alat | Fungsi |
|---|---|
| FTK Imager 8.3.0.27 | Membuka drive fisik secara read-only, menelusuri partisi ext4 dan unallocated space, mengekspor berkas |
| Python 3.13.2 | Dekripsi XOR, perbaikan header zip, pembuatan `payload.zip` |
| PowerShell (`Get-FileHash`, `tar`) | Perhitungan hash SHA-256 dan ekstraksi zip |

---

## 3. Langkah Analisis

### 3.1 Membuka barang bukti (read-only)

Drive berformat ext4 tidak dapat dibuka di File Explorer Windows. Drive dibuka di FTK Imager lewat **File > Add Evidence Item > Physical Drive** (`\\.\PHYSICALDRIVE2`). Menu ini hanya membaca disk, tidak menulis apa pun ke media.

📸 **SS-02: Jendela Add Evidence Item (pilihan Physical Drive)**
<!-- ![SS-02 Add Evidence Item](screenshots/02-add-evidence-item.png) -->

📸 **SS-03: Evidence Tree: `primary (1)`, partisi `CONFIDENTIAL [ext4]`, dan `Unpartitioned Space [GPT]`**
<!-- ![SS-03 Evidence Tree](screenshots/03-evidence-tree.png) -->

### 3.2 Menelusuri sistem file ext4

Struktur direktori standar Linux ditemukan, dengan dua akun pengguna: `billy` dan `bob`.

Berkas yang diperiksa:

| Lokasi | Berkas |
|---|---|
| `/home/billy` | `meeting_notes.txt`, `notes.txt`, `q3_budget_draft.csv`, `node_modules_backup.tar`, `lofi_track1.mp3`, `profile_photo`, `app_settings.json`, `server.py` |
| `/home/bob` | `admin_todo.txt`, `ticket_IT-8842.txt`, `network_inventory.csv`, `nmap_scan_results.txt`, `networks_diagram.png` |
| `/` (root) | `README.md` (panduan tantangan) |
| `/var/log` | `sys_audit.log` |

📸 **SS-04: Struktur folder `/home/billy` dan `/home/bob` di Evidence Tree**
<!-- ![SS-04 Struktur home](screenshots/04-struktur-home.png) -->

### 3.3 Membaca README, tiket, dan log (petunjuk / breadcrumb)

`README.md` di root menyatakan bahwa flag tidak berada di folder atau berkas biasa, melainkan di *unallocated space* sektor 100, terenkripsi XOR satu byte, dengan header zip yang dirusak menjadi `0xDEADBEEF`.

📸 **SS-05: Isi `README.md` di root (tab Text FTK Imager)**
<!-- ![SS-05 README](screenshots/05-readme-root.png) -->

Isi `/var/log/sys_audit.log`:

```
2026-09-28 08:06:45 [DEBUG] storage_mgr: Pre-allocation check OK. Sector offset 0x0000C800 reserved. Mask 0x5A applied.
```

Analisis petunjuk:

- `0x0000C800` = 51.200 byte; 51.200 ÷ 512 = **sektor 100**
- `Mask 0x5A` = **kunci XOR satu byte**

📸 **SS-06: Isi `sys_audit.log`**
<!-- ![SS-06 sys_audit.log](screenshots/06-sys-audit-log.png) -->

📸 **SS-07: Isi `ticket_IT-8842.txt` dan `admin_todo.txt`**
<!-- ![SS-07 Tiket dan todo](screenshots/07-tiket-todo.png) -->

### 3.4 Mengekspor unallocated space

Pada **Unpartitioned Space [GPT] > [unallocated space]** terdapat berkas `000000034` berukuran 1.031.168 byte. FTK Imager menunjukkan `phy sec = 34`, artinya berkas ini dimulai dari sektor 34 dan mencakup hingga sektor 2047 (1.031.168 ÷ 512 = 2014 sektor), sehingga sektor 100 berada di dalamnya.

Berkas ini diekspor lewat **klik kanan > Export Files** ke laptop (bukan ke media bukti).

📸 **SS-08: Berkas `000000034` di File List dan status bar `phy sec = 34`**
<!-- ![SS-08 Berkas 000000034](screenshots/08-berkas-000000034.png) -->

📸 **SS-09: Hasil Export Files (`1 file successful, 1031168 bytes`)**
<!-- ![SS-09 Export berhasil](screenshots/09-export-berhasil.png) -->

### 3.5 Menemukan payload di sektor 100

Offset sektor 100 dalam berkas ekspor: (100 − 34) × 512 = **33.792 byte (`0x8400`)**.

```text
sektor 100: 84f7e4b54e5a5a5a5a5aa72b1807ca22
penanda ditemukan di offset: 33792 0x8400
```

Byte `84 F7 E4 B5` di-XOR dengan `0x5A` menjadi `DE AD BE EF`, yaitu header rusak yang disebutkan di hint. Byte berikutnya `4E 5A 5A 5A` menjadi `14 00 00 00`, sesuai bentuk header zip asli.

📸 **SS-10: Output Python `sektor 100:` dan `penanda ditemukan di offset: 33792 0x8400`**
<!-- ![SS-10 Output sektor 100](screenshots/10-output-sektor-100.png) -->

### 3.6 Dekripsi XOR dan perbaikan header

Seluruh byte dari offset 33.792 di-XOR dengan `0x5A`, lalu magic header diperbaiki dari `DE AD BE EF` menjadi `50 4B 03 04` (`PK\x03\x04`). Data dipotong pada penanda akhir zip `PK\x05\x06` (+22 byte) sehingga menghasilkan `payload.zip` sebesar 181 byte.

```python
import os
folder = r"G:\FORDIG\bb-klp-1"
data = open(os.path.join(folder, "000000034"), "rb").read()
dec = bytearray(b ^ 0x5A for b in data[33792:])   # sektor 100, XOR key 0x5A
dec[0:4] = bytes([0x50, 0x4B, 0x03, 0x04])        # perbaiki magic header zip
end = dec.rfind(b"PK\x05\x06")
dec = dec[:end + 22]
open(os.path.join(folder, "payload.zip"), "wb").write(dec)
```

Output yang diperoleh:

```text
header asli: deadbeef
akhir zip di: 159
181
ukuran zip: 181
```

📸 **SS-11: Output Python (`header asli: deadbeef`, `akhir zip di: 159`, `ukuran zip: 181`)**
<!-- ![SS-11 Output dekripsi](screenshots/11-output-dekripsi.png) -->

### 3.7 Ekstraksi dan flag

```powershell
tar -tf payload.zip          # isi: secret_evidence.txt
mkdir hasil
tar -xf payload.zip -C hasil
type hasil\secret_evidence.txt
```

Catatan: pesan `Can't restore time: Invalid argument` hanya berarti tanggal di dalam zip tidak valid. Isi berkas tetap berhasil diekstrak.

```text
FLAG{anti_forensics_reimage_cover_story_2026}
```

📸 **SS-12: `tar -tf`, proses ekstrak, dan isi `secret_evidence.txt` yang menampilkan flag**
<!-- ![SS-12 Flag](screenshots/12-flag.png) -->

---

## 4. Hash Integritas (SHA-256)

Hash dihitung pada Minggu, 4 Oktober 2026, sekitar pukul 19:16–19:17 WIB, dari **berkas hasil ekspor** (bukan dari media asli).

| Berkas | Ukuran | SHA-256 |
|---|---|---|
| `000000034` | 1.031.168 byte | `42D2F67AA536CA0BA0B077B3F80DCFCD26528AFE3AC91DB32632B9DAE737B9A7` |
| `payload.zip` | 181 byte | `A19D1ABF650FD215A479117CB312722E060BF0F27F116EC03B5C1C31A31636FA` |
| `hasil\secret_evidence.txt` | 45 byte | `40B13F2D38B96A1BAFD0451E7E91784D48141529EF6E544DA1DDE80E68AD5013` |

📸 **SS-13: Output `Get-FileHash` (atau isi `hash_sha256.txt`)**
<!-- ![SS-13 Hash](screenshots/13-hash.png) -->

---

## 5. Integritas Barang Bukti

- Drive dibuka melalui **FTK Imager (Add Evidence Item)** yang bersifat **read-only**. Tidak ada penulisan, penambahan, penghapusan, atau perubahan pada media bukti.
- Seluruh berkas hasil ekspor dan analisis disimpan di laptop penyelidik (`G:\FORDIG\bb-klp-1`), bukan di media bukti.
- Saat Windows menampilkan permintaan format untuk partisi ext4, opsi **Cancel** dipilih.
- **Keterbatasan:** forensic image penuh (`.raw`/`.dd`/`.E01`) dari seluruh drive tidak dibuat karena keterbatasan waktu. Analisis dilakukan langsung pada media secara read-only, dan hash dihitung dari berkas hasil ekspor.
- Media dikembalikan ke Kelompok 1 pada: [isi tanggal dan jam pengembalian].

---

## 6. Kesimpulan

Bob menyembunyikan berkas rahasia di *unallocated space* (sektor 100), yaitu area di antara tabel partisi GPT dan partisi `ext4` pertama (dimulai di sektor 2048). Area ini tidak terlihat saat drive dibuka lewat file manager biasa maupun saat memeriksa isi sistem file. Proses re-imaging pada Tiket #IT-8842 menjadi alibi: log `sys_audit.log` mencatat `Sector offset 0x0000C800 reserved. Mask 0x5A applied` yang merupakan jejak penyembunyian data.

Teknik anti-forensik yang teridentifikasi:

1. Penyimpanan data di luar batas partisi aktif (unallocated space)
2. Enkripsi XOR satu byte (`0x5A`)
3. Perusakan magic header zip (`DE AD BE EF`) agar berkas tidak dikenali alat otomatis
4. Penyamaran sebagai aktivitas rutin (re-imaging)

---

## 7. Logsheet Investigasi

Sesuaikan kolom jam dengan catatanmu.

| No | Waktu (WIB) | Aktivitas | Hasil |
|---|---|---|---|
| 1 | [jam] | Menghubungkan drive, membuka di FTK Imager (Physical Drive, read-only) | Partisi ext4 `CONFIDENTIAL` dan Unpartitioned Space terlihat |
| 2 | [jam] | Menelusuri `/home/billy` dan `/home/bob` | Tidak ditemukan flag di berkas biasa |
| 3 | [jam] | Membaca `README.md` root | Hint: payload di unallocated space, sektor 100, XOR, header rusak |
| 4 | [jam] | Membaca `sys_audit.log`, tiket, `admin_todo.txt` | Offset `0xC800` (sektor 100), mask `0x5A` |
| 5 | 19:01 | Ekspor `000000034` dari Unpartitioned Space | 1.031.168 byte |
| 6 | 19:12 | Dekripsi XOR dan perbaikan header dengan Python | `payload.zip` 181 byte |
| 7 | 19:14 | Ekstraksi zip | `secret_evidence.txt` |
| 8 | 19:14 | Membaca isi berkas | Flag ditemukan |
| 9 | 19:16–19:17 | Menghitung hash SHA-256 | Tabel hash pada bagian 4 |
| 10 | [jam] | Mengembalikan media ke Kelompok 1 | Selesai |

---



