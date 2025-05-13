# Dokumentasi Proses Scraping Komentar YouTube Menggunakan YouTube Data API v3

Catatan ini mendokumentasikan langkah-langkah yang dilakukan untuk scraping (pengambilan data) komentar dari video YouTube. Proses ini menggunakan YouTube Data API v3 melalui Google Cloud Platform dan dieksekusi menggunakan Google Colaboratory. Video target dalam proses ini ditemukan melalui pencarian dengan kata kunci `#chatgpt`.

**Tujuan Proses:** Mendapatkan daftar komentar dari video YouTube spesifik yang relevan dengan `#chatgpt` dan menyimpannya dalam format file CSV.

**Alat/Kondisi yang Digunakan:**
*   Akun Google.
*   Akses internet.
*   Google Cloud Platform (untuk API Key).
*   Google Colaboratory (untuk eksekusi kode Python).

## Tahapan Proses yang Dilakukan:

### Bagian 1: Pengaturan Google Cloud Platform

1.  **Akses Google Cloud Console:**
    *   Halaman [Google Cloud Console](https://console.cloud.google.com/) dikunjungi.
    *   Login dilakukan menggunakan Akun Google yang relevan jika diminta.

2.  **Pemilihan atau Pembuatan Proyek Baru:**
    *   Dropdown **"Select a project"** di bagian atas halaman diklik.
    *   Sebuah dialog muncul.
        *   **Jika belum ada proyek:** Tombol **"+ NEW PROJECT"** diklik. Nama proyek yang sesuai dimasukkan (misalnya, "YouTube Scraper Project"). Organisasi dipilih (jika relevan) atau dibiarkan "No organization". Tombol **"CREATE"** diklik.
        *   **Jika sudah ada proyek:** Proyek yang akan digunakan dipilih dari daftar yang tersedia.

3.  **Akses Menu Library API:**
    *   Setelah proyek terpilih atau terbuat, ikon **Navigation Menu** (tiga garis horizontal) di pojok kiri atas diklik.
    *   Kursor diarahkan ke menu **"APIs & Services"**.
    *   **"Library"** diklik dari submenu yang muncul.

4.  **Pencarian YouTube Data API v3:**
    *   Halaman API Library terbuka.
    *   Pada kolom pencarian (`Search for APIs & Services`), `YouTube Data API v3` diketik.
    *   Tombol **Enter** ditekan atau hasil pencarian yang relevan diklik.

5.  **Aktivasi YouTube Data API v3:**
    *   Hasil pencarian **"YouTube Data API v3"** diklik.
    *   Pada halaman detail API, tombol **"ENABLE"** diklik. Proses aktivasi ditunggu beberapa saat hingga selesai.

6.  **Pembuatan Kunci API (API Key):**
    *   Setelah API diaktifkan, halaman detail API tersebut terbuka.
    *   Di menu sebelah kiri, **"Credentials"** diklik.
    *   Di bagian atas halaman Credentials, tombol **"+ CREATE CREDENTIALS"** diklik.
    *   **"API key"** dipilih dari dropdown yang muncul.

7.  **Penyalinan Kunci API (API Key):**
    *   Sebuah pop-up muncul menampilkan API Key yang baru dibuat.
    *   Ikon **"Copy"** di sebelah kanan API Key diklik untuk menyalinnya ke clipboard.
    *   **Catatan:** API Key ini disimpan di tempat yang aman dan tidak dibagikan secara publik karena akan digunakan dalam kode Python.
    *   Tombol **"Close"** diklik. (Pembatasan kunci API dapat dilakukan nanti untuk keamanan tambahan).

### Bagian 2: Pengambilan Komentar dengan Google Colaboratory

8.  **Persiapan Google Colab Notebook:**
    *   [Google Colaboratory](https://colab.research.google.com/) dikunjungi.
    *   Notebook baru dibuat melalui **"File" > "New notebook"**.

9.  **Identifikasi dan Penyalinan ID Video YouTube Target:**
    *   Pencarian dilakukan di YouTube menggunakan kata kunci `#chatgpt` untuk menemukan video yang relevan.
    *   Halaman spesifik dari video target yang ditemukan dibuka di browser.
    *   URL video pada address bar diperiksa (formatnya seperti `https://www.youtube.com/watch?v=VIDEO_ID` atau `https://www.youtube.com/watch?v=VIDEO_ID&ab_channel=ChannelName`).
    *   Bagian **`VIDEO_ID`** (string unik setelah `v=` dan sebelum `&` jika ada) dari URL disalin.
    *   **Contoh:** Untuk URL `https://www.youtube.com/watch?v=z-U1r4k5r2s&ab_channel=LexClips`, `VIDEO_ID` adalah `z-U1r4k5r2s`.

10. **Input Kode Python ke Colab:**
    *   Kembali ke notebook Google Colab yang telah disiapkan.
    *   Kode Python berikut disalin dan ditempelkan ke dalam sel kode pertama:

    ```python
    # 1. Import libraries
    import googleapiclient.discovery
    import pandas as pd
    import os # Opsional: jika ingin menyimpan ke Google Drive

    # 2. Set up API Key, Video ID, and Max Results
    # !!! Placeholder untuk API Key yang didapatkan dari GCP !!!
    API_KEY = "MASUKKAN_API_KEY_DISINI"

    # !!! Placeholder untuk Video ID target yang didapatkan dari YouTube !!!
    videoId = "MASUKKAN_VIDEO_ID_DISINI"

    # Jumlah maksimal komentar yang diambil per request (maks 100)
    # Untuk >100 komentar, perlu implementasi pagination
    maxResults = 100 # Angka ini bisa diubah (misal: 40, 50, 100)

    # 3. Build the YouTube API service object
    api_service_name = "youtube"
    api_version = "v3"
    try:
        youtube = googleapiclient.discovery.build(
            api_service_name, api_version, developerKey=API_KEY)

        # 4. Request comments from the specified video
        request = youtube.commentThreads().list(
            part="snippet", # Mengambil bagian snippet
            videoId=videoId,
            maxResults=maxResults,
            order="relevance" # Urutan komentar ('relevance' atau 'time')
        )

        # 5. Execute the request
        response = request.execute()

        # 6. Extract comments from the response
        comments = []
        if 'items' in response:
            for item in response['items']:
                comment = item['snippet']['topLevelComment']['snippet']
                comments.append([
                    comment['authorDisplayName'],
                    comment['publishedAt'],
                    comment['updatedAt'],
                    comment['likeCount'],
                    comment['textDisplay'] # Teks dengan format HTML dasar
                    # comment['textOriginal'] # Opsi: Teks asli
                ])
        else:
            print("Tidak ada komentar ditemukan atau terjadi kesalahan.")

        # 7. Create a Pandas DataFrame
        df = pd.DataFrame(comments, columns=[
            'author', 'published_at', 'updated_at', 'like_count', 'text'
            ])

        # 8. Define filename and save to CSV
        output_filename = f'scraped_youtube_comments_{videoId}.csv'
        df.to_csv(output_filename, index=False, encoding='utf-8-sig') # encoding utf-8-sig

        # 9. Display confirmation and data preview
        print(f"Data berhasil disimpan sebagai: {output_filename}")
        print("Lokasi penyimpanan: Direktori utama sesi Colab (lihat panel Files di kiri).")
        print("\nPreview Data:")
        print(df.head())

    except googleapiclient.errors.HttpError as e:
        print(f"Terjadi kesalahan saat memanggil API: {e}")
        print("Pastikan API Key benar, API YouTube Data v3 aktif,")
        print("dan Video ID valid serta memiliki komentar yang publik.")
    except Exception as e:
        print(f"Terjadi kesalahan lain: {e}")

    ```

11. **Pengisian API Key dan Video ID pada Kode:**
    *   Dalam kode yang ditempelkan, baris `API_KEY = "MASUKKAN_API_KEY_DISINI"` ditemukan.
    *   Placeholder `"MASUKKAN_API_KEY_DISINI"` diganti dengan **API Key** aktual yang disalin dari Google Cloud Console (Tahap 7), tetap dalam tanda kutip (`"`).
    *   Baris `videoId = "MASUKKAN_VIDEO_ID_DISINI"` ditemukan.
    *   Placeholder `"MASUKKAN_VIDEO_ID_DISINI"` diganti dengan **Video ID** aktual yang disalin dari YouTube (Tahap 9), tetap dalam tanda kutip (`"`).

12. **Penentuan Jumlah Komentar (Opsional):**
    *   Nilai variabel `maxResults` dapat disesuaikan untuk mengatur jumlah maksimum komentar yang diambil per permintaan (maksimum 100). Kode ini disetel ke 100.
    *   *Catatan: Pengambilan lebih dari 100 komentar memerlukan implementasi logika *pagination*.*

13. **Eksekusi Kode dan Pengunduhan Hasil:**
    *   Tombol **"Run"** (ikon play ►) di sebelah kiri sel kode diklik, atau `Shift + Enter` ditekan.
    *   Proses eksekusi kode ditunggu hingga selesai. Output akan muncul di bawah sel kode, menampilkan preview data dan konfirmasi penyimpanan file.
    *   Setelah selesai, panel **"Files"** (ikon folder) di sebelah kiri jendela Colab diperiksa.
    *   File CSV baru dengan nama seperti `scraped_youtube_comments_VIDEO_ID.csv` akan terlihat.
    *   Ikon **tiga titik vertikal** (⋮) di sebelah nama file tersebut diklik.
    *   **"Download"** dipilih untuk menyimpan file CSV ke penyimpanan lokal.

14. **Penyimpanan Hasil dalam File CSV:**
    *   File CSV yang terunduh berisi data komentar yang berhasil di-scrape dari video YouTube yang ditargetkan, termasuk detail seperti penulis, waktu publikasi, jumlah suka, dan teks komentar. File ini siap dibuka dan dianalisis menggunakan perangkat lunak spreadsheet.

---

Proses scraping komentar dari video YouTube yang ditemukan via `#chatgpt` menggunakan YouTube Data API v3 telah selesai, dan data berhasil disimpan sebagai file CSV.
