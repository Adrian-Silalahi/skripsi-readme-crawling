# Dokumentasi Proses Scraping Ulasan Aplikasi dari Google Play Store

Catatan ini mendokumentasikan langkah-langkah dan kode yang digunakan untuk melakukan scraping (pengambilan data) ulasan aplikasi dari Google Play Store. Proses ini memanfaatkan library Python `google-play-scraper` untuk mengambil data ulasan dan `pandas` untuk mengolah serta menyimpannya ke dalam format CSV. Selain itu, terdapat setup untuk penyimpanan data ke MongoDB, meskipun pada kode akhir, data hanya disimpan ke CSV.

**Tujuan Proses:** Mendapatkan sejumlah besar ulasan (dalam kasus ini, 15.000 ulasan terbaru) dari aplikasi spesifik di Google Play Store (dalam contoh ini, aplikasi dengan ID `com.openai.chatgpt`) dan menyimpannya dalam format file CSV untuk analisis lebih lanjut.

**Alat/Kondisi yang Digunakan:**
*   Lingkungan Python (misalnya, dijalankan di Jupyter Notebook, Google Colaboratory, atau script Python lokal).
*   Akses internet untuk mengunduh library dan mengambil data dari Google Play Store.
*   Library Python:
    *   `google-play-scraper`
    *   `pandas`
    *   `pymongo` (meskipun tidak digunakan untuk output akhir di kode yang diberikan, setupnya ada)
    *   `pprint`
    *   `datetime`
    *   `tzlocal`
    *   `random`
    *   `time`

## Tahapan Proses yang Dilakukan:

1.  **Instalasi Library yang Dibutuhkan:**
    *   Library `google-play-scraper` dan `pymongo` diinstal menggunakan pip. Perintah yang dijalankan adalah:
        ```bash
        !pip install -q google-play-scraper
        !pip install pymongo
        ```
    *   Tanda seru (`!`) mengindikasikan bahwa perintah ini dijalankan dari dalam lingkungan seperti Jupyter Notebook atau Google Colab. Jika dijalankan dari terminal standar, tanda seru dihilangkan.

2.  **Impor Library ke dalam Script Python:**
    *   Semua library yang diperlukan untuk proses scraping, pengolahan data, dan koneksi ke database (opsional) diimpor:
        ```python
        import pandas as pd
        from google_play_scraper import app, Sort, reviews
        from pprint import pprint
        import pymongo
        from pymongo import MongoClient
        import datetime as dt
        from tzlocal import get_localzone
        import random
        import time
        ```

3.  **Setup Koneksi ke MongoDB (Opsional, tidak digunakan untuk output akhir):**
    *   Meskipun data akhir disimpan ke CSV, kode menyertakan setup untuk koneksi ke database MongoDB lokal:
        ```python
        # Set up Mongo client
        client = MongoClient(host='localhost', port=27017)

        # Database for project
        app_proj_db = client['app_proj_db']

        # Set up new collection within project db for app info
        info_collection = app_proj_db['info_collection']

        # Set up new collection within project db for app reviews
        review_collection = app_proj_db['review_collection']
        ```
    *   Bagian ini dapat diabaikan jika target penyimpanan hanya file CSV.

4.  **Proses Scraping Ulasan Aplikasi:**
    *   Fungsi `reviews` dari library `google-play-scraper` digunakan untuk mengambil ulasan dari aplikasi target.
    *   Parameter yang dikonfigurasi untuk pengambilan ulasan adalah:
        *   `app_id`: `'com.openai.chatgpt'` (ID aplikasi yang dapat ditemukan pada URL aplikasi di Google Play Store).
        *   `lang`: `'en'` (bahasa ulasan yang diambil, diatur ke Bahasa Inggris).
        *   `country`: `'us'` (negara dari Play Store yang diakses, diatur ke Amerika Serikat).
        *   `sort`: `Sort.NEWEST` (ulasan diurutkan berdasarkan yang terbaru).
        *   `count`: `15000` (jumlah maksimum ulasan yang ingin diambil).
        ```python
        rvws, token = reviews(
            'com.openai.chatgpt',  # app's ID
            lang='en',
            country='us',
            sort=Sort.NEWEST,
            count = 15000
        )
        ```
    *   Variabel `rvws` akan berisi daftar ulasan, dan `token` digunakan untuk pagination jika diperlukan pengambilan data lebih lanjut (tidak dieksplorasi lebih lanjut dalam kode ini).

5.  **Konversi Data Ulasan ke DataFrame Pandas:**
    *   Daftar ulasan yang telah di-scrape (`rvws`) dikonversi menjadi DataFrame Pandas untuk kemudahan manipulasi dan penyimpanan.
        ```python
        rvws_df = pd.DataFrame(rvws)
        ```

6.  **Penyimpanan DataFrame ke File CSV:**
    *   DataFrame yang berisi ulasan aplikasi disimpan ke dalam file CSV dengan nama `reviews_us_15000.csv`.
    *   `index=None` digunakan agar indeks DataFrame tidak ditulis ke file CSV.
    *   `header=True` digunakan untuk menyertakan nama kolom sebagai baris header di file CSV.
        ```python
        rvws_df.to_csv('reviews_us_15000.csv', index = None, header= True)
        ```
    *   File CSV ini akan berisi detail dari setiap ulasan, seperti ID ulasan, nama pengguna, skor rating, teks ulasan, tanggal, dll.

---

Proses scraping ulasan aplikasi dari Google Play Store telah selesai. Data ulasan untuk aplikasi `com.openai.chatgpt` (sebanyak 15.000 ulasan terbaru dari region US berbahasa Inggris) telah berhasil diambil dan disimpan dalam file `reviews_us_15000.csv` untuk analisis lebih lanjut.
