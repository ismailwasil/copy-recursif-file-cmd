# Copy File Recursif dengan CMD
Ini adalah perintah untuk copy file secara recursif dengan command prompt (CMD).

Misal struktur foldernya begini:
```
D:\Tahun 2026\
 ├─ Folder1\
 │   └─ Bar TW 4 xxx.xlsx
 ├─ Folder2\
 │   └─ Bar TW 4 yyy.xlsx
 └─ ...
```
Dan kita ingin kumpulkan semua file yang mengandung "Bar TW 4" ke:
```
D:\Kumpulan\
```
---
## ✅ Cara Pakai CMD
1. Buka **Command Prompt (CMD)**
2. Jalankan perintah berikut:
    ```
    for /r "D:\Tahun 2026" %f in (*Bar TW 4*) do copy "%f" "D:\Kumpulan\"
    ```
#### 🔍 Penjelasan
* ```for /r``` → mencari file ke semua subfolder (rekursif)
* ```"D:\Tahun 2026"``` → folder utama
* ```*Bar TW 4*``` → filter nama file yang mengandung teks itu
* ```%f``` → variabel file yang ditemukan
* ```copy``` → menyalin ke folder tujuan
#### ⚠ Penting Diperhatikan
1. **Kalau dijalankan dari file** ```.bat```\
   Kalau kita simpan perintah ini di file .bat, harus pakai %%f (double persen):\
   ```
   for /r "D:\Tahun 2026" %%f in (*Bar TW 4*) do copy "%%f" "D:\Kumpulan\"
   ```
2. **Kalau ada file dengan nama sama**\
   CMD akan menimpa (overwrite) tanpa tanya jika pakai ```/Y``` :
   ```
   for /r "D:\Tahun 2026" %f in (*Bar TW 4*) do copy /Y "%f" "D:\Kumpulan\"
   ```
   Kalau tidak pakai /Y, dia akan tanya satu-satu.
3. **Alternatif lebih aman (pakai ROBOCOPY)**\
   Lebih kuat dan cepat:\
   ```
   robocopy "D:\Tahun 2026" "D:\Kumpulan" *Bar TW 4* /S
   ```
   - ```/S``` → termasuk subfolder
   - otomatis handle banyak file lebih stabil
---
## 🔒 Kalau mau dibatasi ekstensi tertentu
#### Contoh hanya Excel:
```
for /r "D:\Tahun 2026" %f in (*Bar TW 4*.xlsx) do copy "%f" "D:\Kumpulan\"
```
#### Contoh beberapa ekstensi (pakai trik sederhana):
CMD tidak langsung support multi-ekstensi dalam satu pattern, jadi bisa jalankan beberapa kali:\
```
for /r "D:\Tahun 2026" %f in (*Bar TW 4*.xlsx) do copy "%f" "D:\Kumpulan\"
for /r "D:\Tahun 2026" %f in (*Bar TW 4*.pdf) do copy "%f" "D:\Kumpulan\"
```
## 💡 Tips penting
Kalau nama file kamu kadang beda spasi atau variasi, kamu bisa pakai lebih longgar:\
```
*Bar*TW*4*
```
Ini akan tetap menangkap:
- ```Bar TW 4```
- ```Bar_TW_4```
- ```Bar-TW-4```
---
## ❓ Lalu Bagaimana untuk multi ekstensi dalam satu ekspresi?
Jawaban singkatnya: **bisa, tapi tidak langsung dalam satu pattern CMD biasa** ❗\
CMD (```for / copy```) tidak mendukung multi-ekstensi seperti ```*.xlsx;*.pdf``` dalam satu ekspresi.\
Tapi ada beberapa cara workaround yang tetap “satu perintah” 👇

#### ✅ Cara 1: Pakai beberapa perintah dalam satu baris
  Gunakan `&` untuk menggabungkan:
  ```
  for /r "D:\Tahun 2026" %f in (*Bar TW 4*.xlsx) do copy "%f" "D:\Kumpulan\" & ^
  for /r "D:\Tahun 2026" %f in (*Bar TW 4*.pdf) do copy "%f" "D:\Kumpulan\" & ^
  for /r "D:\Tahun 2026" %f in (*Bar TW 4*.docx) do copy "%f" "D:\Kumpulan\"
  ```
  ✔ Ini tetap satu kali enter\
  ❌ Tapi agak panjang

#### ✅ Cara 2: Loop ekstensi (lebih rapi 👍)
  Ini yang paling direkomendasikan:
  ```
  for %e in (xlsx pdf docx) do for /r "D:\Tahun 2026" %f in (*Bar TW 4*.%e) do copy "%f" "D:\Kumpulan\"
  ```
  ✔ Kelebihan:
  * Lebih singkat
  * Tinggal tambah ekstensi di `(xlsx pdf docx txt dll)`
