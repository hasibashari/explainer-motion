# Cartoon Collage — Panduan Lengkap (Kartun Kolase)

Starter resmi: **`assets/starter-cartoon-collage.html`** · Diagram visual: **`references/visuals/collage-spatial-blueprints.svg`** · Studi kasus: **`examples/case-cartoon-collage.md`**.
Cocok untuk explainer edukasi sains, penjelasan konsep, cerita penemuan, dan topik populer bergaya hangat & editorial ilustratif.

**Identitas Visual:** Kertas krem bertekstur grain SVG + noda warna lembut, cutout ilustrasi flat dengan bayangan lembut (drop shadow), tipografi **Bricolage Grotesque** tebal + **Caveat** (tulisan tangan), pill warna sebagai sorotan kata kunci, bingkai polaroid/kartu catatan, dan coretan anotasi SVG.

---

## 1. Tulang Cerita & Pola Akresi 3–5 Elemen

### Struktur Narasi yang Terbukti
1. **Hook** — 1 cutout ikonik + 1 klaim/pertanyaan singkat yang memicu rasa ingin tahu.
2. **Pertanyaan Eksplisit** — *"Kenapa bisa?"* atau *"Bagaimana cara kerjanya?"*
3. **Konteks Visual** — diagram/peta/cutout utama + label + anotasi tangan.
4. **Angka Besar / Skala** — angka menghitung naik atau pengulangan cutout menjadi grid (`gridPop()`).
5. **Alur / Tahapan** — 2–3 tahap sebab-akibat atau grafik yang menggambar diri.
6. **Dampak Dekat Penonton** — contoh konkret sehari-hari.
7. **Penutup** — kembali menggema ke hook awal.

### Hukum Akresi: Satu Adegan = 3–5 Elemen Bertahap
Kegagalan paling umum adalah menaruh **1 cutout + 1 kalimat** lalu pindah adegan — itu tetap terasa seperti slideshow meski kameranya bergerak. Gunakan pola akresi per frasa VO:
- **Elemen 1 (Subjek):** Cutout utama masuk lewat `fly()`.
- **Elemen 2 (Pendukung/Konteks):** Cutout kedua, kartu `.polaroid`, atau diagram masuk saat kamera melakukan `look()` kecil.
- **Elemen 3 (Bukti/Skala):** Kartu catatan `.note`, stempel `.stamp` (`slam()`), angka besar, atau grid pengulangan (`gridPop()`).
- **Elemen 4–5 (Penegas):** Kalimat pendek `say()` dengan pill warna + coretan tangan `draw()` / `.hand`.
- Elemen bukti seperti `.note` / `.stamp` (≤ 3 baris pendek) dihitung sebagai **objek visual**, bukan tingkat teks tambahan.

---

## 2. Koreografi Kamera & Spatial World Map

### Pola Kamera per Adegan
Rig `#world` menyimpan state `C = { x, y, z }` dengan `transform-origin: 0 0`.
1. **Mulai Close-Up (`z ≈ 1,5–1,8`):** Adegan lahir fokus pada aset pertama (`cutA`).
2. **Meluncur (`look`):** Kamera bergeser mulus ke aset kedua (`cutB`) atau elemen bukti sambil aset tersebut masuk (`fly`).
3. **Zoom Out (`home`):** Kamera mundur ke titik pusat adegan (`z = 1,0`) dalam `1,0–1,2 dtk`.
4. **Teks Muncul SETELAH Zoom Out Selesai:** Jadwalkan `say()`, `draw()`, dan `label()` minimal **`+0,1 dtk` setelah `home()` selesai** agar kalimat tidak terasa terpotong saat kamera bergerak. Setelah itu kamera diam (atau napas sangat halus `1,00 → 1,02`).
5. **Transisi Push-Through (`into` → `settle`):** `0,9 dtk` sebelum pindah adegan, keluarkan teks (`unsay`), lalu dorong kamera (`into`) menuju area adegan berikutnya di atas kertas yang sama dan mendarat (`settle`). **Bukan section yang di-fade.**

### Perencanaan Koordinat Dunia (Spatial World Map)
Kesalahan fatal pada video 5–8 adegan adalah menempatkan adegan keluar dari batas kertas latar (`.paper`), sehingga muncul ruang hitam di tepi layar.

Gunakan **Grid Spasial 2D** (tata letak zig-zag/ular agar perjalanan kamera tetap dekat dan variatif) dengan jarak aman antar pusat adegan **`dx = 2600 px`**, **`dy = 1700 px`** (untuk 16:9):

```text
Adegan 1: (960, 540)     → Adegan 2: (3560, 540)    → Adegan 3: (6160, 540)
                                                              ↓
Adegan 6: (960, 2240)    ← Adegan 5: (3560, 2240)   ← Adegan 4: (6160, 2240)
```

Di starter v2.0, panggil `sizeWorld(minX, minY, maxX, maxY)` (margin otomatis `±1600 px` di setiap sisi) agar `.paper`, `.grain`, dan `.stain` **selalu menutup 100% area perjalanan kamera** tanpa celah hitam.

---

## 3. Resep Teknis & Perlindungan Visual

- **Pemisahan Wrapper Blur vs Drop-Shadow (Wajib!):**
  Jangan memasang `filter: drop-shadow(...)` pada elemen `.cut` yang menjadi target `blurTween()`. Saat `blurTween` menyetel `style.filter = 'none'` di akhir gerakan, drop-shadow akan hilang permanen.
  **Pola benar di starter v2.0:** `.cut` memegang posisi, transform, dan motion blur; **`.cut-body`** di dalamnya memegang `filter: drop-shadow(0 28px 36px rgba(34,30,27,.26))` (dan filter warna seperti grayscale diletakkan pada `<img>`).
- **Cutout Hidup:** Setelah mendarat lewat `fly()`, tiap cutout memiliki ayunan organik minimal `±22–28 px` (`1,4–1,8 dtk`, `sine.inOut`, `yoyo: true`, `repeat: -1`), rotasi kecil `±1,5°`, dan fase/durasi berbeda agar tidak bergerak serempak seperti robot.
- **Kalimat per Kata (`say/unsay`):** Kata kunci memakai `<span class="hl"><i></i>kata</span>` (varian warna default oranye, `.t` teal, `.r` merah). Jangan membungkus frasa panjang ke satu `.w` nowrap (meluber di 9:16).
- **Teks Hantu (`.ghost`):** Pilih satu gaya konsisten untuk seluruh video dengan opacity **`0,18–0,25`** (bukan `0,07` yang tenggelam ke warna kertas).
- **Anotasi SVG (`draw`):** Set `opacity: 0` hingga detik mulai agar `stroke-linecap: round` tidak menyisakan titik nyasar; gunakan `clip-path: inset()` agar mendukung garis utuh maupun putus-putus.
- **Konsistensi Cutout Generate:** Lihat `references/asset-consistency.md` untuk *Style Lock Prompt* batch generation agar seluruh cutout memiliki ketebalan outline, pencahayaan, dan harmoni warna yang seragam.

---

## Checklist QC Khusus Cartoon Collage

- [ ] Jalankan `?debug=1` → klik **Audit**: nol teks `< 30 px`, nol cutout keluar dari batas `.paper`, dan semua `.cut` memiliki pembungkus `.cut-body` ber-drop-shadow utuh
- [ ] Pada setiap adegan, `say()` dan `label()` dimulai minimal `0,1 dtk` **setelah** `home()` selesai
- [ ] Setiap adegan memuat **3–5 elemen bertahap** (bukan 1 gambar + 1 kalimat)
- [ ] Semua cutout tetap bergoyang hidup setelah mendarat dengan fase berbeda
- [ ] Transisi antar adegan memakai `into()` + `settle()` (push-through di atas dunia kertas yang sama), bukan fade antar `<section>`
- [ ] Ukuran dunia diatur lewat `sizeWorld(...)` sehingga tidak ada tepi hitam terlihat saat kamera berpindah atau zoom-out
- [ ] Teks hantu `.ghost` terbaca jelas (opacity `0,18–0,25`) dan bentuk anotasi bervariasi
