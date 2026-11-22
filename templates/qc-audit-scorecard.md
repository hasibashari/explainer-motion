# QC Audit Scorecard — Validasi Dua Lapis (Langkah 5)

Gunakan kartu skor ini **sebelum menyerahkan `index.html`** kepada pengguna. Sebuah output hanya layak diserahkan bila **Lapis 1 (Teknis `?debug=1`)** DAN **Lapis 2 (Kualitatif & Anti-Kegagalan F-01..F-07)** seluruhnya berstatus **LOLOS**. Jika ada 1 poin gagal, rujuk resep perbaikan di `examples/before-after.md`.

---

## Informasi Proyek
- **Topik:** `[isi topik]`
- **Gaya Terpilih:** `[ ] Cartoon Stage` · `[ ] Continuous Action` · `[ ] Cartoon Collage`
- **Rasio & Durasi:** `[16:9 / 9:16]` · `[__ detik]` · **Total Kata VO:** `[__ kata]` *(patokan 2,2–2,5 kata/dtk)*

---

## Lapis 1 — Audit Teknis Deterministik (`?debug=1` QC HUD)

| # | Pemeriksaan Teknis | Cara Uji | Status | Catatan / Perbaikan |
|---|---|---|---|---|
| 1 | **Starter Resmi & Standalone** | Dibangun dari `assets/starter-*.html`, satu file `index.html`, jalan langsung via `file://` tanpa server/player | `[ ] LOLOS` | |
| 2 | **Tombol Audit (`window.OPENER.audit()`)** | Buka `?debug=1` → klik **Audit** → nol error & nol warning merah | `[ ] LOLOS` | |
| 3 | **Determinisme 100%** | Nol `Date.now()`, `performance.now()`, atau `Math.random()` di jalur render; scrub maju-mundur di tiap transisi persis sama | `[ ] LOLOS` | |
| 4 | **Keterbacaan & Safe Area 9:16** | Semua teks `≥ 30 px` (panggung 1080 px); jika `9:16`, aktifkan overlay **Safe** dan cek margin atas `±190 px`, bawah `±230 px`, & strip ikon UI kanan `±160 px` | `[ ] LOLOS` | |
| 5 | **Mode Putar & Rekam (`?clean=1`)** | Mode normal tanpa player, autoplay + loop; `?clean=1` menahan bersih di frame 0 dan tombol `R`/`Space` memutar/menjeda dengan akurat | `[ ] LOLOS` | |

**Fallback Audit Statis *(Wajib jika lingkungan agent tidak memiliki browser runner)*:**
- [ ] Sintaks seluruh blok `<script>` inline lolos validasi parser (`node -e` / `vm.Script`) tanpa galat
- [ ] Nol pemakaian `Date.now()`, `performance.now()`, maupun `Math.random()` di jalur render/timeline
- [ ] Konstanta `W`/`H` & `#stage` konsisten dengan rasio; seluruh `font-size` ≥ `30 px`; nol `<section>` slide (`F-01`)
- [ ] Sesuai gaya: seluruh `cam()`/`setCam()` Stage `≥ 1.08`; nol `visibility="visible"` pada anak `.view` Action & `VK` tidak `<= 0`; setiap `say()` Collage dijadwalkan `≥ +0,1 dtk` setelah `home()` selesai & setiap `.cut` membungkus `.cut-body`

---

## Lapis 2 — Audit Kualitatif & Deteksi Pola Kegagalan (`examples/good-vs-bad.md`)

| Kode | Pola Kegagalan yang Dicek | Kriteria Lolos | Status |
|---|---|---|---|
| **F-01** | *Disguised Slide* (Slide PPT) | Tidak ada `<section>` cross-fade; maksimum 2 tingkat teks (nol caption di Stage); mekanisme diwujudkan lewat aksi fisik (bukan papan grafik statis); dunia terus hidup tiap detik | `[ ] LOLOS` |
| **F-02** | *Floating / Detached Rig* *(Stage)* | Telapak kaki menyentuh `y=0`, `ch.sdJ` ikut gerak, semua sendi pakai `J()` aditif (nol `transformOrigin` GSAP) | `[ ] LOLOS / N/A` |
| **F-03** | *Dashboard Inset Violation* *(Action)* | Nol kartu mini-map/gauge mengambang di atas `#side`; tiap sudut pandang 100% full-frame via `whip()`; angka di `#deepObjs` | `[ ] LOLOS / N/A` |
| **F-04** | *Direction Flip / Continuity Break* | Arah gerak konsisten kiri→kanan lintas adegan/peta; identitas warna/siluet subjek konsisten; kecepatan tidak pernah beku `0` saat baca | `[ ] LOLOS / N/A` |
| **F-05** | *Flat Collage / Timing Clash* *(Collage)* | Akresi 3–5 elemen/area; `.cut` terpisah dari `.cut-body` ber-drop-shadow; `say()` mulai `≥ +0,1 dtk` setelah `home()` selesai | `[ ] LOLOS / N/A` |
| **F-06** | *Spatial Leak / Edge Crop* | Jarak area cukup (nol area tetangga bocor); `sizeWorld()` menutup penuh (nol tepi hitam); framing tidak terpotong di tepi | `[ ] LOLOS` |
| **F-07** | *Broken Scale / Flat Horizon* *(Stage/Action)* | Proporsi mengikuti jangkar `1 H` (pintu `≥ 1,2 H`, mobil `~0,87 H`, objek besar pakai *Architectural Cropping*); sudut kamera, garis cakrawala, *Color Script 70–20–10*, & arah cahaya/*2-tone shading* konsisten | `[ ] LOLOS / N/A` |

---

## Lapis 3 — Cek Spesifik Gaya (Centang Hanya Gaya yang Dipilih)

- **Jika Cartoon Stage:**
  - [ ] Rentang zoom seluruh panggung `1,08× – 1,95×` (nol miniatur `< 1,08`)
  - [ ] Setiap panggung memiliki `≥ 10` benda bergerak yang bermakna
  - [ ] *Color Script 70–20–10*, *Light Direction Anchor*, & *2-tone split shading* konsisten; pose kunci lolos uji *Silhouette-First*
  - [ ] Palet flat bersih tanpa tekstur kertas/grain; teks hanya diegetik (`≥ 34 px`)
- **Jika Continuous Action:**
  - [ ] Tidak ada 3 adegan berurutan dengan sudut pandang yang sama (≥ 3 sudut pada video ≥ 45 dtk)
  - [ ] Pemisahan kedalaman *Color Script* antar lapis parallax & arah cahaya/pantulan kaca konsisten lintas sudut pandang
  - [ ] Tidak ada pemakaian `setAttribute('visibility', 'visible')` pada anak view (pakai `removeAttribute`)
  - [ ] Semua angka berubah memakai `font-variant-numeric: tabular-nums`
- **Jika Cartoon Collage:**
  - [ ] Semua cutout bergoyang hidup (`±22–28 px`) dengan fase berbeda pasca-`fly()`
  - [ ] Transisi antar area memakai `into()` + `settle()` di atas dunia kertas yang sama
  - [ ] Teks hantu `.ghost` terbaca jelas (`opacity 0,18–0,25`)

---

## Keputusan Final
- [ ] **SIAP DISERAHKAN:** Kirim `index.html` (visual murni siap rekam) + tuliskan naskah VO lengkap & ringkasan Beat Sheet ber-timestamp langsung di pesan penutup & simpan `vo-script.md`.
- [ ] **PERLU REVISI:** Catat kode kegagalan (`F-__`), terapkan perbaikan struktural dari `examples/before-after.md`, lalu ulangi audit.
