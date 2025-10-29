---
title: "Test Scenario, Test Case, dan Bug Report"
date: 2025-10-27
categories: [Artikel, Software Testing]
tags: [test-scenario, test-case, bug-report, software-testing, quality-assurance]
author: "Resky Auliyah Kartini Askin"
image: /assets/gambar/bug_report.png
---

# Tiga Pilar Kualitas: Membedah Test Scenario, Test Case, dan Bug Report

Dalam dunia pengembangan perangkat lunak, memastikan produk yang dirilis bekerja sempurna adalah tantangan utama. Proses ini dikelola oleh tiga dokumen esensial yang saling melengkapi: **Test Scenario**, **Test Case**, dan **Bug Report**. Ketiganya berfungsi sebagai panduan, catatan eksekusi, dan laporan cacat, yang bersama-sama menjamin aplikasi berjalan sesuai harapan dan bebas dari kesalahan.

## 1. Test Scenario: Peta Jalan Pengujian

**Test Scenario** adalah gambaran umum (*high-level*) tentang apa yang harus diuji untuk memastikan fungsi aplikasi sesuai dengan kebutuhan bisnis dan pengguna. Ini adalah tahap perencanaan strategis yang menjawab pertanyaan **"Apa yang harus diuji?"**

| Konsep | Penjelasan |
| :--- | :--- |
| **Test Scenario** | Gambaran umum tentang apa yang diuji untuk memastikan fungsi aplikasi sesuai kebutuhan. |

### Template Sederhana Test Scenario

| Field | Keterangan |
| :--- | :--- |
| **ID Scenario** | Nomor unik skenario |
| **Deskripsi** | Ringkasan skenario pengujian |
| **Modul/Fitur** | Modul atau fitur yang diuji |

---

## 2. Test Case: Resep Detil untuk Eksekusi

Jika Test Scenario adalah peta, maka **Test Case** adalah langkah-langkah detail dan terperinci yang harus diikuti penguji. Test Case menjawab pertanyaan **"Bagaimana melakukan pengujian?"** dan mencakup input, proses, dan hasil yang diharapkan.

| Konsep | Penjelasan |
| :--- | :--- |
| **Test Case** | Langkah detail pengujian yang mencakup input, proses, dan hasil yang diharapkan. |

### Template Sederhana Test Case

| Field | Keterangan |
| :--- | :--- |
| **ID Test Case** | Nomor unik test case |
| **Deskripsi** | Ringkasan singkat tentang pengujian |
| **Precondition** | Kondisi awal sebelum pengujian |
| **Test Steps** | Langkah-langkah detail pengujian |
| **Test Data** | Data yang digunakan untuk pengujian |
| **Expected Result** | Hasil yang diharapkan sesuai *requirement* |
| **Actual Result** | Hasil aktual yang muncul setelah pengujian |
| **Status** | Lulus/Gagal (Pass/Fail) |

### Contoh Pengujian Fungsionalitas Aplikasi BMI

Berikut adalah contoh bagaimana Test Scenario dipecah menjadi Test Case untuk aplikasi penghitung *Body Mass Index* (BMI):

#### Scenario TS002 — Perhitungan dan Klasifikasi BMI

| ID Test Case | Deskripsi | Precondition | Test Data | Expected Result |
| :---: | :--- | :--- | :--- | :--- |
| **TC003** | Verifikasi hasil perhitungan BMI sesuai rumus ($kg/m^2$) | Aplikasi terbuka | Tinggi: 170 cm, Berat: 65 kg | Hasil BMI = **22.49** |
| **TC004** | Verifikasi kategori *Underweight* | Aplikasi terbuka | Tinggi: 170 cm, Berat: 45 kg | Hasil BMI = 15.6 $\rightarrow$ **“Underweight”** |
| **TC005** | Verifikasi kategori *Normal* | Aplikasi terbuka | Tinggi: 165 cm, Berat: 60 kg | Hasil BMI $\approx$ 22.0 $\rightarrow$ **“Normal”** |

---

## 3. Bug Report: Laporan Resmi Cacat Sistem

Ketika hasil aktual tidak sesuai dengan hasil yang diharapkan (*Expected Result*), maka *bug* ditemukan dan harus didokumentasikan dalam **Bug Report**. Laporan ini bersifat formal, berisi detail masalah, langkah untuk mereproduksi, dan membantu pengembang memperbaiki cacat secara efisien.

| Konsep | Penjelasan |
| :--- | :--- |
| **Bug Report** | Laporan kesalahan pada aplikasi yang berisi detail masalah, langkah reproduksi, dan hasil aktual. |

### Klasifikasi Severity (Dampak) dan Priority (Prioritas)

Setiap *bug* diklasifikasikan berdasarkan dua metrik utama untuk menentukan urgensi perbaikannya:

#### Severity (Tingkat Dampak pada Fungsi Sistem)

| Severity | Deskripsi |
| :--- | :--- |
| **Critical** | Sistem gagal total atau data rusak. |
| **Major (High)** | Fitur utama tidak bekerja, namun sistem tidak *crash*. |
| **Minor (Medium)** | Bug kecil yang menimbulkan ketidaknyamanan pengguna. |
| **Low** | Bug tidak memengaruhi sistem secara signifikan. |

#### Priority (Tingkat Urgensi Perbaikan)\

| Priority | Deskripsi |
| :--- | :--- |
| **P1 - Urgent/Critical** | Harus segera diperbaiki karena sangat berdampak. |
| **P2 - High** | Penting dan memengaruhi banyak pengguna. |
| **P3 - Medium** | Dapat diperbaiki di rilis berikutnya. |
| **P4 - Low** | Bug kosmetik atau minor. |

### Contoh Bug Report

| Field | Keterangan |
| :--- | :--- |
| **Bug ID** | BMI-001 |
| **Bug Title** | Perhitungan BMI salah saat input berat 60kg dan tinggi 170cm |
| **Step to Reproduce** | 1. Buka aplikasi BMI. 2. Masukkan Berat = 60. 3. Masukkan Tinggi = 170. 4. Klik “Hitung”. |
| **Actual Result** | Hasil BMI = 12.5 |
| **Expected Result** | Hasil BMI seharusnya = 20.8 |
| **Severity** | Major (High) |
| **Priority** | P2 - High |
| **Reported On** | 31-08-2025 |

---

## Strategi Proaktif: Cara Menghindari Bug

Pencegahan selalu lebih baik daripada perbaikan. Tim dapat mengurangi jumlah *bug* yang masuk ke tahap pengujian dengan:

1.  **Pahami Persyaratan** — Pastikan semua kebutuhan sistem dipahami dengan jelas oleh seluruh tim.
2.  **Unit Testing** — Lakukan pengujian unit untuk mendeteksi *bug* sejak awal.
3.  **Code Review** — Minta pengembang lain meninjau kode untuk menemukan kesalahan logika.
4.  **Rencana Pengujian yang Baik** — Buat *Test Plan* yang komprehensif.
5.  **Pengujian Otomatis** — Gunakan *automation testing* untuk mempercepat deteksi *bug* regresi.
6.  **Kolaborasi Tim** — Jaga komunikasi antara pengembang dan penguji agar sinkron.

## Kesimpulan

Melalui penerapan Test Scenario yang terstruktur, Test Case yang terperinci, dan Bug Report yang formal, proses *software testing* dapat berjalan lebih efisien. Dokumentasi yang baik tidak hanya membantu menemukan cacat, tetapi juga mencegah *bug* yang sama muncul kembali, secara langsung meningkatkan kualitas perangkat lunak secara keseluruhan.