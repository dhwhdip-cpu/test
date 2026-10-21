---
type: 'BigData'
collections: '20261'
title: '#8 Batch Processing'
createdAt: '2026-08-05T03:31:26.273Z'
lastUpdated: '2026-10-19T06:37:20.380Z'
tags: []
url: null
---

# #8 Batch Processing

# MapReduce

Bayangkan menghitung jumlah suara dalam pemilu dengan ribuan TPS. Setiap TPS menghitung sendiri (**Map**), lalu hasilnya dikumpulkan dan dijumlahkan (**Reduce**).

- **Map**: memecah data besar menjadi potongan kecil dan memprosesnya secara paralel. Hasilnya berupa pasangan *key-value*, misalnya `("kucing", 1)`.

- **Shuffle**: mengelompokkan hasil Map berdasarkan key yang sama.

- **Reduce**: menggabungkan nilai per key menjadi hasil akhir, misalnya `("kucing", 250)`.

Contoh klasik adalah **word count**, yaitu menghitung berapa kali setiap kata muncul di jutaan dokumen.

# Distributed Computing

Komputasi terdistribusi berarti **satu pekerjaan besar dibagi ke banyak komputer (node)** yang bekerja bersamaan lewat jaringan, dan hasilnya digabungkan.

Analogi: membangun rumah dengan 50 tukang akan lebih cepat daripada satu tukang yang bekerja sendirian.

- **Keuntungan**: lebih cepat, bisa menangani data yang tidak muat di satu mesin, mudah ditambah kapasitasnya (*scalable*).

- **Tantangan**: koordinasi antar node, kecepatan jaringan, dan kemungkinan ada node yang rusak.

# Fault Tolerance

Dengan ratusan atau ribuan mesin, **kerusakan adalah hal biasa**. Fault tolerance adalah kemampuan sistem untuk **tetap berjalan dan menghasilkan hasil yang benar meskipun sebagian komponennya gagal**.

Cara umumnya:

- **Replikasi data**: data disalin ke beberapa node, sehingga jika satu mati, salinan lain masih ada (misalnya HDFS menyimpan 3 salinan).

- **Retry/re-execution**: tugas yang gagal dijalankan ulang di node lain.

- **Heartbeat**: node rutin melapor “saya masih hidup”; jika berhenti, master menganggapnya gagal dan memindahkan tugasnya.

Analogi: jika satu tukang sakit, pekerjaannya dialihkan ke tukang lain tanpa proyek berhenti.

# Resilient Distributed Dataset

RDD adalah struktur data inti di **Apache Spark**, yaitu kumpulan data yang **terbagi ke banyak node (distributed)** dan **bisa dipulihkan otomatis jika hilang (resilient)**.

Ciri utama:

- **Immutable**: tidak bisa diubah; setiap transformasi menghasilkan RDD baru.

- **Partitioned**: data dipecah ke beberapa bagian (partisi) yang diproses paralel.

- **Lineage**: Spark mencatat “resep” bagaimana RDD dibuat. Jika satu partisi hilang, Spark tinggal **menghitung ulang** dari resep itu tanpa perlu replikasi.

- **Lazy evaluation**: transformasi (`map`, `filter`) baru dijalankan saat ada *action* (`count`, `collect`).

- **In-memory**: data disimpan di memori sehingga jauh lebih cepat dibanding MapReduce yang banyak menulis ke disk.


