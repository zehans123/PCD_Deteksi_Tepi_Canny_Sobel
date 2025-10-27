import cv2
import numpy as np
import matplotlib.pyplot as plt
from skimage import data, img_as_ubyte, color

# === 1. Ambil citra sample dari skimage.data ===
# Menggunakan citra "kamera" yang kaya akan detail dan tepi
image_rgb = data.camera() 
# Mengubah citra ke Grayscale (jika belum) dan ke format unsigned byte (0-255)
image_gray = img_as_ubyte(image_rgb)

# === 2. Preprocessing: Noise Reduction ===
# Pengurangan noise penting sebelum deteksi tepi
blur_image = cv2.GaussianBlur(image_gray, (5, 5), 0)

# === 3. Deteksi Tepi Canny (Metode Terbaik) ===
# Nilai ambang batas (threshold) untuk Canny. Cobalah untuk mengubahnya!
low_threshold = 50
high_threshold = 150
canny_edges = cv2.Canny(blur_image, low_threshold, high_threshold)

# === 4. Deteksi Tepi Sobel (Metode Gradien Klasik) ===
# Sobel mendeteksi gradien horizontal (dx) dan vertikal (dy)

# a. Deteksi Sobel Horizontal (X)
sobel_x = cv2.Sobel(blur_image, cv2.CV_64F, 1, 0, ksize=3)
sobel_x = cv2.convertScaleAbs(sobel_x) # Mengubah nilai absolut menjadi uint8

# b. Deteksi Sobel Vertikal (Y)
sobel_y = cv2.Sobel(blur_image, cv2.CV_64F, 0, 1, ksize=3)
sobel_y = cv2.convertScaleAbs(sobel_y)

# c. Menggabungkan Sobel X dan Y
sobel_combined = cv2.addWeighted(sobel_x, 0.5, sobel_y, 0.5, 0)
# Gabungkan dengan perhitungan akar kuadrat (opsional, untuk tampilan lebih kuat)
# sobel_combined_sqrt = np.sqrt(sobel_x**2 + sobel_y**2)
# sobel_combined_sqrt = np.uint8(sobel_combined_sqrt / np.max(sobel_combined_sqrt) * 255)


# === 5. Visualisasi Hasil ===
plt.figure(figsize=(15, 10))

plt.subplot(2, 3, 1)
plt.imshow(image_gray, cmap='gray')
plt.title("Citra Asli (Kamera)")
plt.axis("off")

plt.subplot(2, 3, 2)
plt.imshow(blur_image, cmap='gray')
plt.title("Citra Setelah Gaussian Blur")
plt.axis("off")

plt.subplot(2, 3, 3)
plt.imshow(canny_edges, cmap='gray')
plt.title(f"Deteksi Tepi CANNY (T={high_threshold})")
plt.axis("off")

plt.subplot(2, 3, 4)
plt.imshow(sobel_x, cmap='gray')
plt.title("Sobel Tepi Horizontal (X)")
plt.axis("off")

plt.subplot(2, 3, 5)
plt.imshow(sobel_y, cmap='gray')
plt.title("Sobel Tepi Vertikal (Y)")
plt.axis("off")

plt.subplot(2, 3, 6)
plt.imshow(sobel_combined, cmap='gray')
plt.title("Sobel Gabungan (X+Y)")
plt.axis("off")

plt.suptitle("Perbandingan Metode Deteksi Tepi Canny vs. Sobel", fontsize=16, weight='bold')
plt.tight_layout(rect=[0, 0, 1, 0.95])
plt.show()
