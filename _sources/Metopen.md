---
title: Metopen

---

# Hitungan manual 
## Matrik konferensi 
### Langkah 1: Mengambil Data dari Confusion Matrix
Target Label:
- 0 = Benign (Jinak)
- 1 = Malignant (Ganas)
Total Data Testing: 114 Sampel.


|  | Prediksi jinak(0) | Prediksi ganas(1) | Total Aktual |
| -------- | -------- | -------- |---------|
| Aktual jinak(0|72 (TN)|0 (FP)|72|
| Aktual Ganas (1)| 3 (FN)|39 (TP)|42|
|Total Prediksi|75|39|114|

> Catatan: Dalam kasus medis (deteksi kanker), biasanya kita menganggap "Ganas/Malignant (1)" sebagai Positive Class dan "Jinak/Benign (0)" sebagai Negative Class.

- True Positive (TP): 39 (Aktual Ganas, Diprediksi Ganas)
- True Negative (TN): 72 (Aktual Jinak, Diprediksi Jinak)
- False Positive (FP): 0 (Aktual Jinak, Diprediksi Ganas) 
- False Negative (FN): 3 (Aktual Ganas, Diprediksi Jinak) 

### Langkah 2: Menghitung Akurasi (Accuracy)
Akurasi adalah seberapa sering model menebak dengan benar secara keseluruhan.

$$
\begin{gathered}
\text { Akurasi }=\frac{T P+T N}{\text { Total Sampel }} \\
\text { Akurasi }=\frac{39+72}{114}=\frac{111}{114}=0.97368 \ldots
\end{gathered}
$$

Hasil Manual: 97.37\% 

### Langkah 3: Menghitung Metrik per Kelas
#### A. Untuk Kelas "Malignant" (Ganas/1) - Fokus Utama
#### 1. Precision (Presisi) Seberapa akurat saat model memprediksi Ganas?

$$
\text { Precision }_1=\frac{T P}{T P+F P}=\frac{39}{39+0}=\frac{39}{39}=1.00
$$

(Artinya: Jika model bilang Ganas, 100\% pasti Ganas).
#### 2. Recall (Sensitivitas) Seberapa banyak kasus Ganas yang berhasil ditemukan dari total kasus Ganas yang ada?

$$
\text { Recall }_1=\frac{T P}{T P+F N}=\frac{39}{39+3}=\frac{39}{42} \approx 0.9285 \ldots
$$

Dibulatkan: 0.93
#### 3. F1-Score Rata-rata harmonis antara Precision dan Recall.

$$
\begin{gathered}
F 1_1=2 \times \frac{\text { Precision × Recall }}{\text { Precision }+ \text { Recall }} \\
F 1_1=2 \times \frac{1.00 \times 0.9285}{1.00+0.9285}=2 \times \frac{0.9285}{1.9285} \approx 0.9629 \ldots
\end{gathered}
$$

Dibulatkan: 0.96

#### B. Untuk Kelas "Benign" (Jinak/0)

Untuk perhitungan ini, kita balik perspektifnya: "Jinak" menjadi target positif sementara.
- TP (Jinak benar) = 72
- FP (Prediksi Jinak, padahal Ganas) = 3
- FN (Prediksi Ganas, padahal Jinak) $=0$
#### 1. Precision (Presisi)

$$
\text { Precision }_0=\frac{72}{72+3}=\frac{72}{75}=0.96
$$

#### 2. Recall (Sensitivitas)

$$
\text { Recall }_0=\frac{72}{72+0}=\frac{72}{72}=1.00
$$

#### 3. F1-Score

$$
F 1_0=2 \times \frac{0.96 \times 1.00}{0.96+1.00}=2 \times \frac{0.96}{1.96} \approx 0.979 \ldots
$$

Dibulatkan: 0.98

### Langkah 4: Menghitung Rata-Rata (Averages)
Di laporan klasifikasi (classification report), ada dua jenis rata-rata:
#### 1. Macro Average (Rata-rata Biasa) Menjumlahkan metrik kedua kelas lalu dibagi 2 (tanpa melihat jumlah data per kelas).

- Macro Precision: $(0.96+1.00) / 2=0.98$

- Macro Recall: $(1.00+0.93) / 2 \approx 0.96$ 

(0.965 dibulatkan ke genap/bawah tergantung library, di sini 0.96).

- Macro F1: $(0.98+0.96) / 2=0.97$

#### 2. Weighted Average (Rata-rata Terbobot) Mempertimbangkan jumlah data (support) tiap kelas.
- Support $0=72$
- Support $1=42$
- Total = 114

$$
\begin{gathered}
\text { Weighted } \mathrm{F} 1=\frac{\left(F 1_0 \times 72\right)+\left(F 1_1 \times 42\right)}{114} \\
\text { Weighted } \mathrm{F} 1=\frac{(0.98 \times 72)+(0.96 \times 42)}{114} \\
\text { Weighted } \mathrm{F} 1=\frac{70.56+40.32}{114}=\frac{110.88}{114} \approx 0.972
\end{gathered}
$$

Dibulatkan: 0.97

### Langkah 5: Analisis Overfitting (Evaluasi Gap)
Berdasarkan output bagian 4.1 di notebook:
#### 1. Akurasi Training: Model mempelajari data latih dengan sempurna (sering terjadi pada Random Forest tanpa pembatasan depth).
- Nilai: 100\% (1.00)
#### 2. Akurasi Testing: Model diuji dengan data yang belum pernah dilihat.
- Nilai: 97.37\% (0.9737)

Perhitungan Gap (Selisih):

$$
\begin{gathered}
\text { Gap }=\text { Akurasi Train }- \text { Akurasi Test } \\
\text { Gap }=100 \%-97.37 \%=2.63 \%
\end{gathered}
$$

Interpretasi Manual: Karena selisih (gap) hanya 2.63\% (di bawah ambang batas wajar 5\% atau $10 \%$ ), maka model disimpulkan Good Fit (Optimal). Model mampu mengenali pola dengan sangat baik tanpa hanya menghafal data latih.

#### 1. Perhitungan Manual Data Splitting (Pembagian Data)

Tujuannya adalah membagi total data menjadi dua bagian: Training Set (untuk melatih model) dan Testing Set (untuk menguji model).

Langkah-langkah:
1. Hitung Total Data ( $N$ ): Dari file data.csv , total baris data adalah 569.
2. Tentukan Rasio Split: Berdasarkan notebook, parameter yang digunakan adalah test_size=0.2 . Artinya, 20\% data untuk testing, dan sisanya 80\% untuk training.
3. Hitung Jumlah Data Testing ( ${N}_{\text {test }}$ ):

$$
\begin{gathered}
N_{\text {test }}=N \times \text { test_size } \\
N_{\text {test }}=569 \times 0.2=113.8
\end{gathered}
$$

Karena jumlah data harus bilangan bulat, Python biasanya membulatkan ke angka terdekat (biasanya ceiling atau round tergantung implementasi, dalam train_test_split sering kali diambil ceil untuk test set jika hasilnya pecahan, atau pembulatan terdekat). Dalam kasus standar sklearn:

$$
N_{\text {test }} \approx 114
$$

4. Hitung Jumlah Data Training ( ${N}_{\text {train }}$ ):

$$
\begin{gathered}
N_{\text {train }}=N-N_{\text {test }} \\
N_{\text {train }}=569-114=455
\end{gathered}
$$

Hasil Akhir:
- Total Data: 569
- Data Training $(80 \%)$ : 455 baris
- Data Testing (20\%): 114 baris

### 2. Perhitungan Manual AUC-ROC (Area Under the Curve - Receiver Operating Characteristic)

AUC-ROC mengukur seberapa baik model dapat membedakan antara dua kelas (dalam kasus ini: Tumor Ganas/Malignant vs Jinak/Benign).

Karena menghitung manual untuk 114 data testing terlalu panjang, saya akan menggunakan sampel 10 data (5 prediksi teratas dan 5 terbawah) dari hasil model Anda untuk mendemonstrasikan prosesnya. Hasil model Anda sangat bagus (AUC $\approx 0.99$ ), jadi datanya terpisah dengan sangat rapi.

Data Sampel (Simulasi dari Model Anda): Kita urutkan data berdasarkan Score Probabilitas (keyakinan model bahwa itu adalah kanker ganas/kelas 1) dari tinggi ke rendah.

$$
\begin{array}{|l|l|l|l|}
\hline \text{Peringkat} & \text{True Label } (y) & \text{Score Probabilitas } (y_{\text{score}}) & \text{Keterangan} \\
\hline 1 & 1 & 1.00 & \text{Positif (Benar)} \\
\hline 2 & 1 & 1.00 & \text{Positif (Benar)} \\
\hline 3 & 1 & 1.00 & \text{Positif (Benar)} \\
\hline 4 & 1 & 1.00 & \text{Positif (Benar)} \\
\hline 5 & 1 & 1.00 & \text{Positif (Benar)} \\
\hline \dots & \dots & \dots & \dots \\
\hline 110 & 0 & 0.00 & \text{Negatif (Benar)} \\
\hline 111 & 0 & 0.00 & \text{Negatif (Benar)} \\
\hline 112 & 0 & 0.00 & \text{Negatif (Benar)} \\
\hline 113 & 0 & 0.00 & \text{Negatif (Benar)} \\
\hline 114 & 0 & 0.00 & \text{Negatif (Benar)} \\
\hline
\end{array}
$$

Langkah-langkah:
1. Hitung Total Positif (P ) dan Negatif (N): Dari data testing Anda:
- Total Positif $(P)=42$ (Pasien Kanker Ganas)
- Total Negatif $(\mathrm{N})=72$ (Pasien Tumor Jinak)
2. Geser Threshold (Batas Ambang): Kita bayangkan sebuah garis batas. Semua data di atas garis dianggap prediksi "Positif", di bawah garis "Negatif". Kita geser garis ini dari skor tertinggi (1.0) ke terendah (0.0).
3. Hitung TPR dan FPR di setiap titik:
- TPR (True Positive Rate): Jumlah data Positif yang benar ditebak dibagi Total Positif ( $T P / P$ ). Ini adalah sumbu Y grafik ROC.
- FPR (False Positive Rate): Jumlah data Negatif yang salah ditebak sebagai Positif dibagi Total Negatif (FP/N). Ini adalah sumbu X grafik ROC.

Simulasi Pergerakan:
- Mulai (Threshold > 1.0): Belum ada yang ditebak positif.
- $T P=0, F P=0 \rightarrow$ Titik $(0,0)$.
- Turun ke Threshold 1.0 (Melewati semua data '1'):
- Model berhasil menangkap semua 42 data positif dengan skor 1.0.
- $T P=42$
- Karena data terpisah sempurna di skor 1.0 , belum ada data negatif ( 0 ) yang "tersangkut". Jadi $F P=0$.
- $T P R=42 / 42=1.0$ (Naik mentok ke atas).
- $F P R=0 / 72=0.0$.
- Titik Baru: $(0,1)$. Grafik naik tegak lurus dari $(0,0)$ ke $(0,1)$.
- Turun ke Threshold 0.0 (Melewati semua data '0'):
- Sekarang kita anggap semuanya positif. Data negatif $(0)$ mulai masuk sebagai "False Positive".
- TP tetap 42.
- FP sekarang jadi 72 (semua data negatif dianggap positif).
- $T P R=1.0$
- $F P R=72 / 72=1.0$.
- Titik Baru: $(1,1)$. Grafik bergerak mendatar ke kanan dari $(0,1)$ ke $(1,1)$.

4. Hitung Luas Area (AUC): Karena grafik membentuk kotak sempurna (naik dari 0,0 ke 0,1 lalu ke kanan ke 1,1), luasnya adalah:

$$
\text { Luas }=\text { Lebar } \text { × } \text { Tinggi }=1.0 \times 1.0=1.0
$$

Catatan: Hasil perhitungan menunjukkan 0.9975 (sedikit di bawah 1.0). Ini berarti ada satu atau dua data di tengah-tengah (tidak terlihat di sampel 10 teratas/terbawah) yang urutannya sedikit tertukar. Misalnya, ada satu data Negatif yang skornya 0.6, sedangkan ada satu data Positif yang skornya 0.55. Kesalahan urutan kecil ini mengurangi luas area sedikit dari 1.0 menjadi 0.9975.

Kesimpulan: Nilai AUC 0.9975 berarti jika Anda mengambil satu pasien acak yang sakit kanker dan satu orang acak yang sehat, model memiliki peluang 99.75\% untuk memberikan skor risiko yang lebih tinggi kepada pasien yang sakit.