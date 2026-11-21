# Reference Index & Protokol Pengambilan Selektif (v2.1.0)

Dokumen ini adalah **peta retrieval utama** bagi AI Agent agar hanya memuat referensi yang benar-benar dibutuhkan pada setiap tahap kerja (mencegah *context bloat*) dan menerapkan **prinsipnya secara kontekstual** tanpa menjiplak isi contoh.

---

## 1. Protokol Reference Selection & Anti-Copy (4 Langkah Wajib)

```text
[1. IDENTIFY]              [2. SELECT]                  [3. ABSTRACT]                [4. APPLY & VERIFY]
Analisis Input User   ──►  Query Indeks di Bawah   ──►  Pisahkan Konten vs Prinsip ──► Terapkan ke Topik User
- Gaya target              - Muat 1 Referensi Gaya      - BUANG: subjek, palet,      - Bangun dengan aset &
- Rasio (16:9 / 9:16)      - Muat 1–2 Diagram SVG         teks, nama tokoh contoh      palet milik topik user
- Risiko utama             - Muat 1 Contoh/Kasus        - PERTAHANKAN: geometri,     - Audit via ?debug=1 &
  (padat teks? rig?        - BATAS: maks. 4 berkas        hierarki layer, timing,      qc-audit-scorecard.md
   multi-view? 9:16?)        tambahan per tahap           kontras, & rumus gerak       (cek F-01 s.d. F-06)
```

### Guardrail Anti-Penjiplakan
1. **Aturan Substitusi Total:** Dilarang menyalin topik, nama tokoh, kalimat naskah, atau warna mentah dari `examples/` maupun `references/visuals/` kecuali diminta eksplisit oleh user.
2. **Abstraksi Sebelum Implementasi:** Sebelum menulis kode di Langkah 4, terjemahkan referensi yang dipilih menjadi aturan terukur di dalam `templates/production-brief.md` (mis. *"pakai 3 whip-cut `#side → #cabin → #map`, arah kiri→kanan, angka di `#deepObjs`"*).
3. **Budget Konteks Ketat:** Jangan memuat seluruh folder `references/` dan `examples/` sekaligus. Pilih **hanya** baris yang cocok dengan gaya dan tahap kerja saat ini.

---

## 2. Tabel Indeks Referensi (Task / Workflow Stage / Failure Mode → File)

| ID | Gaya | Tahap Workflow | Pemicu Tugas (*Task Trigger*) | Pola Kegagalan Dicegah | Format | Jalur Berkas | Prinsip Inti untuk Diekstrak |
|---|---|---|---|---|---|---|---|
| `DOC-CORE` | Semua | Langkah 0, 1, 2, 6 | Mulai proyek baru, pilih gaya, hitung VO, 9:16, retime VO | Salah pilih gaya, VO kepanjangan, 9:16 terpotong (`F-06`) | Teks + Diagram | `references/explainer-core.md` | Decision Tree, rumus 2,2–2,5 kata/dtk, shot grammar, `.safe` 9:16, pemetaan `T(t)` |
| `DOC-ANTIPPT` | Semua | Langkah 2, 5 | Mencegah gaya slide, cek kualitas sebelum serah | Slide terselubung (`F-01` s.d. `F-06`) | Teks | `references/anti-ppt.md` | Daftar larangan slide, perbaikan teknis, pemeriksaan `?debug=1` |
| `DOC-CONSIST` | Semua | Langkah 3 | Ada ≥1 tokoh, cutout generate, atau objek lintas adegan | Karakter berubah wajah/gaya antar adegan | Teks | `references/asset-consistency.md` | Character Bible, Multi-Character Registry, Style Lock Prompt Presets, Subject Anchor |
| `DOC-STAGE` | Cartoon Stage | Langkah 2, 4, 5 | Produksi explainer panggung berkarakter tanpa caption | Miniatur `<1,08`, sendi lepas (`F-02`), caption bocor | Teks | `references/cartoon-stage.md` | 7 hukum Stage, `J()` aditif, zoom logaritmik, ≥10 benda bergerak |
| `DOC-CONT` | Continuous Action | Langkah 2, 4, 5 | Produksi rute, kecepatan, transportasi, proses nonstop | Dashboard inset (`F-03`), arah berbalik (`F-04`) | Teks | `references/continuous-action.md` | 7 prinsip, 7 viewpoints, `VK → D(t)`, `whip()`, `#deepObjs`, anti-`visibility` leak |
| `DOC-COLL` | Cartoon Collage | Langkah 2, 4, 5 | Produksi edukasi sains/konsep kertas krem + pill | Kolase datar (`F-05`), tepi hitam, `say()` kepotong | Teks | `references/cartoon-collage.md` | Akresi 3–5, `CU→look→home→say`, `sizeWorld()`, `.cut` vs `.cut-body` |
| `VIS-SHARED-CAM` | Semua | Langkah 2, 4 | Merancang variasi shot (`WS/MS/MCU/CU`) atau format `9:16` | Framing monoton tengah terus, teks terpotong (`F-06`) | SVG Beranotasi | `references/visuals/shared-camera-composition.svg` | Rule of thirds, lead room, safe-area 9:16 (±190/±230 px), pola kamera per gaya |
| `VIS-SHARED-SCALE` | Semua | Langkah 2, 4 | Menentukan ukuran tokoh vs mobil/rumah/pohon, sudut kamera, & kedalaman | Proporsi tidak masuk akal / panggung datar (`F-07`) | SVG Beranotasi | `references/visuals/shared-scale-perspective.svg` | Tangga proporsi `1 H` (1,7 m), 4 camera angles + horizon line, 2,5D, rasio parallax |
| `VIS-SHARED-GVB` | Semua | Langkah 2, 5 | Kalibrasi ambang kualitas visual sebelum coding & saat QC | Semua pola gagal (`F-01` s.d. `F-07`) | SVG Beranotasi | `references/visuals/shared-good-vs-bad.svg` | Kontras visual langsung versi buruk vs benar untuk ketiga gaya |
| `VIS-STAGE-ANAT` | Cartoon Stage | Langkah 2, 4 | Menyusun panggung, 3 bidang kedalaman, & ≥10 gerak | Panggung sepi (`F-01`), kaki melayang (`F-02`) | SVG Beranotasi | `references/visuals/stage-anatomy.svg` | BG/MG/FG flat, ground `y=0`, `ch.sdJ`, teks diegetik saja, amplop zoom |
| `VIS-STAGE-POSE` | Cartoon Stage | Langkah 3, 4 | Rigging karakter, memilih pose aksi, & ekspresi wajah | Sendi lepas (`F-02`), siluet kaku/tidak terbaca | SVG Beranotasi | `references/visuals/stage-poses-expressions.svg` | Hierarki `J()`, batas sudut anatomis, 6 siluet pose + anticipation, 6 ekspresi |
| `VIS-CONT-VIEW` | Continuous Action | Langkah 2, 4 | Merancang urutan sudut pandang & kontinuitas arah | Dashboard inset (`F-03`), flip arah (`F-04`) | SVG Beranotasi | `references/visuals/continuous-viewpoints-continuity.svg` | Z-order `#deepObjs` di belakang subjek, 3 Subject Anchors, 7 viewpoints penuh |
| `VIS-COLL-MAP` | Cartoon Collage | Langkah 2, 4 | Memetakan koordinat kanvas 2D & lapisan akresi | Kolase datar (`F-05`), area sebelah bocor (`F-06`) | SVG Beranotasi | `references/visuals/collage-spatial-blueprints.svg` | Grid zig-zag `dx=2600/dy=1700`, jeda `+0.1s` pasca-`home()`, `.cut/.cut-body` |
| `EX-GVB` | Semua | Langkah 2, 5 | Mengenali output lemah vs kuat saat review/QC | `F-01` s.d. `F-07` | Teks Kontrastif | `examples/good-vs-bad.md` | Definisi 7 Failure Modes + kriteria lolos vs tolak + cek `?debug=1` |
| `EX-BA` | Semua | Langkah 5, 6 | Memperbaiki draf lemah / revisi user (*"terlalu kaku/ramai"*) | Revisi kosmetik yang tidak membenahi akar masalah | Teks Diff | `examples/before-after.md` | 3 studi transformasi nyata (*Before → 4–5 perubahan konkret → After*) |
| `EX-STAGE` | Cartoon Stage | Langkah 2, 3, 4 | Kalibrasi kualitas *Production Brief → Beat → Kode* Stage | Brief dangkal, lompatan teori ke kode | Studi Kasus | `examples/case-cartoon-stage.md` | Contoh lengkap alur kerja Cartoon Stage dari prompt mentah hingga QC |
| `EX-CONT` | Continuous Action | Langkah 2, 3, 4 | Kalibrasi kualitas *Brief → VK/Viewpoints → Kode* Action | Brief tanpa profil `VK` atau pengulangan sudut | Studi Kasus | `examples/case-continuous-action.md` | Contoh lengkap alur kerja Continuous Action dari prompt mentah hingga QC |
| `EX-COLL` | Cartoon Collage | Langkah 2, 3, 4 | Kalibrasi kualitas *Brief → World Map → Akresi → Kode* | Kolase jadi 1 gambar + 1 kalimat | Studi Kasus | `examples/case-cartoon-collage.md` | Contoh lengkap alur kerja Cartoon Collage dari prompt mentah hingga QC |
| `TPL-BRIEF` | Semua | Langkah 2 | Menyusun dokumen pra-produksi untuk disetujui user | Coding langsung tanpa rencana terukur | Template | `templates/production-brief.md` | Struktur Style Brief, target VO, Beat Sheet, dan Character/Asset Manifest |
| `TPL-QC` | Semua | Langkah 5 | Audit final sebelum penyerahan | Lolos teknis tetapi gagal kualitas visual | Template | `templates/qc-audit-scorecard.md` | Kartu skor 2 lapis: metrik `?debug=1` + audit kualitatif `F-01..F-07` |

---

## 3. Resep Pemuatan Cepat (*Just-in-Time Retrieval Presets*)

Gunakan resep ini agar agen tidak pernah memuat lebih dari **4 berkas tambahan** pada satu fase:

- **Proyek Cartoon Stage baru:**
  - *Fase Perencanaan (Langkah 1–3):* `DOC-STAGE` + `VIS-SHARED-SCALE` + `EX-STAGE` + `TPL-BRIEF`
  - *Fase Implementasi (Langkah 4):* `VIS-STAGE-ANAT` + `VIS-STAGE-POSE` + `VIS-SHARED-CAM` + `assets/starter-cartoon-stage.html`
  - *Fase Audit / Revisi (Langkah 5):* `EX-GVB` + `TPL-QC` *(+ `EX-BA` jika perlu perbaikan)*
- **Proyek Continuous Action baru:**
  - *Fase Perencanaan (Langkah 1–3):* `DOC-CONT` + `VIS-CONT-VIEW` + `VIS-SHARED-SCALE` + `TPL-BRIEF`
  - *Fase Implementasi (Langkah 4):* `VIS-SHARED-CAM` + `EX-CONT` + `assets/starter-continuous-action.html`
  - *Fase Audit / Revisi (Langkah 5):* `EX-GVB` + `TPL-QC` *(+ `EX-BA` jika perlu perbaikan)*
- **Proyek Cartoon Collage baru:**
  - *Fase Perencanaan (Langkah 1–3):* `DOC-COLL` + `VIS-COLL-MAP` + `EX-COLL` + `TPL-BRIEF`
  - *Fase Implementasi (Langkah 4):* `VIS-SHARED-CAM` + `assets/starter-cartoon-collage.html`
  - *Fase Audit / Revisi (Langkah 5):* `EX-GVB` + `TPL-QC` *(+ `EX-BA` jika perlu perbaikan)*
