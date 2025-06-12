# 🩺 Deteksi Penyakit Jantung Menggunakan Machine Learning

## 1. Domain Proyek

Penyakit jantung merupakan salah satu penyebab utama kematian di dunia. Berdasarkan data dari World Health Organization (WHO), sekitar 17,9 juta orang meninggal setiap tahunnya karena penyakit kardiovaskular, yang mencakup 31% dari seluruh kematian global [1].

Diagnosis dini sangat penting untuk mencegah komplikasi serius. Dengan perkembangan teknologi, model machine learning dapat digunakan untuk membantu proses deteksi penyakit jantung secara cepat dan akurat berdasarkan data klinis pasien.

**Referensi**:
[1] World Health Organization. [Cardiovascular diseases (CVDs)](https://www.who.int/news-room/fact-sheets/detail/cardiovascular-diseases-%28cvds%29)

---

## 2. Business Understanding

### Problem Statement

* Bagaimana memanfaatkan data klinis untuk memprediksi apakah seseorang memiliki risiko penyakit jantung?
* Algoritma machine learning apa yang paling efektif untuk melakukan klasifikasi berdasarkan data pasien?

### Goals

* Membangun model machine learning untuk mengklasifikasikan pasien sebagai penderita atau tidak menderita penyakit jantung.
* Mengevaluasi performa model menggunakan metrik klasifikasi dan memilih model dengan performa terbaik.

### Solution Statement

* Menggunakan algoritma Random Forest Classifier untuk membangun model prediksi.
* Melakukan preprocessing data berupa encoding fitur kategorikal, normalisasi, dan penanganan missing values.
* Menggunakan evaluasi berbasis classification report dan confusion matrix.

---

## 3. Data Understanding

### Dataset

Dataset yang digunakan berasal dari Kaggle: [Heart Disease Dataset by Redwan Karim Sony](https://www.kaggle.com/datasets/redwankarimsony/heart-disease-data). Dataset ini merupakan gabungan beberapa dataset penyakit jantung yang telah digabung dan terdiri dari 920 baris dan 16 kolom.

### Fitur-Fitur

| Nama Fitur | Deskripsi                                             |
| ---------- | ----------------------------------------------------- |
| `age`      | Usia pasien                                           |
| `sex`      | Jenis kelamin (0 = perempuan, 1 = laki-laki)          |
| `cp`       | Tipe nyeri dada                                       |
| `trestbps` | Tekanan darah saat istirahat (mm Hg)                  |
| `chol`     | Kadar kolesterol serum (mg/dl)                        |
| `fbs`      | Gula darah puasa > 120 mg/dl (1 = true)               |
| `restecg`  | Hasil elektrokardiografi saat istirahat               |
| `thalach`  | Detak jantung maksimum                                |
| `exang`    | Angina akibat latihan (1 = ya)                        |
| `oldpeak`  | Depresi ST akibat latihan                             |
| `slope`    | Kemiringan segmen ST                                  |
| `ca`       | Jumlah pembuluh darah besar (mengandung nilai kosong) |
| `thal`     | Jenis thalassemia (mengandung nilai kosong)           |
| `num`      | Target (0 = sehat, 1 = menderita penyakit jantung)    |

### Informasi Missing Values

Beberapa fitur memiliki nilai kosong, berikut jumlahnya:

```
trestbps     : 59
chol         : 30
fbs          : 90
restecg      : 2
thalach      : 55
exang        : 55
oldpeak      : 62
slope        : 309
ca           : 611
thal         : 486
```

---

## 4. Data Preparation

### a. Pembersihan Data

* Kolom `id` dan `dataset` dihapus karena tidak memberikan kontribusi pada prediksi.
* Missing values ditangani dengan pengisian menggunakan median (untuk fitur numerik) atau mode (untuk kategorikal).
* Target klasifikasi dibuat dari kolom `num`, yaitu:

  * `0` → tidak menderita penyakit jantung
  * `>0` → dikonversi ke `1` sebagai menderita penyakit jantung

### b. Encoding & Normalisasi

* Label Encoding dilakukan pada fitur kategorikal seperti `sex`, `cp`, `restecg`, `exang`, `slope`, `ca`, dan `thal`.
* StandardScaler digunakan untuk normalisasi fitur numerik seperti `age`, `trestbps`, `chol`, `thalach`, `oldpeak`.

### c. Split Data

* Data dibagi menjadi:

  * 80% data latih
  * 20% data uji

---

## 5. Modeling

Model yang digunakan: **Random Forest Classifier**

**Cara Kerja:**

Random Forest adalah algoritma ensemble learning berbasis decision tree. Model ini bekerja dengan membangun banyak pohon keputusan (decision tree) dari subset data secara acak, lalu menggabungkan hasil prediksi dari masing-masing pohon dengan metode voting (untuk klasifikasi). Hal ini membantu mengurangi overfitting dan meningkatkan akurasi model.

**Parameter yang Digunakan:**

- `n_estimators=100`: Jumlah pohon dalam hutan. Nilai default adalah 100.
- `max_depth=None`: Maksimum kedalaman pohon. Dengan `None`, pohon akan tumbuh hingga semua daun bersifat murni (pure).
- `random_state=42`: Untuk memastikan hasil reproducible.
- Parameter lainnya menggunakan default.

**(Opsional) Kelebihan & Kekurangan:**

- ✅ Kelebihan: Mengurangi overfitting, robust terhadap data noisy, akurat.
- ❌ Kekurangan: Model cenderung sulit diinterpretasikan, dan lebih lambat dari model linear pada dataset besar.

---

## 6. Evaluation

### Confusion Matrix

![Confusion Matrix](attachment\:confussion matrix.png)

|              | Predicted 0 | Predicted 1 |
| ------------ | ----------- | ----------- |
| **Actual 0** | 64          | 11          |
| **Actual 1** | 16          | 93          |

### Classification Report

```text
              precision    recall  f1-score   support

           0       0.80      0.85      0.83        75
           1       0.89      0.85      0.87       109

    accuracy                           0.85       184
   macro avg       0.85      0.85      0.85       184
weighted avg       0.86      0.85      0.85       184
```

### Accuracy

```text
Accuracy: 0.853
```

Model memiliki performa seimbang dalam mengenali kedua kelas (sehat dan sakit), dengan F1-score 0.83 untuk kelas 0 dan 0.87 untuk kelas 1.

---

## 7. Kesimpulan

Model Random Forest yang dibangun mampu mengklasifikasikan pasien penyakit jantung dengan akurasi **85,3%**. Nilai precision, recall, dan f1-score yang seimbang menunjukkan bahwa model cukup andal dan tidak bias terhadap salah satu kelas.

Model ini layak untuk digunakan sebagai dasar pengembangan sistem deteksi dini penyakit jantung berbasis data klinis, terutama untuk mendukung pengambilan keputusan di dunia medis.

---