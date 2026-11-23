# Explainer Core — Aturan Bersama, Decision Tree, Pacing VO, & Interoperabilitas

Dokumen ini berisi seluruh aturan lintas gaya yang berlaku untuk ketiga gaya explainer **Bang Motion**: **Cartoon Stage**, **Continuous Action**, dan **Cartoon Collage**. Setelah gaya dipilih lewat *Decision Tree* di bawah, muat **hanya satu** dokumen referensi gaya spesifik yang sesuai.

---

## 1. Decision Tree Pemilihan Gaya

Gunakan matriks sinyal topik ini sebelum merekomendasikan gaya pada Langkah Nol:

| Sinyal Utama Topik | Gaya Terbaik | Alasan Struktural |
|---|---|---|
| Ada **tokoh/manusia**, cerita sebab-akibat, edukasi anak/umum, atau ingin **tanpa caption** (visual 100% bercerita) | **Cartoon Stage** (`references/cartoon-stage.md`) | Satu panggung hidup per adegan + rig tokoh bersendi (`J()`) + ≥10 benda bergerak membuat narasi terasa seperti film kartun edukasi. |
| Menjelaskan **kecepatan, rute, jarak, transportasi, olahraga, atau alur proses kontinu** di mana satu objek terus melaju | **Continuous Action** (`references/continuous-action.md`) | Dunia parallax `D(t)` tidak pernah berhenti; fakta tampil di dalam dunia + pergantian sudut pandang/instrumen (kabin, peta, profil, interior). |
| Menjelaskan **konsep sains, perbandingan ide, sejarah ringkas, atau topik populer** yang butuh banyak potongan visual, catatan tangan, & penekanan kata | **Cartoon Collage** (`references/cartoon-collage.md`) | Dunia kertas krem dengan cutout ilustrasi, akresi 3–5 elemen per adegan, pill warna, dan koreografi kamera `into → settle → look → home`. |

**Menangani topik ambigu (hibrida):**
- Contoh: *"Bagaimana cara kerja mobil listrik?"*
  - Fokus pada **dampak ke pengguna / proses di rumah & SPKLU** → pilih **Cartoon Stage**.
  - Fokus pada **jarak tempuh, kecepatan pengisian, rute perjalanan** → pilih **Continuous Action**.
  - Fokus pada **komponen baterai, sejarah, dan perbandingan angka** → pilih **Cartoon Collage**.
- Pilih **satu gaya utama** sebagai rumah estetika agar video konsisten dari detik 0 sampai akhir.

### Diagram Alur Kerja Ujung-ke-Ujung (2 Fase + Hard Stop Gate)

```text
═══════════════════ FASE 1: PRA-PRODUKSI & NASKAH (GILIRAN 1) ═══════════════════
[Langkah 0: Kunci 3 Keputusan]
  (Gaya + Rasio + Durasi — lewati yang sudah disebut user)
               │
               ▼
[Langkah 1: Query references/reference-index.md]
  Muat maks. 4 berkas relevan (1 gaya + 1–2 SVG + 1 studi kasus)
               │
               ▼
[Langkah 2 & 3: Susun Paket Pra-Produksi di Workspace User]
  • production-brief.md (Style Brief · Pacing VO · Beat Sheet · Character/Asset Bible)
  • vo-script.md        (Draf naskah narasi VO lengkap, 1 paragraf = 1 babak)
               │
               ▼
╔═══════════════════════════════════════════════════════════════════════════════╗
║  [HARD STOP GATE — WAJIB BERHENTI & AKHIRI GILIRAN DI SINI]                   ║
║  Sajikan Brief, Beat Sheet, Character Bible, & Naskah VO untuk di-ACC user.   ║
║  DILARANG menyalin starter atau menulis index.html sebelum user menyetujui    ║
║  (kecuali user secara eksplisit meminta bypass tahap brief/persetujuan).      ║
╚═══════════════════════════════════════════════════════════════════════════════╝
               │  (User membalas: "oke / lanjut / setuju")
               ▼
════════════════════ FASE 2: PRODUKSI & QC (GILIRAN 2+) ════════════════════════
[Langkah 4: Salin Starter assets/starter-*.html & Bangun index.html]
  Ekstrak prinsip (ANTI-COPY), terapkan sesuai Brief & Beat Sheet yang di-ACC
               │
               ▼
[Langkah 5: Audit Dua Lapis — templates/qc-audit-scorecard.md]
  Lapis 1: ?debug=1 Audit & Scrub  |  Lapis 2: Cek F-01..F-07 (examples/good-vs-bad.md)
               │
        ┌──────┴──────┐
     [GAGAL]       [LOLOS]
        │             │
        ▼             ▼
  Terapkan resep   [Langkah 6: Penyerahan]
  examples/        index.html standalone + vo-script.md final
  before-after.md  (ringkasan Beat Sheet ber-timestamp di pesan penutup)
        │             │
        └─► ulangi    └─► Siap rekam layar (?clean=1) &
            Langkah 5     sinkronisasi audio manual di video editor
```

---

## 2. Langkah Nol: Kunci 3 Keputusan dalam Satu Pertanyaan

Jika user hanya memberi topik singkat (mis. *"buatkan explainer tentang X"*), ajukan **SATU pertanyaan ringkas** berisi 3 keputusan ini (lewati bagian yang sudah disebutkan user):

1. **Gaya** *(selalu sertakan 1 rekomendasi + alasan singkat berdasarkan Decision Tree)*:
   - **Cartoon Stage** — flat cerah, satu panggung vektor hidup per adegan, tokoh bersendi, tanpa caption
   - **Continuous Action** — dunia vektor mengalir, satu subjek terus melaju, angka di dalam dunia, ganti sudut & peta
   - **Cartoon Collage** — kertas krem bertekstur, cutout ilustrasi flat, pill warna, tulisan tangan, koreografi kamera
2. **Rasio**: `16:9` *(default — layar/YouTube)* · `9:16` *(Reels/TikTok/Shorts)*
3. **Durasi**: `30 dtk` · `60 dtk` *(default)* · `90 dtk`

**Jika ketiga keputusan sudah lengkap di prompt pertama:** Lewati pertanyaan Langkah Nol, lalu langsung masuk ke **Fase 1 (Langkah 2–3)** untuk menyusun `production-brief.md` (Style Brief, Beat Sheet, Character/Asset Manifest) dan `vo-script.md`, kemudian **WAJIB BERHENTI (Hard Stop Gate)** untuk meminta persetujuan user sebelum membangun `index.html`.

**Yang tidak perlu ditanyakan:** audio/suara (default: output berfokus murni pada visual HTML tanpa audio tertanam; naskah VO disusun sejak Fase 1 di `vo-script.md` agar siap direview lalu disinkronkan secara manual di video editor), caption (default: tanpa subtitle di bawah layar), dan player (default: tanpa kontrol player, autoplay + loop, `?clean=1` untuk menahan frame 0, scrub hanya lewat `?debug=1`).

---

## 3. Rumus Pacing VO & Pagar Durasi

Jangan mengarang panjang naskah secara acak. Gunakan patokan kecepatan tutur alami Bahasa Indonesia (**2,2–2,5 kata/detik**, atau ±135–150 kata/menit) ditambah **jeda antar paragraf 0,8–1,2 detik**:

| Target Durasi | Total Kata VO Ideal | Jumlah Adegan / Panggung Lazim | Catatan Ritme |
|---|---|---|---|
| **30 detik** | 55 – 65 kata | 3 – 4 adegan | 1 hook kuat + 2 poin inti + 1 penutup |
| **60 detik** | 115 – 135 kata | 5 – 7 adegan | Beri ruang 1–2 beat visual tanpa pergantian adegan |
| **90 detik** | 175 – 200 kata | 7 – 10 adegan | Pecah menjadi 3 babak (Hook → Mekanisme → Dampak/Penutup) |

**Aturan Pagar Durasi:**
- Jumlah adegan ditentukan oleh **perubahan konteks/lokasi/sudut pandang**, bukan membagi durasi rata per 6 detik. Satu panggung atau satu area kolase boleh menampung 2–3 kalimat VO selama aksi visualnya terus berkembang.
- Jika materi terlalu padat untuk durasi target, **ringkas materinya**, jangan mempercepat tempo bicara atau memperpanjang durasi tanpa izin.
- Permintaan sangat panjang (mis. 5–10 menit) wajib dikonfirmasi lalu dipecah per bab ≤ 90 detik dalam berkas terpisah.

---

## 4. Bahasa Kamera, Ukuran Shot, Camera Angle, Skala `1 H`, & Perspektif

> **Referensi Visual Beranotasi:** Lihat **`references/visuals/shared-camera-composition.svg`** (framing & 9:16) dan **`references/visuals/shared-scale-perspective.svg`** (tangga skala `1 H`, 4 *camera angles*, garis cakrawala, & kedalaman perspektif).

### A. Ukuran Shot (*Shot Grammar*)
Gunakan notasi ukuran shot standar saat menyusun `templates/production-brief.md` agar gerak kamera terukur:

| Kode | Ukuran Shot | Penggunaan Ideal |
|---|---|---|
| **WS** | Wide Shot | Memperkenalkan konteks panggung / seluruh rute peta *(di Cartoon Stage tetap zoom ≥ 1,08)* |
| **MS** | Medium Shot | Aksi tokoh + objek interaksi dalam satu framing |
| **MCU** | Medium Close-Up | Fokus ke reaksi tokoh atau pasangan 2 elemen utama |
| **CU** | Close-Up | Detail objek penting, jarum speedometer, ekspresi wajah, atau aset pertama saat adegan dibuka |

### B. Sistem Skala Semi-Realistis Berbasis `1 H` (Manusia Dewasa = 1,7 m)
Jangan menebak tinggi objek secara acak. Kunci seluruh proporsi objek di satu adegan terhadap satuan **1 H (tinggi dewasa ≈ 1,7 m)** agar pintu tidak lebih pendek dari kepala tokoh dan mobil tidak tampak seperti mainan:

| Objek | Rasio Tinggi (`H`) | Rasio Panjang / Lebar | Acuan Piksel (`1 H ≈ 330–345 px`, Stage `baseScale: 0.60`, rig unscaled `~576 px`, kaki di `y = 0`) |
|---|---|---|---|
| **Anak-anak** | `0,68 H` | — | `~225–235 px` *(Stage `scale ≈ 0.41`)* |
| **Manusia Dewasa (Jangkar)** | **`1,00 H`** | bahu `~0,28 H` | **`~330–345 px`** *(Stage `scale: 0.60`; saat zoom 1,15–1,68× tetap 36–56% layar)* |
| **Meja / Kursi / Pagar** | `0,45 – 0,60 H` | — | `145 – 192 px` |
| **Sepeda / Motor** | `0,65 H` | panjang `1,20 H` | `208 × 385 px` |
| **Mobil Sedan** | `0,87 H` | panjang `2,60 H` | `278 × 832 px` *(atap sejajar dada/bahu tokoh)* |
| **SUV / Van Kurir** | `1,15 H` | panjang `2,85 H` | `368 × 912 px` *(sedikit lebih tinggi dari tokoh)* |
| **Pintu Rumah Standar** | `1,24 H` | lebar `0,55 H` | `396 × 176 px` *(kepala tokoh aman masuk)* |
| **Bus / Truk Box** | `1,82 H` | panjang `5,20 H` | `582 × 1664 px` |
| **Rumah 1 Lantai (Dinding + Atap)** | `1,95 H + 0,88 H` | lebar `4,0–5,5 H` | dinding `624 px` + atap `280 px` |
| **Tiang Lampu / Pohon Dewasa** | `2,65 – 3,50 H` | — | `850 – 1120 px` *(boleh dipotong tepi atas frame)* |

- **Aturan *Architectural Cropping & Tilt*:** Untuk objek raksasa (gedung bertingkat, truk besar, jembatan, menara), **jangan mengecilkan objek agar muat utuh di samping tokoh**. Gunakan **Low-Angle** dan biarkan puncaknya terpotong tepi atas frame atau lakukan *tilt* vertikal `cam()` dari manusia ke puncak bangunan.
- **Pengecualian Cartoon Collage:** Kolase boleh memakai skala editorial/metaforis antar-cutout terpisah, tetapi tetap wajib menjaga proporsi `1 H` **di dalam satu ilustrasi gabungan atau adegan perbandingan**, serta menerapkan kedalaman kertas Z-plane (`.ghost` → `.cut-body` → `.note`/`.stamp`).

### C. Empat Camera Angle & Garis Cakrawala (*Horizon Line*)

| Camera Angle | Posisi Cakrawala (`y` di 1080 px) | Efek Visual & Kapan Dipakai |
|---|---|---|
| **Eye-Level** *(default)* | `y ≈ 520 – 600 px` *(sejajar mata)* | Skala jujur dan akrab; semua tokoh yang berdiri sejajar punya mata di ketinggian garis cakrawala sama. |
| **Low-Angle** *(mendongak)* | `y ≈ 780 – 900 px` *(sepertiga bawah)* | Langit dominan, bangunan/mesin menjulang tinggi; dipakai untuk menegaskan **skala masif, kecepatan, atau wibawa**. |
| **High-Angle / Bird's-Eye** | `y ≈ 180 – 300 px` *(atau 90° top-down)* | Jalan, halaman, dan peta sistem meluas; dipakai untuk **rute, kemacetan, ekosistem, atau denah ruang**. |
| **Foreground Framing (OTS)** | Mengikuti Eye / Low-Angle | Elemen dekat lensa (dahan, siluet bahu, tiang, bingkai) skala `1,5–2,2×` warna gelap di tepi frame → **ilusi kedalaman 3D instan**. |

**Aturan Kontinuitas Spasial & Perspektif:**
- **180-Degree Rule (Arah Layar):** Bila subjek bergerak atau melaju dari kiri ke kanan di adegan 1, pertahankan arah kiri → kanan pada adegan berikutnya kecuali ada pemicu visual jelas (berbalik arah).
- **Lead Room / Looking Room:** Beri ruang kosong lebih lebar di depan arah hadap atau arah gerak tokoh/kendaraan — jangan menempelkan hidung subjek ke tepi frame.
- **Kedalaman Atmosfer & 2,5D:** Makin jauh ke belakang (BG), turunkan saturasi & tarik warnanya ke warna langit; tampilkan sisi samping teredup (`sh(hex, 0.85)`) dan jalan/lantai berperspektif trapesium menuju cakrawala.
- **Perubahan Ukuran Shot vs Napas Kamera:** Saat babak tanpa aksi objek besar, lakukan perubahan ukuran shot/sudut nyata (`WS ↔ MCU ↔ CU` atau `Eye ↔ Low`) mengikuti ketukan narasi — osilasi napas beberapa persen saja tidak cukup menghilangkan rasa statis.

### D. *Color Script* (70–20–10), *Vector Lighting*, & *Asset Craft* Lintas-Gaya
Prinsip art direction ini **adaptif lintas variasi estetika** (berlaku baik untuk *ultra-flat*, *cel-shaded editorial*, maupun *2,5D stylized cartoon*) tanpa mengikat hasil akhir pada satu palet atau satu gaya ilustrasi tertentu:
- **Color Script 70–20–10 (*Background Hue Restraint*):** Kunci **~70%** luas environment pada satu keluarga warna analogus/monokrom lembut ber-kontras rendah, **~20%** pada struktur pendukung/arsitektur, dan cadangkan **~10%** warna kontras atau suhu berlawanan (*warm vs cool*) khusus untuk **subjek utama dan titik aksi**. Jangan menyebar banyak warna cerah ber-saturasi tinggi ke seluruh latar sekaligus.
- **Light Direction Anchor & *Vector Lighting*:** Tetapkan **satu arah sumber cahaya konsisten** per adegan (mis. kiri-atas → kanan-bawah). Pastikan sisi gelap 2,5D (`sh(hex, 0.84–0.88)`), bayangan jatuh (*cast shadow*), *rim highlight* tipis, serta *volumetric light beam* (poligon cahaya diagonal semi-transparan dari jendela/lampu) semuanya mengikuti arah cahaya yang sama.
- ***2-Tone Split Shading* & *Micro-Geometry*:** Hindari aset vektor 1-warna datar yang terasa seperti bentuk primitif mentah. Berikan minimal **2 nilai warna** (sisi terang vs sisi bayangan) pada daun, batu, furnitur, wajah/leher, dan kendaraan, lalu tambahkan **3–6 aksen *micro-geometry* bersih** (pantulan kaca diagonal seragam, garis mortar/paving parsial, lipatan/kerah baju) tanpa outline hitam tebal dan tanpa tekstur *grain* raster (kecuali kertas kolase).
- ***Silhouette-First* & *Action-over-Chart*:** Setiap pose kunci tokoh harus langsung terbaca maksudnya (*menunjuk, kaget, berpikir, bekerja, merayakan*) hanya dari siluet tubuhnya dalam ≤ 0,5 detik. Saat menjelaskan data/sebab-akibat, wujudkan lewat **aksi fisik & mekanisme lingkungan**, bukan menempatkan tokoh berdiri di samping papan grafik statis.

---

## 5. Keterbacaan di Ponsel & Format 9:16

Ukuran huruf **minimum** pada panggung referensi sisi pendek `1080 px` (`1920×1080` maupun `1080×1920`, sebelum *safe-scale*):

| Jenis Teks | Minimum |
|---|---|
| Judul / angka utama | ≥ 100 px |
| Anotasi tulisan tangan | ≥ 48 px |
| Body copy / keterangan | ≥ 44 px |
| Label nama / tahun / plat diegetik | ≥ 34 px |
| Kredit / sumber | ≥ 30 px *(opacity ≥ 0,6)* |

**Aturan keras:** Tidak ada satu pun teks di bawah `30 px`. Bila kalimat tidak muat, pecah atau ringkas — jangan memperkecil font.

**Penyesuaian 9:16 (`1080×1920`):**
- Ubah konstanta `W = 1080, H = 1920` dan ukuran `#stage`.
- Hormati **3 zona bahaya UI vertikal (asimetris)** sesuai `references/visuals/shared-camera-composition.svg`: margin atas `±190 px`, margin bawah `±230 px`, dan **strip ikon UI kanan (`±160 px` di sisi kanan tengah-bawah)**. Uji dengan tombol **Safe** di `?debug=1`.
- Bungkus **konten fokus saja** (bukan latar belakang atau `#world`) dalam `.safe { transform: scale(.78); transform-origin: 50% 45%; }` atau tempatkan koordinat subjek langsung di dalam area aman (`x: 70..900, y: 210..1660`), sambil menjaga latar/kertas tetap penuh `100%`.
- **Wajib adaptasi geometri per gaya** — mengganti `W`/`H` saja tidak cukup karena placeholder starter berkoordinat `1920×1080`. Ikuti panduan koordinat `9:16` di modul gaya yang dipilih (`cartoon-stage.md`, `continuous-action.md`, atau `cartoon-collage.md`).
- Skalakan `#stage` ke jendela dengan `position: absolute; left: 50%; top: 50%; transform: translate(-50%,-50%) scale(s)` — bukan CSS Grid `place-items: center`.

---

## 6. Penyerahan Visual & Naskah Narasi

- **Fokus visual murni siap rekam:** `index.html` tidak menyertakan atau memutar audio (musik, VO, maupun SFX disinkronkan secara manual di video editor pada tahap editing akhir). Buka dengan `?clean=1` untuk menahan frame `0.00` secara bersih sebelum merekam layar, lalu tekan `R` atau `Space` untuk memulai pemutaran.
- **Penyerahan wajib (Fase 2, pasca-ACC):** Serahkan satu berkas `index.html`, perbarui `vo-script.md` jika ada penyesuaian durasi/timing saat produksi, dan sertakan **naskah VO final** (satu paragraf = satu adegan, angka ditulis dengan kata, sesuai pacing `2,2–2,5 kata/dtk`) beserta **ringkasan timestamp/Beat Sheet** langsung di dalam pesan penutup.

---

## 7. Matriks Interoperabilitas Lintas Gaya

| Kombinasi | Status | Panduan Eksekusi |
|---|---|---|
| **Continuous Action + Sisipan Peta/Diagram** | **Direkomendasikan** | Sudah bawaan rig Continuous Action; gunakan whip antar `<div class="view">` dengan palet vektor yang sama. |
| **Cartoon Stage + 1 Panggung Peta/Aliran** | **Boleh (Kontekstual)** | Boleh memakai 1 panggung peta/penampang aliran di dalam rig Cartoon Stage, **wajib tetap memakai palet flat cerah tanpa tekstur**. |
| **Cartoon Stage + Cartoon Collage** | **Hindari Pencampuran Acak** | Estetika bertolak belakang (vektor flat tanpa caption vs kertas krem bertekstur + pill kalimat). Hanya boleh jika ada alasan naratif eksplisit (mis. tokoh membuka buku kliping). |
| **Kontrak Runtime Seragam** | **Wajib** | Ketiga starter memakai kontrak identik: `window.OPENER = { W, H, DURATION, tl, ready, seek(t), play, audit() }`, pintasan `R`/`Space`, dan panel `?debug=1` QC HUD. |
