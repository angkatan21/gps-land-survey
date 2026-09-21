# GPS Land Survey

GPS Land Survey is a simple, client-side static web application designed to help users perform basic land surveys by capturing GPS coordinates directly in the browser. It is intended for mapping land areas, calculating perimeters, and estimating land area.

## Features

- Captures GPS coordinates or allows manual pin placement on the map.
- Calculates total survey area and perimeter.
- Calculates individual segment distances between boundary points.
- Allows saving multiple survey points.
- Exports survey data to CSV format.
- Exports survey visual results and reports to high-resolution PNG images.
- Keeps a local history of surveys.
- Supports locking surveys to prevent accidental changes.
- PWA support with offline caching via Service Worker.

## How to Run

This project is a single-file application (`index.html`). To run it locally, simply open `index.html` in any modern web browser or serve it using a local static server.

## How to Use

### 1. Start a New Survey
- Tap the **`＋`** button in the header.
- Enter a name/title for your survey (e.g., `Tanah Wonosari`) and click **Simpan** (Save).

### 2. Choose Map Layer
- Tap the layer button on the map to switch between available map styles:
  - **🌿 Google Hybrid** (Satellite imagery with road and place labels)
  - **🛰️ Google Satelit** (Clean satellite view)
  - **🗺️ Street (OSM)** (Standard OpenStreetMap road map)
  - **🛰️ Esri Satelit** (Esri World Imagery)

### 3. Record Boundary Points
You can record points using field GPS or manual placement on the map:

- **Method A: Real-Time GPS (Recommended for Field Surveys)**
  1. Walk to the first corner or boundary marker of the land.
  2. Tap the **🎯 Locate** button at the bottom-right of the map to enable live GPS tracking.
  3. Monitor the accuracy badge at the top-left of the map:
     - 🟢 **GPS Akurat** ($\le 5\text{ m}$): Optimal accuracy for saving points.
     - 🟡 **GPS Sedang** ($5\text{--}15\text{ m}$): Moderate accuracy.
     - 🔴 **GPS Kasaran** ($> 15\text{ m}$): Waiting for satellite lock; adjust or wait before saving.
  4. The blue circle around the pin shows your current GPS error radius.
  5. Once accuracy is sufficient, tap **📍 Simpan Titik** (Save Point) and optionally give it a label (e.g., `P1 - Pojok Timur`).

- **Method B: Manual Map Pinning**
  1. Tap directly on the map where you want to add a point.
  2. Drag the pin marker to fine-tune its position.
  3. Tap **📍 Simpan Titik** (Save Point) to record it.

- Continue walking sequentially around the property perimeter to capture each corner (Point 1 → Point 2 → Point 3 ...).

### 4. Calculate Area, Perimeter & Segment Distances
- Once at least **3 points** are saved, tap **📐 Hitung Area** (Calculate Area).
- The app will:
  - Draw the closed boundary polygon on the map.
  - Calculate total area in square meters ($m^2$) and hectares ($ha$).
  - Calculate total perimeter in meters ($m$) or kilometers ($km$).
  - Compute individual segment lengths between consecutive points and display them on the map edges and in the summary panel.

### 5. Notes & Survey Lock
- **Notes**: Enter additional field observations in the **Catatan** box (e.g., physical markers, road access, terrain, or border owners).
- **Lock Survey**: Tap **🔒 Lock Survey** to freeze markers and prevent accidental changes during field movement. Tap **🔓 Unlock Survey** to resume editing.

### 6. Export Survey Results
- **📸 Export Gambar** (Image Report): Generates a high-resolution PNG report complete with map view, north arrow, graphic scale bar, survey metrics, point coordinates table, and segment distance table.
- **Export CSV**: Click **Export CSV** in the points header to download coordinate data in `.csv` format.
- **Copy / Google Maps**: Use **Copy Koordinat** to copy the current point coordinates to the clipboard, or tap **Google Maps** to open the coordinates directly in Google Maps.

### 7. History & Offline / PWA Support
- All surveys are saved automatically to the browser's `localStorage`.
- Switch between or manage past surveys in the **History Survey** section at the bottom.
- Works offline in the field and can be installed directly to mobile or desktop home screens as a Progressive Web App (PWA).

## Deployment

This site is automatically deployed to GitHub Pages via the workflow defined in `.github/workflows/static.yml` on every push to the `master` branch.

