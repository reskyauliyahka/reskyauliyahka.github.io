---

title: Logbook & Tutorial Web Scraping Data Tim NHL dengan Selenium
date: 2025-03-03
author: Resky Auliyah Kartini Askin
image: /assets/gambar/web_scraping.png
categories: [Logbook, Tutorial]
tags: [Python, Web Scraping, Selenium, Data Science]

---

Web scraping adalah teknik otomatisasi untuk mengambil informasi dari halaman web menggunakan skrip pemrograman. Metode ini memungkinkan kita mengekstrak data secara efisien dari situs yang tidak menyediakan API publik.

Pada tutorial ini, kita akan melakukan web scraping terhadap halaman "Hockey Teams: Forms, Searching and Pagination" dari situs Scrape This Site. Halaman tersebut menyajikan data statistik tim hoki NHL sejak tahun 1990 dalam bentuk tabel yang interaktif. Tantangan utama yang dihadapi adalah menangani elemen dropdown, paginasi, dan data yang di-render dinamis, yang memerlukan penggunaan Selenium sebagai browser automation tool.

### Tujuan

Membangun scraper menggunakan Python dan Selenium untuk:

* Mengambil data tim hoki NHL dari tahun 1990.
* Menyimpan data dalam format CSV.
* Menangani dropdown dan pagination otomatis.

---

### Deskripsi Website

* **Situs:** [https://www.scrapethissite.com/pages/forms/](https://www.scrapethissite.com/pages/forms/)
* **Fitur:**

  * Tabel statistik tim NHL.
  * Dropdown per halaman: 25, 50, 100.
  * Pagination (Next/Previous).
  * Data tersembunyi dalam HTML, tidak bisa diambil langsung dengan requests/BeautifulSoup.

---

### Tools dan Teknologi

| Library / Tool | Fungsi                                            |
| -------------- | ------------------------------------------------- |
| `Selenium`     | Mengontrol browser untuk scraping halaman dinamis |
| `Pandas`       | Mengelola dan menyimpan data ke CSV               |
| `time`         | Memberi jeda agar halaman memuat sempurna         |

---

### Tutorial Implementasi

#### Persiapan

Install dependensi:

```bash
pip install selenium pandas
```

Download ChromeDriver sesuai versi Chrome dan tambahkan ke PATH:
🔗 [https://sites.google.com/chromium.org/driver/](https://sites.google.com/chromium.org/driver/)

#### Kode Lengkap Web Scraping (Terstruktur)

##### Import Library

Bagian kode ini dimulai dengan mengimpor beberapa library yang dibutuhkan. selenium dipakai untuk ngontrol browser secara otomatis, supaya kita bisa ambil data dari halaman yang isinya dinamis (nggak bisa pakai requests atau BeautifulSoup biasa). pandas dipakai buat nyimpan data ke format tabel (CSV), dan time dipakai buat kasih jeda supaya halaman sempat loading sebelum data diambil. Selain itu, ada juga Select, WebDriverWait, dan expected_conditions yang dipakai buat nunggu elemen muncul dan ngatur dropdown-nya.

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import Select, WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import pandas as pd
import time
```

##### Inisialisasi WebDriver dan Buka Website

Setelah itu, kita buka Chrome pakai webdriver.Chrome() dan arahkan ke halaman yang mau di-scrape.

```python
driver = webdriver.Chrome()
driver.get("https://www.scrapethissite.com/pages/forms/")
```

##### Set Dropdown ke 100 Baris per Halaman

Supaya datanya langsung tampil 100 per halaman, kita atur dulu dropdown-nya jadi "100". Ini penting biar lebih hemat waktu, jadi nggak perlu klik terlalu banyak halaman.

```python
dropdown = WebDriverWait(driver, 10).until(
    EC.presence_of_element_located((By.ID, "per_page"))
)
Select(dropdown).select_by_visible_text("100")
time.sleep(2)
```

##### Looping untuk Scraping + Pagination

Proses scraping-nya pakai while True, artinya looping terus selama masih ada tombol “Next”. Di dalam loop, kita cari semua baris data di tabel, terus ambil isinya satu per satu (tiap kolom di baris itu), lalu simpan ke list all_data.

Setelah selesai ambil data di satu halaman, kita cek apakah tombol "Next" masih aktif. Kalau masih bisa diklik, kita klik buat lanjut ke halaman selanjutnya. Kalau udah gak aktif (berarti halaman terakhir), kita keluar dari loop. Proses ini dibungkus dengan try-except biar kalau tombol “Next” nggak ketemu, program nggak langsung error tapi berhenti dengan baik.

```python
all_data = []

while True:
    time.sleep(1.5)
    rows = driver.find_elements(By.CSS_SELECTOR, "table.table tbody tr.team")
    for row in rows:
        cols = row.find_elements(By.TAG_NAME, "td")
        data = [col.text.strip() for col in cols]
        all_data.append(data)

    try:
        next_button = driver.find_element(By.XPATH, '//a[text()="Next"]')
        parent_li = next_button.find_element(By.XPATH, "./..")
        if "disabled" in parent_li.get_attribute("class"):
            print("Halaman terakhir tercapai.")
            break
        else:
            next_button.click()
    except:
        print("Gagal menemukan tombol Next. Berhenti.")
        break
```

##### Simpan ke CSV

Kalau semua data udah terkumpul, kita ubah list all_data jadi tabel pakai pandas.DataFrame, terus simpan jadi file CSV dengan nama nhl_all_teams.csv. Terakhir, browser ditutup pakai driver.quit() karena proses scraping-nya udah selesai.

```python
df = pd.DataFrame(all_data, columns=[
    "Team Name", "Year", "Wins", "Losses", "OT Losses",
    "Win %", "Goals For (GF)", "Goals Against (GA)", "Goal Difference"
])
print(f"Total data diambil: {df.shape[0]} baris")
df.to_csv("nhl_all_teams.csv", index=False)

driver.quit()
```

### Hasil

| File Output         | Jumlah Data                            |
| ------------------- | -------------------------------------- |
| `nhl_all_teams.csv` | 30–100+ baris (bergantung tahun & tim) |

**Contoh isi CSV:**

| Team Name     | Year | Wins | Losses | OT Losses | Win % | GF  | GA  | Diff |
| ------------- | ---- | ---- | ------ | --------- | ----- | --- | --- | ---- |
| Boston Bruins | 1990 | 44   | 24     | 0         | 0.55  | 299 | 264 | 35   |

---

### Kendala dan Solusi

| Kendala                             | Solusi                                                       |
| ----------------------------------- | ------------------------------------------------------------ |
| Tombol “Next” tidak selalu tersedia | Gunakan pengecekan class `disabled`                          |
| Data hanya muncul sebagian          | Tambahkan `time.sleep()` agar halaman sempat dimuat          |
| Elemen HTML tidak terdeteksi        | Gunakan `WebDriverWait` agar Selenium menunggu elemen muncul |

---

### Rencana Lanjutan

* Menambahkan fitur filter berdasarkan tahun tim
* Menyimpan data ke database SQL
* Menggunakan headless browser agar scraping lebih cepat tanpa tampilan

---

### Referensi

* [Selenium Docs](https://www.selenium.dev/documentation/)
* [ScrapeThisSite - Forms Page](https://www.scrapethissite.com/pages/forms/)
* [Pandas Documentation](https://pandas.pydata.org/docs/)
