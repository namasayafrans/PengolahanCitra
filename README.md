# PengolahanCitra

```
Nama        : Frans Putra Sinaga - 312210046
              Syahbaruddin Abdillah - 312210656
              Azwa Dipani - 312210417
Kelas       : TI.22.A1
Mata kuliah : Pengolahan Citra
Dosen       : Muhammad Fatchan, S.Kom., M.Kom., MTCNA
```

## DAFTAR ISI
| No | Description | Link |
|-----|------|-----|
|1|File Praktikum|[Click Here](#file-praktikum)|
|2|Hasil Praktikum|[Click Here](#hasil-praktikum)|

## File Praktikum
file knn.py lalu di jalankan di Streamlit <br>
```
import streamlit as st
import numpy as np
import cv2
from PIL import Image
import matplotlib.colors as mcolors

# Fungsi untuk melakukan segmentasi gambar menggunakan K-Means Clustering
def segment_image(image, k, max_iter=100, epsilon=0.85):
    image = cv2.cvtColor(image, cv2.COLOR_RGB2BGR)
    pixel_vals = image.reshape((-1, 3))
    pixel_vals = np.float32(pixel_vals)
    criteria = (cv2.TERM_CRITERIA_EPS + cv2.TERM_CRITERIA_MAX_ITER, max_iter, epsilon)
    _, labels, centers = cv2.kmeans(pixel_vals, k, None, criteria, 100, cv2.KMEANS_RANDOM_CENTERS)
    centers = np.uint8(centers)
    segmented_data = centers[labels.flatten()]
    segmented_image = segmented_data.reshape((image.shape))
    segmented_image = cv2.cvtColor(segmented_image, cv2.COLOR_BGR2RGB)
    segment_percentages = [(np.sum(labels == i) / len(labels)) * 100 for i in range(k)]
    return segmented_image, centers, segment_percentages

# Fungsi untuk mengenali warna berdasarkan nilai RGB
def get_color_name(center):
    colors = {
        "Merah": [255, 0, 0],
        "Hijau": [0, 255, 0],
        "Biru": [0, 0, 255],
        "Kuning": [255, 255, 0],
        "Cyan": [0, 255, 255],
        "Magenta": [255, 0, 255],
        "Putih": [255, 255, 255],
        "Hitam": [0, 0, 0],
        "Oranye": [255, 165, 0],
        "Abu-abu": [128, 128, 128]
    }
    color_name = "Tidak diketahui"
    min_dist = float('inf')
    for name, color in colors.items():
        dist = np.linalg.norm(center - np.array(color))
        if dist < min_dist:
            min_dist = dist
            color_name = name
    return color_name

# Konfigurasi halaman Streamlit
st.set_page_config(
    page_title="K-Means",
    page_icon=":art:",
    layout="wide"
)

# Menambahkan gaya CSS untuk mempercantik tampilan
st.markdown(
    """
    <style>
    .stApp {
        background-color: #000000;
    }
    .st-bw {
        background-color: #000000;
        padding: 20px;
        border-radius: 10px;
        box-shadow: 0px 0px 20px rgba(255, 255, 255, 0.1);
        color: white;
    }
    .segmented-image {
        border-radius: 10px;
        box-shadow: 0px 0px 20px rgba(255, 255, 255, 0.1);
        margin-top: 20px;
    }
    .sidebar .sidebar-content {
        background-color: #000000;
        box-shadow: 0px 0px 20px rgba(255, 255, 255, 0.1);
        border-radius: 10px;
        padding: 20px;
        margin-top: 20px;
        color: white;
    }
    .sidebar .sidebar-content h2 {
        font-size: 24px;
        font-weight: bold;
        margin-bottom: 10px;
        color: white;
    }
    .sidebar .sidebar-content p {
        font-size: 16px;
        line-height: 1.5;
        color: white;
    }
    .sidebar .sidebar-content .widget.stSlider {
        margin-top: 20px;
    }
    </style>
    """,
    unsafe_allow_html=True
)

# Tampilan aplikasi
st.markdown(
    """
    <div class="title-wrapper">
        <h6 style="font-size: 30px; text-align: center; color: #FFD700;
            text-shadow: 2px 2px 4px #000000;">Segmentasi Gambar Menggunakan K-Means Clustering</h6>
    </div>
    """,
    unsafe_allow_html=True
)

# Layout kolom
col1, col2 = st.columns((2, 1))

# Unggah gambar di kolom pertama
with col1:
    uploaded_file = st.file_uploader("Unggah gambar...", type=["jpg", "jpeg", "png"])
    if uploaded_file is not None:
        image = np.array(Image.open(uploaded_file))
        st.image(image, caption='Gambar Asli', use_column_width=True)

# Parameter segmentasi di kolom kedua
with col2:
    st.sidebar.header("Pengaturan")
    k = st.sidebar.slider("Jumlah Cluster (k)", min_value=2, max_value=100, value=3)

    if st.sidebar.button("Segmentasikan Gambar"):
        with st.spinner('Sedang memproses...'):
            segmented_image, centers, segment_percentages = segment_image(image, k)
        st.success('Selesai!')

        st.image(segmented_image, caption='Gambar Tersegmentasi', use_column_width=True)

        st.subheader("Persentase Warna Setiap Segmen:")
        for i, (center, percentage) in enumerate(zip(centers, segment_percentages)):
            color_name = get_color_name(center)
            st.write(f"Segmen {i + 1} ({color_name}): {percentage:.2f}%")

            # Tambahkan plot warna untuk setiap segmen
            color_rgb = np.uint8([[center]])
            color_rgb = cv2.cvtColor(color_rgb, cv2.COLOR_BGR2RGB)
            color_hex = mcolors.rgb2hex(color_rgb.squeeze() / 255.)
            st.write(f'<div style="width: 50px; height: 50px; background-color: {color_hex}; border-radius: 50%; display: inline-block;"></div>', unsafe_allow_html=True)

# Menjalankan aplikasi Streamlit
if __name__ == '__main__':
    st.set_option('deprecation.showPyplotGlobalUse', False)
```

## Hasil Praktikum
ini hasil dari codingan tersebut
### foto percobaan 
![gambar2](https://github.com/namasayafrans/PengolahanCitra/assets/115770839/0df3e1bc-de9b-4f1b-8bb6-15c27c321779 width="150" height="280")  

![gambar1](https://github.com/namasayafrans/PengolahanCitra/assets/115770839/c8de0a02-ef60-4919-8dec-140922b572c0)



## Penutup 
Kembali ke [Daftar Isi](#daftar-isi)
