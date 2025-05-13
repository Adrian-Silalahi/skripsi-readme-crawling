# Panduan Scraping Komentar TikTok Menggunakan Ekstensi Chrome "TTCommentExporter"

Dokumentasi ini menjelaskan cara melakukan scraping (pengambilan data) komentar dari video TikTok menggunakan ekstensi Google Chrome bernama "TTCommentExporter - Export TikTok Comments". Metode ini cocok untuk mengambil komentar dari video-video spesifik yang ditemukan, misalnya, melalui pencarian kata kunci tertentu.

**Tujuan:** Mendapatkan daftar komentar (dan balasan, jika diinginkan) dari video TikTok tertentu (yang dalam kasus ini ditemukan menggunakan kata kunci `#chatgpt`) dan menyimpannya dalam format file CSV.

**Prasyarat:**
*   Browser Google Chrome terpasang di komputer Anda.
*   Koneksi internet.

## Langkah-langkah Rinci:

1.  **Instal Ekstensi TTCommentExporter:**
    *   Buka browser Google Chrome.
    *   Kunjungi halaman ekstensi di Chrome Web Store melalui tautan berikut:
        [https://chromewebstore.google.com/detail/ttcommentexporter-export/epjbmmchkjlgmogfoamcleeikmfaffjm](https://chromewebstore.google.com/detail/ttcommentexporter-export/epjbmmchkjlgmogfoamcleeikmfaffjm)
    *   Klik tombol **"Add to Chrome"** (atau "Tambahkan ke Chrome").
    *   Jika muncul pop-up konfirmasi, klik **"Add extension"** (Tambahkan ekstensi). Tunggu hingga proses instalasi selesai.

2.  **Cari dan Buka Video TikTok, lalu Aktifkan Ekstensi:**
    *   **Cari Video:** Gunakan fitur pencarian di TikTok (baik di aplikasi mobile atau web) untuk menemukan video yang relevan dengan topik Anda. **Dalam contoh ini, video target ditemukan dengan mencari menggunakan kata kunci atau tagar `#chatgpt`**.
    *   **Buka Video:** Setelah menemukan video yang diinginkan dari hasil pencarian `#chatgpt`, buka halaman spesifik video tersebut di tab baru Google Chrome. Pastikan video dan bagian komentarnya mulai termuat.
    *   **Aktifkan Ekstensi:** Setelah halaman video TikTok terbuka penuh, klik ikon **Ekstensi** (biasanya berbentuk potongan puzzle) di pojok kanan atas toolbar Chrome.
    *   Dari daftar ekstensi yang muncul, klik **"TTCommentExporter - Export TikTok Comments"**.

3.  **Konfigurasi dan Mulai Ekspor:**
    *   Sebuah kotak dialog (pop-up) dari ekstensi TTCommentExporter akan muncul.
    *   Link video TikTok yang sedang Anda buka seharusnya sudah otomatis terisi di dalam kotak teks.
    *   Anda akan melihat opsi **"Include comment replies"** (Sertakan balasan komentar). Centang kotak ini jika Anda ingin mengambil balasan dari setiap komentar utama. Biarkan tidak dicentang jika Anda hanya menginginkan komentar utama.
    *   Klik tombol **"Export →"**.

4.  **Tunggu Proses Ekspor Selesai:**
    *   Ekstensi akan mulai mengambil data komentar dari video tersebut. Kotak dialog ekstensi akan menampilkan status **"Exporting..."** dan jumlah komentar yang sudah diproses.
    *   Harap tunggu hingga proses ini selesai. Waktu yang dibutuhkan tergantung pada jumlah komentar pada video tersebut. Jangan menutup tab video TikTok atau kotak dialog ekstensi selama proses ini berlangsung.

5.  **Unduh Hasil Komentar:**
    *   Setelah proses ekspor selesai, tombol **"Download (...) Comments and Replies"** (atau serupa, menampilkan jumlah komentar yang berhasil diekspor) akan aktif di kotak dialog ekstensi.
    *   Klik tombol **"Download"** tersebut.
    *   **Catatan Penting:** Seperti yang terlihat pada gambar, versi gratis dari ekstensi ini mungkin memiliki batasan jumlah komentar yang bisa diekspor (misalnya, hingga 225 komentar). Untuk mengekspor lebih banyak, Anda mungkin perlu meng-upgrade ke versi berbayar dari ekstensi tersebut.

6.  **Hasil Disimpan dalam File CSV:**
    *   Browser Anda akan secara otomatis mengunduh file berformat `.csv`. Nama filenya biasanya akan mencakup ID video TikTok dan jumlah komentar (contoh: `TTCommentExporter-7479507366552030469-205-comments-replies.csv`).
    *   File CSV ini berisi data komentar yang berhasil di-scrape (seperti ID Komentar, ID Pengguna, Nama Pengguna, Teks Komentar, Waktu Posting, Jumlah Suka, apakah itu balasan, dll.).
    *   Anda dapat membuka file CSV ini menggunakan program spreadsheet seperti Microsoft Excel, Google Sheets, LibreOffice Calc, atau editor teks.

---

Selesai! Anda telah berhasil melakukan scraping komentar dari video TikTok yang ditemukan melalui pencarian `#chatgpt` menggunakan ekstensi TTCommentExporter dan menyimpannya sebagai file CSV.
