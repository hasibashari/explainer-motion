# Studi Kasus Ujung-ke-Ujung — Cartoon Stage (Kartun Panggung)

> **Aturan Anti-Copy:** Gunakan dokumen ini untuk mempelajari **tingkat presisi perencanaan, pemetaan beat, dan implementasi kode** — jangan menyalin topik panel surya, nama tokoh, atau palet warnanya ke proyek pengguna.

---

## 1. Input Mentah Pengguna
> *"Buatkan video explainer 30 detik format 16:9 tentang cara kerja panel surya rumahan yang sederhana dan mudah dipahami, tanpa teks subtitle di bawah layar."*

## 2. Keputusan Langkah Nol (Ringkas)
- **Gaya dipilih:** **Cartoon Stage** *(ada tokoh pemilik rumah, cerita sebab-akibat visual, dan user meminta tanpa caption)*.
- **Rasio:** `16:9` (`1920×1080`) *(sudah disebut user)*.
- **Durasi:** `30 detik` *(sudah disebut user)*.
- **Suara:** Tanpa audio dulu, naskah VO ditulis lengkap (`vo-script.md`) siap retime.
- **Referensi yang diambil lewat `reference-index.md`:** `DOC-STAGE`, `VIS-STAGE-ANAT`, `VIS-STAGE-POSE`, `VIS-SHARED-CAM`.

---

## 3. Filled Production Brief (Standar Spesifisitas)

### A. Style Brief
- **Gaya:** Cartoon Stage (palet flat cerah, nol tekstur kertas/grain, nol caption).
- **Palet Warna Terkunci:**
  - Langit BG: `#BFE3E8 → #F7F1DF` · Bukit BG: `#B7DEC0` / `#90CB9F`
  - Tanah MG: `#72B583` · Dinding Rumah: `#F4E6CF` · Atap: `#D96B4B`
  - Panel & Gardu: `#254A5C` · Aksen Energi Listrik: `#F4BC52` · Tokoh Baju: `#2F6FD6`
- **Teks Diegetik Saja (≥ 34 px):** Plat `"INVERTER"` (`38 px`), meteran `"kWh"` (`36 px`).

### B. Target Pacing VO (Rumus 2,2–2,5 kata/dtk)
- **Target durasi:** `30 detik` → **62 kata** (3 paragraf, jeda antar-paragraf ~0,9 dtk).

### C. Character & Asset Bible (Lock Konsistensi)
- **Tokoh Utama (`chA` — Raka):** Tinggi vektor `520 px` (`scale: 1`), rambut pendek `#1E293B`, kaus biru `#2F6FD6`, celana `#1D3557`, kulit `#F7C8A0`, bayangan kaki `ch.sdJ` aktif.
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
cam(sc1, 4.2, 2.2, 960, 540, 1.14);

// 2. Rig karakter dengan kanal aditif J() — idle() tidak pernah bentrok point()
const raka = character(sc1.mg, { x: 660, ground: 860, scale: 0.96, facing: 1, shirt: '#2F6FD6' });
idle(raka, 0);
point(raka, 2.4, { side: 'R', ang: -122, hold: 2.2 }); // anticipation otomatis 0.14s

// 3. Aliran elektron parametrik di Panggung 2 (deterministik terhadap tl.time())
flowAlong(p2Path, 8, { start: 10.2, dur: 2.4, color: '#F4BC52', r: 8 });

// 4. Transisi antar panggung (slide hanya saat lokasi nyata berpindah)
slide(sc1, sc2, 9.72, 'x', 96);
cut(sc2, sc3, 20.5);
```

## 6. Hasil Ringkas Audit QC (Sesuai `templates/qc-audit-scorecard.md`)
- **Lapis 1 (`?debug=1` Audit):** `0` warning `zoom < 1.08` (min `1.12`, maks `1.64`), `0` teks `< 30 px`, scrub maju-mundur di `9.72s` dan `20.5s` presisi.
- **Lapis 2 (F-01 s.d. F-06):** Nol caption (`F-01` lolos), kaki & bayangan menempel `y=0` + `J()` utuh (`F-02` lolos), arah hadap konsisten kiri→kanan (`F-04` lolos).
