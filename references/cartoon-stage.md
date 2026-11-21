# Cartoon Stage — Panduan Lengkap (Kartun Panggung)

Starter resmi: **`assets/starter-cartoon-stage.html`** · Diagram visual: **`references/visuals/stage-anatomy.svg`** & **`references/visuals/stage-poses-expressions.svg`** · Studi kasus: **`examples/case-cartoon-stage.md`**.
Cocok untuk explainer edukasi sains/sosial berkarakter, cerita proses & dampak, dan penjelasan visual tanpa caption.

**Definisi:** Setiap adegan adalah *panggung* vektor tersendiri seukuran frame (`1920×1080` + overscan), hidup penuh (**≥10 benda bergerak**), dengan kamera masing-masing. Kamera hanya bergerak **di dalam** panggung (`zoom 1,08–1,95`, tidak pernah wide). Antar panggung memakai **cut** langsung atau **geser 96 px horizontal / 72 px vertikal** dengan blur ≤ 10 px. VO membawa narasi; gambar membawa makna; **nol caption**.

---

## 7 Hukum Cartoon Stage

### Hukum 1 — Satu Panggung per Adegan, Kamera Tidak Pernah Wide
- Rentang zoom wajib: **`1,08` sampai `1,95`**. Tidak ada satu pun frame di bawah `1,08`.
- Karakter di close-up mengisi ~55–65% tinggi frame; di framing paling lebar tetap ~35%.
- **Larangan miniatur:** Jangan pernah membangun satu peta raksasa (mis. 20.000 px) yang dijelajahi kamera pada zoom `0,3–0,7` sehingga rumah tampak sebesar ibu jari dan separuh layar kosong. Pecah dunia menjadi panggung-panggung seukuran frame.
- **Pola kamera per panggung:** `tahan 0,45–0,7 dtk → satu gerak bermakna per beat VO → tahan`.
- **Zoom logaritmik wajib:** `cam()` di starter men-tween `lz = Math.log(z)` lalu menerapkan `Math.exp(lz)` agar laju pembesaran terasa rata secara optik.

### Hukum 2 — Kontinu di Dalam Panggung, Cut/Geser di Antaranya
- Pergantian kalimat VO ≠ wajib ganti panggung. Selama hubungan ruang/sebab-akibatnya masih membantu penonton, tetap di panggung yang sama dan ubah **aksinya** (dinding terangkat membuka mesin, lampu menyala berurutan, jarum naik, tanaman tumbuh).
- Pindah panggung hanya saat lokasi, skala, atau cara menjelaskan berganti (video 40 dtk ≈ 4–5 panggung; 80 dtk ≈ 6–8 panggung).
- Transisi geser (`slide()`) dipakai hanya pada perpindahan lokasi nyata (~25–35% pergantian), sisanya **cut langsung**:
  - Keluar `0,28 dtk`, offset `-d` (`power2.in`), blur `0 → 10 px`.
  - Cut di tengah.
  - Masuk `0,42 dtk`, offset `+d → 0` (`power3.out`), blur `10 → 0 px`.
  - `d = 96 px` horizontal atau `72 px` vertikal, tanpa scale bump dan tanpa garis sapuan.

### Hukum 3 — Tiap Panggung ≥ 10 Benda yang Bergerak
Bukan hiasan kosong — setiap gerak menjelaskan tempat, proses, atau dampak:
- Kipas berputar (`spin()`), jarum meter bergoyang, lampu status berkedip deterministik, tetes/aliran mengikuti jalur pipa parametrik (`flowAlong()`), asap/uap naik (`drift()`), jemuran & daun bergoyang (`sway()`), hewan kecil berjalan, tanaman tumbuh/layu, retakan menjalar (`stroke-dashoffset`), muka air turun.
- **Aturan partikel (`drift()`):** Satu tween gerak + satu tween opacity berkeyframe (`[0, peak, peak, 0]`), keduanya `repeat: -1` dan `immediateRender: false`. Jangan memasang dua tween opacity tak-hingga terpisah pada elemen yang sama.

### Hukum 4 — Rig Sendi Hierarkis & Prinsip Animasi
Hierarki lokal `character()` (telapak kaki di `y = 0`):
```text
place (penempatan x, ground, scale — JANGAN di-tween GSAP langsung)
└── root (J: posisi gerak x/y, facing sx: 1 | -1)
    ├── R/L thigh  (tx: ±26, ty: -214) → shin (0, 102) → foot (0, 96)
    └── torso      (tx: 0, ty: -214)
        ├── Rarm   (tx: 50, ty: -126, di belakang badan) → fore (0, 78) → hand (0, 74) → RhandSlot
        ├── Larm   (tx: -50, ty: -126, di depan badan)   → fore (0, 78) → hand (0, 74) → LhandSlot
        └── head   (tx: 0, ty: -160)
```
- **Arah hadap (`facing`):** Gunakan `facing: 1` (menghadap kanan) atau `facing: -1` (menghadap kiri) — mengubah `root.sx` secara bersih tanpa membalik koordinat dunia.
- **Prop tangan (`RhandSlot` / `LhandSlot`):** Gambar ponsel, gelas, obor, atau kertas langsung di dalam `ch.RhandSlot` atau `ch.LhandSlot` agar otomatis mengikuti ayunan bahu dan siku.
- **Prinsip Animasi Wajib:**
  - *Anticipation:* Sebelum menunjuk, melompat (`jump()`), atau mengangkat beban, beri gerak ancang-ancang berlawanan arah 0,12–0,18 dtk.
  - *Secondary Action:* Bayangan kaki (`ch.sdJ`) ikut bergeser saat berjalan dan mengecil saat melompat; kepala sedikit mendahului atau mengikuti arah pandang tangan.
  - *Idle hidup:* Badan `±1,1°`, kepala `±1,7°`, lengan `±1,5°`, kedip tiap ~3,5 dtk selama `0,13 dtk` (offset unik per tokoh agar tidak serempak).

### Hukum 5 — Transform Manual Aditif (`J()`); GSAP Tidak Boleh Menyentuh Sendi
Dua jebakan fatal SVG:
1. `transformOrigin` GSAP pada `<g>` SVG bergeser saat bounding box anak berubah → bahu, leher, atau baling-baling tampak lepas.
2. Tween `x`/`y` GSAP pada grup ber-`translate(...)` menimpa posisi penempatan awalnya.

**Solusi di starter v2.0 (`J()` aditif):**
Setiap sendi memiliki kanal aksi (`x, y, r`) dan kanal idle (`xIdle, yIdle, rIdle`) yang dijumlahkan:
```js
translate(j.tx + j.x + j.xIdle, j.ty + j.y + j.yIdle) rotate(j.r + j.rIdle) scale(j.sx, j.sy)
```
Akibatnya `idle()` (menganimasikan `rIdle`/`yIdle`) **tidak pernah bentrok** dengan `rot()`, `point()`, `wave()`, atau `walk()` (yang menganimasikan `r`/`x`/`y`). Selalu inisialisasi kamera panggung memakai `setCam(sc, at, x, y, z)` di dalam timeline `tl` agar scrub mundur-maju tetap 100% deterministik.

### Hukum 6 — Nol Caption, Teks Hanya Diegetik
Tidak ada subtitle atau kalimat overlay di layar. Teks hanya muncul sebagai bagian alami dari dunia: papan nama gedung, plat mesin, label tabung `"O2"`, layar ponsel, atau rambu. Jika penonton butuh teks penjelasan untuk paham, perkuat aksi visual di panggungnya.

### Hukum 7 — Palet Cerah Flat Tanpa Tekstur
Gunakan flat fill bersih tanpa grain, tanpa tekstur kertas, tanpa vignette berat, dan tanpa outline tebal. Kedalaman dibangun lewat:
- **3 Bidang Kedalaman (FG / MG / BG):** Langit & bukit pucat di belakang (BG), tokoh & bangunan aktif di tengah (MG), elemen kontras lembut di tepi depan bila perlu (FG).
- **Bayangan sederhana:** Elips kontak di tanah (`opacity: 0.13`) dan sisi gelap objek `0,86–0,90×` warna dasar via helper `sh(hex, f)`.

---

## Staging, Blocking, Skala `1 H`, & Perspektif 2,5D

*(Rujuk **`references/visuals/shared-scale-perspective.svg`** untuk tangga proporsi dan 4 camera angles.)*

- **Baseline Skala Semi-Realistis (`1 H = 320–340 px`):** Gunakan `scale: 0,62 – 0,68` pada `character()` untuk panggung rumah/jalan (`1920×1080`) agar **pintu rumah (`1,24 H ≈ 400 px`) lebih tinggi dari tokoh**, **mobil sedan (`0,87 H ≈ 280 px`) sejajar dada/bahu**, dan **dinding rumah 1 lantai (`1,95 H ≈ 630 px`)** tetap proporsional sambil menjaga tokoh 36–56% tinggi frame saat `zoom 1,15–1,68×`.
- **Architectural Cropping & Low-Angle:** Untuk gedung bertingkat, menara, atau gardu besar, jangan mengecilkan bangunannya menjadi rumah boneka — gunakan **Low-Angle** (cakrawala rendah `y ≈ 780–880`) + *tilt* kamera vertikal `cam()` untuk menonjolkan skala masifnya.
- **Volume 2,5D & Kedalaman Ruang:** Beri sisi samping teredup (`sh(hex, 0.84–0.88)`) pada bangunan/kendaraan, bidang trotoar/jalan berperspektif trapesium menuju garis cakrawala, serta elemen **FG** besar-gelap di tepi sudut bawah/atas untuk ilusi kedalaman instan.
- **Rule of Thirds & Lead Room:** Posisikan tokoh di sepertiga kiri (`x ≈ 580–760`) saat menghadap kanan ke objek interaksi di kanan (`x ≈ 1150–1450`), atau sebaliknya.
- **180-Degree Rule:** Jika tokoh berjalan ke kanan untuk memasuki gedung di Panggung 1, munculkan ia masuk dari kiri bergerak ke kanan di Panggung 2.
- **Proporsi Panggung Penampang Tanah:** Gunakan skala karakter `0,58 – 0,65` dan tinggi lapisan tanah `≤ 420 px` agar tokoh + permukaan + penampang bawah tanah sama-sama proporsional pada `zoom ≥ 1,08`.
- **Kepala Karakter:** Lihat `references/asset-consistency.md` untuk *Character Bible* dan *Style Lock Prompt* bila memakai kepala raster (`HEADS`), atau gunakan kepala vektor bawaan `character()`.

---

## Checklist QC Khusus Cartoon Stage

- [ ] Jalankan `?debug=1` → klik **Audit**: nol peringatan `zoom < 1.08` di seluruh panggung (uji pada awal, tengah, dan akhir tiap panggung)
- [ ] Scrub mundur-maju melewati setiap `cut()` dan `slide()` — kamera dan `display` panggung kembali persis sama
- [ ] Tiap panggung memiliki **≥ 10 elemen bergerak** yang bermakna (bukan dekorasi acak)
- [ ] Proporsi semi-realistis `1 H` konsisten (`F-07` lolos): pintu `> tinggi tokoh`, atap mobil sedan sejajar dada/bahu, sudut kamera & garis cakrawala selaras, ada volume 2,5D + 3 bidang kedalaman (BG/MG/FG)
- [ ] Semua rotasi sendi memakai `J()` (`rot`, `sway`, `spin`, `point`, `wave`, `walk`, `jump`) — nol `transformOrigin`/`rotation` GSAP langsung pada sendi
- [ ] `idle()` aktif pada tokoh tanpa bentrok dengan aksi lengan/kepala
- [ ] Nol caption/kalimat overlay; teks hanya diegetik dengan ukuran ≥ 34 px
- [ ] Palet flat cerah tanpa tekstur kertas/grain; semua tokoh memiliki bayangan kaki (`ch.sdJ`) yang ikut bergerak
