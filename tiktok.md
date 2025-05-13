# Dokumentasi Proses Scraping Komentar TikTok Menggunakan Ekstensi Chrome "TTCommentExporter"

Catatan ini mendokumentasikan langkah-langkah yang dilakukan untuk scraping (pengambilan data) komentar dari video TikTok menggunakan ekstensi Google Chrome "TTCommentExporter - Export TikTok Comments". Video target dalam proses ini ditemukan menggunakan kata kunci `#chatgpt`.

**Tujuan Proses:** Mendapatkan daftar komentar (dan balasan, sesuai pilihan) dari video TikTok spesifik yang relevan dengan `#chatgpt` dan menyimpannya dalam format file CSV.

**Alat yang Digunakan:**
*   Browser Google Chrome.
*   Ekstensi Chrome "TTCommentExporter - Export TikTok Comments".
*   Koneksi internet.

## Tahapan Proses yang Dilakukan:

1.  **Instalasi Ekstensi TTCommentExporter:**
    *   Ekstensi "TTCommentExporter - Export TikTok Comments" diinstal ke browser Google Chrome dari Chrome Web Store melalui tautan:
        [https://chromewebstore.google.com/detail/ttcommentexporter-export/epjbmmchkjlgmogfoamcleeikmfaffjm](https://chromewebstore.google.com/detail/ttcommentexporter-export/epjbmmchkjlgmogfoamcleeikmfaffjm)
    *   Instalasi diselesaikan dengan memilih opsi "Tambahkan ke Chrome" dan mengonfirmasi penambahan ekstensi.

2.  **Pencarian Video Target dan Aktivasi Ekstensi:**
    *   **Pencarian Video:** Fitur pencarian TikTok digunakan untuk mencari video yang relevan dengan kata kunci `#chatgpt`.
    *   **Pembukaan Video:** Video spesifik yang diinginkan dari hasil pencarian `#chatgpt` dibuka pada tab baru di Google Chrome. Halaman dimuat hingga bagian komentar terlihat.
    *   **Aktivasi Ekstensi:** Ikon Ekstensi (puzzle) di toolbar Chrome diklik, lalu "TTCommentExporter - Export TikTok Comments" dipilih dari daftar untuk diaktifkan pada halaman video tersebut.

3.  **Konfigurasi dan Memulai Ekspor:**
    *   Kotak dialog ekstensi TTCommentExporter muncul, secara otomatis menampilkan link video yang sedang terbuka.
    *   Opsi **"Include comment replies"** diperiksa dan dicentang jika balasan komentar diperlukan untuk disertakan dalam data.
    *   Proses ekspor dimulai dengan menekan tombol **"Export →"**.

4.  **Menunggu Penyelesaian Proses Ekspor:**
    *   Ekstensi memulai proses pengambilan data komentar, dengan status **"Exporting..."** dan jumlah komentar terproses terlihat pada kotak dialog.
    *   Proses ini ditunggu hingga selesai. Tab video TikTok dan kotak dialog ekstensi dibiarkan terbuka selama proses berlangsung.

5.  **Pengunduhan Hasil Komentar:**
    *   Setelah proses ekspor selesai, tombol **"Download (...) Comments and Replies"** (menampilkan jumlah data yang diekspor) menjadi aktif pada kotak dialog ekstensi.
    *   Tombol **"Download"** tersebut diklik untuk mengunduh file hasil.
    *   **Catatan:** Berdasarkan pengamatan, versi gratis ekstensi ini memiliki batasan jumlah komentar yang dapat diekspor (misalnya, hingga 225 komentar). Untuk jumlah lebih besar, diperlukan upgrade ke versi berbayar.

6.  **Penyimpanan Hasil dalam Format CSV:**
    *   File berformat `.csv` secara otomatis terunduh oleh browser. Nama file umumnya mencakup ID video dan jumlah data (contoh: `TTCommentExporter-7479507366552030469-205-comments-replies.csv`).
    *   File CSV ini berisi data komentar yang berhasil di-scrape (termasuk ID Komentar, ID Pengguna, Nama Pengguna, Teks Komentar, Waktu Posting, dll.). File ini dapat dibuka dan dianalisis lebih lanjut menggunakan aplikasi spreadsheet (seperti Microsoft Excel, Google Sheets, dll.).

---

Proses scraping komentar dari video TikTok yang ditemukan via `#chatgpt` menggunakan ekstensi TTCommentExporter telah selesai, dan data berhasil disimpan sebagai file CSV.
