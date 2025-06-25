---

title: "Logbook & Tutorial: Web Scraping Data Tim NHL dengan Selenium"
date: 2025-03-03
author: "Resky Auliyah Kartini Askin"
image: /assets/gambar/web_scraping.png
categories: ["Logbook", "Tutorial"]
tags: ["Python", "Web Scraping", "Selenium", "Data Science"]
-------------------------------------------------------------

**Logbook & Tutorial: Web Scraping Data Tim NHL dengan Selenium**
**Tanggal:** 03 Maret 2025
**Disusun oleh:** Resky Auliyah Kartini Askin
**Topik:** Web Scraping — Dropdown, Tabel, dan Pagination
**Website Target:** [Scrape This Site – Forms Page](https://www.scrapethissite.com/pages/forms/)

---

Web scraping adalah teknik otomatisasi untuk mengambil informasi dari halaman web menggunakan skrip pemrograman. Metode ini memungkinkan kita mengekstrak data secara efisien dari situs yang tidak menyediakan API publik.

Pada tutorial ini, kita akan melakukan web scraping terhadap halaman "Hockey Teams: Forms, Searching and Pagination" dari situs Scrape This Site. Halaman tersebut menyajikan data statistik tim hoki NHL sejak tahun 1990 dalam bentuk tabel yang interaktif. Tantangan utama yang dihadapi adalah menangani elemen dropdown, paginasi, dan data yang di-render dinamis, yang memerlukan penggunaan Selenium sebagai browser automation tool.

### 1. Tujuan

Membangun scraper menggunakan Python dan Selenium untuk:

* Mengambil data tim hoki NHL dari tahun 1990.
* Menyimpan data dalam format CSV.
* Menangani dropdown dan pagination otomatis.

---

### 2. Deskripsi Website

* **Situs:** [https://www.scrapethissite.com/pages/forms/](https://www.scrapethissite.com/pages/forms/)
* **Fitur:**

  * Tabel statistik tim NHL.
  * Dropdown per halaman: 25, 50, 100.
  * Pagination (Next/Previous).
  * Data tersembunyi dalam HTML, tidak bisa diambil langsung dengan requests/BeautifulSoup.

---

### 3. Tools dan Teknologi

| Library / Tool | Fungsi                                            |
| -------------- | ------------------------------------------------- |
| `Selenium`     | Mengontrol browser untuk scraping halaman dinamis |
| `Pandas`       | Mengelola dan menyimpan data ke CSV               |
| `time`         | Memberi jeda agar halaman memuat sempurna         |

---

### 4. Tutorial Implementasi

#### a. Persiapan

Install dependensi:

```bash
pip install selenium pandas
```

Download ChromeDriver sesuai versi Chrome dan tambahkan ke PATH:
🔗 [https://sites.google.com/chromium.org/driver/](https://sites.google.com/chromium.org/driver/)

#### b. Kode Lengkap Web Scraping (Terstruktur)

##### 1. Import Library

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import Select, WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import pandas as pd
import time
```

##### 2. Inisialisasi WebDriver dan Buka Website

```python
driver = webdriver.Chrome()
driver.get("https://www.scrapethissite.com/pages/forms/")
```

##### 3. Set Dropdown ke 100 Baris per Halaman

```python
dropdown = WebDriverWait(driver, 10).until(
    EC.presence_of_element_located((By.ID, "per_page"))
)
Select(dropdown).select_by_visible_text("100")
time.sleep(2)
```

##### 4. Looping untuk Scraping + Pagination

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

##### 5. Simpan ke CSV

```python
df = pd.DataFrame(all_data, columns=[
    "Team Name", "Year", "Wins", "Losses", "OT Losses",
    "Win %", "Goals For (GF)", "Goals Against (GA)", "Goal Difference"
])
print(f"Total data diambil: {df.shape[0]} baris")
df.to_csv("nhl_all_teams.csv", index=False)

driver.quit()
```

---

### 5. Penjelasan Kode

| Bagian Kode                                 | Penjelasan                                   |
| ------------------------------------------- | -------------------------------------------- |
| `webdriver.Chrome()`                        | Membuka browser Google Chrome                |
| `Select(...).select_by_visible_text("100")` | Mengatur jumlah data ditampilkan per halaman |
| `while True:`                               | Looping untuk menjelajahi semua halaman      |
| `find_elements(...tr.team)`                 | Mengambil setiap baris data tim              |
| `next_button.click()`                       | Klik halaman berikutnya                      |
| DataFrame → CSV                             | Menyimpan hasil scraping ke file Excel/CSV   |

---

### 6. Hasil

| File Output         | Jumlah Data                            |
| ------------------- | -------------------------------------- |
| `nhl_all_teams.csv` | 30–100+ baris (bergantung tahun & tim) |

**Contoh isi CSV:**

| Team Name     | Year | Wins | Losses | OT Losses | Win % | GF  | GA  | Diff |
| ------------- | ---- | ---- | ------ | --------- | ----- | --- | --- | ---- |
| Boston Bruins | 1990 | 44   | 24     | 0         | 0.55  | 299 | 264 | 35   |

---

### 7. Kendala dan Solusi

| Kendala                             | Solusi                                                       |
| ----------------------------------- | ------------------------------------------------------------ |
| Tombol “Next” tidak selalu tersedia | Gunakan pengecekan class `disabled`                          |
| Data hanya muncul sebagian          | Tambahkan `time.sleep()` agar halaman sempat dimuat          |
| Elemen HTML tidak terdeteksi        | Gunakan `WebDriverWait` agar Selenium menunggu elemen muncul |

---

### 8. Rencana Lanjutan

* Menambahkan fitur filter berdasarkan tahun tim
* Menyimpan data ke database SQL
* Menggunakan headless browser agar scraping lebih cepat tanpa tampilan

---

### 9. Referensi

* [Selenium Docs](https://www.selenium.dev/documentation/)
* [ScrapeThisSite - Forms Page](https://www.scrapethissite.com/pages/forms/)
* [Pandas Documentation](https://pandas.pydata.org/docs/)
