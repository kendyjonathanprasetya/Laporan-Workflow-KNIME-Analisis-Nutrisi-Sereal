## 🎯 Deskripsi Proyek
Workflow KNIME ini membersihkan dan menganalisis data nutrisi 11 merek sereal (cereal) untuk menghasilkan dataset mini yang konsisten, bebas duplikat, dan siap dipakai untuk insight maupun modeling lanjutan.
## 📁 Dataset Masukan
File: CSV SEREAL DUPLIKAT.csv
Jumlah baris/kolom awal: 11 baris × 16 kolom
## 🔍 Penjelasan Kolom
| Kolom    | Emoji | Arti Singkat                                                                                        
| -------- | ----- | ----------------------------------------------------------------------------------------------    | 
| name     | 🏷️    | Nama produk sereal (contoh: Corn\_Flakes)                                                        |
| mfr      | 🏭    | Pabrikan: K=Kellogg’s, G=General Mills, Q=Quaker, R=Ralston, P=Post, N=Nabisco, A=American Home  |
| type     | 🌡️    | Jenis sereal: C = cold (dingin), H = hot (bubuh air panas)                                       | 
| calories | ⚡    | Energi per sajian (kcal); semakin tinggi semakin “berat”                                         |
| protein  | 🥩    | Protein per sajian (g); tinggi = lebih mengenyangkan                                             |
| fat      | 🧈    | Lemak per sajian (g); rendah = lebih sehat                                                       |
| sodium   | 🧂    | Garam (mg); rendah = lebih ramah jantung                                                         |
| fiber    | 🌾    | Serat pangan (g); tinggi = baik pencernaan & kolesterol                                          | 
| carbo    | 🍞    | Karbohidrat total (g); sumber energi utama                                                       |
| sugars   | 🍬    | Gula tambahan (g); rendah = ideal untuk diet rendah gula                                         |
| potass   | 🍌    | Kalium (mg); bagus tekanan darah, tapi sering kosong → kita buang                                | 
| vitamins | 💊    | Persen AKG vitamin tambahan (0, 25, 100 %); variasi kecil → kita buang                           |
| shelf    | 🛒    | Nomor rak display (1,2,3); info penjualan → kita buang                                           |
| weight   | ⚖️    | Berat isi kemasan (oz); info kemasan → kita buang                                                |
| cups     | 🥛    | Volume per sajian (cup); info takaran → kita buang                                               |
| rating   | ⭐    | Skor panelis (0–100); semakin tinggi semakin “enak”                                              |


## 🔄 Alur Workflow & Penjelasan Detail
1. CSV Reader 📥
Membaca file dari link OneDrive; output tabel 11 baris.
2. Column Filter 🔍
Menghapus kolom tidak relevan untuk analisis nutrisi & rating:
potass (banyak missing)
shelf, weight, cups (info kemasan, bukan nutrisi)
vitamins (variasi kecil, hanya 0-25-100)
Hasil: 11 baris, 10 kolom → dataset lebih ringan & fokus.
3. Duplicate Row Filter 🔄
Gunakan semua kolom (Includes)
Retain: first occurrence
Fungsi: buang baris 100 % identik supaya tidak double hit.
Hasil: 11 → 6 baris.
4. Numeric Row Filter 🎚️
Filter column: calories
Operator: <= 150
Tujuan: singkirkan outlier kalori tinggi (contoh: Sugar_Bomb 400 kcal).
Hasil: 6 → 5 baris.
5. Nominal Row Filter 🏷️
Filter column: type
Operator: Equals
Value: C (cold cereal)
Fokus: hanya sereal dingin; hot cereal (H) tidak ikut.
Hasil: 5 → 4 baris.
6. Data Explorer 📊
Menampilkan statistik deskriptif akhir: mean, min, max, std-dev.
Insight cepat:
Rating rata-rata ≈ 67 (IQR kecil) → panelis cukup puas.
Gula maks 6 g → efek filter kalori terlihat.
Fiber range 1–25 g → bisa dibedakan “high-fiber” vs “regular”.
7. Box Plot 📈
Dimension: rating, calories, sugars
Box rating pendek → data konsisten di atas 60.
Calories & sugars tidak lagi memiliki outlier ekstrem.
Median gula ≈ 3 g → cocok untuk klaim “low-sugar”.
8. Line Plot 📉
X: sugars | Y: rating | Color: name
Garis menurun gula → rating; Fiber_Max (gula 0) berada di puncak preferensi.
9. Pie Chart 🥧
Category: name | Frequency: count
Slice besar: Fiber_Max (0 g sugar) → “zero-sugar hero”.
75 % produk cold low-cal ≤ 5 g sugar → dominasi “low-sugar” jelas.
10. CSV Writer 💾 (opsional)
Export final: cereal_clean.csv (4 baris, 10 kolom) → siap dibuka Excel atau dilanjutkan modeling.


## 📊 Output Visual
Box Plot interaktif: perbandingan distribusi rating, kalori, dan gula.
Tabel statistik: ringkasan mean, min, max, std-dev setiap nutrisi.
Line Plot: garis hubungan gula (X) vs rating (Y) per produk; slope menurun memperkuat temuan “low-sugar = disukai”.
Pie Chart: proporsi jumlah produk per kategori gula (0 g, 3 g, 5 g, 6 g) – mayoritas 3 g mendominasi, memperlihatkan konsentrasi “low-sugar” setelah filter.
## 💡 Insight Utama
Duplikat & outlier kalori >150 terbuang; rating tetap tinggi (median 67).
Gula tersaring maks 6 g → layak klaim “low-sugar”.
Fiber menjadi pembeda utama (range 1–25 g) → segmen “high-fiber” tampak jelas.
Dataset akhir bebas missing, duplikat, dan ekstrem value → siap untuk clustering atau regresi mini.
## 🎯 Tujuan Analisis
Membersihkan data dari duplikat, outlier kalori, dan kolom tidak relevan agar siap digunakan untuk pemodelan atau visualisasi lanjutan.
Menvisualisasikan hubungan antara kandungan gula, kalori, dan rating untuk membuktikan bahwa produk dengan gula rendah tetap memiliki rating tinggi.
Menyediakan dataset mini (4 baris, 10 kolom) yang konsisten dan bebas missing value sebagai bahan latihan clustering, regresi, atau storytelling sederhana.
## 🛠️ Cara Menggunakan
Buka KNIME Analytics Platform.
Import workflow ini.
Pastikan file CSV SEREAL DUPLIKAT.csv ada di folder yang sama (atau ubah path di CSV Reader).
Jalankan node secara berurutan (1→10).
Lihat hasil statistik & visual; simpan clean data via CSV Writer bila perlu.
