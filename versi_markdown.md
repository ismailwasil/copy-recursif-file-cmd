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

D:\Kumpulan\
✅ Cara 1 — Copy Semua File (Semua Ekstensi)
for /r "D:\Tahun 2026" %f in (*Bar TW 4*) do copy "%f" "D:\Kumpulan\"
🔍 Penjelasan
for /r → mencari file ke semua subfolder
*Bar TW 4* → filter nama file (tanpa batasan ekstensi)
copy → menyalin file ke folder tujuan
⚠️ Jika Menggunakan File .bat

Gunakan %% вместо %:

for /r "D:\Tahun 2026" %%f in (*Bar TW 4*) do copy "%%f" "D:\Kumpulan\"
⚠️ Overwrite File dengan Nama Sama

Tambahkan /Y agar tidak ditanya:

for /r "D:\Tahun 2026" %f in (*Bar TW 4*) do copy /Y "%f" "D:\Kumpulan\"
🔒 Cara 2 — Copy Hanya Ekstensi Tertentu
Contoh: hanya .xlsx
for /r "D:\Tahun 2026" %f in (*Bar TW 4*.xlsx) do copy "%f" "D:\Kumpulan\"
🔁 Cara 3 — Multiple Ekstensi (Loop)
for %e in (xlsx pdf docx) do for /r "D:\Tahun 2026" %f in (*Bar TW 4*.%e) do copy "%f" "D:\Kumpulan\"
Versi .bat:
for %%e in (xlsx pdf docx) do for /r "D:\Tahun 2026" %%f in (*Bar TW 4*.%%e) do copy "%%f" "D:\Kumpulan\"
🔗 Alternatif: Satu Baris Banyak Perintah
for /r "D:\Tahun 2026" %f in (*Bar TW 4*.xlsx) do copy "%f" "D:\Kumpulan\" & ^
for /r "D:\Tahun 2026" %f in (*Bar TW 4*.pdf) do copy "%f" "D:\Kumpulan\" & ^
for /r "D:\Tahun 2026" %f in (*Bar TW 4*.docx) do copy "%f" "D:\Kumpulan\"
🚀 Alternatif Lebih Cepat (ROBOCOPY)
robocopy "D:\Tahun 2026" "D:\Kumpulan" *Bar TW 4* /S
Kelebihan:
Lebih cepat
Lebih stabil untuk banyak file
Otomatis handle subfolder
💡 Tips
Pattern lebih fleksibel:
*Bar*TW*4*

Akan menangkap variasi seperti:

Bar TW 4
Bar_TW_4
Bar-TW-4
📌 Kesimpulan
✔ Bisa copy dari banyak folder sekaligus
✔ Bisa semua ekstensi atau dibatasi
❌ CMD tidak support multi-ekstensi dalam satu wildcard
✔ Gunakan loop untuk banyak ekstensi
🚀 Gunakan robocopy untuk performa terbaik
