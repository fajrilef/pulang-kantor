# 🏠 Pulang Kantor

Penasihat perjalanan pulang-pergi kerja untuk wilayah Jabodetabek — membandingkan moda transportasi (mobil, motor, MRT+LRT, Transjakarta, ojol) dengan estimasi waktu nyata, plus CCTV lalu lintas DKI langsung dari app.

**Live:** https://fajrilef.github.io/pulang-kantor/ (v1) · [v2](https://fajrilef.github.io/pulang-kantor/v2/)

## Fitur

### Rute & Moda
- **Pilih lokasi** — preset (FX Sudirman, Cibubur Junction, dll), cari alamat via Nominatim/OpenStreetMap, atau klik peta Leaflet
- **Arah Pulang/Pergi** dengan preset kantor↔rumah otomatis
- **5 moda dibandingkan:** Mobil (tol), Motor (arteri), MRT+LRT+ojol, Transjakarta, Ojol — diurutkan tercepat
- **Verdict langsung:** moda tercepat + alasan + selisih dengan moda kedua
- **Deteksi jalur tol** per rute (OSRM steps) — motor diarahkan arteri
- **Riwayat perjalanan:** catat durasi aktual, tersimpan di perangkat (localStorage)

### Data & Estimasi
- **100% statis (sejak 28-Agu-2026):** tanpa backend, tanpa server — estimasi heuristik jam sibuk + rute/jarak real via OSRM (gratis), deteksi tol
- **Riwayat perjalanan:** tersimpan di localStorage perangkat (catat durasi aktual setelah tiba)
- Backend FastAPI versi lama sudah dihapus — app tidak lagi melakukan koneksi ke server pribadi

### 📹 CCTV Jalan Pulang (baru — Agustus 2026)
- 17 kamera live dari [jakcctv.jakarta.go.id](https://jakcctv.jakarta.go.id/publik) (Dishub DKI × Balitower)
- Koridor: Thamrin, S. Parman, Gatot Subroto (JPO 7, C11, C13, C14), Gerbang Pemuda C02–C04, Tentara Pelajar, Flyover Ladokgi, UOB Plaza
- Player: embed resmi + HLS langsung (hls.js) sebagai cadangan
- Cek kondisi jalan sebelum berangkat — tanpa key, tanpa login

### 🌙 Lainnya
- Dark mode
- Mobile-first, installable (Add to Home Screen)

## Teknologi

| Bagian | Teknologi |
|---|---|
| Frontend | HTML/CSS/JS murni, Leaflet + OpenStreetMap, hls.js, OSRM routing (gratis) |
| Data CCTV | Flussonic Balitower (HLS terbuka, CORS `*`) |
| Hosting | GitHub Pages (cadangan) + Cloudflare Pages (utama) |
| Autocomplete | Nominatim (gratis, no key) |

## Struktur

```
/
├── index.html      # v1 — single file
└── v2/
    └── index.html  # v2 — frontend, 100% statis (tanpa backend)
```

## Menjalankan Lokal

Frontend murni static — buka langsung atau:

```bash
cd v2 && python3 -m http.server 8901
# http://localhost:8901
```

## Deploy

```bash
# GitHub Pages (cadangan)
git push origin main

# Cloudflare Pages (utama)
wrangler pages deploy . --project-name=pk --branch=main
```

## Catatan

- Tidak ada kredensial apa pun di repo — app tak menyimpan/mengirim data ke server mana pun
- Estimasi heuristik per jam + durasi real OSRM; riwayat perjalanan hanya di localStorage
- CCTV: feed resmi publik Pemprov DKI; cakupan koridor pusat Jakarta — Cibubur & sekitarnya belum tersedia di portal publik
