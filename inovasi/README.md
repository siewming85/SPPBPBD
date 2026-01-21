Berikut adalah dokumen lengkap yang telah disambungkan dengan langkah-langkah seterusnya dalam format Markdown:

***

# Kaedah Inovasi dalam Menganalisis Data Pentaksiran Bilik Darjah (PBD) Daerah

Dokumen ini menerangkan langkah-langkah standard untuk memuat turun dan menganalisis data PBD bagi peringkat Pejabat Pendidikan Daerah (PPD).

> **Nota Penting:** Pastikan anda mempunyai akses sebagai **Pentadbir PPD PBD** dalam sistem Modul Pentaksiran (idMe) sebelum meneruskan langkah-langkah ini.

![Paparan Utama Aplikasi](https://raw.githubusercontent.com/siewming85/SPPBPBD/refs/heads/main/Screenshot_2026-01-21_14_19_51.png)

---

## Langkah-langkah Pengaksesan & Muat Turun Data

### 1. Akses Modul Pentaksiran
Ikuti langkah berikut untuk masuk ke menu utama:

1.  **Langkah Pertama:** Log masuk ke dalam sistem **idMe**.
2.  **Langkah Kedua:** Pada papan pemuka utama, klik pada ikon **Aplikasi**.
3.  **Langkah Ketiga:** Pilih **Pengurusan Pentaksiran**.

![Menu Analisis PBD](https://raw.githubusercontent.com/siewming85/SPPBPBD/refs/heads/main/Screenshot_2026-01-21_14_24_54.png)

---

### 2. Navigasi ke Menu Analisis
Setelah berada di dalam Modul Pengurusan Pentaksiran, teruskan dengan langkah berikut:

4.  **Langkah Keempat:**
    Pada menu sisi, navigasi ke **Laporan** > **Pentaksiran Bilik Darjah** > **Kelas**.
    ![Laporan Kelas](https://github.com/siewming85/SPPBPBD/blob/main/Screenshot_2026-01-21_14_52_09.png?raw=true)

5.  **Langkah Kelima:**
    Pada bahagian senarai, ubah tetapan paparan rekod kepada **9999** untuk memastikan semua kelas dipaparkan dalam satu halaman.
    ![Papar Rekod](https://github.com/siewming85/SPPBPBD/blob/main/Screenshot_2026-01-21_14_55_17.png?raw=true)

6.  **Langkah Keenam:**
    Lakukan saringan (filter) maklumat mengikut keperluan, contohnya memilih sesi **2025/2026**.
    ![Tapis Maklumat](https://github.com/siewming85/SPPBPBD/blob/main/Screenshot_2026-01-21_14_58_59.png?raw=true)

---

### 3. Automasi Muat Turun Data (Bulk Download)

Langkah ini menggunakan skrip automasi untuk memuat turun semua laporan kelas secara serentak.

7.  **Langkah Ketujuh:**
    *   Klik kanan (Right-click) pada mana-mana bahagian pelayar dan pilih **Inspect**.
    *   Klik pada tab **Console**.
    *   Salin dan tampal (Copy & Paste) kod di bawah ke dalam Console, kemudian tekan **Enter**.

    ![Console Inspect](https://github.com/siewming85/SPPBPBD/blob/main/Screenshot_2026-01-21_15_00_47.png?raw=true)

    **Kod Automasi (Kod 1):**
    ```javascript
    async function fetchAndDownload(url) {
      try {
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        var newname2 = url.replace("https://moeissppb.moe.gov.my/laporan/pbd/laporan-kelas/","");
        newname = newname2.replace("?cetak=1","");
        const htmlContent = await response.text();
        const blob = new Blob([htmlContent], { type: 'text/html' });
        
        const a = document.createElement('a');
        a.href = URL.createObjectURL(blob);
        a.download = newname+'.html';
        
        a.style.display = 'none';
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
      } catch (error) {
        console.error('Error fetching the URL:', error);
        // hello.push(url); // Uncomment if 'hello' array is defined elsewhere
      }
    }

    async function fetchAllUrls() {
      for (const url of matchingLinks) {
        await fetchAndDownload(url);
      }
    }
    
    // Define the prefix you want to match
    var prefixToMatch = "https://moeissppb.moe.gov.my/laporan/pbd/laporan-kelas/";

    // Get all anchor elements on the page
    var allLinks = document.querySelectorAll("a");

    // Create an array to store the matching links
    var matchingLinks = [];

    // Loop through all anchor elements and filter those with the desired prefix
    for (var i = 0; i < allLinks.length; i++) {
      var link = allLinks[i];
      var href = link.getAttribute("href");
      var newhref = href+'?cetak=1';
      if (href && href.startsWith(prefixToMatch)) {
        matchingLinks.push(newhref);
      }
    }

    // Call the function to start fetching and downloading URLs
    fetchAllUrls();
    ```

---

### 4. Pemprosesan Data di Awan (Cloud Processing)

Setelah fail HTML berjaya dimuat turun ke dalam komputer anda:

8.  **Langkah Kelapan:**
    Muat naik (*Upload*) kesemua fail HTML tersebut ke dalam satu folder baharu di **Google Drive** anda. Pastikan "Access Setting" folder tersebut ditetapkan supaya boleh dibaca (jika perlu untuk skrip luaran) atau dapatkan **ID Folder** tersebut.

9.  **Langkah Kesembilan:**
    Untuk mendapatkan Data Mentah (*Raw Data*) dalam bentuk Google Spreadsheet, gunakan format URL berikut pada pelayar web anda:

    ```text
    s1.ppdjulau.com/getallData/<GOOGLE_DRIVE_FOLDER_ID>
    ```

    > **Contoh:** Jika ID folder Google Drive anda adalah `1AbCdEfGhIjK`, maka URL yang perlu ditaip adalah: `s1.ppdjulau.com/getallData/1AbCdEfGhIjK`

---
