
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
Berikut adalah sambungan langkah-langkah untuk pemprosesan dan analisis data di dalam Google Sheets menggunakan Google Apps Script.

---

### 5. Penyediaan Data Google Sheet

Setelah data mentah berjaya ditarik masuk, langkah seterusnya adalah menyusun data tersebut agar sesuai untuk diproses oleh skrip automasi.

10. **Langkah Kesepuluh:**
    Fail yang dijana melalui URL tadi mungkin bersifat sementara atau "Read Only". Anda perlu membuat salinan fail tersebut ke dalam Google Drive anda sendiri.
    *   Klik **File** > **Make a copy**.
    *   Namakan fail tersebut sebagai **Table Data**.
    ![Make Copy](https://github.com/siewming85/SPPBPBD/blob/main/Screenshot_2026-01-21_15_10_43.png?raw=true)

11. **Langkah Kesebelas:**
    Lakukan saringan data (Filter) mengikut jenis pentaksiran yang diperlukan.
    *   Tekan **Ctrl + A** untuk memilih (select) semua data.
    *   Klik ikon **Filter**.
    *   Tapis lajur "Jenis Pentaksiran" kepada **Pentaksiran Berterusan Akhir Sesi Akademik** dan **Ujian Akhir Sesi Akademik**.
    ![Filter Data](https://github.com/siewming85/SPPBPBD/blob/main/Screenshot_2026-01-21_15_16_52.png?raw=true)

12. **Langkah Kedua Belas:**
    Asingkan data mengikut helaian (Sheet) untuk memudahkan analisis:
    *   Salin data **Pentaksiran Berterusan** dan tampal ke dalam *Sheet* baharu bernama **PBD Akhir** (atau "Pertengahan" bergantung pada kod).
    *   Salin data **Ujian Akhir Sesi Akademik** ke dalam *Sheet* baharu bernama **UASA**.
    ![Asing Data](https://github.com/siewming85/SPPBPBD/blob/main/Screenshot_2026-01-21_15_21_23.png?raw=true)

---

### 6. Analisis Automatik Menggunakan Apps Script

Bahagian ini akan menggunakan pengaturcaraan Google Apps Script untuk menjana analisis TP1-TP6 secara automatik.

13. **Langkah Ketiga Belas:**
    Buka editor skrip dengan klik pada **Extensions** > **Apps Script**.
    ![Apps Script Menu](https://github.com/siewming85/SPPBPBD/blob/main/Screenshot_2026-01-21_15_22_40.png?raw=true)

14. **Langkah Keempat Belas:**
    Padamkan sebarang kod sedia ada di dalam editor (`function myfunction...`), kemudian salin dan tampal kod penuh di bawah:

    > **Nota:** Pastikan nama *sheet* dalam kod (contohnya `"PBD AKHIR"`) sepadan dengan nama *sheet* data anda di Langkah 12.

    ```javascript
    function myFunction() {
      var ss = SpreadsheetApp.getActiveSpreadsheet();
      // Pastikan nama sheet ini wujud dalam Google Sheet anda
      var sheet = ss.getSheetByName("PBD AKHIR"); //setting here 
      var schoolnames = getUniqueSchoolNames();
      var dataRange = sheet.getDataRange();
      var values = dataRange.getValues();
      var headers = values[0];
      
      var results = [];

      schoolnames.forEach((school) => {
        var schoolData = values.filter(row => row[2] === school);
        var uniqueTahun = [...new Set(schoolData.map(row => row[3]))];
        
        uniqueTahun.forEach(tahun => {
          var availableSubjects = findAvailableSubjects(schoolData.filter(row => row[3] === tahun), headers);
          availableSubjects.forEach((subject) => {
            var subjectData = processSubjectData(school, tahun, subject, schoolData,headers);
            results.push(subjectData);
          });
        });
      });

      Logger.log(results);
      var sheet2 = ss.insertSheet("RUMUSANKELAS");
      sheet2.appendRow(["SEKOLAH",	"Tahun",	"MATA PELAJARAN",	"FASA",	"Bil Murid Didaftar",	"TP1",	"TP2",	"TP3",	"TP4",	"TP5",	"TP6",	"TD"]);
      sheet2.getRange(2,1,results.length,12).setValues(results);
    }

    function processSubjectData(school, tahun, subject, schoolData,headers) {
      var subjectIndex = headers.indexOf(subject);
      var subjectRows = schoolData.filter(row => row[3] === tahun && row[subjectIndex] !== "");
      
      var bilMuridDitaksir = subjectRows.length;
      var typeOfAssessment = "PBD"; 
      
      var tpCounts = [0, 0, 0, 0, 0, 0];
      var tdCount = 0;
      subjectRows.forEach(row => {
        var value = row[subjectIndex];
        if (value && typeof value === 'string') {
          for (var i = 1; i <= 6; i++) {
            if (value.includes('TP' + i)) {
              tpCounts[i-1]++;
            }
          }
          if (value.includes('TD')) {
            tdCount++;
          }
        }
      });

      return [
        school, tahun, subject, typeOfAssessment, bilMuridDitaksir,
        tpCounts[0], tpCounts[1], tpCounts[2], tpCounts[3], tpCounts[4], tpCounts[5], tdCount
      ];
    }

    function findAvailableSubjects(yearData, headers) {
      var availableSubjects = [];
      var subjectStartIndex = 8; // Assuming subjects start from the 9th column (index 8)
      
      for (var i = subjectStartIndex; i < headers.length; i++) {
        var subjectHasTP = yearData.some(row => {
          var value = row[i];
          return value && (value.toString().includes('TP') || /TP[1-6]/.test(value));
        });
        
        if (subjectHasTP) {
          availableSubjects.push(headers[i]);
        }
      }
      return availableSubjects;
    }

    function getUniqueSchoolNames() {
      var ss = SpreadsheetApp.getActiveSpreadsheet();
      var sheet = ss.getSheetByName("PBD AKHIR"); //setting here
      var dataRange = sheet.getRange("C2:C");
      var values = dataRange.getValues();
      
      var uniqueSchools = [];
      for (var i = 0; i < values.length; i++) {
        var schoolName = values[i][0];
        if (schoolName && uniqueSchools.indexOf(schoolName) === -1) {
          uniqueSchools.push(schoolName);
        }
      }
      return uniqueSchools;
    }

    /**
     * PROCESS PBD ANALYSIS BY SUBJECT
     */
    function runPBDSubjectAnalysis() {
      var ss = SpreadsheetApp.getActiveSpreadsheet();
      var sourceSheetName = "PBD AKHIR"; // Pastikan nama ini sepadan dengan sheet sumber anda
      var sourceSheet = ss.getSheetByName(sourceSheetName);
      
      if (!sourceSheet) {
        SpreadsheetApp.getUi().alert("Sheet '" + sourceSheetName + "' not found.");
        return;
      }

      var data = sourceSheet.getDataRange().getValues();
      var headers = data[0];
      
      for (var col = 8; col < headers.length; col++) {
        var subjectName = headers[col];
        if (subjectName && subjectName !== "") {
          processSingleSubject(ss, data, col, subjectName);
        }
      }
      SpreadsheetApp.getUi().alert("Analysis Complete! Sheets created for each subject.");
    }

    function processSingleSubject(ss, allData, colIndex, subjectName) {
      var stats = {};
      
      for (var i = 1; i < allData.length; i++) {
        var row = allData[i];
        var sekolah = row[2]; 
        var tahun = row[3];   
        var tpValue = row[colIndex]; 
        
        if (!sekolah || !tahun || !tpValue) continue;
        
        tpValue = tpValue.toString().toUpperCase().trim();
        var tpNum = 0;
        var match = tpValue.match(/TP(\d)/);
        if (match) {
          tpNum = parseInt(match[1]);
        } else {
          continue; 
        }
        
        var key = sekolah + "|" + tahun;
        
        if (!stats[key]) {
          stats[key] = {
            sekolah: sekolah, tahun: tahun,
            tp1: 0, tp2: 0, tp3: 0, tp4: 0, tp5: 0, tp6: 0, total: 0
          };
        }
        
        if (tpNum >= 1 && tpNum <= 6) {
          stats[key]["tp" + tpNum]++;
          stats[key].total++;
        }
      }
      
      var outputRows = [];
      for (var key in stats) {
        var item = stats[key];
        var total = item.total;
        if (total === 0) continue; 
        
        var pct1 = item.tp1 / total;
        var pct2 = item.tp2 / total;
        var pct3 = item.tp3 / total;
        var pct4 = item.tp4 / total;
        var pct5 = item.tp5 / total;
        var pct6 = item.tp6 / total;
        var pct12 = (item.tp1 + item.tp2) / total;
        
        outputRows.push([
          item.sekolah, item.tahun, total,
          item.tp1, item.tp2, item.tp3, item.tp4, item.tp5, item.tp6,
          pct1, pct2, pct3, pct4, pct5, pct6, pct12
        ]);
      }
      
      outputRows.sort(function(a, b) { return b[15] - a[15]; });
      
      var sheetName = "ANALISIS " + subjectName.substring(0, 20); 
      var targetSheet = ss.getSheetByName(sheetName);
      
      if (targetSheet) { targetSheet.clear(); } else { targetSheet = ss.insertSheet(sheetName); }
      
      var headerRow = [
        "SEKOLAH", "TAHUN/TINGKATAN", "BIL MURID", 
        "BIL TP1", "BIL TP2", "BIL TP3", "BIL TP4", "BIL TP5", "BIL TP6",
        "% TP1", "% TP2", "% TP3", "% TP4", "% TP5", "% TP6", "% (TP1+TP2)"
      ];
      
      targetSheet.getRange(1, 1, 1, headerRow.length).setValues([headerRow]);
      targetSheet.getRange(1, 1, 1, headerRow.length).setFontWeight("bold").setBackground("#cfe2f3");
      
      if (outputRows.length > 0) {
        targetSheet.getRange(2, 1, outputRows.length, headerRow.length).setValues(outputRows);
        targetSheet.getRange(2, 10, outputRows.length, 7).setNumberFormat("0.00%");
        
        var range = targetSheet.getRange(2, 16, outputRows.length, 1);
        var rule = SpreadsheetApp.newConditionalFormatRule()
          .whenNumberGreaterThan(0).setBackground("#f4cccc").setRanges([range]).build();
        var rules = targetSheet.getConditionalFormatRules();
        rules.push(rule);
        targetSheet.setConditionalFormatRules(rules);
      }
      targetSheet.autoResizeColumns(1, headerRow.length);
    }
    ```

15. **Langkah Kelima Belas:**
    Laksanakan skrip mengikut turutan berikut:
    1.  Klik ikon **Save** (cakera liut).
    2.  Pilih `myFunction` pada menu *dropdown* fungsi, kemudian klik **Run**. Berikan kebenaran (permission) jika diminta.
    3.  Setelah selesai, pilih `runPBDSubjectAnalysis` dan klik **Run** sekali lagi.

    Sistem akan menjana *Sheet* baharu secara automatik yang mengandungi analisis dan rumusan PBD mengikut subjek dan sekolah.

    ![Run Script](https://github.com/siewming85/SPPBPBD/blob/main/Screenshot_2026-01-21_15_26_04.png?raw=true)

    ![Rumusan Semua Kelas Semua MP Semua Tingkatan](https://github.com/siewming85/SPPBPBD/blob/main/Screenshot_2026-01-21_15_44_05.png?raw=true)
    ![ANALISIS IKUT MP](https://github.com/siewming85/SPPBPBD/blob/main/Screenshot_2026-01-21_15_44_23.png?raw=true)
