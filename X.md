# Dokumentasi Proses Scraping Data Platform X (Twitter) Menggunakan TweetHarvest

Catatan ini mendokumentasikan langkah-langkah yang dilakukan untuk proses scraping (pengambilan data) tweet dari Platform X (sebelumnya Twitter). Proses ini memanfaatkan alat `tweet-harvest` yang dieksekusi melalui skrip Python dalam lingkungan Google Colaboratory.

**Tujuan Proses:** Mengumpulkan tweet berdasarkan kriteria pencarian tertentu (kata kunci, rentang waktu, bahasa) dan menyimpannya dalam format file CSV.

**Alat/Kondisi yang Digunakan:**
*   Google Colaboratory.
*   Python (untuk orkestrasi dan definisi variabel).
*   Node.js (karena `tweet-harvest` dibangun di atas Node.js).
*   `tweet-harvest` (alat scraping via npx).
*   `twitter_auth_token` (diperoleh secara manual dari sesi browser X).
*   `pandas` (library Python, diinstal untuk potensi analisis data lanjutan, meskipun tidak wajib untuk scraping itu sendiri).

## Tahapan Proses yang Dilakukan:

1.  **Pengaturan Token Autentikasi X:**
    *   Token autentikasi (`twitter_auth_token`) diperlukan agar `tweet-harvest` dapat mengakses data X seolah-olah sebagai pengguna yang login.
    *   Token ini diperoleh secara manual dari sesi browser yang aktif login ke X:
        1.  Buka situs web X di browser (misalnya Chrome, Firefox).
        2.  Login ke akun X.
        3.  Klik kanan di mana saja pada halaman, pilih "Inspect" atau "Inspect Element".
        4.  Navigasi ke tab "Application" (Chrome) atau "Storage" (Firefox).
        5.  Di panel kiri, cari bagian "Cookies" dan pilih domain yang berhubungan dengan X (misalnya `https://x.com` atau `https://twitter.com`).
        6.  Cari cookie dengan nama `auth_token`.
        7.  Salin nilai (value) dari cookie `auth_token` tersebut.
    *   Nilai token yang disalin kemudian dimasukkan ke dalam variabel Python di Google Colab.

    ```python
    # @title Twitter Auth Token
    twitter_auth_token = 'a16b6e94f00ea1cd9d94f6533efd0799b9b6dd60' # Change this auth token
    ```
    *   **Penting:** Token ini bersifat sensitif dan merepresentasikan sesi login. Token ini perlu diperbarui jika sesi login di browser berakhir atau token kadaluarsa.

2.  **Instalasi Dependensi:**
    *   Library `pandas` diinstal (meskipun tidak secara langsung digunakan oleh `tweet-harvest`, seringkali berguna untuk analisis data setelahnya).
    *   Karena `tweet-harvest` adalah alat berbasis Node.js, maka Node.js perlu diinstal terlebih dahulu di lingkungan Google Colab.

    ```python
    # Import required Python package
    !pip install pandas

    # Install Node.js (because tweet-harvest built using Node.js)
    !sudo apt-get update
    !sudo apt-get install -y ca-certificates curl gnupg
    !sudo mkdir -p /etc/apt/keyrings
    !curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg

    !NODE_MAJOR=20 && echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list

    !sudo apt-get update
    !sudo apt-get install nodejs -y

    # Verifikasi instalasi Node.js
    !node -v
    ```

3.  **Konfigurasi Parameter Scraping:**
    *   Parameter untuk proses scraping didefinisikan dalam variabel Python:
        *   `filename`: Nama file CSV yang akan dihasilkan untuk menyimpan data tweet.
        *   `search_keyword`: Kueri pencarian yang akan digunakan di X. Kueri ini dapat mencakup kata kunci, filter rentang waktu (`since:`, `until:`), filter bahasa (`lang:`), dan operator pencarian X lainnya.
        *   `limit`: Batasan jumlah maksimal tweet yang ingin coba di-crawl. Alat mungkin berhenti sebelum mencapai limit ini jika tidak ada lagi tweet yang ditemukan sesuai kriteria.

    ```python
    # Crawl Data
    filename = 'chatGPT.csv'
    search_keyword = 'chatgpt since:2023-01-01 until:2024-12-31 lang:en'
    limit = 4000
    ```

4.  **Eksekusi TweetHarvest:**
    *   Alat `tweet-harvest` dijalankan menggunakan `npx`, yang memungkinkan eksekusi paket Node.js tanpa harus menginstalnya secara global.
    *   Perintah `npx` dipanggil dari dalam sel kode Colab menggunakan tanda seru (`!`).
    *   Parameter yang telah didefinisikan sebelumnya (`filename`, `search_keyword`, `limit`, `twitter_auth_token`) dimasukkan ke dalam argumen perintah `tweet-harvest`:
        *   `-o "{filename}"`: Menentukan nama file output.
        *   `-s "{search_keyword}"`: Menentukan kueri pencarian.
        *   `--tab "LATEST"`: Menentukan untuk mengambil hasil dari tab "Terbaru" (Latest) di X.
        *   `-l {limit}`: Menentukan batas jumlah tweet.
        *   `--token {twitter_auth_token}`: Menyediakan token autentikasi yang diperlukan.

    ```python
    !npx -y tweet-harvest@0.2.6.1 -o "{filename}" -s "{search_keyword}" --tab "LATEST" -l {limit} --token {twitter_auth_token}
    ```

5.  **Hasil dan Penyimpanan Data:**
    *   Setelah perintah eksekusi dijalankan, `tweet-harvest` akan mulai melakukan crawling data berdasarkan parameter yang diberikan.
    *   Output di sel Colab akan menampilkan progres crawling dan konfirmasi penyimpanan data.
    *   Data tweet yang berhasil di-crawl disimpan ke dalam file CSV dengan nama yang ditentukan (`filename`) di dalam direktori `/content/tweets-data/` pada lingkungan sesi Google Colab.
    *   Contoh output konfirmasi:
        ```
        Your tweets saved to: /content/tweets-data/chatGPT.csv
        Total tweets saved: 385
        ```
    *   File CSV ini kemudian dapat diunduh dari panel "Files" di Google Colab untuk analisis lebih lanjut.

---

Proses scraping data tweet dari Platform X menggunakan `tweet-harvest` dengan autentikasi token dan parameter pencarian spesifik telah selesai, dan data berhasil disimpan sebagai file CSV.
