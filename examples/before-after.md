# Before & After — Studi Transformasi Perbaikan Draf Lemah → Matang

Gunakan panduan ini pada **Langkah 5 (QC & Perbaikan)** dan **Langkah 6 (Revisi User)** ketika hasil awal terasa kaku, terlalu mirip slide, terlalu ramai, atau gagal melewati audit `?debug=1`. Fokus pada **perbaikan struktural**, bukan sekadar mengganti warna atau mempercepat durasi.

---

## Transformasi 1 — Cartoon Stage: Dari "Slide Berkarakter" ke Panggung Hidup

### Skenario & Keluhan Awal (*Before*)
Topik: *Cara kerja kompos dapur menjadi pupuk tanaman.*  
Draf awal memiliki gejala:
- 3 `<section>` berganti via fade-out/fade-in tiap 8 detik.
- Ada teks judul besar di atas layar (*"Tahap 1: Sampah Organik"*) dan caption 2 baris di bawah.
- 1 tokoh berdiri di tengah layar kosong bersama 1 tempat sampah diam (hanya 2 benda bergerak: tangan tokoh + 1 daun jatuh).
- Kamera diam di `zoom = 1.0` sehingga tokoh tampak kecil dan layar terasa kosong (**Gagal F-01 & F-02**).

### 5 Perubahan Struktural yang Dilakukan
1. **Hapus 100% caption & judul overlay → ubah menjadi teks diegetik:** Pindahkan kata `"KOMPOS"` ke plat logam di badan tong komposter (`font-size: 38px`).
2. **Pecah menjadi 2 panggung padat (bukan 3 section fade):** Panggung 1 (Dapur & Halaman Belakang) pindah lewat `slide('x', 96)` ke Panggung 2 (Penampang Dalam Tong Kompos & Kebun).
3. **Naikkan aktivitas panggung ke 11 benda bergerak bermakna:** Tambahkan uap hangat naik (`drift()`), kipas ventilasi berputar (`spin()`), cacing/mikroba bergerak di sepanjang jalur tanah (`flowAlong()`), daun pohon & jemuran bergoyang (`sway()`), awan melintas, jarum termometer tong naik ke `60°C`, serta kecambah tumbuh bertahap.
4. **Kunci blocking, rig `J()`, & *Visual Craft* (`Color Script 70–20–10`):** Pindahkan tokoh ke sepertiga kiri (`x = 680`, `facing: 1`, `pose: '3/4'`, `handR: 'grip'`), pasang potongan kulit buah di `ch.RhandSlot`, tambahkan bayangan kontak `ch.sdJ`, *anticipation* 0,14 dtk sebelum melempar ke tong, serta kunci 70% latar ke palet analogus lembut + *Light Direction Anchor* searah (*2-tone split shading* pada tong, pohon, & irisan tanah berfaset).
5. **Terapkan zoom logaritmik bermakna:** Mulai `MCU (1.58)` pada tokoh & tong, lalu tarik halus ke `MS (1.16)` ketika penampang tanah terbuka memperlihatkan akar tanaman menyerap nutrisi.

### Hasil (*After*)
- `0` teks overlay, `11` benda bergerak yang seluruhnya menjelaskan proses biologis, `zoom` minimum `1.16` (lolos Audit `?debug=1`), fokus mata langsung terkunci ke subjek lewat *Color Script 70–20–10* & pose *Silhouette-First*, dan narasi terbaca bahkan saat suara dimatikan.

---

## Transformasi 2 — Continuous Action: Dari "Dashboard Inset" ke Aliran Full-Frame

### Skenario & Keluhan Awal (*Before*)
Topik: *Bagaimana kereta cepat menghubungkan dua kota dalam 45 menit.*  
Draf awal memiliki gejala:
- Seluruh durasi 30 detik bertahan di tampilan samping `#side`.
- Pojok kanan atas ditempeli kartu putih mini-map, pojok kanan bawah ditempeli widget speedometer, dan pojok kiri atas berisi 3 tingkat teks (*eyebrow + angka + paragraf*) (**Gagal F-03**).
- Saat angka `350 km/jam` dibaca, kecepatan kereta disetel ke `0` selama 3 detik agar teks mudah dibaca (**Gagal F-04**).

### 5 Perubahan Struktural yang Dilakukan
1. **Bongkar seluruh kartu inset & widget overlay:** Hapus kartu mini-map dan speedometer mengambang dari `#side`.
2. **Pecah fakta ke 3 sudut pandang full-frame via `whip()`:**
   - `0–10 dtk` **`#side`**: Fokus sensasi melesat + angka raksasa `"350"` hidup di `#deepObjs` **di belakang** kereta.
   - `10–19 dtk` **`#cabin`**: Full-frame ruang masinis, jarum speedometer menyapu sesuai `vAt(t)`, marka melesat di kaca depan, odometer digital `tabular-nums`.
   - `19–30 dtk` **`#map`**: Full-frame simulasi peta kiri→kanan, path tergambar via `stroke-dasharray`, penanda oranye melaju mengikuti tangen, zoom `2.3× → 1.0×` lalu `"45 MENIT"` muncul setelah zoom selesai.
3. **Jaga dunia tetap mengalir saat baca:** Ubah profil `VK` dari `0` menjadi melambat terkontrol `950 → 420 px/dtk` (~44%) ketika angka `350` melintas di `#deepObjs`.
4. **Ringkas teks ke maksimum 2 tingkat:** Hapus eyebrow dan paragraf; cukup angka besar + 1 satuan/label singkat.
5. **Kunci *Color Script* & *4-Plane Atmospheric Attenuation* via `paletteAt(t)`:** Geser rona langit dari pagi cerah (`#side`) ke kontras biru-oranye berenergi tinggi (`#cabin`) lalu ke hijau-krem bersih (`#map`), sambil menjaga pegunungan jauh *2-tone* tanpa detail dan arah pantulan kaca diagonal tetap seragam antara `#side` dan kaca depan `#cabin`.

### Hasil (*After*)
- Nol dashboard inset, 3 instrumen full-frame berganti dinamis lewat `whip()`, kedalaman parallax 4 lapis & pantulan kaca konsisten lintas-view, scrub maju-mundur bersih tanpa view bertumpuk, dan dunia terus hidup dari detik `0.0` hingga `30.0`.

---

## Transformasi 3 — Cartoon Collage: Dari "1 Gambar + 1 Kalimat Datar" ke Akresi Berkedalaman

### Skenario & Keluhan Awal (*Before*)
Topik: *Kenapa kopi bisa menahan rasa kantuk.*  
Draf awal memiliki gejala:
- Tiap adegan hanya menaruh 1 ilustrasi cangkir kopi di kiri + 1 kalimat panjang di kanan, lalu pindah section (**Gagal F-01 & F-05**).
- `filter: drop-shadow(...)` dipasang langsung di `.cut` sehingga bayangan kertas hilang tepat setelah animasi `fly()` selesai (**Gagal F-05**).
- Kalimat `say()` mulai pada detik yang sama dengan `home()` sehingga teks meluncur keluar frame saat kamera masih bergerak.
- Area 2 ditaruh di `x = 2400` (`dx = 1440` saja) sehingga ujung ilustrasi Area 2 bocor masuk ke pinggir kanan Area 1 (**Gagal F-06**).

### 5 Perubahan Struktural yang Dilakukan
1. **Terapkan pola akresi 4–5 elemen per area:**
   - Area 1 *(Rasa Kantuk)*: (1) Cutout otak `fly()`, (2) Cutout molekul adenosin menempel (`look()`), (3) Kartu `.note` *"Sinyal lelah aktif"*, (4) `say()` kalimat singkat + pill oranye `"adenosin"`, (5) Panah coretan tangan `draw()`.
   - Area 2 *(Blokade Kafein)*: (1) Cutout cangkir kopi + molekul kafein `fly()`, (2) Polaroid `.polaroid` reseptor tertutup, (3) Stempel `.stamp` `"TERBLOKIR"` via `slam()`, (4) `say()` + pill teal `"kafein menyamar"`.
2. **Pisahkan DOM `.cut` dan `.cut-body`:** Biarkan `.cut` menangani `fly()` dan `blurTween()`, pindahkan `filter: drop-shadow(0 28px 36px rgba(34,30,27,.26))` ke `.cut-body`.
3. **Kunci jeda `+0,15 dtk` setelah `home()`:** Selesaikan `home()` di `4,80 dtk`, baru jalankan `say()` di `4,95 dtk`.
4. **Perlebar jarak spasial & panggil `sizeWorld()`:** Pindahkan Area 2 ke `(3560, 540)` (`dx = 2600 px`) dan panggil `sizeWorld(960, 540, 3560, 540)` agar kertas krem + grain selalu menutup penuh tanpa tepi hitam maupun kebocoran area sebelah.
5. **Hidupkan cutout pasca-mendarat:** Beri ayunan organik `±24 px` dengan durasi berbeda (`1,45 dtk`, `1,72 dtk`, `1,58 dtk`) agar kolase terasa bernapas secara alami.

### Hasil (*After*)
- Setiap adegan terasa seperti meja riset editorial yang hidup, bayangan kertas utuh di seluruh durasi, nol kebocoran area sebelah, dan seluruh teks nyaman dibaca setelah kamera stabil di `z = 1.0`.
