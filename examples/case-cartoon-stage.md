# Studi Kasus Ujung-ke-Ujung — Cartoon Stage (Kartun Panggung)

> **Aturan Anti-Copy:** Gunakan dokumen ini untuk mempelajari **tingkat presisi perencanaan, pemetaan beat, dan implementasi kode** — jangan menyalin topik panel surya, nama tokoh, atau palet warnanya ke proyek pengguna.

---

## 1. Input Mentah Pengguna
> *"Buatkan video explainer 30 detik format 16:9 tentang cara kerja panel surya rumahan yang sederhana dan mudah dipahami, tanpa teks subtitle di bawah layar."*

## 2. Keputusan Langkah Nol (Ringkas)
- **Gaya dipilih:** **Cartoon Stage** *(ada tokoh pemilik rumah, cerita sebab-akibat visual, dan user meminta tanpa caption)*.
- **Rasio:** `16:9` (`1920×1080`) *(sudah disebut user)*.
- **Durasi:** `30 detik` *(sudah disebut user)*.
- **Naskah VO:** Ditulis lengkap (`vo-script.md`) sesuai pacing untuk sinkronisasi manual di video editor.
- **Referensi yang diambil lewat `reference-index.md`:** `DOC-STAGE`, `VIS-STAGE-ANAT`, `VIS-STAGE-POSE`, `VIS-SHARED-CAM`.

---

## 3. Filled Production Brief (Standar Spesifisitas)

### A. Style Brief
- **Gaya:** Cartoon Stage (palet flat bersih tanpa tekstur kertas/grain, nol caption).
- **Color Script 70–20–10 Terkunci:**
  - *70% Latar Analogus Tenang:* Langit `#C4E2EB → #EFF6F1` · Bukit `#CFE3DF` / `#A9CBC6` · Tanah `#9AC2AD`
  - *20% Arsitektur & Sisi 2,5D:* Dinding `#F5F0E6` / sisi gelap `#D4C9B6` · Atap `#D96B4B` · Panel & Gardu `#223B4E`
  - *10% Subjek & Titik Aksi Kontras:* Jaket Tokoh `#2563EB` · Aliran Energi Emas `#F59E0B`
- **Light Direction Anchor & Resep Ruang:** Cahaya konsisten `kiri-atas → kanan-bawah` (seluruh sisi gelap 2,5D & *light beam* diagonal searah); P1 *Eksterior S-Curve* + pohon *2-tone*, P2 *Penampang Inverter*, P3 *Interior Window-Depth + Edge Anchors*.
- **Teks Diegetik Saja (≥ 34 px):** Plat `"INVERTER"` (`38 px`), meteran `"kWh"` (`36 px`).

### B. Target Pacing VO (Rumus 2,2–2,5 kata/dtk)
- **Target durasi:** `30 detik` → **62 kata** (3 paragraf, jeda antar-paragraf ~0,9 dtk).

### C. Character & Asset Bible (Lock Konsistensi)
- **Tokoh Utama (`chA` — Raka):** Tinggi vektor `~345 px` (`scale: 0.60` = `1 H`, kaki di `y = 0`, sesuai acuan skala semi-realistis vs pintu `1,24 H`), sudut `pose: '3/4'`, rambut `#182436`, kemeja kobalt `#2563EB` berkerah + manset gulung, celana `#1E3A5F`, kulit `#E5A77B`, aksen gesper `#F59E0B`, bayangan kaki `ch.sdJ` aktif.
- **Arah Layar (180-Degree Rule):** Di P1 tokoh di kiri (`x = 660`, `facing = 1`) menghadap rumah/panel di kanan; di P3 tokoh tetap di kiri (`x = 700`, `facing = 1`) menekan sakelar di kanan.

---

## 4. Full Beat Sheet (30 Detik · 3 Panggung · ≥10 Gerak/Panggung)

| Waktu | Panggung | Paragraf VO (62 kata total) | Aksi Visual & Daftar ≥10 Benda Bergerak | Shot & Zoom Logaritmik | Transisi |
|---|---|---|---|---|---|
| `0.0–10.0` | **P1 — Atap & Halaman** | *"Setiap pagi, sinar matahari jatuh gratis ke atap rumah kita, membawa energi yang jauh lebih besar dari kebutuhan satu keluarga."* (21 kata) | (1) Matahari terbit + halo berdenyut, (2) 2 awan melintas, (3) 2 burung terbang, (4) Sinar menyapu atap, (5) Kilau panel surya, (6) Asap cerobong `drift()`, (7) Daun pohon `sway()`, (8) Jemuran bergoyang, (9) Raka `idle()` + kedip, (10) Raka `point()` ke atap dengan anticipation `0.14s`, (11) Bayangan `ch.sdJ` sinkron. | Buka `MCU (1.58)` di Raka → tahan `0.6s` → tarik ke `MS (1.14)` memperlihatkan atap & matahari (`4.2–6.4s`). | `slide('x', 96)` (`0.28s` + `0.42s`, blur `10px`) |
| `10.0–20.5` | **P2 — Penampang Inverter** | *"Di dalam panel, cahaya mendorong elektron mengalir menjadi arus listrik, lalu kotak inverter mengubahnya agar aman bagi seluruh peralatan rumah."* (21 kata) | (1) Penampang dinding terangkat, (2–9) 8 titik elektron emas mengalir lewat `flowAlong()` di sepanjang pipa atap → `"INVERTER"` → kabel rumah, (10) 2 kipas pendingin `spin()`, (11) Jarum voltase naik, (12) 4 LED status berkedip deterministik. | `WS (1.12)` seluruh jalur pipa → `CU (1.64)` pada kotak `"INVERTER"` saat jarum naik (`14.5–16.2s`). | `cut()` langsung di `20.5s` |
| `20.5–30.0` | **P3 — Ruang Keluarga** | *"Hasilnya, lampu menyala terang, kipas berputar sejuk, dan meteran listrik rumah berjalan jauh lebih hemat sepanjang hari."* (20 kata) | (1) Raka menekan sakelar `point()` + ekspresi `senang`, (2) Lampu gantung menyala + halo hangat, (3) Kipas plafon `spin()`, (4) Gelombang layar TV bergerak, (5) Teko mengepulkan uap `drift()`, (6) Tirai jendela `sway()`, (7) Awan di luar jendela melintas, (8) Jarum meteran `"kWh"` melambat halus, (9) Tanaman pot bergoyang, (10) Raka `idle()` + `ch.sdJ`. | `MCU (1.50)` Raka & sakelar → mundur ke `MS (1.16)` memperlihatkan ruang keluarga hidup (`23.8–25.8s`). | Loop bersih ke `0.0s` |

---

## 5. Pemetaan Keputusan Implementasi Kode (`assets/starter-cartoon-stage.html`)

```js
// 1. Inisialisasi kamera deterministik di dalam timeline (selalu >= 1.08)
setCam(sc1, 0, 720, 620, 1.58);
cam(sc1, 4.2, 960, 540, 1.14, 2.2);

// 2. Rig karakter skala 1 H (0.60), pose 3/4 organik + kanal aditif J() — idle() tidak bentrok point()
const raka = character(sc1.g, 660, 860, 0.60, { who: 'raka', facing: 1, pose: '3/4', handL: 'grip', shirt: '#2563EB', accent: '#F59E0B' });
idle(raka, 0);
point(raka, 2.4, 0.78); // otomatis mengaktifkan setHand(raka, 'R', 'point', 2.4) + anticipation 0.14s

// 3. Aliran elektron parametrik di Panggung 2 (deterministik terhadap tl.time())
const p2Drops = Array.from({ length: 8 }, () => CI(0, 0, 8, '#F59E0B', sc2.g, { opacity: 0 }));
flowAlong(p2Drops, p2Path, 10.2, 2.4);

// 4. Transisi antar panggung (slide hanya saat lokasi nyata berpindah)
slide(9.72, sc1, sc2, 'x', 96);
cut(20.5, sc2, sc3);
```

## 6. Hasil Ringkas Audit QC (Sesuai `templates/qc-audit-scorecard.md`)
- **Lapis 1 (`?debug=1` Audit):** `0` warning `zoom < 1.08` (min `1.12`, maks `1.64`), `0` teks `< 30 px`, scrub maju-mundur di `9.72s` dan `20.5s` presisi.
- **Lapis 2 (F-01 s.d. F-07):** Nol caption & *Action-over-Chart* (`F-01` lolos), kaki & bayangan menempel `y=0` + `J()` utuh + pose kunci lolos uji *Silhouette-First* (`F-02` lolos), arah hadap konsisten kiri→kanan (`F-04` lolos), skala tokoh `0.60` (`1 H`) lebih pendek dari pintu `1,24 H` + *Color Script 70–20–10*, *Light Direction Anchor* searah, *2-tone shading*, & kedalaman BG/MG/FG (`F-07` lolos).
