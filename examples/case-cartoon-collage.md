# Studi Kasus Ujung-ke-Ujung — Cartoon Collage (Kartun Kolase)

> **Aturan Anti-Copy:** Gunakan studi kasus ini untuk mempelajari **perencanaan Spatial World Map, akresi 3–5 elemen, urutan waktu `home() → +0.1s → say()`, dan pemisahan `.cut`/`.cut-body`** — jangan menyalin topik langit biru, cutout, atau teks contoh ini ke proyek pengguna.

---

## 1. Input Mentah Pengguna
> *"Buatkan video explainer sains 30 detik format 9:16 untuk Reels tentang kenapa langit terlihat biru di siang hari, gaya kliping kertas kolase yang hangat."*

## 2. Keputusan Langkah Nol (Ringkas)
- **Gaya dipilih:** **Cartoon Collage** *(konsep sains populer, gaya kliping kertas krem, pill kata kunci, dan coretan tangan)*.
- **Rasio:** `9:16` (`1080×1920`, wajib bungkus konten tiap area — bukan `#world` — dalam `.safe { transform: scale(.78); }` agar kertas latar tetap penuh 100%) *(sudah disebut user)*.
- **Durasi:** `30 detik` *(sudah disebut user)*.
- **Naskah VO:** Ditulis lengkap (`vo-script.md`) sesuai pacing untuk sinkronisasi manual di video editor.
- **Referensi yang diambil lewat `reference-index.md`:** `DOC-COLL`, `VIS-COLL-MAP`, `VIS-SHARED-CAM`, `EX-GVB`.

---

## 3. Filled Production Brief (Standar Spesifisitas)

### A. Style Brief & Tipografi
- **Kertas Dunia:** Krem `#F6EFE2` + SVG grain + noda warna lembut oranye `#EA580C` (`0.14`) & teal `#0D9488` (`0.14`).
- **Tipografi:** **Bricolage Grotesque** (judul/kalimat `≥ 62 px`) + **Caveat** (catatan tangan `≥ 50 px`). Semua teks di atas `30 px`.
- **Teks Hantu (`.ghost`):** Konsisten `opacity: 0.20` di tiap area.

### B. Spatial World Map (3 Area · Anti-Bocor + `sizeWorld()`)
Untuk `9:16` (`1080×1920`), jarak aman antar pusat area `dx = 2200 px`, `dy = 2600 px`:
- **Area 1 (Hook — Cahaya Putih):** `(540, 960)`
- **Area 2 (Mekanisme — Hamburan Molekul):** `(2740, 960)`
- **Area 3 (Hasil — Kubah Langit Biru):** `(2740, 3560)`
- **Panggilan wajib:** `sizeWorld(540, 960, 2740, 3560)` → otomatis memperluas `.paper`, `.grain`, dan `.stain` sebesar `±1600 px` ke semua sisi.

### C. Target Pacing VO
- **Target durasi:** `30 detik` → **57 kata** *(dalam rentang ideal 55–65 kata: 3 paragraf × 19 kata, 3 area, masing-masing 4–5 elemen bertahap)*.

---

## 4. Full Beat Sheet (30 Detik · 3 Area · Akresi 4–5 Elemen/Area)

| Waktu | Area Dunia | Paragraf VO (57 kata total) | Akresi 4–5 Elemen Bertahap | Koreografi Kamera (`home` → +0,15s `say`) | Transisi |
|---|---|---|---|---|---|
| `0.0–10.5` | **Area 1 `(540, 960)`** | *"Cahaya matahari tampak putih polos, padahal di dalamnya tersimpan seluruh warna pelangi yang melesat bersama menuju bumi."* (19 kata) | (1) Cutout matahari `fly()`, (2) Cutout prisma spektrum `fly()` saat `look()`, (3) Kartu `.note` miring `-4°` *"7 warna spektrum"*, (4) `say()` kalimat + pill oranye `"putih polos"`, (5) Coretan panah biru `draw()`. | `CU (1.72)` di matahari → `look` ke prisma (`1.52`) → `home(3.4, 540, 960)` selesai `4.5s` → `say()` mulai `4.65s`. | `unsay(9.4)` → `into/settle` ke `(2740, 960)` |
| `10.5–20.5` | **Area 2 `(2740, 960)`** | *"Saat menabrak molekul udara di atmosfer, gelombang cahaya biru yang pendek paling mudah terpencar cepat ke segala arah."* (19 kata) | (1) Cutout molekul nitrogen `fly()`, (2) Grid partikel biru menyebar via `gridPop()`, (3) Stempel `.stamp` `"HAMBURAN RAYLEIGH"` dihentak `slam()`, (4) `say()` kalimat + pill teal `"biru terpencar"`, (5) Lingkaran coretan merah `draw()`. | Mendarat `CU (1.65)` di molekul → `look` ke grid → `home(13.8, 2740, 960)` selesai `14.9s` → `say()` mulai `15.1s`. | `unsay(19.4)` → `into/settle` ke `(2740, 3560)` |
| `20.5–30.0` | **Area 3 `(2740, 3560)`** | *"Hamburan biru dari jutaan sudut itulah yang akhirnya memenuhi pandangan kita setiap kali menengadah ke langit siang hari."* (19 kata) | (1) Cutout bingkai `.polaroid` langit cerah `fly()`, (2) Cutout siluet orang menengadah `fly()`, (3) Label tangan `.hand` *"jutaan sudut!"*, (4) `say()` penutup + pill biru `"langit biru"`. | Mendarat `CU (1.60)` di polaroid → `home(23.6, 2740, 3560)` selesai `24.7s` → `say()` mulai `24.85s`. | Loop bersih ke `0.0s` |

---

## 5. Pemetaan Keputusan Implementasi Kode (`assets/starter-cartoon-collage.html`)

```html
<!-- 1. Pemisahan wajib .cut (posisi & motion blur) vs .cut-body (drop-shadow kertas) -->
<div class="cut" id="cutSun" style="left:360px; top:740px">
  <div class="cut-body">
    <svg viewBox="0 0 220 220" width="220" height="220">...</svg>
  </div>
</div>
```

```js
// 2. Perluas kertas dunia sebelum membangun timeline (mencegah tepi hitam & bocor)
sizeWorld(540, 960, 2740, 3560);

// 3. Urutan keras: home() selesai dulu di 4.50s, baru say() mulai di 4.65s (+0.15s)
home(3.4, 540, 960, 1.1);
say('#t1', 4.65);
draw('#arrow1', 5.4);

// 4. Transisi push-through di kertas yang sama (into mulai di cutAt - 0.9 = 9.6s)
unsay('#t1', 9.4);
into(10.5, 2740, 960, 1.65);
settle(10.5, 2740, 960, 1.65);
```

## 6. Hasil Ringkas Audit QC (Sesuai `templates/qc-audit-scorecard.md`)
- **Lapis 1 (`?debug=1` Audit):** `0` teks `< 30 px`, semua `.cut` memiliki `.cut-body` utuh, `0` elemen di luar `.paper`, `say()` selalu dimulai pada `z = 1.00`, dan overlay **Safe 9:16** bersih dari zona atas `±190 px`, bawah `±230 px`, serta strip UI kanan `±160 px`.
- **Lapis 2 (F-01 s.d. F-07):** 4–5 elemen per area (`F-01` lolos), bayangan kertas utuh pasca-`fly()` & `say()` selalu `≥ +0.15s` pasca-`home()` (`F-05` lolos), nol kebocoran area sebelah (`F-06` lolos), hierarki ukuran & Z-plane kertas konsisten (`F-07` lolos).
