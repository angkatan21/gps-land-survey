# GPS Land Survey

GPS Land Survey is a simple, client-side static web application designed to help users perform basic land surveys by capturing GPS coordinates directly in the browser. It is intended for mapping land areas, calculating perimeters, and estimating land area.

## Features

- Captures GPS coordinates or allows manual pin placement on the map.
- Calculates total survey area and perimeter.
- Allows saving multiple survey points.
- Exports survey data to CSV format.
- Exports survey visual results to images.
- Keeps a local history of surveys.
- Supports locking surveys to prevent accidental changes.

## How to Run

This project is a single-file application (`index.html`). To run it locally, simply open `index.html` in a web browser.

## Deployment

This site is automatically deployed to GitHub Pages via the workflow defined in `.github/workflows/static.yml` on every push to the `master` branch.

## Technical Documentation

For details on the technology stack, development conventions, and data persistence, please see [GEMINI.md](./GEMINI.md).
