# Dokumentasi Proses Scraping Data Web Menggunakan ScrapeStorm

Catatan ini mendokumentasikan langkah-langkah yang dilakukan menggunakan tool ScrapeStorm untuk melakukan scraping data dari web. Secara spesifik, proses ini menargetkan data artikel dari Medium.com yang ditemukan menggunakan kata kunci "chatgpt".

**Tujuan Proses:** Mendapatkan data terstruktur (seperti judul, link, penulis, dll.) dari halaman hasil pencarian Medium.com untuk kata kunci "chatgpt" dan menyimpannya dalam format file Excel.

**Alat yang Digunakan:**
*   Aplikasi ScrapeStorm (Versi 4.0.4 seperti terlihat pada gambar).
*   Akun Google (untuk login ScrapeStorm).
*   Koneksi internet.
*   Web Browser (untuk mengakses Medium.com jika diperlukan verifikasi).

## Tahapan Proses yang Dilakukan:

1.  **Instalasi ScrapeStorm:**
    *   Proses instalasi ScrapeStorm (versi 4.0.4) dilakukan pada sistem (laptop/pc).
    *   Perjanjian lisensi (License Agreement) disetujui dengan mengklik tombol "I Agree".
    *   Lokasi folder instalasi dipilih (atau dibiarkan default pada `C:\Program Files (x86)\ScrapeStorm\ScrapeStorm`).
    *   Instalasi dimulai dengan mengklik tombol "Install".
    *   Proses instalasi ditunggu hingga selesai, kemudian ditutup dengan mengklik "Finish".

2.  **Login Menggunakan Akun Google:**
    *   Saat aplikasi ScrapeStorm pertama kali dijalankan setelah instalasi, proses login menggunakan akun Google dipicu secara otomatis.
    *   Akun Google yang sesuai dipilih dari daftar yang muncul untuk melanjutkan proses autentikasi ke ScrapeStorm.
    *   Setelah login berhasil, dashboard utama aplikasi ScrapeStorm ditampilkan.

3.  **Penentuan Target Scraping Data Web:**
    *   URL target untuk scraping ditentukan. Dalam kasus ini, data artikel dari Medium.com terkait "chatgpt" menjadi target.
    *   URL halaman hasil pencarian Medium untuk kata kunci "chatgpt" (`https://medium.com/search?q=chatgpt`) dimasukkan ke dalam kolom input utama pada dashboard ScrapeStorm.
    *   Tombol "Get Started" diklik untuk memulai proses deteksi struktur data pada URL tersebut.

4.  **Peninjauan Struktur Data Terdeteksi:**
    *   ScrapeStorm secara otomatis mendeteksi tabel atau daftar data pada halaman target.
    *   Panel bawah aplikasi menampilkan preview kolom-kolom data yang berhasil dideteksi dan akan diambil (contoh: Title, Title\_link, Avatar, bf, ef, dll.).
    *   Kolom-kolom ini ditinjau untuk memastikan relevansi data yang akan diekspor. *Opsi kostumisasi (seperti rename kolom, merge, modifikasi data, atau menghapus kolom yang tidak relevan) tersedia melalui klik kanan pada header kolom jika diperlukan penyesuaian.*

5.  **Konfigurasi Pengaturan Eksekusi (Opsional) dan Memulai Scraping:**
    *   Sebelum memulai, pengaturan tambahan dapat dikonfigurasi melalui menu "Run Settings" (di bagian atas, atau diakses sebelum klik Start). Ini termasuk opsi seperti:
        *   Schedule (Penjadwalan)
        *   IP Rotation & Delay (jika diperlukan dan langganan mendukung)
        *   Automatic Export (jika langganan mendukung)
        *   Download Files (misalnya, gambar/dokumen yang terdeteksi di halaman - memerlukan langganan Premium/Bisnis)
        *   Speed Boost, Data Deduplication, Email Alerts, Developer options.
    *   *Dalam proses ini, diasumsikan pengaturan default digunakan atau disesuaikan seperlunya.*
    *   Proses scraping data inti dimulai dengan mengklik tombol "Start" di bagian bawah panel preview data.

6.  **Pemantauan Proses Scraping dan Penghentian:**
    *   ScrapeStorm beralih ke tampilan "Running Task". Proses scraping berjalan, dan data yang berhasil diambil ditampilkan secara real-time pada tab "Data".
    *   Status proses (Time, Downloaded Files, Data Count, Speed) dipantau di bagian atas.
    *   Proses ini dibiarkan berjalan hingga ScrapeStorm secara otomatis mendeteksi akhir dari data (misalnya, akhir halaman atau pagination) atau dihentikan secara manual dengan mengklik tombol "Stop".

7.  **Ekspor Data Hasil Scraping:**
    *   Setelah proses scraping berhenti (baik karena selesai otomatis atau dihentikan manual), panel konfirmasi "Scraping is Stopped" muncul. Panel ini menampilkan ringkasan hasil (Total Data, Waktu, URL Terakhir, Jumlah Halaman).
    *   Tombol "Export" pada panel konfirmasi tersebut diklik untuk membuka dialog "Export Format".
    *   Pada dialog "Export Format", pengaturan ekspor berikut dikonfigurasi:
        *   **Local File:** Dipilih sebagai target penyimpanan.
        *   **Tipe File:** Excel (`*.xlsx`) dipilih.
        *   **File Path:** Lokasi penyimpanan dan nama file ditentukan (misalnya, `C:\Users\ASUS\Downloads\namafile_medium_chatgpt.xlsx`).
        *   **Export Scope:** Rentang data yang akan diekspor ditentukan (misalnya, dari baris 1 hingga 398).
        *   **Export File Settings:** Opsi penanganan jika nama file sudah ada dipilih (misalnya, "Add a time prefix to the file name...").
    *   Ekspor data dimulai dengan mengklik tombol "Export" pada dialog "Export Format".

8.  **Verifikasi Hasil Ekspor:**
    *   File Excel hasil ekspor (`.xlsx`) yang telah disimpan di lokasi yang ditentukan (sesuai langkah 7) dibuka menggunakan aplikasi spreadsheet (seperti Microsoft Excel).
    *   Data di dalam file diperiksa untuk memastikan kelengkapan dan kesesuaian dengan data yang ditampilkan selama proses scraping.

---

Proses scraping data artikel dari Medium.com yang terkait dengan kata kunci "chatgpt" menggunakan ScrapeStorm telah berhasil diselesaikan. Data terstruktur berhasil diekstraksi dan disimpan dalam format file Excel untuk analisis lebih lanjut.
