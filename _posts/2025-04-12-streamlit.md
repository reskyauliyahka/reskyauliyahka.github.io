---
title: Aplikasi Streamlit Sederhana-Konversi Mata Uang IDR
date: 2025-04-12
author: Resky Auliyah Kartini Askin
image: /assets/gambar/streamlit_contoh.png
categories: [Data Mining, Streamlit]
tags: [Python, Streamlit, Aplikasi Sederhana]
---

Streamlit adalah library Python **open-source** yang memungkinkan kita membuat **aplikasi web interaktif** hanya dengan beberapa baris kode Python. Streamlit sangat cocok digunakan oleh **data scientist, peneliti, mahasiswa**, atau siapa pun yang ingin membangun antarmuka visual dari program Python secara cepat tanpa perlu keahlian frontend (HTML, CSS, JavaScript).

## Mengapa Menggunakan Streamlit?

* Sangat mudah dipelajari (cukup dengan Python)
* Cepat membuat prototipe dashboard atau visualisasi
* Tidak perlu mengatur routing, CSS, atau backend secara manual
* Cocok untuk aplikasi analisis data, ML, dan visualisasi interaktif

## Studi Kasus: Aplikasi Konversi Mata Uang

Pada tutorial ini, kita akan membangun **aplikasi konversi mata uang sederhana** dari **Rupiah (IDR)** ke tiga mata uang asing yaitu **USD, EUR, dan JPY** menggunakan Streamlit.

### Tujuan Aplikasi

- Menerima input jumlah dalam Rupiah
- Memberikan pilihan mata uang tujuan
- Menghitung hasil konversi berdasarkan kurs statis
- Menampilkan hasil konversi secara instan dan interaktif

### Tools yang Digunakan

| Library / Tool | Fungsi                              |
| -------------- | ----------------------------------- |
| `Streamlit`    | Membuat antarmuka web interaktif    |
| `Python`       | Bahasa pemrograman utama aplikasi   |

## Kode Lengkap Aplikasi Streamlit

Simpan kode berikut sebagai `currency_converter_app.py`:

```python
import streamlit as st

# Judul Aplikasi
st.title("Konversi Mata Uang: IDR ke USD / EUR / JPY")

# Input jumlah dalam Rupiah
jumlah_idr = st.number_input(
    "Masukkan jumlah dalam Rupiah (IDR)", 
    min_value=0.0, 
    step=1000.0
)

# Pilihan mata uang tujuan
mata_uang = st.selectbox(
    "Pilih mata uang tujuan", 
    ["USD (Dolar Amerika)", "EUR (Euro)", "JPY (Yen Jepang)"]
)

# Kurs statis untuk demo
kurs = {
    "USD (Dolar Amerika)": 0.000065,
    "EUR (Euro)": 0.000060,
    "JPY (Yen Jepang)": 0.0098,
}

# Tombol konversi
if st.button("Konversi"):
    hasil = jumlah_idr * kurs[mata_uang]
    st.success(f"{jumlah_idr:,.0f} IDR = {hasil:,.2f} {mata_uang.split()[0]}")
```

## Cara Menjalankan Aplikasi

1. Pastikan Python sudah terinstal. Jika belum, unduh dan instal dari [python.org](https://www.python.org/downloads/).
2. Install Streamlit menggunakan pip:
   ```bash
   pip install streamlit
   ```
3. Simpan kode aplikasi sebagai file Python:
   ```bash
   currency_converter_app.py
   ```
4. Jalankan aplikasi Streamlit:
   ```bash
   streamlit run currency_converter_app.py
   ```
5. Aplikasi akan terbuka secara otomatis di browser, biasanya di alamat:
   ```
   http://localhost:8501
   ```

## Tampilan Aplikasi

Aplikasi ini memiliki fitur berikut:

- Input jumlah uang dalam Rupiah
- Dropdown untuk memilih mata uang tujuan (USD, EUR, JPY)
- Tombol "Konversi" untuk memproses perhitungan
- Output hasil konversi ditampilkan langsung di halaman

**Contoh hasil:**

> 100.000 IDR = 6.50 USD

## Catatan Tambahan

Nilai kurs yang digunakan bersifat statis dan hanya untuk simulasi.

Untuk mendapatkan data kurs real-time, kamu dapat menggunakan API eksternal seperti:

- [ExchangeRate API](https://www.exchangerate-api.com/)
- [Open Exchange Rates](https://openexchangerates.org/)
- [Fixer.io](https://fixer.io/)

## Rencana Pengembangan

Beberapa ide pengembangan lanjutan dari aplikasi ini:

- Integrasi API kurs live agar data lebih akurat
- Visualisasi grafik konversi historis dengan `matplotlib` atau `plotly`
- Simpan riwayat konversi ke CSV atau database lokal
- Tambah pilihan mata uang seperti SGD, GBP, AUD, dll
- Mode dua arah: konversi bolak-balik (IDR ke USD dan sebaliknya)

## Referensi

- [Streamlit Documentation](https://docs.streamlit.io/)
- [ExchangeRate API](https://www.exchangerate-api.com/)
- [Streamlit Cheat Sheet – GitHub](https://github.com/daniellewisDL/streamlit-cheat-sheet)
- [Python Official Website](https://www.python.org/)
