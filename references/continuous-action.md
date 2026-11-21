# Continuous Action — Panduan Lengkap (Aksi Kontinu)

Starter resmi: **`assets/starter-continuous-action.html`** · Diagram visual: **`references/visuals/continuous-viewpoints-continuity.svg`** · Studi kasus: **`examples/case-continuous-action.md`**.
Cocok untuk explainer tentang kecepatan, rute, transportasi, olahraga, energi, manufaktur, atau alur proses di mana fakta disampaikan **tanpa menghentikan aksinya**. Dapat dibangun 100% dengan SVG vektor prosedural di JS tanpa bergantung pada gambar generate eksternal. **Larangan kritis (`F-03` — `examples/good-vs-bad.md`):** Jangan menempelkan mini-map atau gauge speedometer mengambang di atas `#side`; setiap sudut pandang wajib mengisi 100% frame lewat `whip()`.

---

## 7 Prinsip Continuous Action

1. **Satu Subjek Utama dari Detik 0 sampai Akhir.** Pahlawan visual (kereta, kendaraan, atlet, kurir, paket, aliran elektron) tidak pernah hilang dari cerita. Yang berubah adalah dunia, kecepatan, dan sudut pandang di sekitarnya.
2. **Dunia Selalu Mengalir (`VK → D(t)`).** Semua strip parallax, roda berputar, garis kecepatan, dan objek dunia adalah fungsi murni dari jarak terintegrasi `D(t)`. Saat angka penting sedang dibaca, perlambat kecepatan ke ~35–45% — jangan bekukan dunia menjadi slide diam.
3. **Angka adalah Properti Panggung, Bukan Overlay.** Letakkan angka besar di lapisan `#deepObjs` (parallax `0,75–0,85`), lalu biarkan subjek melintas **di depannya** dan menyapunya keluar. Untuk angka yang berubah naik/turun, pakai `odometer()` bergulir deterministik — bukan fade in/out.
4. **Sudut Pandang WAJIB Berganti.** Tidak boleh ada 3 adegan berturut-turut dengan sudut yang sama. Tiap fakta baru idealnya mendapat instrumen visualnya sendiri (lihat Katalog 7 Sudut Pandang di bawah).
5. **Perubahan Palet = Perubahan Tempo.** Saat cerita memuncak (kecepatan tinggi, tanjakan ekstrem, bahaya) atau melandai, transisikan CSS variables (`--sky`, `--ink`, `--accent`) secara mulus lewat `paletteAt(t)` mengikuti waktu.
6. **Perbandingan Tetap di Dalam Dunia yang Sama.** Hindari membelah layar menjadi panel kiri-kanan (terasa template PPT). Tampilkan perbandingan lewat balapan beberapa penanda di peta dengan satu jam berjalan, atau objek pembanding melintas di jalur paralel.
7. **Maksimum Dua Tingkat Teks.** Hanya angka besar + satu label dunia. Tanpa subtitle, tanpa paragraf keterangan, tanpa tumpukan badge.

---

## Katalog 7 Sudut Pandang & Instrumen

Gunakan variasi sudut ini untuk menjaga ritme tetap segar sepanjang 30–90 detik:

| Jenis Fakta | Sudut / Instrumen | Elemen Kunci Deterministik |
|---|---|---|
| **Pembuka & Penutup** | **Samping (`#side`)** *(jangkar visual)* | 3 lapis strip parallax `D(t)`, garis kecepatan, hero bergoyang halus + roda berputar |
| **Kecepatan / Tenaga** | **Kabin / Speedometer (`#cabin`)** | Jarum speedometer menyapu sesuai `vAt(t)`, angka digital `tabular-nums`, garis lintasan mengalir di kaca depan |
| **Rute / Jarak / Titik Henti** | **Simulasi Peta (`#map`)** | Path rute tergambar (`stroke-dasharray`), penanda melaju via `getPointAtLength` & diputar mengikuti tangen, titik stasiun pop, kamera zoom awal `2,3×` → seluruh rute |
| **Medan / Tanjakan / Terowongan** | **Profil Ketinggian** | Grafik kontur tanah, rute memotong bukit, terowongan berongga disorot lalu di-zoom |
| **Perbandingan Waktu / Efisiensi** | **Balapan di Peta** | 2–3 penanda berangkat bersamaan di jalur masing-masing dengan 1 jam tempuh berjalan |
| **Kapasitas / Jumlah Unit** | **Interior Perspektif** | Lorong kabin/pabrik perspektif, deretan unit/kursi menyala berurutan, jendela menampilkan latar mengalir |
| **Skala / Momen Klimaks** | **Tampak Depan (Head-On)** | Subjek mendekat dari titik hilang (`scale` dari pusat lokal) hingga memenuhi frame |

**Contoh urutan ritme 60 detik:** `samping → kabin → peta → profil ketinggian → balapan peta → tampak depan → samping`.

---

## Kontinuitas Visual, Skala `1 H`, & Perspektif Parallax

*(Rujuk **`references/visuals/shared-scale-perspective.svg`** untuk acuan skala dan sudut kamera.)*

Agar penonton langsung mengenali bahwa seluruh sudut pandang masih menceritakan **subjek yang sama** dengan skala lingkungan yang meyakinkan:
- **Kunci 3 Penanda Identitas Subjek:** (1) Warna bodi + garis aksen identik (`var(--accent)`), (2) Siluet khas (bentuk moncong, helm, atau bodi), (3) Ikon penanda peta memakai warna dan bentuk yang diturunkan dari subjek.
- **Skala Subjek vs Lingkungan (`1 H`):** Sesuaikan ukuran tiang listrik, rambu, pohon, dan jembatan di `#hills`/`#ground`/`#frontObjs` terhadap ukuran subjek sebenarnya:
  - *Kereta / Truk (`~3,2–4,0 m`):* Tinggi bodi `140–170 px`; tiang listrik/lampu ~`1,8×` tinggi bodi.
  - *Mobil Sedan / Van (`~1,5–2,0 m`):* Tinggi bodi `115–145 px` (panjang `2,6–2,85×` tinggi); pohon/tiang ~`3,0–3,5×` tinggi mobil.
  - *Pelari / Pesepeda (`~1,7 m`):* Tinggi figur `180–230 px`; bangku taman/pagar setinggi pinggang (`0,5 H`).
- **Rasio Kecepatan Parallax Berbasis Jarak Nyata:** Urutan faktor `f` pada `strip()` wajib meningkat konsisten dari belakang ke depan agar ilusi kedalaman terasa optik nyata:
  `Awan/langit (0,06)` → `Gunung jauh (0,16)` → `Bukit/kota (0,36)` → `Ladang/gedung menengah (0,64)` → `Jalur/viaduk (0,95–1,00)` → `Tiang/semak FG dekat lensa (1,35–1,60)`.
- **Perspektif 1-Titik & Low-Angle:** Di `#cabin` dan *Tampak Depan*, gunakan kurva percepatan kuadratik (`p = q*q`) dari titik hilang ke tepi bawah layar; pada momen klimaks kecepatan, turunkan garis cakrawala (Low-Angle) dan tambahkan *banking* tikungan `6–12°` pada `#sideCam`.
- **Konsistensi Arah Gerak:** Di tampilan samping, subjek selalu menghadap dan melaju dari **kiri ke kanan** (dunia mengalir kanan → kiri). Di tampilan peta, usahakan jalur utama juga mengalir dominan kiri → kanan atau barat → timur agar selaras dengan arah layar.

---

## Resep Teknis & Jebakan SVG

- **Profil Kecepatan `VK`:** Definisikan `VK = [[t0, v0], [t1, v1], ...]`. Tabel jarak `DA[]` dihitung sekali saat load (step `1/240 dtk`), sehingga `D(t)` 100% deterministik dan sangat cepat saat scrub.
- **Objek Dunia (`wobj`):** Selalu tentukan `t0` (masuk) dan `t1` (dibuang) agar jembatan, papan angka, atau portal terowongan yang sudah dilewati tidak muncul lagi secara keliru.
- **Jebakan `visibility` SVG:** Jangan pernah menulis `setAttribute('visibility', 'visible')` pada elemen anak di dalam `<div class="view">`. Atribut anak `'visible'` menembus `visibility: hidden` milik view induk! Untuk menampilkan elemen anak, **hapus atributnya** dengan `removeAttribute('visibility')`.
- **Whip Antar View (`whip()`):** Jangan menggunakan `className` GSAP 3. Gunakan `autoAlpha` / `visibility` + translasi `y` + blur searah (`0,28 dtk` keluar + `0,36 dtk` masuk) agar scrub mundur-maju tetap bersih tanpa view bertumpuk.
- **Transform Lokal SVG:** Untuk grup yang diskalakan/diputar dari pusat lokalnya (subjek miring, roda berputar, jarum speedometer), tulis atribut `transform` langsung (`translate(x y) rotate(deg)`), jangan `gsap.set` dengan `transformOrigin`.
- **Kamera Samping Bernapas (`CAMK`):** Terapkan keyframe `[t, zoom, cx, cy]` + osilasi halus `±0,6%` hanya pada `<g id="sideCam">`, bukan pada teks UI `#ui` di luar kamera.

---

## Checklist QC Khusus Continuous Action

- [ ] Jalankan `?debug=1` → klik **Audit**: nol teks `< 30 px` dan nol konflik view
- [ ] Scrub mundur-maju melewati setiap `whip()` — tidak ada dua `<div class="view">` tampil bersamaan atau objek view lama tembus ke view baru
- [ ] Subjek utama konsisten warna, siluet, arah gerak, dan proporsi terhadap tiang/pohon/jembatan (`F-04` & `F-07` lolos)
- [ ] Tidak ada 3 adegan berturut-turut dengan sudut pandang yang sama (minimal memakai 3 sudut/instrumen berbeda pada video ≥ 45 detik)
- [ ] Dunia tetap mengalir (kecepatan > 0 atau melambat ~40%) saat angka/fakta ditampilkan — tidak ada detik yang beku total
- [ ] Angka yang berubah (speedometer, odometer, jarak) memakai `font-variant-numeric: tabular-nums` dan muncul **setelah** zoom-out peta/kamera selesai
