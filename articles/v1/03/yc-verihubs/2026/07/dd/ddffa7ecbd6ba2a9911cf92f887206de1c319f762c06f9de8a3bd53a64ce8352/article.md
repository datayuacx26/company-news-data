---
schema_version: "1.0.0"
document_id: "ddffa7ecbd6ba2a9911cf92f887206de1c319f762c06f9de8a3bd53a64ce8352"
company_key: "yc-verihubs"
company: "Verihubs"
source_id: "yc-verihubs-news-import-dc58b182d48b"
canonical_url: "https://verihubs.com/blog/fraud-scoring-adalah"
published_at: "2026-07-29T18:56:41+00:00"
first_seen_at: "2026-07-29T14:33:08.972338+00:00"
fetched_at: "2026-07-29T18:56:44.308009+00:00"
content_hash: "sha256:944149959d69b3a02e81c35db1be121cff25316d580aa4f099607cca2bfb4a6a"
---

# Fraud Scoring Adalah: Cara Kerja, Algoritma, dan Evaluasi Modelnya

Fraud scoring adalah metode penilaian risiko yang memberi skor numerik pada setiap transaksi atau pendaftaran untuk memperkirakan kemungkinan fraud secara real-time. Skor dihitung model machine learning dari puluhan hingga ratusan fitur perilaku dan identitas, lalu dibandingkan dengan ambang keputusan untuk meloloskan, menolak, atau meninjau manual.


Artikel ini fokus pada sisi model dan algoritma: bagaimana skor dihitung, dievaluasi, dan dijelaskan. Untuk konsep dasarnya lihat[fraud](https://verihubs.com/blog/apa-itu-fraud-pengertian-ciri-ciri-dan-dampaknya) , untuk arsitektur sistem menyeluruh lihat[fraud detection system](https://verihubs.com/blog/fraud-detection-system) , dan untuk proses penilaian risiko di tingkat organisasi lihat[fraud risk assessment](https://verihubs.com/blog/fraud-risk-assessment) .


## Apa Itu Fraud Scoring dan Bagaimana Skor Dihitung


Fraud scoring mengubah pertanyaan biner “apakah ini penipuan” menjadi ukuran probabilitas berskala, umumnya 0 sampai 100 atau 0 sampai 1. Model menerima vektor fitur, misalnya kecepatan pengisian formulir, riwayat perangkat, kesesuaian data identitas, dan pola transaksi, lalu mengeluarkan skor risiko dalam hitungan milidetik.


Skor itu sendiri tidak mengambil keputusan. Tim risiko menetapkan ambang: di bawah ambang bawah diloloskan otomatis, di atas ambang atas ditolak, dan di antaranya masuk antrean manual review. Menggeser ambang berarti menukar antara fraud yang lolos dan pengguna sah yang terhambat.


## Rule-Based vs Machine Learning dalam Fraud Scoring


Sebagian besar organisasi memulai dengan aturan tetap, lalu bermigrasi ke model pembelajaran mesin saat pola fraud makin adaptif.


Aspek Rule-Based Machine Learning


Cara kerja Kondisi eksplisit yang ditulis analis Pola dipelajari dari data historis


Transparansi Sangat tinggi Perlu teknik penjelasan tambahan


Adaptasi pola baru Lambat, perlu penulisan ulang Cepat melalui pelatihan ulang


Interaksi antarfitur Terbatas Menangkap interaksi kompleks


Beban pemeliharaan Menumpuk seiring jumlah aturan Bergeser ke pemantauan model


Pendekatan yang matang menggabungkan keduanya. Aturan menangani kasus kepatuhan yang wajib deterministik, sementara model menangkap pola halus yang tidak terbaca aturan.


## Algoritma yang Umum Dipakai untuk Fraud Scoring


Pilihan algoritma menentukan keseimbangan antara akurasi, kecepatan inferensi, dan kemudahan penjelasan.


Algoritma Kekuatan Pertimbangan


Logistic Regression Sangat mudah dijelaskan, cepat Terbatas pada hubungan linier


Random Forest Tahan outlier, menangkap non-linearitas Ukuran model besar


Gradient Boosting seperti XGBoost Akurasi tinggi pada data tabular Perlu penyetelan hiperparameter


Neural Network Menangkap pola urutan dan interaksi rumit Butuh data besar, sulit dijelaskan


Anomaly Detection seperti Isolation Forest Bekerja tanpa label fraud Rentan positif palsu


Untuk data tabular pada kasus fraud, gradient boosting sering menjadi titik awal terbaik karena akurasinya tinggi dan waktu inferensinya masih memenuhi kebutuhan real-time.


## Mengevaluasi Model Fraud Scoring


Fraud adalah kejadian langka. Jika hanya 1 dari 1.000 transaksi berupa fraud, model yang meloloskan semuanya tetap terlihat 99,9 persen akurat namun tidak berguna. Karena itu evaluasi memakai metrik yang sensitif terhadap ketidakseimbangan kelas.


### AUC-ROC dan Precision-Recall


AUC-ROC mengukur kemampuan model membedakan kelas pada seluruh ambang. Untuk data sangat tidak seimbang, kurva Precision-Recall lebih informatif karena menyoroti performa pada kelas minoritas, yaitu fraud itu sendiri.


### Trade-off Precision dan Recall


Precision tinggi berarti sedikit pengguna sah yang salah ditolak. Recall tinggi berarti sedikit fraud yang lolos. Menaikkan salah satunya menurunkan yang lain, sehingga ambang harus dipilih berdasarkan biaya bisnis nyata dari masing-masing kesalahan.


## Explainable AI untuk Fraud Scoring: SHAP dan LIME


Regulator dan tim kepatuhan menuntut alasan di balik penolakan. Explainable AI menjawab kebutuhan ini. SHAP menghitung kontribusi setiap fitur terhadap skor akhir berdasarkan teori permainan kooperatif, sementara LIME membangun model sederhana di sekitar satu keputusan untuk menjelaskannya secara lokal.


Dalam praktik operasional, keluaran SHAP membantu analis memahami mengapa satu pendaftaran diberi skor tinggi, misalnya karena kombinasi perangkat baru, kecepatan pengisian tidak wajar, dan ketidaksesuaian data identitas. Penjelasan ini mempercepat manual review sekaligus memenuhi tuntutan audit.


## Arsitektur Operasional: Feature Store, Streaming, dan Model Drift


Model yang baik gagal di produksi jika arsitekturnya lemah. Tiga komponen menentukan keberhasilan penerapan fraud scoring.


Feature store menyimpan definisi fitur secara terpusat sehingga fitur yang dipakai saat pelatihan identik dengan saat inferensi, mencegah training-serving skew. Pemrosesan streaming menghitung fitur berbasis waktu secara real-time, misalnya jumlah percobaan dalam sepuluh menit terakhir, sedangkan pemrosesan batch menyiapkan fitur agregat historis. Keduanya biasanya berjalan berdampingan.


### Model Drift dan Pemantauan Berkelanjutan


Pelaku fraud beradaptasi, sehingga distribusi data bergeser dan performa model menurun seiring waktu. Fenomena ini disebut model drift. Pemantauan wajib mencakup distribusi skor, tingkat persetujuan, performa pada label yang terlambat tiba, dan pemicu pelatihan ulang terjadwal.


## Fraud Scoring Bekerja Setelah Identitas Terverifikasi


Fraud scoring menilai perilaku dan konteks, tetapi tidak dapat memastikan bahwa orang di balik akun benar-benar pemilik identitas tersebut. Kedua lapisan ini saling melengkapi:[verifikasi identitas](https://verihubs.com/blog/verifikasi-identitas) menutup pintu masuk, fraud scoring memantau apa yang terjadi setelahnya.


Verihubs memperkuat lapisan gerbang tersebut melalui OCR KTP, face matching, dan[liveness detection](https://verihubs.com/blog/liveness-detection) , dilengkapi Deepfake Detection yang bekerja pada tingkat keberhasilan 95% sebagai satu-satunya penyedia deteksi deepfake lokal di Indonesia. Face Recognition Verihubs tercatat berakurasi 99,9% dan diakui NIST sebagai peringkat pertama di Indonesia. Ketika identitas sudah tervalidasi kuat di awal, sinyal yang masuk ke model fraud scoring menjadi jauh lebih bersih, sehingga skor lebih dapat dipercaya.


## Pertanyaan yang Sering Diajukan tentang Fraud Scoring


### Apa itu fraud scoring?


Fraud scoring adalah metode penilaian risiko yang memberi skor numerik pada setiap transaksi atau pendaftaran untuk memperkirakan kemungkinan fraud, dihitung model machine learning dari sejumlah fitur perilaku dan identitas secara real-time.


### Apa beda fraud scoring dan fraud detection system?


Fraud scoring adalah komponen penilaian yang menghasilkan skor risiko, sedangkan fraud detection system adalah keseluruhan sistem yang mencakup pengumpulan data, aturan, model scoring, alur keputusan, dan penanganan kasus.


### Algoritma apa yang terbaik untuk fraud scoring?


Tidak ada yang terbaik untuk semua kasus. Gradient boosting seperti XGBoost sering menjadi titik awal kuat untuk data tabular, sedangkan logistic regression dipilih bila kemudahan penjelasan menjadi prioritas utama.


### Mengapa akurasi bukan metrik yang tepat untuk model fraud?


Karena fraud sangat langka. Model yang meloloskan semua transaksi bisa tampak sangat akurat tetapi tidak menangkap satu pun fraud. Evaluasi sebaiknya memakai AUC-ROC serta kurva Precision-Recall yang sensitif terhadap kelas minoritas.


### Apa itu model drift dalam fraud scoring?


Model drift adalah penurunan performa model akibat perubahan pola data seiring waktu, terutama karena pelaku fraud beradaptasi. Penanganannya melalui pemantauan distribusi skor dan pelatihan ulang berkala.


### Bagaimana menjelaskan keputusan model fraud scoring kepada auditor?


Gunakan teknik Explainable AI seperti SHAP untuk menghitung kontribusi tiap fitur terhadap skor, atau LIME untuk menjelaskan satu keputusan secara lokal. Keluarannya memberi alasan konkret di balik setiap penolakan.


## Fraud Scoring Menjadi Andal Ketika Data Identitasnya Bersih


Pembahasan fraud scoring biasanya berhenti pada pemilihan algoritma, padahal faktor penentu terbesarnya ada di kualitas sinyal yang masuk. Model gradient boosting terbaik pun menghasilkan skor rapuh bila identitas penggunanya tidak pernah divalidasi dengan benar sejak pendaftaran. Karena itu urutannya penting: kuatkan verifikasi identitas di gerbang, baru optimalkan model penilaian risiko di belakangnya. Organisasi yang membalik urutan ini akan terus menyetel ambang tanpa pernah menyentuh akar masalahnya.


Ingin memastikan sinyal identitas yang masuk ke sistem fraud scoring Anda sudah tervalidasi sejak onboarding?[Diskusikan arsitektur verifikasi identitas dengan tim Verihubs](https://verihubs.com/kontak) yang telah dipercaya lebih dari 400 klien di Indonesia.
