🧠 Deteksi Tepi Menggunakan Metode Canny dan Sobel

📚 Mata Kuliah: Pengolahan Citra Digital

👤 Oleh: Rizky Zehans Onassis — Universitas Pamulang

🎯 Deskripsi Proyek

Proyek ini bertujuan untuk membandingkan dua metode deteksi tepi yang populer dalam pengolahan citra digital, yaitu Canny Edge Detection dan Sobel Operator.
Keduanya digunakan untuk mendeteksi batas objek dalam gambar dengan pendekatan berbeda:

- Canny menggunakan pendekatan berbasis gradien dan threshold adaptif.

- Sobel menggunakan perhitungan turunan pertama untuk mendeteksi perubahan intensitas piksel.

🧩 Tahapan Implementasi
1️⃣ Pengambilan Citra

Menggunakan dataset bawaan dari skimage.data, yaitu citra “camera” yang kaya akan detail dan kontras tepi.

image_rgb = data.camera()
image_gray = img_as_ubyte(image_rgb)

2️⃣ Preprocessing (Noise Reduction)

Sebelum proses deteksi tepi, citra difilter menggunakan Gaussian Blur untuk mengurangi noise yang dapat mengganggu hasil.

blur_image = cv2.GaussianBlur(image_gray, (5, 5), 0)

3️⃣ Deteksi Tepi Canny

Metode Canny Edge Detection memberikan hasil tepi yang lebih halus dan presisi melalui tahapan gradient calculation, non-maximum suppression, dan hysteresis thresholding.

canny_edges = cv2.Canny(blur_image, 50, 150)

4️⃣ Deteksi Tepi Sobel

Metode Sobel menghitung gradien intensitas pada arah horizontal dan vertikal, kemudian menggabungkannya untuk mendapatkan citra tepi.

sobel_x = cv2.Sobel(blur_image, cv2.CV_64F, 1, 0, ksize=3)
sobel_y = cv2.Sobel(blur_image, cv2.CV_64F, 0, 1, ksize=3)
sobel_combined = cv2.addWeighted(sobel_x, 0.5, sobel_y, 0.5, 0)

5️⃣ Visualisasi Hasil

Perbandingan hasil dari kedua metode divisualisasikan menggunakan Matplotlib, seperti pada contoh berikut:

Citra Asli	Gaussian Blur	Canny Edge Detection
	
Sobel X	Sobel Y	Sobel Gabungan
	
🧠 Kesimpulan

- Canny lebih unggul dalam mendeteksi tepi halus dengan tingkat noise rendah.
- Sobel lebih sederhana dan cepat, namun hasilnya bisa lebih kasar tergantung pada kondisi pencahayaan dan noise citra.
