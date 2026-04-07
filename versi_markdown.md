# 📂 Copy File "Bar TW 4" dari Banyak Folder (CMD Windows 10)

Saya memiliki struktur folder seperti ini:
```
D:\Tahun 2026\
 ├─ Folder1\
 │   └─ Bar TW 4 xxx.xlsx
 ├─ Folder2\
 │   └─ Bar TW 4 yyy.xlsx
 └─ ...
```
Tujuan: Mengumpulkan semua file yang mengandung "Bar TW 4" dari 42 folder ke satu folder:
```
D:\Kumpulan\
```
### ✅ Cara 1 — Copy Semua File (Semua Ekstensi)
```
for /r "D:\Tahun 2026" %f in (*Bar TW 4*) do copy "%f" "D:\Kumpulan\"
```
🔍 Penjelasan
`for /r` → mencari file ke semua subfolder
`*Bar TW 4*` → filter nama file (tanpa batasan ekstensi)
`copy` → menyalin file ke folder tujuan
##

#### ⚠️ Jika Menggunakan File .bat

Gunakan `%%` вместо `%`:
```
for /r "D:\Tahun 2026" %%f in (*Bar TW 4*) do copy "%%f" "D:\Kumpulan\"
```
##

#### ⚠️ Overwrite File dengan Nama Sama

Tambahkan `/Y` agar tidak ditanya:
```
for /r "D:\Tahun 2026" %f in (*Bar TW 4*) do copy /Y "%f" "D:\Kumpulan\"
```
##
### 🔒 Cara 2 — Copy Hanya Ekstensi Tertentu
Contoh: hanya `.xlsx`
```
for /r "D:\Tahun 2026" %f in (*Bar TW 4*.xlsx) do copy "%f" "D:\Kumpulan\"
```
### 🔁 Cara 3 — Multiple Ekstensi (Loop)
```
for %e in (xlsx pdf docx) do for /r "D:\Tahun 2026" %f in (*Bar TW 4*.%e) do copy "%f" "D:\Kumpulan\"
```
Versi `.bat`:
```
for %%e in (xlsx pdf docx) do for /r "D:\Tahun 2026" %%f in (*Bar TW 4*.%%e) do copy "%%f" "D:\Kumpulan\"
```
##

#### 🔗 Alternatif: Satu Baris Banyak Perintah
```
for /r "D:\Tahun 2026" %f in (*Bar TW 4*.xlsx) do copy "%f" "D:\Kumpulan\" & ^
for /r "D:\Tahun 2026" %f in (*Bar TW 4*.pdf) do copy "%f" "D:\Kumpulan\" & ^
for /r "D:\Tahun 2026" %f in (*Bar TW 4*.docx) do copy "%f" "D:\Kumpulan\"
```
##
### 🚀 Alternatif Lebih Cepat (ROBOCOPY)
```
robocopy "D:\Tahun 2026" "D:\Kumpulan" *Bar TW 4* /S
```
Kelebihan:
* Lebih cepat
* Lebih stabil untuk banyak file
* Otomatis handle subfolder
####
### 💡 Tips
Pattern lebih fleksibel:
```
*Bar*TW*4*
```
Akan menangkap variasi seperti:
* Bar TW 4
* Bar_TW_4
* Bar-TW-4
##
### 📌 Kesimpulan
* ✔ Bisa copy dari banyak folder sekaligus
* ✔ Bisa semua ekstensi atau dibatasi
* ❌ CMD tidak support multi-ekstensi dalam satu wildcard
* ✔ Gunakan loop untuk banyak ekstensi
* 🚀 Gunakan robocopy untuk performa terbaik
##
##
## 🧠 ADVANCED USAGE

### 🟡 8. Hindari File Tertimpa (Rename Otomatis)

Menambahkan nama folder asal ke file:
```
for /r "D:\Tahun 2026" %f in (*Bar TW 4*) do copy "%f" "D:\Kumpulan\%~nxf_%~pf"
```
⚠️ **Catatan:**

* `%~nxf` → nama file + ekstensi
* `%~pf` → path folder (akan panjang)
## 

### ✅ Versi lebih rapi (pakai nama folder saja)
```
for /r "D:\Tahun 2026" %f in (*Bar TW 4*) do (
  set "folder=%~dpf"
  for %%a in ("%~dpf.") do copy "%f" "D:\Kumpulan\%%~nxa_%~nxf"
)
```
##
### 🟢 9. Skip File Duplikat (Tidak Ditimpa)
```
for /r "D:\Tahun 2026" %f in (*Bar TW 4*) do if not exist "D:\Kumpulan\%~nxf" copy "%f" "D:\Kumpulan\"
```
##

### 🔵 10. Hanya Copy File Terbaru Saja

Gunakan robocopy:
```
robocopy "D:\Tahun 2026" "D:\Kumpulan" *Bar TW 4* /S /XO
```
##### Penjelasan:
* `/XO` → hanya copy file yang lebih baru
##

### 🟣 11. Copy Berdasarkan Tanggal (Misalnya Hari Ini)
```
forfiles /P "D:\Tahun 2026" /S /M *Bar TW 4* /D 0 /C "cmd /c copy @path D:\Kumpulan\"
```
##

### 🟠 12. Pattern Lebih Fleksibel
```
*Bar*TW*4*
```

Menangkap:
* Bar TW 4
* Bar_TW_4
* Bar-TW-4
##

### 🔴 13. Logging (Simpan Hasil Copy)
```
robocopy "D:\Tahun 2026" "D:\Kumpulan" *Bar TW 4* /S /LOG:hasil_copy.txt
```

##
## 📌 KESIMPULAN
| Kebutuhan | Solusi |
| --- | :---: |
| Semua file | `for /r` |
| Banyak ekstensi | loop `%e` |
| Cepat & stabil | robocopy |
| Hindari overwrite | rename / skip |
| Hanya terbaru | `/XO` |
| Ada Log | `/LOG` |
