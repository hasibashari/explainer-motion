# Anti-PPT — Hukum Desain Adegan & Kalibrasi Visual

Dokumen ini memastikan hasil explainer (**Cartoon Stage**, **Continuous Action**, dan **Cartoon Collage**) benar-benar bergerak seperti motion graphic — bukan slide presentasi yang diberi efek masuk-keluar.

## Kenapa Animasi Web Sering Jatuh ke Pola Slide

Model terbiasa menyusun informasi seperti dokumen: judul → sub-judul → daftar poin → kotak pendukung. Struktur itu bagus untuk slide, tetapi mematikan ritme video. Dalam explainer motion graphic, **dunia dan aksi visual** adalah pembawa cerita utama; teks (bila dipakai) hanya menjadi penegas.

> **Referensi Visual & Contoh:** Lihat pasangan visual **`references/visuals/shared-good-vs-bad.svg`**, lembar skala & sudut kamera **`references/visuals/shared-scale-perspective.svg`**, bedah kontrastif **`examples/good-vs-bad.md`**, dan studi perbaikan **`examples/before-after.md`**.

## Tabel Pelanggaran (`F-01..F-07`) → Perbaikan → Pemeriksaan `?debug=1`

| Kode | Pola Gagal (Terasa PPT) | Kenapa Gagal | Perbaikan Wajib | Cara Cek di `?debug=1` |
|---|---|---|---|---|
| **F-01** | `<section class="scene">` dinyalakan-dimatikan bergantian dengan `autoAlpha`/`opacity` | Ganti section = ganti slide, apa pun efeknya | Dunia mengalir + `whip()` (**Continuous Action**), kertas disusuri kamera + `into/settle` (**Cartoon Collage**), atau panggung hidup penuh + `cut/slide` (**Cartoon Stage**) | Scrub melewati tiap transisi — pastikan tidak ada cross-fade section diam |
| **F-01** | Eyebrow/kicker + judul 2 baris + paragraf + badge | 3+ tingkat teks = layout slide | Maksimum 2 tingkat teks (angka/kalimat utama + label dunia); untuk **Cartoon Stage**, **nol caption** | Hitung tingkat teks per layar |
| **F-01** | 1 gambar diam + 1 kalimat per adegan, lalu ganti | Tetap slideshow walau kamera bergerak | **Cartoon Collage**: akresi 3–5 elemen per frasa; **Cartoon Stage**: ≥10 benda bergerak per panggung | Cek jumlah elemen aktif pada akhir tiap adegan |
| **F-01** | Peta raksasa dengan kamera zoom `< 1,08` | Rumah/tokoh tampak miniatur, separuh layar kosong | Pecah jadi panggung seukuran frame, zoom selalu `1,08–1,95` (**Cartoon Stage**) | Lihat indikator `zoom` di panel `?debug=1` & klik **Audit** (merah bila `< 1,08`) |
| **F-01** | Hanya Ken Burns (scale `1.0 → 1.05`) di semua adegan | Gerak repetitif = template editor | Parallax `D(t)`, instrumen berganti, koreografi `into/look/home`, atau aksi sendi + *anticipation* | Scrub beberapa detik acak; cek apakah aksi dunia nyata berlangsung |
| **F-02** | `transformOrigin`/`rotation` GSAP langsung pada sendi SVG atau kaki melayang | Bbox berubah → bahu/kepala tampak lepas | Pakai proxy `J()` aditif (`r + rIdle`) + bayangan kaki `ch.sdJ` di **Cartoon Stage** | Scrub di tengah gerakan tangan/jalan — cek sambungan sendi & kontak tanah |
| **F-03** | Mini-map inset, gauge mengambang, atau layar dibelah panel kiri-kanan | Terasa dashboard/template dan merusak aliran penuh | Setiap sudut pandang 100% full-frame via `whip()`; perbandingan lewat balapan peta (**Continuous Action**) | Pastikan nol kartu inset di atas `#side` dan tidak ada layout split-card |
| **F-04** | Subjek terus dari samping, arah berbalik, atau latar beku total saat baca | Monoton atau membingungkan arah gerak | Ganti sudut pandang (tidak 3 berurutan sama); jaga arah kiri→kanan; perlambat ke ~40% saat baca | Periksa label `view` aktif & pastikan dunia tetap bergerak selama fase baca |
| **F-05** | Drop-shadow hilang pasca-`fly()` atau teks `say()` muncul saat kamera masih zoom out | Kolase tampak datar & teks terpotong | Pisahkan `.cut` (blur) & `.cut-body` (drop-shadow); selesaikan `home()` dulu minimal `0,1 dtk`, baru `say()` (**Cartoon Collage**) | Klik **Audit** & scrub ke waktu mulai `say()` — pastikan `z = 1.0` |
| **F-06** | Teks terlalu kecil (`< 30 px`), area kolase bocor, atau terpotong tepi 9:16 | Tidak terbaca di ponsel / visual tabrakan | Jaga semua teks ≥ 30 px; pakai `.safe` pada 9:16; `dx = 2600 px` + `sizeWorld()` di Collage | Klik **Audit** & aktifkan overlay **Safe 9:16** di `?debug=1` |
| **F-07** | Proporsi tidak masuk akal (pintu lebih pendek dari tokoh, mobil seperti mainan, gedung diperkecil) atau panggung 2D datar tanpa sudut kamera/cakrawala | Dunia terasa tempelan mainan dan kehilangan kedalaman ruang serta wibawa skala | Terapkan jangkar skala semi-realistis **`1 H`** (`explainer-core.md` §4B), pilih 1 dari 4 *camera angle* & garis cakrawala, beri sisi 2.5D + *Architectural Cropping* + bingkai FG (`shared-scale-perspective.svg`) | Scrub ke shot lebar tiap panggung — cek tinggi pintu `≥ 1,2 H`, mobil `~0,85–0,95 H`, garis cakrawala & kedalaman BG/MG/FG |

## 5 Prinsip Inti yang Selalu Berlaku

1. **Satu dunia yang terhubung.** Baik lewat kamera menyusuri kertas kolase, pahlawan yang terus melaju melintasi dunia, maupun rangkaian panggung karakter — penonton selalu punya jangkar visual.
2. **Hierarki atensi bertahap.** Jangan memunculkan semua elemen sekaligus; munculkan berurutan mengikuti ketukan frasa atau sebab-akibat aksi.
3. **Kamera adalah pemersatu.** Saat kamera bergerak, seluruh dunia ikut bergerak — jangan hanya men-zoom teks sementara latar diam.
4. **Ritme masuk-keluar asimetris + anticipation.** Masuk tegas terkontrol (`power4.out` / `back.out`), keluar cepat (`power2.in`); beri ancang-ancang kecil sebelum aksi besar.
5. **Teks ringkas dan kontras.** Jika klaim butuh lebih dari satu kalimat pendek, ubah sebagiannya menjadi aksi visual, angka di dunia, atau peta/diagram.
