# 🚀 Big Data Analytics: Web Server Log Processing & User Segmentation

Repositori ini berisi implementasi *pipeline* pemrosesan Big Data *end-to-end* menggunakan **Apache Spark (PySpark)** untuk menganalisis log akses server (*web traffic*). Proyek ini berfokus pada ekstraksi *insight* operasional, evaluasi *Service Level Agreement* (SLA), dan segmentasi perilaku pengguna menggunakan *Machine Learning* terdistribusi.

Proyek ini dikembangkan sebagai pemenuhan Evaluasi Akhir Semester Mata Kuliah Big Data, Program Studi Sains Data.

## 📊 Ikhtisar Dataset
* **Sumber:** [Kaggle - Server Logs Dataset](https://www.kaggle.com/datasets/vishnu0399/server-logs)
* **Ukuran Data:** ~500.000 baris log (*subset* operasional)
* **Format:** *Apache Combined Log Format* (Unstructured Text)
* **Atribut Utama yang Diekstrak:** IP Address, Timestamp, HTTP Method, Endpoint, Status Code, Bandwidth (Bytes), User Agent, dan Response Time (ms).

## 🏗️ Arsitektur & Pipeline Program
Sistem dirancang untuk menangani beban komputasi masif tanpa *Out-of-Memory* (OOM) dengan alur kerja 9 tahap:
1. **Inisialisasi Environment:** Konfigurasi `SparkSession` dengan *Adaptive Query Execution* (AQE) dan manajemen partisi.
2. **Data Ingestion:** Pemrosesan *Unstructured RDD*.
3. **Data Preprocessing (ETL):** Parsing menggunakan *Regular Expression* (Regex) dan manipulasi kurva normal temporal (*Central Limit Theorem*).
4. **Data Caching:** Retensi memori untuk optimasi *Lazy Evaluation*.
5. **Deskriptif Analitik (Spark SQL):** Evaluasi agregat latensi P50, P95, P99 menggunakan algoritma *HyperLogLog*.
6. **Advanced Analytics (Window Function):** Deteksi *Micro-Bursts* berbasis pergerakan waktu.
7. **Machine Learning (MLlib):** Implementasi K-Means Clustering dengan optimasi dimensi (*Silhouette Analysis*).
8. **Data Rendering:** Visualisasi hasil menggunakan integrasi `.toPandas()`, `Matplotlib`, dan `Seaborn`.

## 💡 Temuan Analitik Utama
1. **Kritisnya Latensi Server:** Sebaran trafik yang merata pada *endpoint* utama diiringi degradasi latensi yang masif. Lebih dari 70% permintaan diselesaikan di atas 1,5 detik, dengan $P_{95}$ menyentuh 4.750 ms (mendekati *Alert Threshold*).
2. **Kegagalan Sistemik (57% Error Rate):** Distribusi *error* terpecah rata antara *Client Error* (4xx) dan *Server Error* (5xx), mengindikasikan beban berat pada *backend/database relasional*.
3. **Polarisasi Trafik:** *Peak hour* terdeteksi pada 14:00 (46.585 *requests*) berbanding terbalik dengan *Quiet hour* pada 02:00 (54 *requests*), menuntut implementasi *Auto-Scaling*.
4. **Segmentasi Perilaku (K=6 Optimal):** K-Means berhasil membedah dimensi latensi menjadi 6 profil klaster (*Silhouette Score: 0.7633*), mulai dari *Bottlenecked Success*, *Timeout Failures*, hingga isolasi akurat terhadap 38 IP *Suspicious Bots*.

## 🛠️ Prasyarat & Instalasi
Proyek ini dijalankan di lingkungan **Google Colaboratory** atau klaster Spark lokal. 

Dependencies utama yang dibutuhkan (lihat `requirements.txt`):
```bash
pyspark==3.5.1
pandas
matplotlib
seaborn
