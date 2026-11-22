# Good vs Bad — Katalog Pola Kegagalan Visual (F-01 s.d. F-07)

Pasangan visual ringkas tersedia di **`references/visuals/shared-good-vs-bad.svg`** dan kalibrasi proporsi & sudut kamera tersedia di **`references/visuals/shared-scale-perspective.svg`**.  
Gunakan dokumen ini sebagai **rubrik diskriminatif** pada Langkah 2 (*review Beat Sheet*) dan Langkah 5 (*self-audit* sebelum penyerahan) untuk membedakan hasil yang **kuat** dari hasil yang **lemah secara visual** meski secara teknis tidak menimbulkan error JavaScript.

---

## Ringkasan 7 Pola Kegagalan Kritis (*Failure Modes*)

| Kode | Nama Pola Kegagalan | Gaya Terdampak | Tingkat Bahaya |
|---|---|---|---|
| **F-01** | *Disguised Slide* (Slide PPT Terselubung) | Semua gaya (terutama Cartoon Stage) | **KRITIS — Wajib Rombak** |
| **F-02** | *Floating / Detached Rig* (Kaki Melayang / Sendi Lepas) | Cartoon Stage | **KRITIS — Wajib Perbaiki** |
| **F-03** | *Dashboard Inset Violation* (Mini-Map / Gauge Mengambang) | Continuous Action | **KRITIS — Wajib Rombak** |
| **F-04** | *Direction Flip / Continuity Break* (Arah Gerak Berbalik) | Continuous Action & Cartoon Stage | **TINGGI — Wajib Perbaiki** |
| **F-05** | *Flat Collage / Timing Clash* (Tanpa Shadow / `say()` Bocor) | Cartoon Collage | **KRITIS — Wajib Perbaiki** |
| **F-06** | *Spatial Leak / 9:16 Edge Crop* (Area Tetangga Bocor / Terpotong) | Cartoon Collage & semua gaya `9:16` | **TINGGI — Wajib Perbaiki** |
| **F-07** | *Broken Scale / Flat Horizon* (Proporsi Mainan & Panggung Datar) | Cartoon Stage & Continuous Action | **TINGGI — Wajib Perbaiki** |

---

## Bedah Kontrastif per Pola Kegagalan

### F-01 — *Disguised Slide* (Slide PPT Terselubung)
- **Contoh BURUK (Tolak):**
  - Latar putih/polos dengan judul di atas, 3 poin bullet di kiri, dan 1 ilustrasi karakter diam di kanan.
  - Menggunakan `<section>` yang diganti via `autoAlpha`/`opacity` tiap 6 detik.
  - Di Cartoon Stage: terdapat subtitle/caption di bawah layar atau hanya ada 3–4 objek diam di panggung.
- **Contoh BENAR (Terima):**
  - **Cartoon Stage:** Panggung penuh 3 bidang kedalaman (BG/MG/FG), ≥10 benda bergerak bermakna (asap, kipas, aliran pipa, jarum, daun, awan), **nol caption**, teks hanya diegetik (papan toko, plat mesin).
  - **Cartoon Collage:** Minimal 3–5 elemen muncul bertahap (*accretion*) mengikuti frasa VO, bukan 1 gambar + 1 kalimat sekaligus.
- **Batas Ambang (*Acceptable vs Weak*):**
  - *Lemah:* Menaruh 10 lingkaran dekoratif berkedip acak hanya untuk mengejar angka ≥10.
  - *Kuat:* Setiap dari ≥10 gerak punya peran naratif/atmosfer yang menjelaskan lokasi atau sebab-akibat.
- **Cek di `?debug=1`:** Scrub ke titik tengah tiap adegan; pastikan selalu ada aksi dunia nyata, bukan transisi cross-fade section diam.

---

### F-02 — *Floating / Detached Rig* (Kaki Melayang & Sendi Lepas)
- **Contoh BURUK (Tolak):**
  - Tokoh ditaruh di tengah layar tanpa garis tanah atau tanpa elips bayangan kontak (`ch.sdJ`).
  - Leher tampak terpisah dari torso atau lengan terlepas dari bahu saat berputar karena menggunakan `gsap.to(el, { rotation: ..., transformOrigin: '...' })` langsung pada `<g>` SVG.
  - Animasi `idle()` menimpa rotasi `point()`/`wave()` sehingga tangan tersentak balik ke 0°.
- **Contoh BENAR (Terima):**
  - Telapak kaki menyentuh `y = 0` di dalam grup `place`, dengan bayangan `ch.sdJ` persis di bawah kaki yang ikut bergeser saat `walk()` dan mengecil saat `jump()`.
  - Seluruh sendi memakai proxy **`J()` aditif** (`r + rIdle`, `x + xIdle`) dengan rentang sudut wajar (`references/visuals/stage-poses-expressions.svg`).
- **Cek di `?debug=1`:** Scrub tepat di puncak gerakan `point()`, `wave()`, dan `walk()`; periksa sambungan leher, bahu, siku, dan posisi bayangan kaki.

---

### F-03 — *Dashboard Inset Violation* (Kartu Mini-Map / Gauge Mengambang)
- **Contoh BURUK (Tolak):**
  - Pada sudut pandang `#side` Continuous Action, menempelkan kartu mini-map mengambang di pojok kanan atas, widget speedometer di pojok kanan bawah, serta teks angka + 2 baris subjudul di pojok kiri atas secara bersamaan.
  - Membelah layar menjadi panel perbandingan kiri-kanan statis.
- **Contoh BENAR (Terima):**
  - Setiap sudut pandang/instrumen mengisi **100% layar penuh** dan berganti mulus lewat `whip()` (`#side → #cabin → #map → Profil → Balapan Peta`).
  - Pada `#side`, angka besar hidup di lapisan **`#deepObjs`** (di belakang subjek) sehingga subjek melintas di depannya dan menyapunya keluar.
  - Maksimum 2 tingkat teks (angka utama + 1 label pendek).
- **Cek di `?debug=1`:** Pantau indikator `view` aktif sepanjang timeline; pastikan minimal 3 view berbeda pada video ≥45 dtk dan tidak ada kartu inset mengambang di atas `#side`.

---

### F-04 — *Direction Flip / Continuity Break* (Arah Gerak Berbalik)
- **Contoh BURUK (Tolak):**
  - Di `#side`, kendaraan melaju dari kiri → kanan, tetapi di `#map` garis rute justru bergerak dari kanan → kiri, atau di Panggung 2 tokoh keluar dari sisi kanan bergerak ke kiri tanpa alasan naratif.
  - Subjek berubah warna bodi atau kehilangan siluet khasnya saat pindah dari `#side` ke `#map` atau Tampak Depan.
  - Dunia berhenti total (kecepatan = 0) saat angka sedang dibaca.
- **Contoh BENAR (Terima):**
  - Arah layar konsisten **kiri → kanan** (180-degree rule) lintas adegan & peta.
  - 3 *Subject Anchor* (warna bodi + garis `--accent`, siluet khas, warna ikon peta) identik di seluruh view.
  - Saat angka dibaca, kecepatan diturunkan ke ~35–45% agar dunia tetap mengalir halus.
- **Cek di `?debug=1`:** Scrub bolak-balik di setiap `whip()` / `slide()` dan verifikasi konsistensi arah pandang serta identitas warna subjek.

---

### F-05 — *Flat Collage / Timing Clash* (Kolase Tanpa Shadow & `say()` Terpotong)
- **Contoh BURUK (Tolak):**
  - Memasang `filter: drop-shadow(...)` pada node `.cut` yang sama dengan `blurTween()` → bayangan kertas hilang permanen begitu animasi masuk selesai, membuat potongan tampak datar/tempelan mentah.
  - Cutout masih menyisakan latar kotak/segitiga putih yang belum terpotong bersih.
  - Memulai `say()` ketika kamera `home()` masih bergerak mundur → teks berebut fokus dan tampak terpotong.
  - Semua cutout diam beku setelah mendarat atau bergerak serempak dengan durasi identik.
- **Contoh BENAR (Terima):**
  - `.cut` khusus posisi/transform/motion-blur; **`.cut-body`** di dalamnya memegang `filter: drop-shadow(...)`.
  - `say()`, `draw()`, dan `label()` dimulai minimal **`+0,1 dtk` setelah `home()` selesai (`z = 1.0`)**.
  - Tiap cutout punya ayunan hidup `±22–28 px` dengan durasi/fase berbeda.
- **Cek di `?debug=1`:** Klik **Audit** (cek `.cut-body` utuh), lalu scrub tepat di awal `say()` pada tiap area untuk memastikan `z === 1.00`.

---

### F-06 — *Spatial Leak / 9:16 Edge Crop* (Kebocoran Area & Pemotongan Vertikal)
- **Contoh BURUK (Tolak):**
  - Jarak antar area Cartoon Collage terlalu sempit (`dx < 2200`) sehingga pinggiran cutout area berikutnya ikut masuk ke sudut layar saat `home()`.
  - Lupa memanggil `sizeWorld(...)` sehingga muncul tepi hitam pekat di pinggir kertas saat kamera bergerak `into()`.
  - Pada rasio `9:16`, kepala tokoh atau kata terakhir kalimat menempel di tepi layar atau tertimpa zona UI atas (`±190 px`), bawah (`±230 px`), atau strip ikon UI kanan (`±160 px`).
  - Ukuran font jatuh di bawah `30 px` pada panggung 1080 px.
- **Contoh BENAR (Terima):**
  - Cartoon Collage memakai grid zig-zag `dx = 2600 px, dy = 1700 px` + `sizeWorld(minX, minY, maxX, maxY)` (margin otomatis `±1600 px`).
  - Format `9:16` membungkus **konten fokus saja** (bukan latar belakang atau `#world`) dalam `.safe { transform: scale(.78); transform-origin: 50% 45%; }` atau mengadaptasi koordinat langsung ke area aman (`±190 px` atas, `±230 px` bawah, `±160 px` kanan), dengan latar penuh 100% dan semua teks `≥ 30 px`.
- **Cek di `?debug=1`:** Klik **Audit** (cek font `<30px` dan batas `.paper`), aktifkan overlay **Safe** saat mode `9:16`, dan scrub seluruh transisi `into()`.

---

### F-07 — *Broken Scale / Flat Horizon* (Proporsi Mainan & Panggung Datar)
- **Contoh BURUK (Tolak):**
  - Tinggi tokoh dewasa lebih tinggi daripada pintu rumah di belakangnya, atau mobil sedan di sebelah tokoh hanya setinggi lutut seperti mobil mainan.
  - Gedung bertingkat, gardu listrik, atau pohon besar diperkecil paksa agar seluruh atapnya muat di dalam bingkai 1080 px (menghancurkan kesan besar).
  - Semua bangunan digambar sebagai persegi datar 2D tanpa sisi kedalaman, tanah hanya berupa garis horizontal datar tanpa bidang perspektif, serta tidak ada variasi sudut kamera (*Eye-Level* datar di seluruh panggung).
  - Di **Continuous Action**, semua lapisan parallax bergerak dengan rasio kecepatan hampir sama sehingga jarak jauh-dekat terasa menempel datar.
- **Contoh BENAR (Terima):**
  - Mengunci jangkar **`1 H`** (dewasa `≈ 330–345 px`, `baseScale: 0.58–0.64`, default `0.60` pada *Medium Shot*): pintu standar `1,24 H` (`~400–420 px`), mobil sedan `0,87 H × 2,55 H` (atap sejajar dada–bahu tokoh), bus/truk `1,85 H`, dan bangunan 1 lantai `1,95–2,6 H`.
  - Objek raksasa (gedung bertingkat, menara, gardu) menerapkan ***Architectural Cropping*** (biarkan atap melampaui tepi atas kanvas) dan dipadukan dengan ***Low-Angle*** (cakrawala rendah) + ***Vertical Tilt*** kamera ke atas — tanpa pernah menurunkan zoom di bawah `1,08×`.
  - Bangunan menampilkan sisi kedalaman **2.5D** lebih gelap (`sh(hex, 0.85)`), bidang jalan/halaman trapesium mengarah ke garis cakrawala, atmosfer BG lebih pudar, dan bingkai FG besar gelap di tepi bawah/sudut.
  - Di **Continuous Action**, rasio kecepatan `strip()` berjenjang wajar mengikuti jarak nyata (`0.06 → 0.16 → 0.36 → 0.64 → 0.95–1.00 → 1.40–1.55`). *(Catatan: Cartoon Collage dikecualikan untuk skala metaforis/editorial yang disengaja, selama hierarki ukuran utama vs pendukung tetap jernih.)*
- **Cek di `?debug=1`:** Scrub ke shot terlebar tiap panggung/view dan bandingkan tinggi tokoh terhadap pintu, kendaraan, dan bangunan menggunakan lembar **`references/visuals/shared-scale-perspective.svg`**; pastikan garis cakrawala konsisten dengan sudut kamera yang dipilih.
