# UTS Data Acquisition & Management

## 📝 Identitas Mahasiswa

-   **Nama:** I Kadek Ananta Wijaya Astawa
-   **NIM:** 2501010124
-   **Program Studi:** Informatika MDA
-   **Mata Kuliah:** Data Acquisition & Management
-   **Dosen Pengampu:** Dr. I Gede Totok Suryawan, S.Kom., M.T.

------------------------------------------------------------------------

## 📌 Deskripsi Proyek

Proyek ini merupakan pengerjaan Ujian Tengah Semester (UTS) mata kuliah
Data Acquisition & Management. Tujuan proyek ini adalah melakukan proses
akuisisi, pemeriksaan, pembersihan, dan integrasi data dari beberapa
sumber secara terstruktur serta memperhatikan aspek etika penggunaan
data.

Proyek menggunakan tiga metode akuisisi data, yaitu:

1.  **Dataset Terbuka (Dataset Lokal)**

    Menggunakan data statistik bulanan banyaknya pengunjung daya tarik
    wisata di Kabupaten Kuningan, Provinsi Jawa Barat, periode tahun
    2016--2022.

2.  **API (Application Programming Interface)**

    Menggunakan Open-Meteo API untuk memperoleh data cuaca historis
    berdasarkan koordinat Kabupaten Kuningan. Data cuaca harian kemudian
    diolah menjadi data bulanan.

3.  **Web Scraping**

    Mengambil informasi daftar tempat wisata di Bali melalui halaman
    kategori Wikipedia bahasa Indonesia menggunakan library
    BeautifulSoup. Sebelum scraping dilakukan, terlebih dahulu diperiksa
    ketentuan `robots.txt` untuk memastikan aktivitas pengambilan data
    diizinkan.

------------------------------------------------------------------------

## 🎯 Tujuan Proyek

-   Memahami proses akuisisi data dari berbagai sumber.
-   Membaca dan mengidentifikasi karakteristik dataset.
-   Melakukan pemeriksaan kualitas dan pembersihan data.
-   Mengakses API publik dan mengolah respons JSON menjadi tabel.
-   Melakukan web scraping secara etis.
-   Memeriksa lisensi, privasi, dan aspek etika penggunaan data.
-   Mengintegrasikan data menggunakan metode merge berdasarkan kolom
    kunci yang relevan.
-   Mendokumentasikan sumber data melalui Data Provenance.

------------------------------------------------------------------------

## 📂 Struktur Direktori

``` text
UTS_DAM_2501010124/
│
├── data/
│   ├── raw/
│   │   ├── banyaknya__pengunjung_daya_tarik_wisata_per_bulan.csv
│   │   ├── open_meteo_kuningan_2016_2022.json
│   │   ├── wikipedia_robots.txt
│   │   └── wikipedia_kategori_tempat_wisata_bali.html
│   │
│   └── processed/
│       ├── pengunjung_wisata_per_bulan_clean.csv
│       ├── cuaca_kuningan_bulanan.csv
│       ├── wikipedia_tempat_wisata_bali.csv
│       └── hasil_integrasi_pengunjung_cuaca_kuningan.csv
│
├── docs/
│   ├── data_ethics_checklist.csv
│   └── provenance.json
│
├── README.md
├── UTS_DAM_2501010124.ipynb
└── hasil_uts_dam.zip
```

### Keterangan Folder

-   **data/raw/**: Menyimpan seluruh data mentah hasil unduhan, API, dan
    web scraping tanpa perubahan.
-   **data/processed/**: Menyimpan data yang telah dibersihkan,
    dinormalisasi, dan hasil integrasi.
-   **docs/**: Menyimpan dokumentasi etika data dan catatan provenance.
-   **README.md**: Berisi penjelasan mengenai proyek, sumber data, dan
    tahapan pengerjaan.
-   **UTS_DAM_2501010124.ipynb**: Notebook utama yang berisi seluruh
    proses pengerjaan UTS.
-   **hasil_uts_dam.zip**: Arsip hasil pengerjaan proyek.

------------------------------------------------------------------------

## 🛠️ Teknologi dan Library

Proyek ini menggunakan Python dan beberapa library berikut:

  Library         Kegunaan
  --------------- ---------------------------------------------
  Pandas          Membaca, membersihkan, dan mengolah dataset
  Requests        Mengirim HTTP request ke API dan website
  BeautifulSoup   Melakukan web scraping HTML
  JSON            Mengolah respons API
  Hashlib         Menghitung hash MD5
  Pathlib         Mengelola direktori dan file
  RobotParser     Memeriksa aturan robots.txt

------------------------------------------------------------------------

## 🔄 Alur Kerja Akuisisi dan Pengolahan Data

### 1. Persiapan Lingkungan

Menyiapkan library, direktori penyimpanan, konfigurasi HTTP request,
timeout, User-Agent, retry, dan jeda antar-request.

### 2. Akuisisi Dataset Terbuka

Mengunduh dataset statistik pengunjung daya tarik wisata Kabupaten
Kuningan. Dataset disimpan terlebih dahulu ke folder `data/raw/` sebagai
data mentah.

### 3. Pembacaan dan Identifikasi Dataset

Membaca dataset menggunakan Pandas, kemudian mengidentifikasi:

-   Jumlah baris dan kolom.
-   Tipe data setiap kolom.
-   Nilai kosong dan duplikasi.
-   Cakupan wilayah dan waktu.
-   Ukuran file.
-   Estimasi kebutuhan penyimpanan selama 12 bulan dengan tambahan
    cadangan 30%.

### 4. Data Cleaning

Melakukan proses pembersihan data, meliputi:

-   Normalisasi spasi kosong.
-   Penggantian penanda missing value menjadi `pd.NA`.
-   Penghapusan data duplikat.
-   Standardisasi angka dengan format Indonesia.

Hasil pembersihan disimpan di folder `data/processed/`.

### 5. Akuisisi Data Melalui API

Menggunakan Open-Meteo API untuk mengambil data cuaca historis
berdasarkan koordinat Kabupaten Kuningan.

Data cuaca harian kemudian diolah menjadi data bulanan dengan menghitung
rata-rata suhu, curah hujan, dan kecepatan angin sesuai variabel yang
digunakan.

Respons JSON asli disimpan di folder `data/raw/`, sedangkan hasil
pengolahan disimpan di folder `data/processed/`.

### 6. Web Scraping

Mengambil daftar tempat wisata dari kategori Wikipedia bahasa Indonesia.

Tahapan scraping meliputi:

-   Mengakses `robots.txt`.
-   Memeriksa izin akses halaman target.
-   Mengambil halaman kategori Wikipedia.
-   Memproses HTML menggunakan BeautifulSoup.
-   Mengekstrak nama tempat wisata dan URL artikel.
-   Menyimpan hasil scraping.

### 7. Integrasi Data

Menggabungkan dataset jumlah pengunjung wisata Kabupaten Kuningan dengan
data cuaca bulanan Open-Meteo.

Penggabungan dilakukan berdasarkan dua kolom kunci:

-   `tahun`
-   `bulan_num`

Integrasi dilakukan menggunakan metode `merge` berdasarkan kesesuaian
periode waktu, bukan menggunakan kolom bernilai tetap.

Hasil integrasi disimpan sebagai file CSV di folder `data/processed/`.

Data scraping Wikipedia Bali disimpan sebagai hasil akuisisi tersendiri
dan tidak dicampurkan dengan dataset Kuningan karena memiliki cakupan
wilayah berbeda.

### 8. Data Ethics Checklist

Melakukan pemeriksaan aspek etika penggunaan data, termasuk:

-   Legalitas dan lisensi sumber data.
-   Tujuan penggunaan data.
-   Minimalisasi data.
-   Identifikasi data pribadi (PII).
-   Quasi-identifier.
-   Persetujuan dan ekspektasi penggunaan data.
-   Keamanan data.
-   Potensi bias data.
-   Atribusi dan penyimpanan data.

Hasil pemeriksaan disimpan dalam `docs/data_ethics_checklist.csv`.

### 9. Data Provenance

Mencatat asal-usul setiap sumber data untuk menjaga transparansi dan
keterlacakan.

Informasi yang dicatat meliputi:

-   Nama sumber data.
-   URL sumber.
-   Tanggal akses.
-   Lisensi.
-   Metode akuisisi.
-   Nama file.
-   Ukuran file.
-   Hash MD5.

Dokumentasi disimpan dalam `docs/provenance.json`.

------------------------------------------------------------------------

## 📊 Hasil Akhir Proyek

Hasil pengerjaan proyek terdiri dari:

1.  Dataset mentah dari portal data terbuka.
2.  Data cuaca historis hasil akuisisi API Open-Meteo.
3.  Data tempat wisata Bali hasil web scraping Wikipedia.
4.  Dataset yang telah dibersihkan.
5.  Dataset hasil integrasi pengunjung wisata dan data cuaca.
6.  Data Ethics Checklist.
7.  Dokumentasi Data Provenance.
8.  Notebook Python yang berisi seluruh tahapan pengerjaan.

------------------------------------------------------------------------

## ▶️ Cara Menjalankan Proyek

1.  Buka Google Colab.
2.  Upload file notebook `UTS_DAM_2501010124.ipynb`.
3.  Pastikan library yang diperlukan sudah tersedia.
4.  Jalankan setiap sel notebook secara berurutan dari awal hingga
    akhir.
5.  Periksa hasil akuisisi, proses pembersihan, dan integrasi data.
6.  Pastikan seluruh file hasil pengerjaan tersimpan dalam folder yang
    sesuai.

------------------------------------------------------------------------

## ⚖️ Etika Penggunaan Data

Proyek ini memperhatikan etika dalam proses akuisisi data dengan cara:

-   Menggunakan sumber data terbuka dan API publik.
-   Memeriksa ketentuan `robots.txt` sebelum melakukan scraping.
-   Menggunakan User-Agent yang mencantumkan identitas kontak mahasiswa.
-   Menerapkan timeout dan jeda antar-request.
-   Tidak mengambil data pribadi pengunjung.
-   Mencatat sumber dan lisensi data.
-   Menyimpan data mentah secara terpisah dari data hasil pengolahan.

------------------------------------------------------------------------

## 📝 Kesimpulan

Melalui proyek UTS ini, dilakukan penerapan tiga metode akuisisi data,
yaitu pengunduhan dataset terbuka, penggunaan API, dan web scraping.

Data yang diperoleh kemudian diperiksa, dibersihkan, dan diolah sesuai
kebutuhan analisis. Integrasi dataset pengunjung wisata Kabupaten
Kuningan dengan data cuaca Open-Meteo dilakukan berdasarkan kesamaan
tahun dan bulan.

Selain proses teknis, proyek ini juga menerapkan pemeriksaan etika data
dan dokumentasi provenance agar proses akuisisi dapat ditelusuri dan
dipertanggungjawabkan.

------------------------------------------------------------------------

**UTS Data Acquisition & Management**

**Nama:** I Kadek Ananta Wijaya Astawa\
**NIM:** 2501010124\
**Program Studi:** Informatika MDA
