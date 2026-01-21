***

# Kaedah Asal dalam Menganalisis Data Pentaksiran Bilik Darjah (PBD) Daerah

Dokumen ini menerangkan langkah-langkah standard untuk memuat turun dan menganalisis data PBD bagi peringkat Pejabat Pendidikan Daerah (PPD).

> **Nota Penting:** Pastikan anda mempunyai akses sebagai **Pentadbir PPD PBD** dalam sistem Modul Pentaksiran (idMe) sebelum meneruskan langkah-langkah ini.
![Paparan Utama Aplikasi](https://raw.githubusercontent.com/siewming85/SPPBPBD/refs/heads/main/Screenshot_2026-01-21_14_19_51.png)

## Bahagian A: Pengaksesan & Muat Turun Data

### 1. Akses Modul Pentaksiran
Ikuti langkah berikut untuk masuk ke menu utama:

1.  **Langkah Pertama:** Log masuk ke dalam sistem **idMe**.
2.  **Langkah Kedua:** Pada papan pemuka utama, klik pada ikon **Aplikasi**.
3.  **Langkah Ketiga:** Pilih **Pengurusan Pentaksiran**.
![Menu Analisis PBD](https://raw.githubusercontent.com/siewming85/SPPBPBD/refs/heads/main/Screenshot_2026-01-21_14_24_54.png)

---

### 2. Navigasi ke Menu Analisis
Setelah berada di dalam Modul Pengurusan Pentaksiran:

4.  **Langkah Keempat:** Pada menu navigasi, tekan **Analisis** > **Pentaksiran Bilik Darjah** > **Analisis Mengikut JPN/PPD**.

---

### 3. Tetapan Parameter & Muat Turun
Lakukan tetapan data yang ingin dimuat turun:

5.  **Langkah Kelima:** Pilih kriteria berikut:
    *   **Sesi Persekolahan:** Pilih tahun/sesi semasa.
    *   **Aktiviti Pentaksiran:** Pilih jenis pentaksiran yang berkaitan.
    *   **Mata Pelajaran:** Pilih mata pelajaran (anda boleh memilih lebih daripada satu subjek).
    *   Setelah selesai, tekan butang **XLS** untuk memuat turun fail.

![Tetapan Parameter dan Muat Turun](https://raw.githubusercontent.com/siewming85/SPPBPBD/refs/heads/main/Screenshot_2026-01-21_14_32_50.png)

---

### 4. Penyimpanan Data
6.  **Langkah Keenam:** Muat naik (*upload*) fail `.xls` yang telah dimuat turun tadi ke dalam **Google Drive** anda untuk tujuan analisis lanjut atau penyimpanan.

---
---

## Bahagian B: Proses Pembersihan Data (Data Cleaning)

Setelah fail `.xls` dimuat turun dan dibuka dalam Microsoft Excel, ikuti langkah-langkah berikut untuk memastikan data sedia dianalisis.

### 1. Wujudkan Lajur "Tahun & Subjek" (Penting)
Oleh kerana maklumat seperti "TAHUN DUA | BAHASA IBAN" berada di baris berasingan (*header*), kita perlu meletakkannya sebaris dengan data murid.

1.  Klik kanan pada huruf **Column A** (lajur pertama), pilih **Insert** untuk menambah satu lajur kosong di kiri sekali.
2.  Di sel **A2**, masukkan formula berikut:
    ```excel
    =IF(ISNUMBER(SEARCH("|",B2)), B2, A1)
    ```
    *(Maksud formula: Jika Lajur B mempunyai simbol "|", ambil teks tersebut. Jika tidak, ikut teks di atasnya).*
3.  **Double-click** bucu bawah kanan sel A2 untuk menyalin (*copy*) formula sehingga ke baris terakhir data.
4.  Pilih keseluruhan **Column A**, tekan **Copy**, kemudian **Paste Values** (*Paste Special > Values/123*) di tempat yang sama untuk mematikan formula.

---

### 2. Buang Lajur Peratus (%)
Data asal mempunyai susunan: `Bil`, `%`, `Bil`, `%`. Kita perlu membuang semua lajur peratusan untuk memudahkan analisis.

1.  Kenal pasti lajur yang mengandungi simbol **%** atau nombor perpuluhan (contoh: 0.00). Biasanya ia berada selang satu lajur selepas markah TP.
    *   *TP1 (Kekalkan), TP1 % (Delete)*
    *   *TP2 (Kekalkan), TP2 % (Delete)*
2.  **Cara Cepat:** Tekan dan tahan butang **Ctrl** pada papan kekunci, kemudian klik huruf *column header* bagi setiap lajur peratus (biasanya F, H, J, L, N, P, R).
3.  Klik kanan pada mana-mana header yang dipilih > **Delete**.

---

### 3. Tapis (Filter) Data Sekolah Sahaja
Kita perlu membuang baris-baris tajuk yang tidak diperlukan supaya hanya tinggal data murid/sekolah.

1.  Klik pada baris tajuk (**Row 1**). Pergi ke tab **Data** > **Filter**.
2.  Di **Column C** (Kod Sekolah), klik butang panah Filter.
3.  **Nyahtanda (Uncheck)** pada kotak berikut:
    *   (Blanks)
    *   Perkataan "Kod Sekolah"
    *   Ayat yang mengandungi simbol "|"
4.  Kini hanya tinggal data sekolah yang sah.
    > *Pilihan:* Jika anda hanya mahu satu sekolah (cth: SEKOLAH KEBANGSAAN NANGA DAYU), anda boleh menaip kod sekolah atau nama sekolah di filter Column D.

---

### 4. Pecahkan Tahun & Subjek
Sekarang Column A mengandungi gabungan "TAHUN | MATA PELAJARAN". Kita perlu memisahkan maklumat ini.

1.  Pilih **Column A**.
2.  Pergi ke tab **Data** > **Text to Columns**.
3.  Pilih **Delimited** > **Next**.
4.  Tandakan **Other** dan taip simbol paip `|` di dalam kotak tersebut. Klik **Finish**.
5.  Hasilnya:
    *   **Column A:** TAHUN (Tingkatan/Tahun).
    *   **Column B:** MATA PELAJARAN.

---

### 5. Format & Standardisasi Subjek (Find & Replace)
Tukarkan nama subjek yang panjang kepada singkatan standard untuk kekemasan data.

1.  Pilih lajur **Mata Pelajaran** (Column B).
2.  Tekan **Ctrl + H** (Find and Replace).
3.  Lakukan penggantian berikut (Pastikan tiada *space* berlebihan):
    *   Find: ` BAHASA IBAN` (ada space di depan) -> Replace with: `BIN` -> **Replace All**
    *   Find: `BAHASA MELAYU` -> Replace with: `BM` -> **Replace All**
    *   Find: `BAHASA INGGERIS` -> Replace with: `BI` -> **Replace All**
    *   Find: `MATEMATIK` -> Replace with: `MM` -> **Replace All**
    *   Find: `SAINS` -> Replace with: `SAINS` -> **Replace All**

---

### 6. Susun Mengikut Format Sasaran
Akhir sekali, susun semula lajur (*Cut & Insert Cut Cells*) supaya mengikut urutan standard berikut:

1.  **SEKOLAH** (Nama Sekolah)
2.  **TAHUN** (Dari lajur A tadi)
3.  **MATA PELAJARAN** (Dari lajur B tadi)
4.  **FASA** (Taip "PBD" di baris pertama dan salin ke bawah)
5.  **BIL MURID DIDAFTAR** (Ambil dari lajur hujung sekali - 'Jumlah')
6.  **TP1 hingga TP6** (Lajur markah yang tinggal)
7.  **TD** (Lajur Tidak Diduduki)
