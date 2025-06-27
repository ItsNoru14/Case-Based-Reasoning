# Sistem Case-Based Reasoning (CBR) untuk Putusan Hukum

Fokus utama proyek ini adalah membandingkan dua pendekatan retrieval yang berbeda, keduanya menggunakan representasi teks TF-IDF:
1.  **Retrieval Berbasis Kemiripan (TF-IDF + Cosine Similarity)**: Sebuah pendekatan pencarian murni untuk menemukan kasus-kasus yang paling mirip secara leksikal (berdasarkan kata kunci).
2.  **Retrieval sebagai Klasifikasi (TF-IDF + Multinomial Naive Bayes)**: Sebuah pendekatan yang melatih model klasifikasi untuk memprediksi kasus yang paling relevan.

## Daftar Isi
* [Struktur Proyek](#-struktur-proyek)
* [Teknologi yang Digunakan](#️-teknologi-yang-digunakan)
* [Instalasi](#-instalasi)
* [Alur Kerja End-to-End](#-alur-kerja-end-to-end)
* [Lisensi](#-lisensi)

## Struktur Proyek
Proyek ini diorganisir ke dalam struktur folder berikut untuk menjaga agar data, model, dan hasil tetap rapi.
```
Case Based Reasoning/
│
├───data/
│   ├───eval/               # Berisi file query uji dan hasil akhir metrik evaluasi
│   ├───models/             # Berisi file model (.pkl) dan vectorizer yang sudah dilatih
│   ├───processed/          # Berisi data kasus yang sudah terstruktur (cases.json)
│   ├───raw/                # Berisi teks bersih hasil ekstraksi dari PDF (case_XXX.txt)
│   │   └───CBR Case/       # Berisi file pdf yang telah di download
│   └───results/            # Berisi output CSV dan plot dari tahap evaluasi & prediksi
│
├───Logs/                   # Folder untuk menyimpan file log dari proses
├── CaseBase_Build.ipynb    # NOTEBOOK UTAMA: Berisi seluruh pipeline dari Tahap 1-5
└── README.md               # Dokumentasi proyek ini
```
## Teknologi yang Digunakan
* **Python 3.8+**
* **Jupyter Notebook**: Untuk pengembangan dan eksekusi pipeline secara interaktif.
* **Library Utama**:
    * **Pandas**: Untuk manipulasi dan analisis data.
    * **Scikit-learn**: Untuk TF-IDF, model Naive Bayes, pipeline, dan metrik evaluasi.
    * **Joblib**: Untuk menyimpan dan memuat model Scikit-learn.
    * **Matplotlib**: Untuk visualisasi data performa model.
    * **SciPy**: Untuk menyimpan dan memuat matriks sparse TF-IDF secara efisien.

## Instalasi

Untuk menjalankan notebook ini di lingkungan baru, ikuti langkah-langkah berikut.

### 1. Buat `requirements.txt` (Jika Belum Ada)
Jika Anda sudah menginstal semua library di *virtual environment*, jalankan perintah ini di terminal untuk membuat file dependensi secara otomatis:
```bash
pip freeze > requirements.txt
```

### 2.  Clone Repositori
```bash
git clone https://github.com/ItsNoru14/Case-Based-Reasoning.git
cd Case-Based-Reasoning
```

### 3. Buat dan Aktifkan Virtual Environment
```bash
# Membuat environment
python -m venv venv

# Mengaktifkan environment di Windows
venv\Scripts\activate

# Mengaktifkan environment di macOS/Linux
source venv/bin/activate
```

### 4. Instal Semua Dependensi
```bash
pip install -r requirements.txt
```

## Alur Kerja End-to-End

Proses ini menjelaskan langkah-langkah yang harus dijalankan secara berurutan untuk menjalankan keseluruhan pipeline, mulai dari data mentah hingga hasil evaluasi model.

### Langkah 1: Persiapan Data Mentah (Input Awal)
* **Aksi:** Letakkan semua file putusan hukum yang relevan dalam format `.pdf` ke dalam folder `data/raw/CBR Case`.
* **Hasil:** Folder `data/raw/CBR Case` berisi semua data sumber yang siap diproses oleh pipeline.

### Langkah 2: Membangun & Merepresentasikan Case Base (Tahap 1 & 2)
* **Aksi:** Jalankan sel-sel awal di notebook utama (`CaseBase_Build.ipynb`) yang bertanggung jawab untuk memproses data mentah.
* **Proses:**
    1.  Skrip akan membaca setiap file `.pdf` dari folder `data/raw/CBR Case`.
    2.  Melakukan ekstraksi teks dan pembersihan (menghapus header, footer, dll.).
    3.  Menyimpan hasil teks bersih sebagai file `case_XXX.txt` di dalam folder `data/raw/`.
    4.  Membaca kembali setiap file teks bersih, lalu mengekstrak metadata terstruktur (nomor perkara, tanggal, pihak, pasal, amar putusan, dll.).
* **Hasil:** Sebuah file database utama **`data/processed/cases.json`**. File ini berisi semua kasus Anda dalam format yang terstruktur dan menjadi dasar untuk semua tahapan selanjutnya.

### Langkah 3: Persiapan Model & Artefak (Tahap 3)
* **Aksi:** Jalankan sel di notebook yang berfungsi untuk melatih model dan mempersiapkan aset.
* **Proses:**
    1.  **Untuk Pipeline Naive Bayes**: Melatih model `MultinomialNB` dengan `TfidfVectorizer` dan menyimpannya sebagai `nb_pipeline.pkl`.
    2.  **Untuk Pipeline Cosine Similarity**: Membuat matriks `TF-IDF` dari seluruh korpus dan menyimpan matriks beserta *vectorizer*-nya.
    3.  **Membuat Kueri Uji**: Mengambil sampel acak dari dataset untuk membuat file `queries.json` yang akan digunakan untuk evaluasi.
* **Hasil:**
    * `data/models/nb_pipeline.pkl`: Model Naive Bayes yang sudah siap pakai.
    * `data/models/tfidf_cosine_vectorizer.pkl`: Vectorizer untuk pipeline Cosine Similarity.
    * `data/models/tfidf_cosine_matrix.npz`: Database vektor untuk pipeline Cosine Similarity.
    * `data/eval/queries.json`: File berisi "soal ujian" untuk model.

### Langkah 4: Prediksi & Evaluasi (Tahap 4 & 5)
* **Aksi:** Jalankan sel-sel terakhir di notebook yang berisi logika untuk penggunaan model dan evaluasi.
* **Proses:**
    1.  Memuat semua aset yang telah dibuat di Tahap 3.
    2.  Untuk setiap kueri di `queries.json`, kedua pipeline (Naive Bayes dan Cosine Similarity) akan dijalankan untuk menghasilkan prediksi solusi.
    3.  Hasil prediksi dari kedua pipeline disimpan ke dalam file `.csv` terpisah di dalam folder `data/results/`.
    4.  Skrip kemudian membandingkan hasil prediksi dengan jawaban benar (*ground truth*) untuk menghitung metrik performa.
* **Hasil:**
    * **Laporan Prediksi**: File `.csv` di `data/results/`.
    * **Laporan Metrik**: File `.csv` di `data/eval/` yang berisi perbandingan skor performa kedua model.
    * **Visualisasi**: Gambar grafik batang (`.png`) yang membandingkan performa.
    * **Analisis Kesalahan**: File `.json` yang mencatat kueri-kueri yang gagal diprediksi dengan benar.

## Lisensi
Proyek ini dikembangkan sebagai tugas akhir mata kuliah Penalaran Komputer di Universitas Muhammadiyah Malang. Segala sumber daya, kode, dokumentasi ditujukan untuk keperluan tugas akhir dan harus di dokumentasikan di GitHub untuk syarat pengumpulan.
Anggota tim :
    * Dimas Arief Wicaksono (202210370311237)
    * Inzazi Nur Mujaini (202210370311246)
**Terimakasih untuk Professor/Instruktur kami Ir.Galih Wasis Wicaksono S.kom., M.Cs. atas arahan dan support dalam menjalani proyek ini.**