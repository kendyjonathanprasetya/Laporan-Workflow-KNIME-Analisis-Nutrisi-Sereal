## 🎯 Deskripsi Proyek
Workflow KNIME ini membersihkan dan menganalisis data nutrisi 11 merek sereal (cereal) untuk menghasilkan dataset mini yang konsisten, bebas duplikat, dan siap dipakai visualisasi maupun modeling lanjutan.
## 📁 Dataset Masukan
File: https://d.docs.live.net/8F1EA89ED3B71980/Documents/CSV%20SEREAL%20DUPLIKAT.csv
Jumlah baris/kolom awal: 11 baris × 16 kolom

Kolom:

name → nama produk sereal (contoh: “Corn_Flakes”, “All-Bran”).

mfr → singkatan “manufacturer” (pabrikan); K = Kellogg’s, G = General Mills, Q = Quaker, R = Ralston, P = Post, N = Nabisco, A = American Home.

type → jenis sereal: C = cold (makan dingin), H = hot (bubuk, diseduh air/panas).

calories → energi per serving (kcal); semakin tinggi semakin “berat” kalorinya.

protein → protein per serving (gram); tinggi = lebih mengenyangkan.

fat → lemak per serving (gram); kita kejar yang rendah.

sodium → garam (mg); rendah = lebih sehat untuk jantung.

fiber → serat pangan (gram); tinggi = baik pencernaan, kolesterol.

carbo → karbohidrat total (gram); sumber energi utama.

sugars → gula tambahan (gram); rendah = ideal untuk diet rendah gula.

potass → kalium (mg); bagus tekanan darah, tapi sering kosong → kita buang.

vitamins → persen AKG vitamin tambahan (0, 25, atau 100 %); kita anggap tidak variatif.

shelf → nomor rak display (1, 2, 3); info penjualan, bukan nutrisi → kita buang.

weight → berat isi kemasan (oz); info kemasan → kita buang.

cups → volume per serving (cup); info takaran → kita buang.

rating → skor panelis (0–100); semakin tinggi semakin “enak” menurut penguji.


## 🔄 Alur Workflow & Penjelasan Langkah
- CSV Reader 📥
Membaca file mentah; output berupa tabel utuh 11 baris.

- Column Filter 🔍
Menghapus kolom yang tidak relevan untuk analisis nutrisi & rating:
potass (banyak missing)
shelf, weight, cups (info kemasan, bukan nutrisi)
vitamins (variasi kecil, hanya 0-25-100)
Hasil: 11 baris, 10 kolom → dataset lebih ringan & fokus.

- Duplicate Row Filter 🔄
Gunakan semua kolom (Includes)
Retain: first occurrence
Fungsi: buang baris 100 % identik supaya tidak double-hit.
Hasil: 11 → 6 baris.

- Numeric Row Filter 🎚️
Filter column: calories
Operator: <= 150
Tujuan: singkirkan outlier kalori tinggi (contoh: Sugar_Bomb 400 kcal).
Hasil: 6 → 5 baris.

- Nominal Row Filter 🏷️
Filter column: type
Operator: Equals
Value: C (cold cereal)
Fokus: hanya sereal dingin; hot cereal (H) tidak ikut.
Hasil: 5 → 4 baris.

- Data Explorer 📊
Menampilkan statistik deskriptif akhir: mean, min, max, std-dev.
Insight cepat:
Rating rata-rata ≈ 67 (IQR sempit) → panelis cukup puas.
Gula maks 6 g → efek filter kalori terlihat.
Fiber range 1–25 g → bisa dibedakan “high-fiber” vs “regular”.

- Box Plot 📈
Dimension: rating, calories, sugars
Box rating pendek → data konsisten di atas 60.
Calories & sugars tidak lagi memiliki outlier ekstrem.
Median gula ≈ 3 g → cocok untuk klaim “low-sugar”.

- Line Plot 📉
X: sugars | Y: rating | Color: name
Garis menurun gula → rating; Fiber_Max (gula 0) berada di puncak preferensi.

## 📊 Output Visual
Box Plot interaktif: perbandingan distribusi rating, kalori, dan gula.
Tabel statistik: ringkasan mean, min, max, std-dev setiap nutrisi.
## 💡 Insight Utama
Duplikat & outlier kalori >150 terbuang; rating tetap tinggi (median 67).
Gula tersaring maks 6 g → layak klaim “low-sugar”.
Fiber menjadi pembeda utama (range 1–25 g) → segmen “high-fiber” tampak jelas.
Dataset akhir bebas missing, duplikat, dan ekstrem value → siap untuk clustering atau regresi mini.
## 🛠️ Cara Menggunakan
Buka KNIME Analytics Platform.
Import workflow ini.
Pastikan cereal_dirty.csv ada di folder yang sama.
Jalankan node secara berurutan (1→8).
Lihat hasil statistik & Box Plot; simpan clean data via CSV Writer bila perlu.
