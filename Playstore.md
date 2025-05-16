# Dokumentasi Proses Scraping Ulasan Aplikasi dari Google Play Store

Catatan ini mendokumentasikan tahapan dan kode yang dieksekusi dalam lingkungan Google Colaboratory untuk melakukan scraping (pengambilan data) ulasan aplikasi dari Google Play Store. Proses ini memanfaatkan library Python `google-play-scraper` untuk mengambil data ulasan dan `pandas` untuk mengolah serta menyimpannya ke dalam format CSV.

**Tujuan Proses:** Mendapatkan 15000 ulasan terbaru dari aplikasi `com.openai.chatgpt` di Google Play Store, dengan preferensi bahasa ulasan Bahasa Indonesia (lang='id') dari region 'us', dan menyimpannya dalam format file CSV.

**Lingkungan dan Alat yang Digunakan:**
*   Google Colaboratory sebagai lingkungan eksekusi Python.
*   Akses internet untuk mengunduh library dan mengambil data dari Google Play Store.
*   Library Python:
    *   `google-play-scraper`
    *   `pandas`

## Tahapan Proses yang Dilakukan:

1.  **Instalasi Library yang Dibutuhkan:**
    *   Library `google-play-scraper` diinstal dalam sesi Google Colaboratory menggunakan perintah berikut dalam sel kode:
        ```python
        !pip install -q google-play-scraper
        ```
    *   Tanda seru (`!`) menunjukkan bahwa perintah ini dieksekusi sebagai perintah shell dalam sel kode Google Colaboratory.

2.  **Impor Library ke dalam Script Python:**
    *   Library yang esensial untuk proses scraping dan pengolahan data diimpor ke dalam script:
        ```python
        import pandas as pd
        from google_play_scraper import app, Sort, reviews
        ```

3.  **Proses Scraping Ulasan Aplikasi:**
    *   Fungsi `reviews` dari library `google-play-scraper` dipanggil untuk mengambil ulasan dari aplikasi target.
    *   Parameter yang dikonfigurasi untuk panggilan fungsi ini adalah:
        *   `app_id`: `'com.openai.chatgpt'` (ID aplikasi target).
        *   `lang`: `'id'` (mengindikasikan preferensi untuk ulasan dalam Bahasa Indonesia).
        *   `country`: `'us'` (menentukan region Play Store yang diakses adalah Amerika Serikat. *Catatan: Kombinasi bahasa 'id' dan negara 'us' mungkin menghasilkan ulasan dalam bahasa Indonesia jika tersedia dari pengguna di region tersebut, atau dapat default ke bahasa lain jika ulasan Bahasa Indonesia tidak dominan/tersedia di region 'us' untuk aplikasi ini.*).
        *   `sort`: `Sort.NEWEST` (menginstruksikan untuk mengambil ulasan yang paling baru).
        *   `count`: `500` (menetapkan jumlah maksimum ulasan yang akan diambil).
        ```python
        rvws, token = reviews(
            'com.openai.chatgpt',
            lang='id',
            country='us',
            sort=Sort.NEWEST,
            count = 15000
        )
        ```
    *   Variabel `rvws` menampung daftar ulasan yang berhasil diambil. Variabel `token` (untuk pagination) tidak digunakan lebih lanjut dalam alur kerja ini.

4.  **Konversi Data Ulasan ke DataFrame Pandas:**
    *   Daftar ulasan yang disimpan dalam `rvws` dikonversi menjadi struktur DataFrame menggunakan library Pandas untuk memudahkan analisis dan penyimpanan.
        ```python
        rvws_df = pd.DataFrame(rvws)
        ```

5.  **Penyimpanan DataFrame ke File CSV:**
    *   DataFrame `rvws_df` yang berisi data ulasan kemudian diekspor dan disimpan sebagai file CSV dengan nama `reviews.csv`.
    *   Parameter `index=None` digunakan untuk mencegah penulisan indeks DataFrame ke dalam file CSV.
    *   Parameter `header=True` memastikan bahwa nama kolom disertakan sebagai baris pertama (header) dalam file CSV.
        ```python
        rvws_df.to_csv('reviews.csv', index = None, header= True)
        ```
    *   File `reviews.csv` ini kemudian dapat diunduh dari lingkungan Google Colaboratory. File ini berisi detail dari setiap ulasan yang berhasil di-scrape.

---

Proses scraping 500 ulasan terbaru untuk aplikasi `com.openai.chatgpt` dari Google Play Store (region 'us', preferensi bahasa 'id') telah selesai dilaksanakan di Google Colaboratory. Hasilnya telah disimpan dalam file `reviews.csv`.
