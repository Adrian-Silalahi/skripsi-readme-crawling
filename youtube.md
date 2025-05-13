# Panduan Scraping Komentar YouTube Menggunakan YouTube Data API v3

Dokumentasi ini menjelaskan langkah-langkah untuk melakukan scraping (pengambilan data) komentar dari video YouTube menggunakan YouTube Data API v3 melalui Google Cloud Platform dan Google Colaboratory.

**Tujuan:** Mendapatkan daftar komentar dari video YouTube tertentu dan menyimpannya dalam format file CSV.

**Prasyarat:**

- Akun Google.
- Akses internet.

## Langkah-langkah Rinci:

### Bagian 1: Pengaturan Google Cloud Platform

1.  **Buka Google Cloud Console:**

    - Kunjungi halaman [Google Cloud Console](https://console.cloud.google.com/).
    - Login menggunakan Akun Google Anda jika diminta.

2.  **Pilih atau Buat Proyek Baru:**

    - Di bagian atas halaman, klik dropdown **"Select a project"** (Pilih Proyek).
    - Sebuah dialog akan muncul.
      - **Jika Anda belum memiliki proyek:** Klik tombol **"+ NEW PROJECT"** (Proyek Baru).
        - Masukkan nama proyek yang diinginkan (misalnya, "YouTube Scraper Project").
        - Pilih Organization (jika ada/diperlukan) atau biarkan "No organization".
        - Klik **"CREATE"**.
      - **Jika Anda sudah memiliki proyek:** Pilih proyek yang ingin Anda gunakan dari daftar yang tersedia.

3.  **Akses Menu Library API:**

    - Setelah proyek dipilih atau dibuat, klik ikon **Navigation Menu** (ikon tiga garis horizontal atau "hamburger menu") di pojok kiri atas.
    - Arahkan kursor mouse ke menu **"APIs & Services"**.
    - Dari submenu yang muncul, klik **"Library"**.

4.  **Cari YouTube Data API v3:**

    - Anda akan diarahkan ke halaman API Library.
    - Pada kolom pencarian di bagian atas (`Search for APIs & Services`), ketik `YouTube Data API v3`.
    - Tekan **Enter** atau klik hasil pencarian yang relevan.

5.  **Aktifkan YouTube Data API v3:**

    - Klik pada hasil pencarian **"YouTube Data API v3"**.
    - Pada halaman detail API, klik tombol **"ENABLE"**. Tunggu beberapa saat hingga proses aktivasi selesai.

6.  **Buat Kunci API (API Key):**

    - Setelah API diaktifkan, Anda akan diarahkan ke halaman detail API tersebut.
    - Di menu sebelah kiri, klik **"Credentials"** (Kredensial).
    - Di bagian atas halaman Credentials, klik tombol **"+ CREATE CREDENTIALS"**.
    - Dari dropdown yang muncul, pilih **"API key"**.

7.  **Salin Kunci API (API Key):**
    - Sebuah pop-up akan muncul menampilkan API Key yang baru saja dibuat.
    - Klik ikon **"Copy"** (Salin) di sebelah kanan API Key untuk menyalinnya ke clipboard.
    - **Penting:** Simpan API Key ini di tempat yang aman dan jangan bagikan secara publik. Anda akan menggunakannya nanti dalam kode Python.
    - Anda bisa mengklik **"Close"**. (Disarankan untuk membatasi kunci API nanti untuk keamanan tambahan, tetapi untuk tutorial ini kita akan melewatinya).

### Bagian 2: Pengambilan Komentar dengan Google Colaboratory

8.  **Buka Google Colab dan Siapkan Notebook:**

    - Kunjungi [Google Colaboratory](https://colab.research.google.com/).
    - Buat notebook baru dengan mengklik **"File" > "New notebook"**.

9.  **Salin ID Video YouTube Target:**

    - Buka YouTube di browser Anda dan cari video yang komentarnya ingin Anda scrape.
    - Buka halaman video tersebut.
    - Lihat URL video di address bar browser Anda. URL akan terlihat seperti ini: `https://www.youtube.com/watch?v=VIDEO_ID` atau `https://www.youtube.com/watch?v=VIDEO_ID&ab_channel=ChannelName`.
    - Salin bagian **`VIDEO_ID`** dari URL tersebut. `VIDEO_ID` adalah string unik setelah `v=` dan sebelum karakter `&` (jika ada).
    - **Contoh:** Jika URL adalah `https://www.youtube.com/watch?v=z-U1r4k5r2s&ab_channel=LexClips`, maka `VIDEO_ID` adalah `z-U1r4k5r2s`.

10. **Masukkan Kode Python ke Colab:**

    - Kembali ke notebook Google Colab Anda.
    - Salin kode Python berikut dan tempelkan ke dalam sel kode pertama di notebook:

    ```python
    # 1. Import libraries
    import googleapiclient.discovery
    import pandas as pd
    import os # Tambahkan ini jika ingin menyimpan ke Google Drive

    # 2. Set up API Key, Video ID, and Max Results
    # !!! GANTI DENGAN API KEY ANDA YANG SEBENARNYA !!!
    API_KEY = "MASUKKAN_API_KEY_ANDA_DISINI"

    # !!! GANTI DENGAN VIDEO ID TARGET ANDA !!!
    videoId = "MASUKKAN_VIDEO_ID_ANDA_DISINI"

    # Tentukan jumlah maksimal komentar yang ingin diambil (maksimum per request adalah 100)
    # Jika ingin lebih, perlu implementasi pagination (tidak dicakup di kode dasar ini)
    maxResults = 100 # Anda bisa ubah angka ini (misal: 40, 50, 100)

    # 3. Build the YouTube API service object
    api_service_name = "youtube"
    api_version = "v3"
    try:
        youtube = googleapiclient.discovery.build(
            api_service_name, api_version, developerKey=API_KEY)

        # 4. Request comments from the specified video
        request = youtube.commentThreads().list(
            part="snippet", # Ambil bagian snippet yang berisi info komentar
            videoId=videoId,
            maxResults=maxResults,
            order="relevance" # Urutkan berdasarkan relevansi (atau 'time' untuk terbaru)
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
                    comment['textDisplay'] # Mengambil teks dengan format HTML dasar
                    # comment['textOriginal'] # Opsi: Mengambil teks asli tanpa format
                ])
        else:
            print("Tidak ada komentar ditemukan atau terjadi kesalahan.")

        # 7. Create a Pandas DataFrame
        df = pd.DataFrame(comments, columns=[
            'author', 'published_at', 'updated_at', 'like_count', 'text'
            ])

        # 8. Define filename and save to CSV
        output_filename = f'scraped_youtube_comments_{videoId}.csv'
        df.to_csv(output_filename, index=False, encoding='utf-8-sig') # utf-8-sig untuk kompatibilitas Excel

        # 9. Display the first few rows of the DataFrame and confirm save location
        print(f"Data berhasil disimpan sebagai: {output_filename}")
        print("Lokasi penyimpanan: Di direktori utama sesi Colab Anda (lihat panel Files di kiri).")
        print("\nPreview Data:")
        print(df.head())

    except googleapiclient.errors.HttpError as e:
        print(f"Terjadi kesalahan saat memanggil API: {e}")
        print("Pastikan API Key Anda benar, API YouTube Data v3 sudah diaktifkan,")
        print("dan Video ID valid serta memiliki komentar yang diizinkan untuk dilihat.")
    except Exception as e:
        print(f"Terjadi kesalahan lain: {e}")

    ```

11. **Masukkan API Key dan Video ID:**

    - Dalam kode yang baru saja Anda tempel, cari baris:
      `API_KEY = "MASUKKAN_API_KEY_ANDA_DISINI"`
    - Ganti `"MASUKKAN_API_KEY_ANDA_DISINI"` dengan **API Key** yang Anda salin dari Google Cloud Console (Step 7). Pastikan API Key tetap berada di dalam tanda kutip (`"`).
    - Cari baris:
      `videoId = "MASUKKAN_VIDEO_ID_ANDA_DISINI"`
    - Ganti `"MASUKKAN_VIDEO_ID_ANDA_DISINI"` dengan **Video ID** yang Anda salin dari YouTube (Step 9). Pastikan Video ID tetap berada di dalam tanda kutip (`"`).

12. **Tentukan Jumlah Komentar (Opsional):**

    - Anda dapat mengubah nilai variabel `maxResults` untuk menentukan jumlah maksimum komentar yang ingin diambil dalam satu kali permintaan (maksimum 100 per permintaan oleh API). Kode di atas disetel ke 100.
    - *Catatan: Untuk mengambil lebih dari 100 komentar, diperlukan logika *pagination* (menggunakan `nextPageToken` dari respons API), yang tidak termasuk dalam kode dasar ini.*

13. **Jalankan Kode dan Unduh Hasil:**

    - Klik tombol **"Run"** (ikon play ►) di sebelah kiri sel kode, atau tekan `Shift + Enter` pada keyboard Anda.
    - Tunggu hingga kode selesai dieksekusi. Output akan muncul di bawah sel kode, menampilkan beberapa baris pertama data dan pesan konfirmasi penyimpanan file.
    - Setelah selesai, lihat panel **"Files"** (ikon folder) di sebelah kiri jendela Colab.
    - Anda akan melihat file CSV baru dengan nama seperti `scraped_youtube_comments_VIDEO_ID.csv` (misalnya, `scraped_youtube_comments_z-U1r4k5r2s.csv`).
    - Klik ikon **tiga titik vertikal** (⋮) di sebelah nama file tersebut.
    - Pilih **"Download"** untuk menyimpan file CSV ke komputer Anda.

14. **Hasil Tersimpan dalam File CSV:**
    - File CSV yang Anda unduh berisi komentar-komentar yang berhasil di-scrape dari video YouTube yang ditentukan, beserta informasi tambahan seperti penulis, waktu publikasi, jumlah suka, dll. Anda bisa membukanya menggunakan program seperti Microsoft Excel, Google Sheets, atau editor teks.

---

Selesai! Anda telah berhasil melakukan scraping komentar YouTube menggunakan API dan menyimpannya sebagai file CSV.
