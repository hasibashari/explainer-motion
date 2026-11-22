# Asset & Character Consistency — Character Bible & Style Lock Prompt Presets

Dokumen ini memastikan seluruh karakter, subjek, dan cutout ilustrasi dalam satu video terlihat berasal dari **satu dunia visual yang konsisten** — baik saat digambar murni SVG vektor inline maupun saat menggunakan aset gambar generate.

> **Default Tanpa Tool Gambar & Aturan Single-File (`file://`):** Jika lingkungan agen tidak memiliki tool image-generation atau user menginginkan nol dependensi eksternal, bangun **100% memakai SVG vektor inline** (sebagaimana dicontohkan di ketiga starter resmi `assets/starter-*.html`). Jika memakai gambar raster (`HEADS` / cutout PNG), sematkan langsung sebagai **Base64 Data URI** (`data:image/png;base64,...`) agar deliverable tetap berupa **satu berkas `index.html` mandiri** yang berjalan via `file://`.

---

## 1. Cartoon Stage — Character Bible & Multi-Character Registry

Saat proyek menggunakan **≥ 1 karakter**, kunci spesifikasi identitas setiap tokoh di `templates/production-brief.md` sebelum menulis kode atau men-generate kepala:

| Field | Fungsi | Contoh |
|---|---|---|
| `who` | ID unik tokoh (dipakai juga untuk offset kedip unik) | `raka`, `sari` |
| `scale` | Skala rig berbasis jangkar `1 H` (`baseScale` panggung `0.58–0.64`, default `0.60` ≈ `345 px` dewasa; remaja `0.90 × baseScale ≈ 0.54`; anak `0.68 × baseScale ≈ 0.41`) | `0.60` *(1.00 H)* |
| `facing` | Arah hadap default (`1` kanan, `-1` kiri) sesuai *180° rule* | `1` |
| `palette` | Warna kulit, rambut, baju/jaket (`outfit`: `jacket`/`shirt`/`tee`), celana, sepatu | `outfit: 'jacket', shirt: #237C80, pants: #1E3A5F` |
| `hairStyle` | Arketipe rambut berlapis (`short`, `bob`, `bun`, `curly`, `hijab`, `cap` — lihat `VIS-STAGE-CHAR`) | `short` / `hijab` |
| `silhouette` | 1 ciri siluet pembeda (kacamata, topi, hijab polos, celemek, tas) | `kacamata bundar tipis` |
| `props` | Daftar prop yang ditempel di `RhandSlot` / `LhandSlot` (diapit `handBack → handSlot → handFront`) | `P1: ponsel belakang (carry), P3: layar HP/cangkir (grip)` |

**Aturan Konsistensi Multi-Tokoh & Batas Perubahan Desain:**
- Jangan memberi warna baju identik pada dua tokoh yang tampil di panggung yang sama.
- Gunakan nama `who` dengan panjang berbeda atau offset kedip eksplisit agar dua tokoh tidak berkedip di milidetik yang sama.
- **Perubahan yang diperbolehkan antar-adegan:** pose turnaround (`3/4`, `front`, `side`, `back`), arah hadap (`facing`), ekspresi (`netral`, `senang`, `kaget`, `berpikir`, `khawatir`, `yakin`), 9 gestur tangan (`relaxed`, `open`, `point`, `grip`, `carry`, `pinch`, `fist`, `thumbs`, `present`), prop di `RhandSlot`/`LhandSlot`, dan kostum kontekstual terbatas (mis. memakai jas lab atau helm keselamatan).
- **Perubahan yang dilarang tanpa izin user:** mengubah proporsi `1 H` relatif antar-tokoh, warna kulit, bentuk/warna rambut, ciri siluet identitas utama, atau mengganti palet identitas secara acak antar-panggung.

### Style Lock Prompt — Kepala Karakter Cartoon Stage
Jika menggunakan kepala raster (`HEADS`), generate seluruh tokoh memakai **Style Lock Prefix identik** ini (ganti hanya blok `[IDENTITY]`):

```text
[STYLE LOCK]
ONE isolated front-facing 2D educational cartoon HEAD, centered with generous transparent margin on all sides. Very minimal flat vector-cutout style: broad softly rounded head shape, simple small ears, hair as ONE clean solid silhouette (no individual strands, no specular highlights), eyes ONLY TWO SMALL SOLID BLACK DOTS (no sclera, no iris, no highlights, no eyelids), two tiny clean eyebrow strokes, nose omitted, mouth a tiny curved smile line. Strictly flat solid fills (~3-4 colors total), zero gradients, zero outlines, zero shading, zero paper texture.

[IDENTITY]
<usia/gender, warna kulit, model & warna rambut, aksesoris polos seperti kacamata bundar atau hijab polos satu warna>

[NEGATIVE CONSTRAINTS]
No neck below jaw, no shoulders, no body, no background, no drop shadow, no text, no watermark, no 3D render, no anime eyes, no realistic skin shading.
```

Untuk ekspresi (`netral`, `senang`, `kaget`, `berpikir`, `khawatir`, `yakin`) dan kedip: pertahankan posisi kanvas dan bounding box identik, lalu daftarkan langsung sebagai **Base64 Data URI** pada objek `HEADS[who]` di starter tanpa folder eksternal `karakter/`:
```js
HEADS.raka = {
  bbox: [220, 140, 804, 860],
  src: {
    netral: 'data:image/png;base64,...', 'netral-kedip': 'data:image/png;base64,...',
    senang: 'data:image/png;base64,...', 'senang-kedip': 'data:image/png;base64,...',
    kaget: 'data:image/png;base64,...'
  }
};
```
Untuk jalur paling ringan dan deterministik, gunakan kepala vektor bawaan `character()` yang sudah mendukung 6 ekspresi kanonik (alis + mata + mulut) dan kedip otomatis tanpa aset eksternal.

---

## 2. Cartoon Collage — Style Lock Prompt Presets (Batch Cutout)

Kesalahan paling sering di Cartoon Collage adalah men-generate 8 cutout dengan prompt berbeda sehingga sebagian terlihat seperti kartun tebal, sebagian seperti 3D, dan sebagian seperti sketsa tipis.

**Aturan Batch Generation:**
1. Tulis **satu Master Style Prefix** di `templates/production-brief.md` dan tempel persis sama pada setiap aset dalam proyek tersebut.
2. Selalu minta **latar putih polos + margin aman 15%** agar proses *remove background* bersih dan tidak memotong tepi ilustrasi.
3. Setelah latar dihapus, pangkas margin transparan (*trim alpha bbox*) agar lebar/tinggi CSS mencerminkan ukuran visual sebenarnya.
4. Batasi palet aset agar selaras dengan kertas krem (`#F4E9D3`) dan maksimal 3–4 warna aksen proyek.

### Template Prompt — Cutout Ilustrasi Flat (Cartoon Collage)
```text
[STYLE LOCK]
Single isolated editorial flat cartoon illustration of [SUBJECT], warm mid-century educational book aesthetic, clean bold dark-brown outlines (#221E1B, uniform medium stroke weight), slightly textured matte flat color fills, soft warm lighting from top-left, harmonious limited palette anchored in [PRIMARY HEX], [SECONDARY HEX], and cream accents. Centered with 15% clean white margin around the entire object.

[SUBJECT]
<objek/aksi spesifik, sudut pandang jelas, tanpa elemen latar belakang tambahan>

[NEGATIVE CONSTRAINTS]
Plain pure solid white background (#FFFFFF), NO cast shadow on floor, NO background scenery, NO text, NO labels, NO watermark, NO frame/border, NO 3D glossy render, NO photorealism.
```

**Catatan Bayangan:** Minta `NO cast shadow on floor` saat generate karena starter `starter-cartoon-collage.html` sudah memberi bayangan kertas konsisten lewat `.cut-body { filter: drop-shadow(...) }`.

---

## 3. Continuous Action — Subject Visual Anchor

Karena Continuous Action idealnya 100% SVG vektor prosedural, konsistensi dijaga melalui **Token Subjek Terpusat**:

- Definisikan satu objek konfigurasi `SUBJECT` di bagian atas JS (mis. `bodyColor`, `stripeColor`, `windowColor`, `noseShape`) yang dipakai ulang di **seluruh sudut pandang**:
  - **Tampak Samping (`#side`):** bodi utama + garis aksen + roda/kaki.
  - **Kabin (`#cabin`):** warna dashboard & jarum memakai `var(--accent)` yang sama.
  - **Peta (`#map`):** penanda rute memakai warna `var(--accent)` + ikon siluet mini subjek.
  - **Tampak Depan / Interior:** garis aksen & warna bodi tetap identik dengan tampak samping.
- **Jangan mengubah arah hadap tanpa pemicu:** Jika di `#side` subjek melaju ke kanan, maka arah dominan rute di `#map` dan orientasi di profil ketinggian juga bergerak ke kanan.

---

## 4. Environment, Props, & Object State Continuity

Untuk mencegah prop atau elemen lingkungan berubah warna, skala, atau kondisi secara tidak sengaja antar-panggung/view:
- **Prop Berulang & Interaksi (`handSlot` / Dunia):** Catat ID, skala relatif terhadap `1 H`, palet, orientasi, dan daftar *state* (mis. `sakelar: off → on`, `pintu: tertutup → terbuka`, `dinding: utuh → terangkat`, `gelas: penuh → kosong`) di tabel **§4D** `templates/production-brief.md` bila prop muncul pada ≥ 2 adegan atau berubah kondisi.
- **Kontinuitas Lokasi:** Jika dua panggung menggambarkan lokasi yang sama dari jarak/sudut berbeda (mis. WS → CU), kunci warna dinding/lantai, *Light Direction Anchor*, arah pantulan kaca diagonal, serta posisi relatif furnitur utama agar penonton tidak merasa berpindah ruangan.
- **Aturan Pembuatan Prop Tanpa Referensi Eksplisit (`[INFERRED]`):** Prop generik (pohon, meja, kursi, rambu, tiang, kendaraan umum) boleh dibangun langsung tanpa meminta gambar user, selama mematuhi: (1) tangga skala `1 H` (`explainer-core.md` §4B), (2) *Color Script 70–20–10*, (3) *2-tone split shading* searah cahaya, dan (4) tanpa mengubah desainnya di adegan berikutnya tanpa alasan naratif.

