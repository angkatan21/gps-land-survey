# GPS Land Survey

## 1. Tujuan

Web app ringan untuk membantu survey lokasi/tanah menggunakan HP.

Aplikasi memungkinkan user untuk:

- Mengambil koordinat GPS posisi saat ini.
- Memilih titik langsung dari peta.
- Menyimpan beberapa titik survey.
- Melihat latitude, longitude, dan akurasi GPS.
- Membuat polygon dari beberapa titik untuk memperkirakan batas tanah.
- Menghitung luas dan keliling area.
- Membuka lokasi di Google Maps.
- Menyimpan catatan dan foto survey.
- Export hasil survey agar bisa digunakan kembali.

Aplikasi dirancang sebagai **mobile-first PWA**, sehingga dapat dibuka langsung melalui browser HP tanpa perlu install aplikasi native.

---

# 2. Konsep Penggunaan

Contoh workflow ketika survey tanah:

```text
Buka aplikasi
     │
     ▼
Buat Survey Baru
     │
     ▼
Masukkan nama/lokasi survey
     │
     ▼
Aktifkan GPS
     │
     ▼
Berjalan ke titik pertama
     │
     ▼
Simpan Point A
     │
     ▼
Berjalan ke titik kedua
     │
     ▼
Simpan Point B
     │
     ▼
Berjalan ke titik ketiga
     │
     ▼
Simpan Point C
     │
     ▼
Berjalan ke titik berikutnya
     │
     ▼
Selesai
     │
     ▼
Polygon terbentuk
     │
     ├── Luas
     ├── Keliling
     ├── Koordinat
     └── Peta
```

---

# 3. Tampilan Utama

## Map View

Map menjadi layar utama aplikasi.

```text
┌─────────────────────────────────┐
│ GPS LAND SURVEY             ⚙️  │
├─────────────────────────────────┤
│                                 │
│                                 │
│             📍 P3               │
│            /                    │
│           /                     │
│      📍 P2                      │
│       \                         │
│        \                        │
│         📍 P1                   │
│                                 │
│                                 │
│                         🎯      │
├─────────────────────────────────┤
│ GPS Accuracy: ±4 m              │
│ Lat : -7.79558321               │
│ Lng : 110.36949213              │
├─────────────────────────────────┤
│ [ 📍 Current Location ]         │
│ [ ➕ Save Point ]               │
│ [ 📐 Finish Area ]              │
└─────────────────────────────────┘
```

---

# 4. Fitur MVP

## 4.1 Current Location

Menggunakan Browser Geolocation API.

Data yang disimpan:

```text
latitude
longitude
accuracy
altitude
heading
speed
timestamp
```

Contoh:

```json
{
  "latitude": -7.79558321,
  "longitude": 110.36949213,
  "accuracy": 4.2,
  "altitude": 118.4,
  "timestamp": "2026-08-16T07:30:00+07:00"
}
```

### Catatan

Accuracy wajib ditampilkan.

Contoh:

```text
GPS Accuracy: ±4.2 meter
```

Jangan menganggap koordinat GPS sebagai titik yang presisi sampai centimeter. GPS HP biasa bisa memiliki error beberapa meter atau lebih tergantung kondisi.

---

# 5. Save Survey Point

User dapat menyimpan posisi sebagai titik survey.

Contoh:

```text
P1
Latitude  : -7.79558321
Longitude : 110.36949213
Accuracy  : ±4.2 m
Time      : 07:31
```

User dapat memberikan nama:

```text
P1 - Pojok Utara
P2 - Pojok Timur
P3 - Pojok Selatan
P4 - Pojok Barat
```

Setiap point dapat memiliki:

- ID
- nama
- latitude
- longitude
- accuracy
- altitude
- timestamp
- note
- photo

---

# 6. Polygon Area

Setelah minimal 3 titik disimpan, user dapat membuat area.

Contoh:

```text
P1 ───────────── P2
│                 │
│                 │
│                 │
P4 ───────────── P3
```

Aplikasi kemudian menghitung:

```text
Luas
Keliling
Jumlah titik
```

Contoh:

```text
AREA SURVEY

Points   : 4
Area     : 1,248 m²
Perimeter: 145.8 m
```

Untuk perhitungan area, gunakan library geospatial yang sesuai daripada membuat formula sendiri.

---

# 7. Measurement Mode

Tambahkan mode khusus untuk mengukur jarak.

User memilih:

```text
📏 Distance
```

Kemudian:

```text
Tap P1
   ↓
Tap P2
```

Aplikasi menampilkan:

```text
Distance

P1 ───────────── P2

Distance: 42.8 m
```

Bisa dikembangkan menjadi:

```text
P1 → P2 = 42.8 m
P2 → P3 = 31.2 m
P3 → P4 = 40.1 m
```

---

# 8. Reverse Geocoding

Koordinat dapat diterjemahkan menjadi informasi lokasi.

Contoh:

```text
Coordinate
-7.79558321, 110.36949213

Location
Wonosari, Gunungkidul
Daerah Istimewa Yogyakarta
Indonesia
```

Fitur ini bersifat optional untuk MVP karena membutuhkan layanan geocoding.

---

# 9. Google Maps Integration

Setiap point memiliki tombol:

```text
[ Open Google Maps ]
```

URL yang dihasilkan:

```text
https://www.google.com/maps?q=-7.79558321,110.36949213
```

Sehingga hasil survey bisa langsung dikirim ke orang lain.

---

# 10. Photo Documentation

Setiap titik dapat memiliki foto.

Contoh:

```text
P1 - Pojok Utara

📷 IMG_001.jpg
📷 IMG_002.jpg

Note:
Patok tanah terlihat.
Berbatasan dengan jalan kecil.
```

Foto sebaiknya menggunakan kamera HP langsung.

---

# 11. Survey Detail

Setiap survey memiliki metadata:

```text
Survey
├── ID
├── Name
├── Description
├── Created At
├── Updated At
├── Points[]
├── Photos[]
└── Polygon
```

Contoh:

```text
SURVEY TANAH WONOSARI

Tanggal:
16 August 2026

Points:
4

Area:
1,248 m²

Perimeter:
145.8 m

Notes:
Tanah relatif datar.
Akses jalan berada di sisi utara.
```

---

# 12. Offline-First

Ini penting untuk survey lapangan.

Jangan mengandalkan internet untuk fungsi utama.

MVP sebaiknya menggunakan:

```text
Browser
   │
   ├── Geolocation API
   ├── IndexedDB
   └── Local Storage
```

Data survey disimpan lokal terlebih dahulu.

Contoh:

```text
Survey
   │
   ├── P1
   ├── P2
   ├── P3
   └── P4
```

Walaupun koneksi internet hilang, user tetap bisa:

- mengambil GPS
- menyimpan point
- melihat data survey
- menghitung area

Untuk map tiles, caching/offline map bisa ditambahkan kemudian karena implementasinya lebih kompleks.

---

# 13. Export

User dapat melakukan export:

```text
[ Export JSON ]
[ Export CSV ]
```

Contoh CSV:

```csv
point,name,latitude,longitude,accuracy,timestamp
1,P1,-7.79558321,110.36949213,4.2,2026-08-16T07:31:00
2,P2,-7.79542111,110.37011231,5.1,2026-08-16T07:35:00
3,P3,-7.79600121,110.37022142,3.8,2026-08-16T07:40:00
4,P4,-7.79620112,110.36961231,4.5,2026-08-16T07:44:00
```

---

# 14. Teknologi

## Frontend

Rekomendasi:

```text
Next.js
TypeScript
Tailwind CSS
Leaflet
React-Leaflet
```

Alasannya:

- mobile-friendly
- TypeScript
- mudah dibuat sebagai PWA
- Leaflet ringan
- tidak membutuhkan Google Maps API untuk fungsi dasar

## Map

```text
Leaflet
+
OpenStreetMap
```

## Storage

Untuk MVP:

```text
IndexedDB
```

Bisa menggunakan:

```text
Dexie.js
```

Sehingga tidak membutuhkan backend.

---

# 15. Arsitektur MVP

```text
                 ┌─────────────────────┐
                 │      Mobile Web      │
                 │       Browser        │
                 └──────────┬──────────┘
                            │
             ┌──────────────┼──────────────┐
             │              │              │
             ▼              ▼              ▼
       Geolocation       Leaflet        IndexedDB
           API             Map             │
             │              │              │
             └──────────────┼──────────────┘
                            │
                            ▼
                       Survey Data
```

Backend:

```text
NONE
```

Untuk versi pertama, gw **sangat menyarankan tidak membuat backend**.

---

# 16. Versi Berikutnya

Kalau MVP sudah stabil, baru tambahkan backend:

```text
                Mobile PWA
                    │
                    ▼
                REST API
                    │
                    ▼
                PostgreSQL
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
       Surveys             Photos
```

Backend bisa menggunakan:

```text
Go
Echo/Gin
PostgreSQL
S3-compatible Storage
```

Kemudian user bisa login dan survey tersimpan di cloud.

---

# 17. Fitur Advanced yang Sangat Berguna

## GPS Tracking

User menekan:

```text
[ Start Tracking ]
```

Aplikasi merekam perjalanan:

```text
P1 ── P2 ── P3 ── P4 ── P5
```

Kemudian menghasilkan:

```text
Total Distance: 328.4 m
Walking Time  : 12m 43s
```

## Bearing / Direction

Menampilkan arah:

```text
Heading: 127°
Direction: Southeast
```

## Elevation

Jika perangkat/API mendukung:

```text
Altitude: 118.4 m
```

Berguna untuk survey kontur kasar.

## Compare Location

Simpan lokasi dan bandingkan dengan survey sebelumnya.

```text
Survey 2026
Area: 1,248 m²

Survey 2027
Area: 1,261 m²

Difference:
+13 m²
```

---

# 18. Prioritas Development

### Phase 1 — MVP

```text
[ ] Mobile-first UI
[ ] Leaflet map
[ ] Current GPS location
[ ] Show latitude/longitude
[ ] Show GPS accuracy
[ ] Save point
[ ] List points
[ ] Delete point
[ ] Google Maps link
[ ] IndexedDB storage
```

### Phase 2 — Land Survey

```text
[ ] Polygon
[ ] Area calculation
[ ] Perimeter calculation
[ ] Distance measurement
[ ] Point notes
[ ] Photo attachment
[ ] Export CSV
[ ] Export JSON
```

### Phase 3 — Field Tool

```text
[ ] PWA install
[ ] Offline support
[ ] GPS tracking
[ ] Bearing
[ ] Altitude
[ ] Survey history
[ ] Import/export survey
```

### Phase 4 — Cloud

```text
[ ] Authentication
[ ] Backend API
[ ] PostgreSQL
[ ] Cloud sync
[ ] Photo storage
[ ] Multiple devices
[ ] Share survey
```

---

# 19. Rekomendasi UX

Karena aplikasi akan digunakan sambil **jalan-jalan survey**, jangan membuat UI seperti dashboard admin.

Prioritaskan:

```text
BIG BUTTON
BIG TEXT
MINIMAL INPUT
ONE-HAND OPERATION
DARK/LIGHT MAP
```

Contoh bottom navigation:

```text
┌─────────────────────────────────┐
│                                 │
│              MAP                │
│                                 │
│               📍                │
│                                 │
│                                 │
├─────────────────────────────────┤
│ Accuracy: ±4.2m                 │
│                                 │
│       [ 📍 SAVE POINT ]         │
│                                 │
├─────────────────────────────────┤
│ Map       Points      Survey    │
└─────────────────────────────────┘
```

**Fokus utama: user bisa mengambil titik dalam 2–3 tap.**

---

# 20. Important GPS Accuracy Warning

Aplikasi harus selalu menunjukkan accuracy.

Contoh:

```text
🟢 ±3 m     Good
🟡 ±5–15 m  Moderate
🔴 >15 m    Poor
```

Dan jangan menampilkan hasil seolah-olah:

```text
Latitude: -7.795583214512
```

berarti akurasinya sampai milimeter.

Jika GPS accuracy hanya ±8 meter, angka desimal tambahan tidak memberikan presisi nyata.

Untuk kebutuhan **batas legal tanah/sertifikat**, aplikasi ini hanya boleh dianggap sebagai alat bantu survey awal, bukan pengganti surveyor/alat ukur geodesi profesional.

---

# 21. Recommended MVP Stack

```text
Next.js
TypeScript
Tailwind CSS
Leaflet
React-Leaflet
Dexie.js
IndexedDB
OpenStreetMap
PWA
```

Dan **belum perlu**:

```text
❌ Backend
❌ PostgreSQL
❌ Redis
❌ Authentication
❌ Docker
❌ Kubernetes
```

Bangun versi offline/local terlebih dahulu.

Kalau ternyata setelah dipakai memang berguna, baru naikkan menjadi cloud-based land survey platform.

## Target MVP

Tujuan akhirnya sederhana:

> **Buka website → GPS aktif → lihat posisi → simpan titik → tandai semua sudut tanah → otomatis dapat luas + keliling + koordinat → export/share hasil.**

Ini sudah cukup powerful untuk kebutuhan survey tanah pribadi dan jauh lebih praktis daripada membuat mobile native app.
