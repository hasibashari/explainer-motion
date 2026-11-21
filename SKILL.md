---
name: bang-motion
description: Membangun video explainer motion graphic deterministik di browser (16:9 atau 9:16) memakai satu berkas HTML + CSS + SVG + GSAP yang bergerak seperti video sungguhan — bukan slide presentasi. Berfokus pada tiga gaya modular: Cartoon Stage, Continuous Action, dan Cartoon Collage. Gunakan skill ini setiap kali user meminta "explainer", "video penjelasan", "kartun edukasi", "cartoon stage", "continuous action", "cartoon collage", atau mengeluh animasi web terasa seperti PPT/slide.
license: MIT
metadata:
  author: Bang Tutorial
  author_url: https://youtube.com/bangtutorial
  version: "2.1.0"
  homepage: https://github.com/bangtutorial/bang-motion
---

# Bang Motion — Agent Skill Explainer Motion Graphic (v2.1.0)

Dokumen ini adalah **router utama**. Baca `references/explainer-core.md`, `references/anti-ppt.md`, dan **`references/reference-index.md`** untuk aturan bersama serta pemuatan referensi selektif, lalu muat **hanya satu** modul gaya yang dipilih agar konteks tetap bersih dan bebas kebocoran instruksi lintas gaya.

## 1. Peta Routing 3 Gaya

| Gaya | Kapan Dipilih | Referensi & Diagram Visual | Starter Resmi |
|---|---|---|---|
| **Cartoon Stage** | Edukasi berkarakter, sebab-akibat, cerita proses & dampak, **nol caption**, palet flat cerah | `references/cartoon-stage.md` · `visuals/stage-anatomy.svg` · `visuals/stage-poses-expressions.svg` | `assets/starter-cartoon-stage.html` |
| **Continuous Action** | Kecepatan, rute, transportasi, olahraga, alur proses nonstop, **100% vektor + ganti sudut & peta** | `references/continuous-action.md` · `visuals/continuous-viewpoints-continuity.svg` | `assets/starter-continuous-action.html` |
| **Cartoon Collage** | Edukasi sains, penjelasan konsep, topik populer bergaya **kertas krem + cutout + pill kata** | `references/cartoon-collage.md` · `visuals/collage-spatial-blueprints.svg` | `assets/starter-cartoon-collage.html` |

Gunakan *Decision Tree*, *Diagram Alur Kerja*, dan *Matriks Interoperabilitas* di `references/explainer-core.md` bila topik bersifat hibrida.

## 2. Protokol Reference Selection & Anti-Copy (Wajib)

Jangan memuat seluruh folder `references/` atau `examples/` sekaligus dan **dilarang menjiplak** subjek, teks, atau warna dari contoh/diagram:
1. **Identify:** Kenali gaya target, rasio (`16:9` / `9:16`), dan risiko utama proyek user.
2. **Select (`references/reference-index.md`):** Muat **maksimal 4 berkas tambahan** per fase (1 panduan gaya + 1–2 diagram SVG beranotasi + 1 contoh/kartu audit).
3. **Abstract:** Pisahkan *konten contoh* (dibuang 100%) dari *prinsip struktural* (geometri, lapisan z-order, jeda waktu, kontras) lalu tuliskan prinsipnya di `templates/production-brief.md`.
4. **Apply & Verify:** Terapkan pada topik asli user dan uji hasil terhadap pola kegagalan `F-01` s.d. `F-07` (`examples/good-vs-bad.md` + `templates/qc-audit-scorecard.md`).

## 3. Tiga Hukum Inti & Prasyarat

1. **Ini Video, Bukan Slide (`references/anti-ppt.md`):** Tidak ada `<section>` yang di-fade bergantian; maksimum 2 tingkat teks (nol caption untuk Cartoon Stage); dunia/subjek selalu hidup tiap detik; ukuran teks minimum `30 px` pada panggung 1080 px.
2. **Deterministik 100%:** Nol pemakaian `Date.now()`, `performance.now()`, atau `Math.random()` di jalur render. Semua gerak adalah fungsi murni dari `tl.time()`. Scrub lewat `window.OPENER.seek(t)` wajib akurat maju maupun mundur.
3. **Satu Berkas `index.html`, Tanpa Player:** CSS dan JS ditulis inline (GSAP & Google Fonts dari CDN) agar langsung jalan saat diklik dua kali (`file://`) tanpa `npm install` dan tanpa server. Default: autoplay + loop tanpa panel kontrol; `R` ulang; `Space` jeda; `?debug=1` membuka panel scrub + QC HUD; `?clean=1` menahan di frame 0.

## 4. Larangan Struktural (Gagal 1 Poin → Rombak Sebelum Diserahkan)

1. `≥ 3` adegan berupa `<section>` yang dinyalakan-dimatikan dengan `opacity/autoAlpha` (`F-01`) → **Slide PPT**.
2. Tidak ada benang merah visual, atau menempelkan kartu mini-map/gauge inset di atas `#side` Continuous Action (`F-03`) → **Gagal**.
3. Pola `1 gambar diam + 1 kalimat` di Cartoon Collage (wajib akresi 3–5 elemen + `.cut-body` drop-shadow, `F-05`) atau `< 10` benda bergerak per panggung di Cartoon Stage → **Slide PPT**.
4. Zoom `< 1,08` (efek miniatur) / sendi lepas (`F-02`) di Cartoon Stage, atau `say()` mulai sebelum `home()` selesai di Cartoon Collage → **Gagal**.
5. Membangun dari nol — selalu salin dari starter resmi di `assets/` sesuai gaya yang dipilih.

## 5. Alur Kerja Produksi (6 Langkah)

1. **Langkah Nol (Kunci 4 Keputusan):** Baca `references/explainer-core.md` dan ajukan **SATU pertanyaan** (gaya + 1 rekomendasi, rasio, durasi, suara) — lewati yang sudah disebut user.
2. **Pra-Produksi (`templates/production-brief.md`):** Rujuk `references/reference-index.md` untuk memuat diagram visual & 1 studi kasus `examples/case-*.md` sesuai gaya, lalu isi *Style Brief*, *Target Pacing VO* (`2,2–2,5 kata/dtk`), *Beat Sheet*, dan *Character/Asset Manifest*. Tunjukkan ke user untuk disetujui sebelum menulis kode.
3. **Konsistensi Aset (`references/asset-consistency.md`):** Bila memakai ≥ 1 karakter, cutout generate, atau subjek lintas-view, kunci *Character/Subject Bible* dan *Style Lock Prompt Prefix* sebelum generasi aset.
4. **Scaffold & Bangun:** Salin starter resmi dari `assets/` lalu muat **hanya** referensi gaya & diagram SVG yang dipilih (`references/visuals/shared-camera-composition.svg` untuk framing/9:16 dan `references/visuals/shared-scale-perspective.svg` untuk skala `1 H`, sudut kamera, & perspektif).
5. **Verifikasi Mandiri Dua Lapis (`templates/qc-audit-scorecard.md`):**
   - *Lapis 1 (Teknis):* Buka `?debug=1`, jalankan tombol **Audit** (`window.OPENER.audit()`), aktifkan overlay **Safe** bila `9:16`, scrub maju-mundur di semua transisi.
   - *Lapis 2 (Kualitatif):* Audit hasil terhadap `F-01..F-07` (`examples/good-vs-bad.md` & `references/visuals/shared-good-vs-bad.svg`). Bila gagal atau saat revisi, terapkan perbaikan struktural dari `examples/before-after.md`.
6. **Penyerahan:** Serahkan satu berkas `index.html` + tuliskan **naskah VO lengkap** langsung di dalam pesan penutup (sekaligus simpan sebagai `vo-script.md`), lalu ajak user mengirim rekaman/TTS bila ingin disinkronkan.

## 6. Checklist QC Inti (Wajib untuk Semua Gaya)

*(Gunakan kartu skor terpadu `templates/qc-audit-scorecard.md` beserta checklist khusus di akhir dokumen referensi gaya.)*

- [ ] Gaya berasal dari salah satu 3 gaya resmi dan dimulai dari starter yang tepat di `assets/`
- [ ] `templates/production-brief.md` disetujui sebelum coding; total kata VO sesuai rumus pacing
- [ ] Lolos seluruh 5 poin **Larangan Struktural** & 7 **Failure Modes (`F-01..F-07`)**
- [ ] Zero `Date.now()` / `Math.random()` di render loop; `?debug=1` **Audit** lolos tanpa peringatan & scrub maju-mundur akurat
- [ ] Tanpa player di mode normal; autoplay + loop; bila ada VO, menahan di frame 0 saat autoplay diblokir
- [ ] Deliverable berupa satu `index.html` jalan via `file://` + naskah VO disertakan di pesan penutup & `vo-script.md`

## 7. Indeks Paket (4 Folder Inti Ramping)

| Folder / Jalur | Peran bagi Agent |
|---|---|
| `references/reference-index.md` | **Indeks retrieval** pemetaan tugas/tahap/failure-mode → berkas relevan + protokol anti-copy |
| `references/explainer-core.md` | Aturan bersama: Decision Tree, Diagram Alur Kerja, Langkah Nol, Pacing VO, Skala `1 H` & Sudut Kamera, 9:16, Retime VO, Interoperabilitas |
| `references/anti-ppt.md` | Hukum desain anti-slide + peta Failure Modes `F-01..F-07` → perbaikan & cek `?debug=1` |
| `references/asset-consistency.md` | Character Bible, Multi-Character Registry, Style Lock Prompt Presets, Subject Anchor |
| `references/cartoon-stage.md` · `continuous-action.md` · `cartoon-collage.md` | Panduan teknis & QC spesifik per gaya |
| `references/visuals/*.svg` (7 berkas) | Diagram SVG beranotasi (*single source of truth*, terbaca visual & kode): kamera/9:16, skala `1 H`/sudut kamera/perspektif, Good-vs-Bad, anatomi Stage, pose/ekspresi, 7 viewpoints Action, peta spasial Collage |
| `examples/good-vs-bad.md` · `before-after.md` | Pembelajaran kontrastif (`F-01..F-07`) & 3 studi transformasi perbaikan nyata |
| `examples/case-*.md` (3 berkas) | 3 studi kasus lengkap ujung-ke-ujung (*Input → Brief → Beat Sheet → Code Map → QC*) |
| `templates/production-brief.md` · `qc-audit-scorecard.md` | Kerangka pra-produksi siap isi & kartu skor audit dua lapis |
| `assets/starter-*.html` (3 berkas) | Starter executable resmi dengan `?debug=1` QC HUD |
