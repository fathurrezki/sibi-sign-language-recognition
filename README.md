# SIBI Sign Language Recognition

Pengenalan bahasa isyarat SIBI (Sistem Isyarat Bahasa Indonesia) secara real-time dari kamera, dilayani sebagai video stream lewat Flask.

Dibuat sebagai proyek deployment computer vision, 2022.

## Cara kerja

1. Flask membuka webcam dengan OpenCV dan menyiarkan frame sebagai MJPEG stream ke halaman web
2. Setiap frame diproses **MediaPipe Hands** untuk mendeteksi tangan dan mengambil koordinat titik-titik sendinya
3. Koordinat keypoint itu menjadi fitur yang diumpankan ke model **SVM** (scikit-learn)
4. Huruf hasil prediksi digambar langsung di atas frame, bersama penghitung FPS

Fitur spasial diturunkan dari keypoint tangan, bukan dari piksel mentah, sehingga model tidak terganggu perubahan latar belakang dan pencahayaan.

## Teknologi

Python - Flask - OpenCV - MediaPipe - scikit-learn

| Berkas | Isi |
|---|---|
| `model/svm_model_v3.sav` | model SVM terlatih |
| `SIBI_Lang_Spatio.csv` | dataset keypoint tangan (7,8 MB) |

## Menjalankan secara lokal

```bash
cd "29 SEPTEMBER"
pip install -r requirements.txt
python app.py
```

Lalu buka http://127.0.0.1:5000 dan izinkan akses kamera.

Aplikasi memakai `cv2.VideoCapture(0)`, yaitu webcam bawaan. Untuk kamera lain, ganti indeksnya di `app.py`.

Dependensi dipatok ke versi 2022 (MediaPipe 0.8.10.1, OpenCV 4.6), jadi paling aman dijalankan di Python 3.9 atau 3.10.

## Struktur

```
29 SEPTEMBER/
|- app.py               server Flask dan pipeline deteksi per frame
|- model/               svm_model_v3.sav
|- SIBI_Lang_Spatio.csv dataset keypoint
|- templates/           halaman video stream
|- static/
|- requirements.txt
```

Laporan lengkap proyek ada di `Self-Learning Deployment-CV with flask.pdf`.
