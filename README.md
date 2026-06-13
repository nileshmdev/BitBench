# BitBench

> **Version 1.0.0**

A single-file, zero-dependency engineering utility — number conversion, bit manipulation, data-size conversion, frequency/time, dB ↔ linear, rise-time ↔ bandwidth, Ohm's law, and IEEE 754 visualization. Open `index.html` directly in any modern browser — no build step, no server required.

### 🚀 [Open Web App →](https://nileshmdev.github.io/BitBench/)

Runs entirely in your browser, installable as a PWA, works offline.

---

## Tabs Overview

| # | Tab | What it does |
|---|-----|--------------|
| 1 | **Number** | Expression evaluator + 64-bit binary grid + bit fields |
| 2 | **Memory** | Data-size converter — bytes ↔ KB/MB/GB/TB (binary & decimal) |
| 3 | **Freq ↔ Time** | Frequency ↔ period two-way conversion with clock presets |
| 4 | **dB** | Power (×10) and Voltage (×20) dB ↔ linear conversion |
| 5 | **Rise / BW** | Rise time ↔ bandwidth using Gaussian and single-pole models |
| 6 | **Ohm** | Solve any 2 of V, I, R, P for the other two |
| 7 | **IEEE 754** | Float32 / Double64 bit-level visualizer with field decoding |
| 8 | **Guide** | How to use each tab, with examples and best use cases |

---

## Features

### 1. Number Converter

- **Auto-detect input** — decimal `255`, hex `0xFF`, binary `0b1010`, octal `0o17` all parsed automatically
- **Real-time evaluation** — result updates 200 ms after you stop typing; press **Enter** to commit + show errors
- **Four output formats** — HEX · DEC · OCT · i64 (signed two's complement), each with a ⎘ copy button
- **64-bit binary grid** — 4 rows × 16 bits, MSB → LSB, grouped by nibble and byte
- **Click any bit to toggle it** — the expression input and all outputs update instantly
- **Pop count badge** — shows how many bits are set
- **Byte breakdown** — `B7` → `B0` strip showing each byte as `0xNN`
- **Endian toggle** — `BE` / `LE` button reverses the byte strip order
- **Named bit fields** — click `Fields` to open the field-map editor; define fields like `31:16 ADDR` and see them colored in the grid plus a decoded field-value table
- **Expression history** — ↑ / ↓ arrow keys cycle through your last 20 expressions (persisted across reloads)
- **Scroll hint** — gradient fade on the right edge when the grid overflows on narrow screens

#### Supported Operators

| Category   | Operators / Functions |
|------------|----------------------|
| Arithmetic | `+`  `-`  `*`  `/`  `%`  `**` |
| Bitwise    | `&`  `\|`  `^`  `~`  `<<`  `>>` |
| Functions  | `lsr(v, n)`  `rol(v, n)`  `ror(v, n)` |
| Literals   | `255` (dec) · `0xFF` (hex) · `0b1010` (bin) · `0o17` (oct) |

- **`lsr(v, n)`** — logical (unsigned) right shift; masks `v` to 64 bits first
- **`rol(v, n)`** — rotate left 64-bit
- **`ror(v, n)`** — rotate right 64-bit
- **`~`** — bitwise NOT, correctly 64-bit masked: `~0` → `0xFFFFFFFFFFFFFFFF`
- All results auto-masked to 64-bit unsigned

#### Expression Examples

```
255
0xFF
0b11001010
0o755
0xFF & 0x0F
(0x10 + 4) << 2
~0xDEAD
5 % 3
2 ** 10
0xAB ^ 0xFF
lsr(~0, 4)
rol(1, 4)
ror(0x8000000000000000, 1)
```

#### Named Bit Field Example

Click **Fields**, then in the textarea enter:
```
31:16 ADDR
15:8  FLAGS
7:4   MODE
3:0   STATUS
```
The corresponding bits in the grid are colored per field, and a decoded table shows each field's hex and decimal value.

---

### 2. Memory / Data Size

- **Auto-detect input** — enter a value in **decimal** (`4096`, `1.5`) or **hex** (`0x100000`), interpreted in the unit you pick
- **Mode toggle** — Binary (1024) for `KiB`/`MiB`/`GiB`/`TiB`/`PiB`, or Decimal (1000) for `KB`/`MB`/`GB`/`TB`/`PB`
- **Exact byte count** — output line shows the total in comma-grouped decimal **and** hex
- **All units at once** — grid shows the value across every unit in the current mode; **click any cell to copy**
- Switching modes keeps the same magnitude position (e.g. `GiB` ↔ `GB`)

**Binary units:** `B` `KiB` `MiB` `GiB` `TiB` `PiB`
**Decimal units:** `B` `KB` `MB` `GB` `TB` `PB`

---

### 3. Freq ↔ Time Converter

- Two-way live conversion between frequency and period
- Both sides show the value in **all units simultaneously**
- **Clock presets** — quick buttons for common values: `24M` `33M` `48M` `100M` `125M` `200M` `1G` `2.4G` `5G`
- **Click any unit cell to copy** its value (green flash confirms copy)
- **Derived values row** — shows `½ period` and `¼ period` for the current frequency

**Frequency units:** `THz` `GHz` `MHz` `kHz` `Hz` `mHz`
**Period units:** `s` `ms` `µs` `ns` `ps` `fs`

---

### 4. dB ↔ Linear

- **Mode toggle** — Power (×10·log₁₀) or Voltage (×20·log₁₀)
- Two-way live conversion: type dB → linear updates, or vice versa
- Linear value shown in all SI units
- **Common references table** at the bottom — click a row to populate the inputs

| Mode | Reference | Common values |
|------|-----------|---------------|
| Power | 1 mW (dBm) | `-30dBm = 1µW` … `30dBm = 1W` |
| Voltage | 1 V (dBV) | `-20dBV = 100mV` … `20dBV = 10V` |

---

### 5. Rise Time ↔ Bandwidth

- Two-way conversion using two models, shown side-by-side:
  - **Gaussian** system: `BW = 0.35 / t_r` (standard oscilloscope spec)
  - **Single-pole RC**: `BW = 0.22 / t_r` (conservative)
- Auto-SI formatting for both rise time and bandwidth

---

### 6. Ohm's Law

- Enter any **2 of V, I, R, P** — the other two are computed
- All 6 combinations supported
- Each field has its own SI unit selector (e.g. kV/V/mV/µV, A/mA/µA/nA, MΩ/kΩ/Ω/mΩ, kW/W/mW/µW/nW)
- Result cells show **known values in blue** and **computed values in green**
- If you fill more than 2 fields, the 2 most recently edited are used as inputs

---

### 7. IEEE 754 Visualizer

- **Mode toggle** — Float32 (32-bit) or Double64 (64-bit)
- Accepts decimal floats (`3.14`, `-1.5e-5`, `NaN`, `Infinity`) **or** hex (`0x3F800000`, `0x3FF0000000000000`)
- **Color-coded bit grid**:
  - 🟥 **Sign** (1 bit)
  - 🟧 **Exponent** (8 bits f32 / 11 bits f64)
  - 🟦 **Mantissa** (23 bits f32 / 52 bits f64)
- **Decoded info table** shows:
  - Sign, raw + biased exponent, actual exponent (2ⁿ)
  - Mantissa hex value, implied leading bit (1 for normal, 0 for denormal)
  - Category badge — Normal · Denormal · Zero · Infinity · NaN
  - Final value and its hex representation

---

### 8. Guide

- **In-app help** — one card per tool with a plain-language description
- **"Best for"** line on each card summarizing its ideal use case
- **Worked examples** for every tab (e.g. `(0x10 + 4) << 2`, `0 dBm = 1 mW`, `3.3 V + 10 mA → 330 Ω`)
- **Operator reference** for the Number tab (arithmetic, bitwise, literals, functions)
- **Open ›** button on each card jumps straight to that tab

---

## How to Use

### Open the app
```
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```
Or just drag `index.html` into any browser tab.

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `1` | Number tab |
| `2` | Memory tab |
| `3` | Freq ↔ Time tab |
| `4` | dB tab |
| `5` | Rise / BW tab |
| `6` | Ohm tab |
| `7` | IEEE 754 tab |
| `8` | Guide tab |
| `Enter` | Evaluate expression (Number tab) |
| `↑` / `↓` | Cycle through expression history (Number tab) |

> Tab shortcuts (`1`–`8`) are active only when focus is not inside an input field.

### Light / Dark Theme
Click **☾ / ☀** in the header to toggle. Saved across sessions.

### Persistence
The following are saved in `localStorage` and restored on reload:
- Active tab
- Theme (light/dark)
- Last 20 expressions entered in the Number tab

---

## Install as an App (PWA)

BitBench is a Progressive Web App — install it on any device and it opens in its own window with offline support.

### Desktop (Chrome / Edge / Brave)
1. Open the site in a browser tab
2. Click the **install icon** (⊕ or computer-with-arrow) in the address bar — or use the browser menu → "Install BitBench"
3. The app opens in a standalone window and gets a launcher entry

### Android (Chrome / Edge)
1. Open the site
2. Tap the menu (⋮) → **Add to Home screen** / **Install app**
3. Confirm — the app appears on the home screen with its own icon

### iOS (Safari)
1. Open the site in Safari
2. Tap the Share button → **Add to Home Screen**
3. The app icon is added to the home screen and launches in standalone mode

### Offline Support
After the first visit, a service worker caches the app shell, so it works without a network connection.

---

## Deploy to GitHub Pages

This repo includes a GitHub Actions workflow that automatically publishes the site whenever you push to `main`.

### One-time setup
1. Push this repo to GitHub:
   ```bash
   git remote add origin https://github.com/<you>/<repo>.git
   git push -u origin main
   ```
2. **Enable Pages manually** (required — the workflow can't do this for you):
   - Go to the repo on GitHub → **Settings → Pages**
   - Under **Build and deployment → Source**, choose **GitHub Actions**
   - Save (no workflow selection needed — yours is already detected)
3. Re-run the failed deploy:
   - Go to **Actions → Deploy to GitHub Pages → Re-run failed jobs**
   - Or push any new commit to `main`
4. Once green, the site URL appears in the workflow run summary (also visible at **Settings → Pages**).

> **Why this manual step?** GitHub's default workflow token can't create a Pages site from scratch — only an authenticated repo admin can flip it on the first time. After that, the workflow handles every subsequent deploy on its own.

### Workflow
See [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml). It uses the official GitHub Pages actions:
- `actions/checkout@v4`
- `actions/configure-pages@v5`
- `actions/upload-pages-artifact@v3`
- `actions/deploy-pages@v4`

The deployed URL will be `https://<you>.github.io/<repo>/` (printed in the Actions log).

### Local development
Service workers don't work over `file://`, so to test PWA install / offline behavior locally use a tiny static server:
```bash
python3 -m http.server 8000
# then open http://localhost:8000/
```
Plain editing without PWA testing works fine by just opening `index.html` in a browser.

---

## Files

| File | Purpose |
|------|---------|
| `index.html` | The entire app (HTML + CSS + JS) |
| `manifest.webmanifest` | PWA manifest — name, icons, theme color, display mode |
| `sw.js` | Service worker — caches the app shell for offline use |
| `icon.svg` | App icon (scalable, used by PWA install and browser tab) |
| `.github/workflows/deploy.yml` | GitHub Actions workflow that deploys to Pages |

---

## Browser Requirements

BigInt support required:
- Chrome 67+
- Firefox 68+
- Safari 14+
- Edge 79+

PWA install support varies — Chrome/Edge/Brave on desktop and Android offer the smoothest experience. Safari supports Add-to-Home-Screen on iOS.

**Responsive** — the layout adapts to phones: the header reflows to a full-width tab bar, converter grids collapse to two columns, and the 64-bit grid scales to fit common screen widths.

---

<p align="center">Designed by <strong>Nilesh M</strong> · © 2026 · All rights reserved · v1.0.0</p>
