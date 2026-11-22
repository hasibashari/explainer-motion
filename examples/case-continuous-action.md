# Studi Kasus Ujung-ke-Ujung — Continuous Action (Aksi Kontinu)

> **Aturan Anti-Copy:** Gunakan studi kasus ini untuk mempelajari **struktur profil `VK`, kontinuitas 3 Subject Anchors, dan pergantian sudut pandang full-frame** — jangan menyalin topik kurir malam, angka, atau warna contoh ini ke proyek pengguna.

---

## 1. Input Mentah Pengguna
> *"Bikin video explainer 30 detik gaya Continuous Action soal bagaimana paket belanja online bisa sampai antarkota dalam satu malam."*

## 2. Keputusan Langkah Nol (Ringkas)
- **Gaya dipilih:** **Continuous Action** *(proses perjalanan nonstop, kecepatan, rute antarkota, 100% vektor)*.
- **Rasio:** `16:9` *(default)*.
- **Durasi:** `30 detik` *(sudah disebut user)*.
- **Naskah VO:** Ditulis lengkap (`vo-script.md`) sesuai pacing untuk sinkronisasi manual di video editor.
- **Referensi yang diambil lewat `reference-index.md`:** `DOC-CONT`, `VIS-CONT-VIEW`, `VIS-SHARED-CAM`, `EX-GVB` *(fokus pencegahan F-03 dashboard inset)*.

---

## 3. Filled Production Brief (Standar Spesifisitas)

### A. Subject Visual Anchor (Terkunci di Semua View)
1. **Warna Bodi & Aksen:** Bodi van putih `#F8FAFC` + garis pita oranye `--accent: #E85D2A`.
2. **Siluet Khas:** Moncong landai berjendela miring + boks kargo kotak berlogo panah cepat.
3. **Arah Layar Konsisten:** Selalu melaju **kiri → kanan** di `#side` dan mengikuti rute **barat → timur** (kiri → kanan) di `#map`.

### B. Transisi Palet Tempo (`paletteAt(t)`)
- `0–10 dtk` (`#side` — Tol Malam): `--sky: #0F1E36`, `--ink: #F8FAFC`, `--accent: #E85D2A`
- `10–20 dtk` (`#cabin` — Kecepatan Puncak): `--sky: #162238`, `--ink: #F8FAFC`, `--accent: #F59E0B`
- `20–30 dtk` (`#map` — Fajar Tiba): `--sky: #1E3A34`, `--ink: #ECFDF5`, `--accent: #E85D2A`

### C. Target Pacing VO
- **Target durasi:** `30 detik` → **64 kata** (3 paragraf, 3 sudut pandang penuh yang berbeda — nol pengulangan sudut).

---

## 4. Full Beat Sheet (30 Detik · 3 Viewpoints Full-Frame · Nol Inset)

| Waktu | View Full-Frame | Paragraf VO (64 kata total) | Aksi Deterministik (`D(t)` & Instrumen) | Profil `VK` & Kamera | Transisi |
|---|---|---|---|---|---|
| `0.0–10.5` | **`#side` (Samping)** | *"Begitu pesanan diklik malam hari, paket langsung masuk ke armada ekspres yang melaju nonstop menembus jalur tol antarkota."* (20 kata) | 3 lapis kota malam parallax mengalir via `D(t)`, lampu jalan menyapu, roda berputar deterministik, angka raksasa `"95"` + `"KM/JAM"` muncul di `#deepObjs` **di belakang** van lalu tersapu ke kiri. | `VK`: `720 → 980 → 440` (melambat 45% saat `"95"` dibaca) → `960 px/dtk`. `CAMK` zoom `1.0 → 1.08`. | `whip(10.2, view.side, view.cabin, +1)` |
| `10.5–20.0` | **`#cabin` (Kabin)** | *"Di ruang kemudi, kecepatan dijaga stabil sementara sistem memantau jarak tempuh ratusan kilometer tanpa berhenti lama di jalan."* (20 kata) | Full-frame kaca depan dengan garis marka melesat, jarum speedometer menyapu mengikuti `vAt(t)` ke `95`, odometer digital `tabular-nums` bergulir `0 → 420 KM`, indikator rute berkedip lembut. | `VK`: stabil tinggi `960 → 1020 px/dtk`. Banking halus `±3°` mengikuti tikungan. | `whip(19.7, view.cabin, view.map, +1)` |
| `20.0–30.0` | **`#map` (Simulasi Peta)** | *"Dalam delapan jam, rute lintas provinsi tuntas ditempuh dan paket tiba di depan rumah tepat sebelum pagi menyapa."* (24 kata) | Full-frame peta vektor (kiri→kanan), path tol tergambar via `stroke-dasharray`, penanda oranye melaju lewat `getPointAtLength` menghadap tangen, 3 titik hub pop berurutan, kamera zoom `2.3× → 1.0×` selesai di `25.8s`, lalu `"8 JAM"` tampil di `26.0s`. | Zoom out selesai `25.8s` → teks `"8 JAM"` muncul `26.0s` (tidak bertabrakan gerak kamera). | Loop bersih ke `0.0s` |

---

## 5. Pemetaan Keputusan Implementasi Kode (`assets/starter-continuous-action.html`)

```js
// 1. Profil kecepatan deterministik VK — tidak pernah turun ke 0 saat fase baca
const VK = [
  [0, 720], [3.5, 980], [5.2, 440], [7.8, 440], [9.5, 960],
  [18.0, 1020], [20.0, 880], [30.0, 880]
];

// 2. Angka raksasa diletakkan di #deepObjs (parallax 0.80, DI BELAKANG subjek)
const statG = wobj($('#deepObjs'), 0.80, W + 80, 4.4, 1000, 8.8);
T('95', 0, 490, { class: 'big', 'font-size': 400, fill: 'var(--ink)' }, statG);
T('KM/JAM', 580, 486, { 'font-size': 92, 'font-weight': 800, fill: 'var(--accent)' }, statG);

// 3. Pindah sudut pandang full-frame pakai whip() berbasis autoAlpha (nol dashboard inset!)
whip(10.2, view.side, view.cabin, 1);
whip(19.7, view.cabin, view.map, 1);

// 4. Jangan pakai setAttribute('visibility','visible') pada anak view — pakai removeAttribute
// agar elemen anak tidak tembus saat view induk tersembunyi oleh whip().
```

## 6. Hasil Ringkas Audit QC (Sesuai `templates/qc-audit-scorecard.md`)
- **Lapis 1 (`?debug=1` Audit):** `0` teks `< 30 px`, `0` konflik view saat scrub bolak-balik di `10.2s` dan `19.7s`, semua odometer memakai `tabular-nums`.
- **Lapis 2 (F-01 s.d. F-07):** Nol mini-map/gauge inset (`F-03` lolos), arah kiri→kanan terjaga di `#side` dan `#map` (`F-04` lolos), dunia tetap mengalir ≥44% saat angka dibaca (`F-01` lolos), rasio parallax berjenjang (`0.16→1.40`) & proporsi van selaras terhadap tiang/viaduk (`F-07` lolos).
