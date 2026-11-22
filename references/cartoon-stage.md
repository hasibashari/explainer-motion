# Cartoon Stage — Panduan Lengkap (Kartun Panggung)

Starter resmi: **`assets/starter-cartoon-stage.html`** · Diagram visual: **`references/visuals/stage-anatomy.svg`**, **`references/visuals/stage-poses-expressions.svg`**, & **`references/visuals/stage-character-details.svg`** · Studi kasus: **`examples/case-cartoon-stage.md`**.
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

### Hukum 4 — Rig Sendi Hierarkis Modern (~5 Kepala), 6 Ekspresi Kanonik, & Prinsip Animasi
Hierarki lokal `character()` (proporsi modern ~5 tinggi kepala, telapak sepatu tepat di `y = 0`, tinggi unscaled `~570 px` → `scale: 0.60 ≈ 1 H`):
```text
place (penempatan x, ground, scale — JANGAN di-tween GSAP langsung)
└── root (J: posisi gerak x/y, facing sx: 1 | -1, squash/stretch sy; pelvis jembatan membulat)
    ├── R/L thigh  (tx: ±20, ty: -274, rounded caps) → shin (0, 124 + cuff) → foot (0, 126, sol tepat y = 0)
    └── torso      (tx: 0, ty: -270, layering jacket/shirt/tee)
        ├── Rarm   (tx: 44, ty: -140, di belakang badan) → fore (0, 92) → hand (0, 86) → RhandSlot
        ├── Larm   (tx: -44, ty: -140, di depan badan)   → fore (0, 92) → hand (0, 86) → LhandSlot
        └── head   (tx: 0, ty: -172) → browL/browR (J), eyes (J + highlight + 3 mode), mouths (6 bentuk)
```
- **4 Sudut Turnaround (`pose`), Pakaian Berlapis (`outfit`), Gaya Rambut (`hairStyle`), & Arah Hadap (`facing`):** Pilih `pose: '3/4'` *(default organik dengan rahang tirus asimetris)*, `'front'`, `'side'`, atau `'back'` *(tampak belakang penuh)*; pilih `outfit: 'jacket' | 'shirt' | 'tee'` *(default `'jacket'` berlapis kaus dalam + saku + lipatan kain)*; pilih `hairStyle: 'short' | 'bob' | 'bun' | 'curly' | 'hijab' | 'cap'` yang dibangun berlapis (`backHair` → kepala/telinga berlipatan dalam → `frontHair` ber-tuft organik & *highlight* aman di dalam mahkota); serta `facing: 1` (kanan) atau `-1` (kiri).
- **9 Gestur Tangan Siluet Bezier Tunggal & Z-Order 3 Lapis (`setHand` / `handSlot`):** Rujuk `references/visuals/stage-character-details.svg`. Semua pose tangan digambar sebagai **satu siluet `<path>` Bezier organik menyatu + garis lipatan tipis** (hindari susunan `<rect>` kaku) dengan sumbu **`+Y` = ujung jari** dan **`+X` = sisi ibu jari**. Gunakan `setHand(ch, 'R'|'L', 'relaxed'|'open'|'point'|'grip'|'carry'|'pinch'|'fist'|'thumbs'|'present', t)` *(default `'relaxed'` saat berdiri/berjalan)*. Aturan orientasi anatomis memegang HP/prop:
  - Gunakan **`carry`** (punggung tangan di `handFront` menghadap kamera dengan 4 jari melingkari tepi bawah dan ibu jari menjepit tepi atas, sisi belakang HP bermodul kamera di `handSlot` berorientasi **miring-horizontal ~+14°**) saat **lengan menggantung santai di samping pinggul** agar genggaman tampak wajar dan pergelangan tidak terputar 180°.
  - Gunakan **`grip`** (telapak di `handBack`, layar HP/gelas di `handSlot`, 4 ruas jari + ibu jari di `handFront`) khusus saat **siku ditekuk/diangkat ke depan dada** untuk melihat atau menunjukkan layar HP maupun memegang gelas.
- **6 Ekspresi Wajah Kanonik (`face(ch, e, at)`):** Sinkron 1:1 dengan `references/visuals/stage-poses-expressions.svg` (menggerakkan alis, mode mata berkilau pupil, dan mulut termasuk senyum lebar bergigi putih + lidah lembut): `'netral'`, `'senang'`, `'kaget'`, `'berpikir'`, `'khawatir'` *(alias `'cemas'`)*, dan `'yakin'`.
- **Prinsip Animasi Wajib:**
  - *Anticipation & Squash/Stretch:* `point()` memberi ancang-ancang berlawanan `0,14 dtk`; `jump()` menekuk lutut + squash di `y = 0` (`0,14 dtk`, tanpa kaki menembus tanah) lalu melayang dengan lengan terangkat dan bayangan mengecil; `walk()` menyinkronkan jumlah/amplitudo langkah terhadap `dx` dan mengayunkan kedua lengan berlawanan.
  - *Body Mechanics Tambahan (`bend`, `turn`, Keseimbangan, & Follow-Through):* Gunakan `bend(ch, at, deg, dur)` saat membungkuk/mengambil objek (lutut & pinggul mengompensasi pusat gravitasi agar telapak kaki tetap terkunci di `y = 0`) dan `turn(ch, at, dir)` untuk berbalik arah hadap dengan squash halus. Beri *follow-through* kepala atau lengan `0,06–0,12 dtk` setelah torso berhenti agar gerak tidak kaku.
  - *Interaksi Tokoh & Objek:* Jaga jarak jangkauan lengan efektif sekitar `0,45–0,55 H` dari pusat tokoh ke objek interaksi (atau pasang prop langsung di `RhandSlot`/`LhandSlot`). Untuk dialog 2 tokoh, sejajarkan garis pandang (*eye-line matching*) dan hadapkan berlawanan (`facing: 1` vs `-1`).
  - *Secondary Action:* Bayangan kaki (`ch.sdJ`) ikut bergeser saat `walk()` dan mengecil saat `jump()`; kepala mengikuti arah pandang tangan.
  - *Idle hidup:* Badan `±1,1°`, kepala `±1,7°`, lengan `±1,5°`, kedip tiap ~3,5 dtk selama `0,13 dtk` (offset unik per tokoh agar tidak serempak).

### Hukum 5 — Transform Manual Aditif (`J()`); GSAP Tidak Boleh Menyentuh Sendi
Dua jebakan fatal SVG:
1. `transformOrigin` GSAP pada `<g>` SVG bergeser saat bounding box anak berubah → bahu, leher, atau baling-baling tampak lepas.
2. Tween `x`/`y` GSAP pada grup ber-`translate(...)` menimpa posisi penempatan awalnya.

**Solusi di starter v2.1 (`J()` aditif):**
Setiap sendi memiliki kanal aksi (`x, y, r`) dan kanal idle (`xIdle, yIdle, rIdle`) yang dijumlahkan:
```js
translate(j.tx + j.x + j.xIdle, j.ty + j.y + j.yIdle) rotate(j.r + j.rIdle) scale(j.sx, j.sy)
```
Akibatnya `idle()` (menganimasikan `rIdle`/`yIdle`) **tidak pernah bentrok** dengan `rot()`, `point()`, `wave()`, `walk()`, `bend()`, atau `turn()` (yang menganimasikan `r`/`x`/`y`/`sx`). Selalu inisialisasi kamera panggung memakai `setCam(sc, at, x, y, z)` di dalam timeline `tl` agar scrub mundur-maju tetap 100% deterministik.

### Hukum 6 — Nol Caption, Teks Hanya Diegetik
Tidak ada subtitle atau kalimat overlay di layar. Teks hanya muncul sebagai bagian alami dari dunia: papan nama gedung, plat mesin, label tabung `"O2"`, layar ponsel, atau rambu. Jika penonton butuh teks penjelasan untuk paham, perkuat aksi visual di panggungnya.

### Hukum 7 — Palet Terarah Flat Tanpa Tekstur (*70–20–10* & *2-Tone Split*)
Gunakan flat fill bersih tanpa grain, tanpa tekstur kertas, tanpa vignette berat, dan tanpa outline tebal. Kedalaman & fokus dibangun lewat:
- **Color Script 70–20–10:** Kunci ~70% luas latar pada keluarga warna analogus berkontras rendah agar tokoh dan titik aksi (~10% warna kontras/suhu berlawanan) langsung menonjol tanpa bantuan outline.
- **3 Bidang Kedalaman (FG / MG / BG):** Langit & siluet jauh pucat tanpa detail di belakang (BG), tokoh & bangunan aktif tajam di tengah (MG), elemen *edge anchor* gelap-lembut di tepi depan (FG).
- **Arah Cahaya Konsisten (*Light Direction Anchor*) & *2-Tone Split Shading*:** Tetapkan 1 arah cahaya per panggung untuk menyelaraskan elips kontak di tanah (`opacity: 0,13–0,16`), bayangan dagu di leher, sisi gelap 2,5D (`sh(hex, 0.84–0.90)`), serta *volumetric light beam* (poligon cahaya diagonal semi-transparan dari jendela/lampu) bila adegan interior atau sore.

---

## Staging, Blocking, Environment Craft, & Perspektif 2,5D

*(Rujuk **`references/visuals/shared-scale-perspective.svg`** untuk tangga proporsi dan 4 camera angles.)*

- **Baseline Skala Semi-Realistis (`1 H ≈ 330–345 px`):** Gunakan `baseScale: 0,58 – 0,64` (default `0,60`) pada `character()` untuk panggung rumah/jalan (`1920×1080`) agar **pintu rumah (`1,24 H ≈ 400–420 px`) lebih tinggi dari tokoh**, **mobil sedan (`0,87 H ≈ 280–295 px`) sejajar dada/bahu**, dan **dinding rumah 1 lantai (`1,95 H ≈ 630–660 px`)** tetap proporsional sambil menjaga tokoh 36–56% tinggi frame saat `zoom 1,15–1,68×`.
- **Resep Environment Adaptif (pilih sesuai jenis lokasi — jangan memakai satu templat latar datar untuk semua topik):**
  - *Interior (Window Depth + Edge Anchors):* Gunakan bukaan jendela yang memperlihatkan siluet luar berkontras rendah, kunci tepi kiri/kanan frame dengan properti parsial (pot tanaman, ujung meja, rak), dan tambahkan *light beam* jendela diagonal bila relevan dengan suasana.
  - *Eksterior (S-Curve & Progressive Rhythm):* Gunakan jalan/sungai berkelok (*S-curve*) atau bidang trapesium perspektif ke garis cakrawala, disertai pengulangan properti (pohon, tiang lampu, bangku) yang mengecil bertahap.
  - *Penampang Sains/Geologi (Faceted Cross-Section):* Bangun irisan lapisan tanah, es, atau air dengan potongan berfaset, garis patahan, dan sisi samping 2,5D — bukan balok persegi polos.
- **Asset Craft (*Micro-Geometry* & Anti-Primitif):** Ganti pohon 3-lingkaran dan kotak polos dengan tajuk asimetris bertingkat *2-tone* + 3–6 aksen *micro-geometry* bersih (garis pantulan kaca diagonal seragam, garis bata/paving parsial, lekuk kanopi). Pada karakter, pastikan sambungan bahu–pakaian menyatu (lengan baju, kerah, bayangan leher) dan utamakan keterbacaan siluet (*Silhouette-First*) serta sudut hadap yang wajar (`front` / `3/4` / `side`).
- **Architectural Cropping & Low-Angle:** Untuk gedung bertingkat, menara, atau gardu besar, jangan mengecilkan bangunannya menjadi rumah boneka — gunakan **Low-Angle** (cakrawala rendah `y ≈ 780–880`) + *tilt* kamera vertikal `cam()` untuk menonjolkan skala masifnya.
- **Rule of Thirds & Lead Room:** Posisikan tokoh di sepertiga kiri (`x ≈ 580–760`) saat menghadap kanan ke objek interaksi di kanan (`x ≈ 1150–1450`), atau sebaliknya.
- **180-Degree Rule:** Jika tokoh berjalan ke kanan untuk memasuki gedung di Panggung 1, munculkan ia masuk dari kiri bergerak ke kanan di Panggung 2.
- **Proporsi Panggung Penampang Tanah:** Gunakan skala karakter `0,58 – 0,62` dan tinggi lapisan tanah `≤ 420 px` agar tokoh + permukaan + penampang bawah tanah sama-sama proporsional pada `zoom ≥ 1,08`.
- **Adaptasi `9:16` (`1080×1920`):** Jangan memasang `.safe` langsung pada `sc.g` karena akan mengecilkan langit/tanah. Biarkan lapis BG (`skyOf`, `hillsOf`, `groundOf`, diperpanjang mengisi tinggi `1920 px` + overscan) tetap penuh `100%`, lalu tempatkan tokoh & objek interaksi di dalam area aman vertikal (`x: 110..860`, `ground y ≈ 1380–1480`, hindari margin atas `190 px`, bawah `230 px`, & strip UI kanan `160 px`) atau bungkus khusus grup konten MG dalam `<g class="safe">`. Rentang zoom tetap `1,08–1,65×`.
- **Kepala Karakter:** Lihat `references/asset-consistency.md` untuk *Character Bible* dan format `HEADS[who].src` (Base64 Data URI) bila memakai kepala raster, atau gunakan kepala vektor bawaan `character()`.

---

## Checklist QC Khusus Cartoon Stage

- [ ] Jalankan `?debug=1` → klik **Audit**: nol peringatan `zoom < 1.08` di seluruh panggung (uji pada awal, tengah, dan akhir tiap panggung)
- [ ] Scrub mundur-maju melewati setiap `cut()` dan `slide()` — kamera dan `display` panggung kembali persis sama
- [ ] Tiap panggung memiliki **≥ 10 elemen bergerak** yang bermakna (bukan dekorasi acak dan bukan tokoh berdiri di depan papan grafik statis)
- [ ] Proporsi semi-realistis `1 H` konsisten (`F-07` lolos): pintu `> tinggi tokoh`, atap mobil sedan sejajar dada/bahu, sudut kamera & garis cakrawala selaras, ada volume 2,5D + 3 bidang kedalaman (BG/MG/FG)
- [ ] *Color Script 70–20–10*, *Light Direction Anchor*, & *2-tone split shading* konsisten (latar analogus terkendali, sisi gelap/bayangan searah, aset tidak berupa primitif 1-warna datar)
- [ ] Semua rotasi sendi memakai `J()` (`rot`, `sway`, `spin`, `point`, `wave`, `walk`, `jump`, `bend`, `turn`) — nol `transformOrigin`/`rotation` GSAP langsung pada sendi; pose kunci terbaca dari siluetnya (*Silhouette-First*)
- [ ] `idle()` aktif pada tokoh tanpa bentrok dengan aksi lengan/kepala
- [ ] Nol caption/kalimat overlay; teks hanya diegetik dengan ukuran ≥ 34 px
- [ ] Palet flat bersih tanpa tekstur kertas/grain; semua tokoh memiliki bayangan kaki (`ch.sdJ`) yang ikut bergerak
