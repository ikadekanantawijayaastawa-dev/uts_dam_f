# UTS Data Acquisition & Management
## Sebaran Daya Tarik Wisata di Provinsi Bali

Proyek ini merupakan tugas Ujian Tengah Semester (UTS) mata kuliah **Data Acquisition & Management**. Notebook menggunakan Python untuk membaca dan membersihkan dataset, mengambil data cuaca melalui API, melakukan scraping halaman web statis, mengintegrasikan data, serta menyimpan dokumentasi provenance dan pemeriksaan etika data.

## Identitas Mahasiswa

- **Nama:** I Kadek Ananta Wijaya Astawa
- **NIM:** 2501010124
- **Program Studi:** Informatika MDA
- **Mata Kuliah:** Data Acquisition & Management
- **Dosen:** Dr. I Gede Totok Suryawan, S.Kom., M.T.

## Tujuan

1. Membaca dan memeriksa dataset daya tarik wisata.
2. Melakukan pembersihan data tanpa mengubah file mentah.
3. Mengambil data cuaca historis menggunakan Open-Meteo API.
4. Mengambil daftar tempat wisata dari kategori Wikipedia setelah memeriksa `robots.txt`.
5. Mengintegrasikan dataset utama dengan hasil scraping menggunakan kolom nama yang dinormalisasi.
6. Membuat dokumentasi sumber data (`provenance.json`) dan Data Ethics Checklist.

## Sumber Data

| Sumber | Metode | Keterangan |
|---|---|---|
| Dataset pengunjung daya tarik wisata per bulan | Dataset lokal (CSV) | File CSV harus disediakan di `data/raw/` sebelum notebook dijalankan. |
| [Open-Meteo Historical Weather API](https://archive-api.open-meteo.com/v1/archive) | REST API | Mengambil data cuaca harian historis dan mengagregasikannya menjadi data bulanan. |
| [Wikipedia – Kategori Tempat wisata di Bali](https://id.wikipedia.org/wiki/Kategori:Tempat_wisata_di_Bali) | Static web scraping | Mengambil nama artikel anggota kategori setelah pemeriksaan [robots.txt](https://id.wikipedia.org/robots.txt). |

**Catatan penting untuk verifikasi sebelum pengumpulan:**
- Notebook saat ini menggunakan koordinat `-6.98, 108.48` untuk API cuaca, yang merupakan koordinat representatif Kabupaten Kuningan, bukan Bali. Jika topik penelitian tetap Bali, sesuaikan koordinat dan keterangan wilayah API agar konsisten.
- Pastikan file dataset utama benar-benar berasal dari sumber yang dicantumkan, cakupan wilayah dan tahunnya sesuai metadata sumber, serta jumlah baris memenuhi ketentuan tugas.
- Pastikan ketentuan lisensi/atribusi masing-masing sumber telah diperiksa pada halaman resmi sumber. Jangan menganggap status lisensi hanya berdasarkan keterangan di notebook.

## Teknologi dan Library

- Python
- pandas
- requests
- BeautifulSoup (`beautifulsoup4`)
- lxml
- openpyxl
- Jupyter Notebook / Google Colab

## Struktur Direktori

Siapkan struktur folder berikut:

```text
UTS_DAM_2501010124/
├── UTS_DAM_2501010124.ipynb
├── README.md
├── data/
│   ├── raw/
│   │   ├── banyaknya__pengunjung_daya_tarik_wisata_per_bulan.csv
│   │   └── (berkas mentah API dan HTML dibuat saat notebook dijalankan)
│   └── processed/
│       └── (hasil pembersihan, normalisasi, dan integrasi)
└── docs/
    ├── provenance.json
    └── data_ethics_checklist.csv
```

File CSV dataset utama tidak otomatis tersedia dari notebook. Letakkan file tersebut di `data/raw/` dengan nama yang persis sama seperti yang digunakan pada kode, atau ubah variabel `raw_dataset_path` agar sesuai dengan nama file sebenarnya.

## Cara Menjalankan

### 1. Siapkan project

Unduh atau clone repository, lalu buka folder project di Google Colab atau Jupyter Notebook.

### 2. Siapkan dataset mentah

Buat folder `data/raw/` dan masukkan dataset CSV utama ke dalamnya. File mentah disimpan apa adanya dan tidak ditimpa oleh proses pembersihan.

### 3. Periksa konfigurasi

Pada notebook, pastikan:
- `EMAIL_KAMPUS` berisi alamat email yang memang digunakan untuk identifikasi pada `User-Agent`.
- Nama file pada `raw_dataset_path` cocok dengan file di `data/raw/`.
- Koordinat dan rentang tanggal API sesuai dengan wilayah dan topik penelitian.
- URL sumber serta keterangan lisensi sesuai dengan sumber yang benar-benar digunakan.

### 4. Jalankan notebook sesuai urutan

**Perbaiki urutan dua bagian awal sebelum menjalankan seluruh notebook:** pada file notebook saat ini, sel *Data Cleaning* berada sebelum sel yang membaca CSV menjadi `df_dataset_raw`. Pindahkan sel pembacaan dataset ke atas, kemudian jalankan sel *Data Cleaning* setelah `df_dataset_raw` terbentuk. Urutan minimalnya:

1. Instalasi dan import library serta konfigurasi folder.
2. Pemeriksaan lokasi dan pembacaan dataset mentah menjadi `df_dataset_raw`.
3. Data cleaning dan penyimpanan CSV bersih.
4. Identifikasi kolom dan pemeriksaan karakteristik dataset.
5. Estimasi kebutuhan penyimpanan.
6. Akuisisi data cuaca dari Open-Meteo API.
7. Pemeriksaan `robots.txt` dan scraping Wikipedia.
8. Normalisasi nama dan integrasi data.
9. Data Ethics Checklist dan provenance.
10. Pemeriksaan file keluaran.

Jalankan setiap sel dari atas ke bawah. Pastikan setiap sel selesai tanpa error dan periksa hasil tabel serta file yang dihasilkan.

### 5. Periksa hasil

Periksa bahwa:
- Data mentah tetap berada di `data/raw/`.
- Dataset bersih dan hasil olahan berada di `data/processed/`.
- Data API berhasil dinormalisasi menjadi tabel bulanan.
- Hasil scraping tidak kosong.
- Proses merge menggunakan kunci yang benar-benar cocok dari kedua sumber.
- Jumlah baris dan duplikasi hasil merge telah diperiksa.
- `docs/provenance.json` dan `docs/data_ethics_checklist.csv` berhasil dibuat.

**Integritas proses merge:** jangan mengubah nama atau isi dataset utama secara buatan hanya agar proses `merge` menghasilkan baris. Jika hasil merge kosong, periksa nilai kunci, normalisasi, dan kesesuaian kedua sumber. Laporkan hasil yang sebenarnya.

## File Keluaran

Notebook dirancang untuk menghasilkan berkas berikut:

| Lokasi | Berkas | Isi |
|---|---|---|
| `data/raw/` | `open_meteo_kuningan_2016_2022.json` | Respons mentah API cuaca dalam format JSON. |
| `data/raw/` | `wikipedia_robots.txt` | Salinan aturan robots.txt yang diperiksa. |
| `data/raw/` | `wikipedia_kategori_tempat_wisata_bali.html` | HTML mentah halaman kategori Wikipedia. |
| `data/processed/` | `pengunjung_wisata_per_bulan_clean.csv` | Dataset utama setelah pembersihan. |
| `data/processed/` | `cuaca_kuningan_bulanan.csv` | Data cuaca yang telah diagregasi per bulan. |
| `data/processed/` | `wikipedia_tempat_wisata_bali.csv` | Hasil ekstraksi daftar tempat wisata. |
| `data/processed/` | `hasil_integrasi_dataset_wikipedia_bali.csv` | Hasil integrasi dataset utama dan data Wikipedia. |
| `docs/` | `data_ethics_checklist.csv` | Pemeriksaan etika pengumpulan dan penggunaan data. |
| `docs/` | `provenance.json` | Informasi sumber, waktu akses, ukuran file, dan MD5. |

Nama file keluaran di atas mengikuti nama yang tertulis pada notebook. Beberapa nama masih menyebut Kuningan karena konfigurasi API notebook saat ini; sesuaikan nama dan keterangannya jika wilayah API diubah menjadi Bali.

## Catatan Etika dan Penggunaan

- Gunakan data hanya sesuai tujuan dan ketentuan sumber.
- Periksa `robots.txt` sebelum scraping dan hentikan proses jika akses tidak diizinkan.
- Gunakan timeout, jeda antar-request, dan mekanisme retry untuk permintaan HTTP.
- Jangan mengumpulkan data pribadi yang tidak diperlukan.
- Cantumkan sumber dan atribusi sesuai ketentuan lisensi.
- Jangan memasukkan API key atau token rahasia ke dalam notebook atau repository publik.

## Status Pengerjaan

README ini menjelaskan struktur dan alur yang direncanakan berdasarkan notebook. Status keberhasilan akuisisi, jumlah baris aktual, dan validitas hasil integrasi harus diisi berdasarkan hasil eksekusi notebook, bukan diasumsikan dari kode.

---
**Mata Kuliah:** Data Acquisition & Management  
**Nama:** I Kadek Ananta Wijaya Astawa  
**NIM:** 2501010124
