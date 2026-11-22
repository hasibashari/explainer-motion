# Production Brief Template — Style Brief, Pacing VO, Beat Sheet, & Asset Manifest

Salin dan isi kerangka ringkas ini ke brief proyek pada **workspace output user** (jangan menimpa berkas template asli skill) pada **Langkah 2**, lalu tunjukkan kepada user untuk disetujui **sebelum menulis kode HTML/GSAP**.

---

## 1. Style Brief

- **Topik & pesan utama (1 kalimat):** `<apa satu argumen yang ingin dibawa pulang penonton?>`
- **3 kata sifat nuansa:** `<mis. cerah, taktis, penasaran>`
- **Gaya terpilih:** `<Cartoon Stage | Continuous Action | Cartoon Collage>`
  - *Alasan pemilihan (dari Decision Tree):* `<mengapa gaya ini paling tepat?>`
- **Rasio & Durasi:** `<16:9 | 9:16>` · `<30 | 60 | 90>` detik
- **Color Script 70–20–10 (diturunkan dari topik/brand — bukan warna cerah merata atau placeholder abu-abu):**
  - 70% Latar / Environment *(keluarga analogus berkontras rendah)*: `#______` · `#______` *(sumber/alasan)*
  - 20% Arsitektur / Pendukung *(termasuk sisi gelap 2,5D `sh()`)*: `#______` *(sumber/alasan)*
  - 10% Subjek Utama / Titik Aksi *(kontras tinggi / suhu berlawanan)*: `#______` *(sumber/alasan)*
  - Tinta / Teks: `#______`
- **Arah Cahaya (*Light Direction Anchor*) & Resep Ruang:** `<mis. kiri-atas → kanan-bawah>` · `<Interior Window-Depth + Light Beam | Eksterior S-Curve | Faceted Cross-Section | Parallax>`
- **Tipografi:** Display `<nama font>` / Pendamping `<nama font>` *(untuk Cartoon Stage: khusus label diegetik)*
- **Arah gerak & kontinuitas (180° rule):** `<mis. subjek selalu melaju kiri → kanan>`
- **Momen visual istimewa (1× puncak video):** `<momen paling berkesan di detik ke-...>`

### 1B. Reference Coverage & Traceability *(Isi Hanya Kategori yang Relevan dengan Topik)*
*(Gunakan klasifikasi `[VERIFIED]`, `[INFERRED]`, `[TEMPORARY]`, `[MISSING]` sesuai `references/reference-index.md` §1B. Abaikan kategori yang tidak relevan.)*

| Kategori | Kebutuhan Aktual Proyek | Sumber Referensi (ID Skill / File User) | Status & Tindakan |
|---|---|---|---|
| **Karakter & Ekspresi** | `<mis. 1 tokoh dewasa, 3 ekspresi>` | `<VIS-STAGE-POSE / file user>` | `<[VERIFIED] / [INFERRED] / [MISSING]>` |
| **Gerak & Gestur** | `<mis. jalan, menunjuk, lompat>` | `<VIS-STAGE-POSE>` | `<[VERIFIED]>` |
| **Lokasi & Gaya** | `<mis. halaman rumah siang, flat 70-20-10>` | `<VIS-STAGE-ANAT, VIS-SHARED-SCALE>` | `<[VERIFIED] / [INFERRED]>` |
| **Aset / Prop / Brand** | `<mis. logo / produk / kendaraan>` | `<file user / belum tersedia>` | `<jika [MISSING] → minta ke user sebelum coding>` |

---

## 2. Target Pacing VO

Patokan Bahasa Indonesia: **2,2–2,5 kata/detik** + jeda antar paragraf **0,8–1,2 detik**.
*(30 dtk ≈ 55–65 kata · 60 dtk ≈ 115–135 kata · 90 dtk ≈ 175–200 kata)*

- **Estimasi total kata:** `___` kata
- **Jumlah paragraf / babak:** `___` paragraf

---

## 3. Beat Sheet / Storyboard

| Detik | Panggung / Area / View | Paragraf VO *(angka ditulis kata)* | Beat Visual & Aksi Utama | Ukuran Shot & Gerak Kamera | Transisi |
|---|---|---|---|---|---|
| `0.0 – 8.0` | `<P1 / #side / Area 1 (960,540)>` | `<kalimat VO paragraf 1>` | `<aksi bertahap / ≥10 benda bergerak / 3–5 elemen>` | `<CU → MCU / zoom 1.6→1.15 / into→look→home>` | `<cut / slide 96px / whip / push-through>` |
| `8.0 – 16.0` | `<...>` | `<...>` | `<...>` | `<...>` | `<...>` |

**Checklist Cepat Beat Sheet:**
- [ ] Total kata sesuai target durasi (`~2,3 kata/dtk`)
- [ ] Tidak ada pergantian adegan yang tidak perlu (beberapa beat boleh berada di panggung/area yang sama)
- [ ] Tidak ada adegan yang hanya berisi 1 gambar diam + 1 kalimat, atau tokoh berdiri di depan papan grafik statis (*Action-over-Chart*)

---

## 4. Character Bible / Asset Manifest

### A. Jika Cartoon Stage — Character Bible
| ID (`who`) | Skala Rig (`baseScale ≈ 0.60` = `1 H`) | Arah & Sudut | Palet (Baju / Celana / Rambut) | Ciri Siluet & Detail Pakaian | Prop per Panggung | Sumber Kepala & Ref |
|---|---|---|---|---|---|---|
| `<nama>` | `0.60` *(1.00 H)* | `facing: 1 · 3/4` | `<#hex / #hex / #hex>` | `<mis. kerah, kacamata, tas selempang>` | `<P1: ponsel>` | `<vektor / Data URI · [INFERRED]>` |

### B. Jika Cartoon Collage — Master Style Prefix & Daftar Cutout
- **Master Style Prefix (tetap untuk semua aset):** `<salin dari references/asset-consistency.md & isi palet>`

| ID Elemen | Adegan & Koordinat `(x, y)` | Deskripsi Aset / Komponen | Tipe (`cutout / polaroid / note / stamp / SVG`) |
|---|---|---|---|
| `#cutA` | `Area 1 (220, 180)` | `<objek spesifik>` | `cutout` |

### C. Jika Continuous Action — Subject Visual Anchor & Daftar Sudut Pandang
- **Subject Anchor:** Bodi `#______`, Aksen `#______`, Siluet `<deskripsi>`, Arah gerak `kiri → kanan`.
- **Urutan Sudut Pandang:** `<contoh: #side → #cabin → #map → #side>` *(tidak ada 3 berurutan sama)*.

### D. Environment, Prop, & Object State Tracker *(Opsional — Isi Jika Ada Prop Berulang / Berubah Kondisi)*
| ID Aset / Prop | Muncul di Adegan | Skala (`H`) & Palet Kunci | Kondisi / State & Transisi | Interaksi Tokoh / Lokasi |
|---|---|---|---|---|
| `<mis. tong-kompos>` | `P1, P2` | `0.55 H · #223B4E / #F59E0B` | `P1: tertutup → P2: penampang terbuka` | `<P1: tangan kanan membuang sampah>` |

