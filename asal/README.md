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


***

## Bahagian C: Penghasilan Analisis Mengikut Mata Pelajaran (Satu Tab Satu Subjek)

Langkah ini bertujuan untuk memecahkan data induk kepada helaian (*sheet*) berasingan mengikut subjek (Contoh: BM, BI, MM) dan mengira peratusan TP serta peratusan intervensi (TP1+TP2).

### 1. Persediaan Data Induk (Master Data)
Pastikan anda berada di helaian data yang telah dibersihkan dalam **Bahagian B**. Kita akan menggunakan data ini sebagai sumber utama.

1.  Klik pada tajuk lajur **MATA PELAJARAN** (biasanya Column C).
2.  Pergi ke tab **Data** > klik butang **Filter**.

---

### 2. Proses Pengasingan Mata Pelajaran (Contoh: Bahasa Melayu)
Ulangi proses ini untuk setiap mata pelajaran (BM, BI, MM, Sains, dll). Kita mulakan dengan **BM**.

1.  Pada *filter* **MATA PELAJARAN**, tandakan (**check**) hanya `BM`. Klik **OK**.
2.  Pilih semua data yang terpapar (Tekan `Ctrl + A`).
3.  **Copy** (`Ctrl + C`).
4.  Buka *Sheet* baru (klik tanda `+` di bawah). Namakan sheet ini **"ANALISIS BM"**.
5.  **Paste** (`Ctrl + V`) di dalam sheet baru tersebut.
6.  Di sheet baru ini, anda boleh **Delete** lajur *FASA*, *MATA PELAJARAN*, dan *TD* kerana kita tidak memerlukannya dalam format akhir sasaran.

---

### 3. Menambah Lajur Peratusan & Formula
Sekarang kita akan wujudkan lajur pengiraan peratusan di sebelah kanan lajur TP6.

**Susunan Lajur Anda Sepatutnya Begini Sekarang:**
*   A: SEKOLAH
*   B: TAHUN
*   C: BIL MURID
*   D: TP1 | E: TP2 | F: TP3 | G: TP4 | H: TP5 | I: TP6

**Langkah Menambah Formula:**

1.  Di sel **J1**, taip tajuk `% TP1`.
2.  Di sel **J2**, masukkan formula:
    ```excel
    =IF(C2=0, 0, D2/C2)
    ```
3.  Salin formula ini ke kanan sehingga lajur **O** (`% TP6`).
    *   *Nota: Excel akan automatik mengira TP2/Bil Murid, TP3/Bil Murid dan seterusnya.*
4.  Di sel **P1**, taip tajuk `% (TP1+TP2)`.
5.  Di sel **P2**, masukkan formula untuk murid yang belum menguasai tahap minimum:
    ```excel
    =IF(C2=0, 0, (D2+E2)/C2)
    ```
6.  Pilih sel **J2 hingga P2**, kemudian **Double-click** bucu bawah kanan untuk menyalin formula ke semua baris di bawah.

---

### 4. Format Peratusan (%)
Supaya data mudah dibaca (contoh: 0.5 jadi 50%):

1.  Pilih lajur **J** hingga **P**.
2.  Klik kanan > **Format Cells**.
3.  Pilih **Percentage**. Tetapkan *Decimal places* kepada **2**.
4.  Klik **OK**.

---

### 5. Susunan (Sorting) Mengikut Keutamaan Intervensi
Untuk mendapatkan paparan seperti contoh sasaran (sekolah yang paling ramai TP1+TP2 berada di atas):

1.  Klik pada mana-mana sel dalam data.
2.  Pergi ke tab **Data** > **Sort**.
3.  Tetapkan aturan berikut:
    *   **Sort by:** `% (TP1+TP2)` (Lajur P)
    *   **Sort On:** Cell Values
    *   **Order:** Largest to Smallest (Z to A)
4.  Klik **OK**.

> **Hasil:** Sekolah dengan peratus murid TP1 & TP2 tertinggi akan berada di baris paling atas untuk memudahkan PPD mengenal pasti sekolah fokus.

---

### 6. Kemasan Akhir (Header)
Tukar tajuk lajur supaya sama dengan format sasaran anda:

*   Tukar "Tahun" kepada **TAHUN/TINGKATAN**.
*   Tukar "Bil Murid Didaftar" kepada **BIL MURID**.
*   Tukar "TP1" kepada **BIL TP1** (dan seterusnya untuk TP2-TP6).

### 7. Ulang untuk Subjek Lain
Untuk subjek seterusnya (contoh: BI):
1.  Kembali ke **Sheet Data Induk**.
2.  Tukar Filter Mata Pelajaran kepada **BI**.
3.  Ulang langkah 2 hingga 6 di atas dalam sheet baru bernama **"ANALISIS BI"**.

---

## Ringkasan Formula Penting

| Data Sasaran | Formula (Andaikan C=Bil Murid, D=TP1, E=TP2) |
| :--- | :--- |
| **% TP1** | `=D2/C2` |
| **% TP2** | `=E2/C2` |
| ... | ... |
| **% (TP1+TP2)** | `=(D2+E2)/C2` |

*(Pastikan format cell ditukar kepada Percentage)*
