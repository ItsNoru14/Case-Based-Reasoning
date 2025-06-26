# Sistem Case-Based Reasoning (CBR) untuk Putusan Hukum
## Case Study : Perdata Waris
Seluruh alur kerja, mulai dari pemrosesan PDF mentah hingga evaluasi model, terkandung dalam satu notebook Jupyter utama (`CaseBase_Build.ipynb`). Proyek ini mengeksplorasi dan membandingkan beberapa pendekatan retrieval:
1.  **Retrieval Berbasis Kemiripan**: Menggunakan representasi TF-IDF dan Cosine Similarity untuk menemukan kasus yang paling relevan secara semantik.
2.  **Retrieval sebagai Klasifikasi**: Menggunakan representasi TF-IDF dengan model Machine Learning seperti SVM dan Naive Bayes untuk memprediksi kasus yang paling cocok.

## Overview
* [Struktur Proyek](#-struktur-proyek)
* [Teknologi yang Digunakan](#️-teknologi-yang-digunakan)
* [Instalasi](#-instalasi)
* [Cara Menjalankan Pipeline End-to-End](#-cara-menjalankan-pipeline-end-to-end)
* [Lisensi](#-lisensi)

## Struktur Proyek
Proyek ini diorganisir ke dalam struktur folder berikut untuk menjaga agar data dan model tetap rapi.

proyek-cbr/
│
├── .venv/                  # Folder virtual environment Python (opsional, diabaikan Git)
├── data/
│   ├── eval/               # Berisi file query uji dan hasil akhir metrik evaluasi
│   ├── models/             # Berisi file model (.pkl), vectorizer, dan embedding (.npy)
│   ├── processed/          # Berisi data kasus yang sudah terstruktur (cases.json)
│   └── raw/                # Berisi teks bersih hasil ekstraksi dari PDF (case_XXX.txt)
        └── CBR CASE/       # Berisi file pdf yang telah di download
├── results/                # Berisi output CSV dan plot dari tahap evaluasi & prediksi
│
├── CaseBase_Build.ipynb    # NOTEBOOK UTAMA: Berisi seluruh pipeline dari Tahap 1-5
└── README.md               # Dokumentasi proyek ini

## Teknologi yang Digunakan
* **Python 3.8+**
* **Jupyter Notebook**: Untuk pengembangan dan eksekusi pipeline secara interaktif.
* **Library Utama**:
    * **Pandas**: Untuk manipulasi dan analisis data.
    * **Scikit-learn**: Untuk TF-IDF, model SVM, Naive Bayes, pipeline, dan metrik evaluasi.
    * **PyTorch**: Sebagai backend untuk model Transformers.
    * **PDFMiner.six**: Untuk ekstraksi teks dari file PDF.
    * **Joblib**: Untuk menyimpan dan memuat model Scikit-learn.
    * **Matplotlib**: Untuk visualisasi data performa model.
    * **Tqdm**: Untuk menampilkan progress bar pada proses.
  
## Instalasi
Untuk menjalankan notebook ini di lingkungan baru, ikuti langkah-langkah berikut.
1. **Clone Repositori
