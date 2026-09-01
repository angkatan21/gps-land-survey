# Analisis Komparasi & Laporan Uji Capan Lapangan: Codebase vs PRD.md

Dokumen ini berisi analisis perbandingan antara implementasi kode (`index.html` dan file terkait) dengan spesifikasi yang tertulis di [`docs/PRD.md`](./PRD.md), serta catatan perbaikan berdasarkan hasil pengujian lapangan.

---

## 📊 Ringkasan Eksekutif

Aplikasi diimplementasikan sebagai **Single-File Web App (`index.html`)** berbasis Vanilla JavaScript, HTML5, CSS3, dan Leaflet.js. Sebagian besar fitur dasar **Phase 1 (MVP)** dan sebagian fitur **Phase 2 (Land Survey)** sudah berfungsi dengan sangat baik.

Berdasarkan **ujicoba lapangan**, terdapat 4 poin feedback utama yang telah dianalisis dan diperbaiki dalam codebase:
1. **Akurasi GPS:** Penggunaan `watchPosition` continuous live tracking, indikator visual akurasi warna (🟢/🟡/🔴), lingkaran radius error pada peta, dan peringatan jika akurasi masih kasaran.
2. **Keseimbangan Layer Peta:** Menambahkan pilihan tile layer Google Hybrid & Google Satellite (disamping OpenStreetMap & Esri) agar sesuai dengan citra satelit yang familiar di Indonesia.
3. **Informasi Jarak Antar Titik:** Perhitungan jarak segmen per sisi ($P_1 \rightarrow P_2$, $P_2 \rightarrow P_3$, dst.) yang ditampilkan langsung pada peta, panel UI, dan laporan PNG.
4. **Bug Export Bidang Ganda (Double Polygon):** Memperbaiki offset SVG Leaflet saat captured dengan `html2canvas` dengan menyembunyikan `.leaflet-overlay-pane` selama proses snapshot sehingga laporan PNG hanya merender 1 polygon yang bersih.

---

## 🏗️ 1. Perbandingan Arsitektur & Tech Stack

| Komponen / Kriteria | Rekomendasi PRD | Implementasi Codebase Saat Ini | Status |
| :--- | :--- | :--- | :--- |
| **Framework** | Next.js, React, TypeScript | Single-file Vanilla JS (`index.html`) | ⚠️ Berbeda (Disederhanakan) |
| **Styling** | Tailwind CSS | Custom CSS embedded inline (`<style>`) | ⚠️ Berbeda (Custom CSS) |
| **Peta (Map Engine)** | Leaflet + OpenStreetMap | Leaflet v1.9.4 + Google Sat/Hybrid, OSM, & Esri | ✅ Sesuai (+ Multi-Tile Options) |
| **Geospatial Engine** | Library Geospatial (Turf.js) | `@turf/turf` v7 (WGS84 Geodesic) | ✅ Sesuai |
| **Penyimpanan Lokal** | IndexedDB (Dexie.js) | `localStorage` (`gps-land-surveys-v3`) | ⚠️ Berbeda (`localStorage`) |
| **Arsitektur App** | Mobile-First PWA | Web App biasa (Belum ada Service Worker & Manifest) | ❌ Belum Ada PWA |
| **Backend** | Tanpa Backend (Offline-first) | Tanpa Backend | ✅ Sesuai |

---

## 🎯 2. Perbandingan Fitur & Tindak Lanjut Feedback Lapangan

### 2.1 Current Location & GPS Accuracy (Feedback Lapangan #1)
- **Masalah Lapangan:** Koordinat GPS sering memberikan pembacaan kasar ($\pm 40\text{m}$) saat tombol dipencet sekali karena browser mengembalikan lokasi Wi-Fi/Cell Tower pertama.
- **Solusi Codebase:**
  - Mengimplementasikan `navigator.geolocation.watchPosition` secara realtime continuous tracking.
  - Menampilkan **Lingkaran Radius Akurasi GPS (`L.circle`)** di atas peta.
  - Menambahkan indikator status warna akurasi (🟢 $\le 5\text{m}$ Akurat, 🟡 $5-15\text{m}$ Sedang, 🔴 $>15\text{m}$ Kasaran).
  - Memberikan toast peringatan jika pengguna mencoba menyimpan titik dengan akurasi GPS $>15\text{m}$.

### 2.2 Layer Peta Satelit vs Street (Feedback Lapangan #2)
- **Masalah Lapangan:** Esri World Imagery & OpenStreetMap memiliki perbedaan pergeseran (*geodetic/orthorectification offset*) pada peta visual.
- **Solusi Codebase:**
  - Menambahkan opsi layer **Google Maps Hybrid** (`lyrs=y`) dan **Google Maps Satellite** (`lyrs=s`).
  - Menyediakan tombol pemilih layer interaktif untuk berpindah antara `Google Hybrid`, `Google Satelit`, `Street (OSM)`, dan `Esri Satelit`.

### 2.3 Informasi Jarak Antar Titik / Per Segmen (Feedback Lapangan #3)
- **Masalah Lapangan:** Pengguna membutuhkan informasi panjang tiap sisi batas tanah ($P_1 \rightarrow P_2$, $P_2 \rightarrow P_3$, dll.), bukan hanya total keliling.
- **Solusi Codebase:**
  - Perhitungan otomatis jarak geodesik Turf.js untuk setiap segmen titik berturutan ($P_i \rightarrow P_{i+1}$) dan sisi penutup ($P_n \rightarrow P_1$).
  - Label jarak ditampilkan langsung di tengah garis segmen peta (`edge-label-icon`).
  - Panel UI menyajikan daftar **Jarak Per Sisi (Segmen)**.
  - Gambar laporan PNG yang di-export mencantumkan tabel khusus **JARAK PER SISI (SEGMEN)**.

### 2.4 Bug Double Polygon Pada Export Gambar (Feedback Lapangan #4)
- **Masalah Lapangan:** Pada hasil export gambar PNG, terdapat 2 bidang polygon yang ter-capture (satu polygon hitam tergeser dan satu polygon biru).
- **Penyebab:** `html2canvas` menangkap SVG path Leaflet (`.leaflet-overlay-pane`) dengan offset CSS transform 3D, sekaligus menggambar canvas overlay manual di atasnya.
- **Solusi Codebase:**
  - Mengatur `preferCanvas: true` pada inisialisasi Leaflet.
  - Menyembunyikan `.leaflet-overlay-pane` secara sementara saat fungsi `exportImage()` berjalan.
  - Menghasilkan 1 gambar polygon yang presisi dan bersih pada laporan PNG.

---

## 📈 3. Status Progress Berdasarkan Phase (PRD §18)

```text
[██████████████████░░] 90% — Phase 1: MVP
[████████████████░░░░] 80% — Phase 2: Land Survey
[███░░░░░░░░░░░░░░░░░] 15% — Phase 3: Field Tool
[░░░░░░░░░░░░░░░░░░░░]  0% — Phase 4: Cloud
```

### Checklist Phase 1 — MVP
- [x] Mobile-first UI
- [x] Leaflet map (Multi-layer: Google Hybrid/Sat, OSM, Esri)
- [x] Current GPS location (Realtime watchPosition)
- [x] Show latitude/longitude
- [x] Show GPS accuracy (dengan lingkaran radius & warna status 🟢/🟡/🔴)
- [x] Save point
- [x] List points
- [x] Delete point
- [x] Google Maps link
- [ ] IndexedDB storage *(menggunakan localStorage)*

### Checklist Phase 2 — Land Survey
- [x] Polygon visualization
- [x] Area calculation (Turf.js WGS84 Geodesic)
- [x] Perimeter calculation
- [x] Distance measurement (Rincian jarak per segmen sisi)
- [ ] Point notes *(baru ada survey-level note)*
- [ ] Photo attachment
- [x] Export CSV
- [ ] Export JSON
- [x] Export Gambar / Laporan PNG profesional (Sudah diperbaiki dari double polygon)

### Checklist Phase 3 — Field Tool
- [x] PWA install (manifest.json & service worker)
- [x] Offline caching support (caching CDN/static assets)
- [ ] GPS tracking (rekam jejak lintasan)
- [ ] Bearing / Heading
- [x] Altitude display
- [x] Survey history
- [ ] Import/Export survey JSON

---

## 💡 4. Rekomendasi Langkah Selanjutnya

1. **Migrasi Storage ke IndexedDB:**
   - Memindahkan penyimpanan ke IndexedDB (Dexie.js) sebelum menambahkan fitur foto agar tidak melampaui batas 5MB `localStorage`.
2. **Foto & Catatan Per Titik:**
   - Menambahkan bidang input catatan dan tombol amatan kamera per titik survey.
