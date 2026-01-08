# PRESENTASI PENELITIAN
## Evaluasi Sistem CDM-CPM pada Reklamasi Pelabuhan Patimban Paket 6

---

## SLIDE 1: JUDUL PRESENTASI
**Judul:**
- Evaluasi Kinerja Sistem Cement Deep Mixing (CDM) dan Cement Pipe Mixing (CPM) terhadap Penurunan Konsolidasi Tanah Lunak pada Area Reklamasi Pelabuhan Patimban Paket 6

**Sub-judul:**
- Analisis Konsolidasi dan Pemodelan Numerik Menggunakan Finite Element Method (Plaxis 2D v8.6)

**Identitas:**
- [Nama Mahasiswa]
- [Program Studi/Institusi]
- [Tahun]

---

## SLIDE 2: LATAR BELAKANG (1)
**Poin-poin Utama:**
- Pelabuhan Patimban sebagai Proyek Strategis Nasional
  - Peran penting dalam efisiensi logistik nasional
  - Mendukung pertumbuhan ekonomi sektor otomotif dan perdagangan internasional
  
- Tantangan Geoteknik
  - Reklamasi skala besar di atas tanah sangat lunak (soft marine clay)
  - Nilai N-SPT: 0-2 (very soft)
  - Ketebalan lempung lunak: 20-30 meter

- Karakteristik Tanah Bermasalah
  - Permeabilitas rendah
  - Kompresibilitas tinggi
  - Daya dukung sangat rendah

---

## SLIDE 3: LATAR BELAKANG (2)
**Permasalahan Tanah Lunak:**
- Deformasi besar saat diberi beban timbunan
- Potensi penurunan konsolidasi jangka panjang yang signifikan
- Dampak pada kestabilan timbunan dan infrastruktur pelabuhan

**Keterbatasan Metode Konvensional:**
- Preloading + PVD membutuhkan waktu puluhan tahun
- Risiko shear failure pada undrained shear strength rendah
- Beban surcharge berpotensi menyebabkan kelongsoran

**Solusi:**
- Sistem kombinasi Cement Deep Mixing (CDM) dan Cement Pipe Mixing (CPM)

---

## SLIDE 4: RUMUSAN MASALAH
**Pertanyaan Penelitian:**
1. Bagaimana karakteristik penurunan konsolidasi tanah dasar tanpa perbaikan?

2. Bagaimana respons deformasi dan perilaku konsolidasi dengan sistem CDM–CPM berdasarkan FEM (Plaxis 2D v8.6)?

3. Seberapa besar perbedaan penurunan antara kondisi tanpa perbaikan dan dengan CDM–CPM?

4. Sejauh mana efektivitas sistem CDM–CPM dalam meningkatkan stabilitas dan mengurangi penurunan?

---

## SLIDE 5: TUJUAN PENELITIAN
**Tujuan Utama:**
1. Menganalisis besarnya penurunan konsolidasi tanah dasar tanpa perbaikan (baseline)

2. Melakukan pemodelan numerik FEM untuk mempelajari perilaku konsolidasi dengan CDM–CPM

3. Membandingkan hasil penurunan kondisi tanpa dan dengan perbaikan

4. Mengevaluasi kemampuan sistem CDM–CPM dalam meningkatkan stabilitas dan mengendalikan penurunan jangka panjang

---

## SLIDE 6: BATASAN MASALAH
**Ruang Lingkup Penelitian:**
- **Area Studi:** Reklamasi Pelabuhan Patimban Paket 6

- **Data:** Mengacu pada dokumen Patimban Port Development Study (2020)

- **Metode Analisis:**
  - Teori Konsolidasi 1D Terzaghi (kondisi tanpa perbaikan)
  - Plaxis 2D v8.6 dengan FEM (kondisi dengan perbaikan)

- **Batasan Teknis:**
  - Analisis plane strain (2D)
  - Fokus: penurunan konsolidasi dan disipasi tekanan air pori
  - Tidak termasuk: analisis gempa, likuifaksi, creep sekunder, deformasi lateral

---

## SLIDE 7: MANFAAT PENELITIAN
**Manfaat Akademis:**
- Kontribusi ilmiah studi kasus CDM–CPM pada proyek reklamasi
- Referensi penelitian perbaikan tanah berbasis FEM
- Memperkuat literatur analisis konsolidasi dan evaluasi kinerja

**Manfaat Praktis:**
- Dasar evaluasi teknis untuk konsultan dan kontraktor Pelabuhan Patimban
- Masukan perancangan dan pelaksanaan CDM–CPM pada proyek serupa
- Bantuan pengambilan keputusan teknis pengendalian penurunan jangka panjang

---

## SLIDE 8: TINJAUAN PUSTAKA (1)
**Tanah Lunak (Soft Marine Clay):**
- **Definisi:** Lempung dengan kuat geser undrained (su) rendah, kadar air tinggi, permeabilitas sangat kecil

- **Karakteristik Patimban:**
  - Layer 1: N-SPT ≈ 0 (very soft)
  - Layer 2: N-SPT 1-4 (very soft)
  - Layer 3: N-SPT 4-8 (soft to stiff)
  - Layer 4: N-SPT 8-15 (stiff)
  - Layer 5: N-SPT >15 (very stiff to hard)

**Penurunan Tanah (Settlement):**
- St = Si + Sc + Ss
- Penurunan segera (Si)
- Penurunan konsolidasi primer (Sc) ← Fokus penelitian
- Penurunan konsolidasi sekunder (Ss)

---

## SLIDE 9: TINJAUAN PUSTAKA (2)
**Teori Konsolidasi Terzaghi:**
- Proses berkurangnya volume tanah jenuh akibat keluarnya air pori
- Pengalihan beban dari air pori ke struktur partikel tanah
- Peningkatan tegangan efektif → partikel merapat → volume berkurang

**Tiga Fase Konsolidasi:**
1. **Initial Loading:** Δu = Δσ, Δσ' = 0 (beban dipikul air pori)
2. **Primary Consolidation:** u menurun, σ' meningkat (transfer beban bertahap)
3. **End of Primary:** u = 0, σ' = Δσ (konsolidasi selesai)

**Formula Penurunan:**
- Sc = (Cc × H)/(1 + e₀) × log((P₀ + ΔP)/P₀)

---

## SLIDE 10: METODE PERBAIKAN TANAH
**Metode Konvensional:**
- **Preloading + PVD:**
  - Mempercepat konsolidasi dengan jalur drainase vertikal
  - Keterbatasan: waktu lama pada tanah sangat lunak tebal (>20-30m)
  - Risiko shear failure pada beban surcharge tinggi

**Metode Non-Konvensional:**
- **Deep Soil Mixing / CDM:**
  - Meningkatkan kekuatan dan kekakuan tanah secara langsung
  - Transfer beban ke lapisan keras melalui kolom kaku
  - Reduksi penurunan: 50-70%
  - Waktu konstruksi singkat

---

## SLIDE 11: SISTEM CDM-CPM (1)
**Cement Deep Mixing (CDM):**
- Pencampuran in-situ tanah lunak dengan semen
- Membentuk kolom tanah-semen keras
- Diameter: 1,3 m | Spacing: 4,4 m
- Fungsi: Rigid inclusion untuk transfer beban
- Modulus elastisitas: 10-100x tanah lunak

**Cement Pipe Mixing (CPM):**
- Pengolahan material hasil pengerukan (dredged material)
- Pencampuran lumpur + semen dalam pipa (in-line mixing)
- Berat isi: 12-15 kN/m³ (vs konvensional: 18-20 kN/m³)
- Fungsi: Material timbunan ringan untuk mengurangi beban vertikal

---

## SLIDE 12: SISTEM CDM-CPM (2)
**Kelebihan Sistem Kombinasi CDM–CPM:**
1. Pengurangan settlement lebih efektif (kombinasi penurunan beban + pengalihan beban)
2. Peningkatan stabilitas timbunan signifikan
3. Waktu konstruksi lebih singkat (tidak perlu menunggu konsolidasi lama)
4. Efisiensi biaya dan sumber daya
5. Keberlanjutan lingkungan (40% material pengerukan digunakan kembali)
6. Pengurangan dampak lingkungan (cement dengan 55% slag content)
7. Percepatan konstruksi hingga 2/3 waktu konvensional
8. Pengendalian settlement terbukti efektif di lapangan

---

## SLIDE 13: METODE PENELITIAN
**Pendekatan:**
- Metode kuantitatif–analitis dengan studi kasus
- Area: Reklamasi Pelabuhan Patimban Paket 6

**Tahapan Analisis:**
1. **Analisis Teoritis:**
   - Teori konsolidasi 1D Terzaghi
   - Baseline condition (tanpa perbaikan)

2. **Analisis Numerik:**
   - Finite Element Method (FEM)
   - Software: Plaxis 2D versi 8.6
   - Pemodelan perilaku tanah dengan CDM–CPM

3. **Evaluasi:**
   - Perbandingan hasil kedua kondisi
   - Penilaian efektivitas sistem

---

## SLIDE 14: PEMODELAN NUMERIK (1)
**Setup Model Plaxis 2D v8.6:**
- **Model Type:** Plane Strain
- **Element Type:** 15-node triangular elements
- **Sistem Satuan:** kN – m – day
- **Jenis Analisis:** Consolidation Analysis (coupled)

**Model Konstitutif:**
- **Tanah Lunak:** Soft Soil Model (SSM)
  - Memperhitungkan stress-dependent stiffness
  - Parameter: γsat, e₀, Cc, Cs, kv, kh
  
- **CDM & CPM:** Linear Elastic Model
  - CDM: Material kaku (E tinggi)
  - CPM: Material ringan (γ ≈ 15 kN/m³)

---

## SLIDE 15: PEMODELAN NUMERIK (2)
**Parameter Input Tanah Lunak (Soft Soil Model):**
| Parameter | Simbol | Satuan | Nilai Tipikal |
|-----------|--------|---------|---------------|
| Berat isi jenuh | γsat | kN/m³ | 15-16 |
| Void ratio awal | e₀ | - | 1.5-2.0 |
| Compression index | Cc | - | 0.6-1.2 |
| Swelling index | Cs | - | 0.05-0.15 |
| Permeabilitas | k | m/day | 1E-4–1E-3 |
| OCR | - | - | 1.0 (NC) |

**Parameter CDM:**
- Diameter: 1,3 m | Spacing: 4,4 m
- Modulus: 2E7 – 5E7 kN/m²
- Embedded Pile Row

**Parameter CPM:**
- Berat isi: ±15 kN/m³
- Modulus: 5E4 – 1E5 kN/m²

---

## SLIDE 16: PROSEDUR ANALISIS
**Tahapan Konstruksi (Staged Construction):**

1. **Initial Phase**
   - K₀ Procedure untuk tegangan awal
   - Kondisi normally consolidated

2. **Phase Pemasangan CDM**
   - Aktivasi Embedded Pile Row
   - Peningkatan kekakuan sistem tanah

3. **Phase Penimbunan CPM**
   - Aktivasi timbunan bertahap
   - Beban ringan CPM

4. **Phase Konsolidasi**
   - Consolidation Analysis time-dependent
   - Observasi disipasi tekanan air pori
   - Kurva settlement vs time

---

## SLIDE 17: OUTPUT ANALISIS
**Hasil yang Dievaluasi:**

1. **Penurunan Total (Settlement)**
   - Nilai penurunan di permukaan timbunan
   - Deformasi vertikal lapisan tanah lunak kritis

2. **Kurva Settlement – Time**
   - Laju konsolidasi tanah lunak
   - Waktu mencapai kondisi stabil

3. **Distribusi Tekanan Air Pori Berlebih**
   - Excess pore pressure pada berbagai kedalaman
   - Mekanisme disipasi air pori

4. **Pola Deformasi Vertikal**
   - Zona penurunan paling signifikan
   - Evaluasi keseragaman penurunan

---

## SLIDE 18: HASIL PENELITIAN (Contoh Format)
**[Slide ini untuk hasil aktual dari analisis Anda]**

**Perbandingan Penurunan:**
- Tanpa Perbaikan: ___ cm
- Dengan CDM–CPM: ___ cm
- Reduksi Penurunan: ___ %

**Waktu Konsolidasi:**
- Tanpa Perbaikan: ___ hari
- Dengan CDM–CPM: ___ hari
- Percepatan: ___ %

**Grafik/Diagram:**
- Kurva settlement vs time (perbandingan)
- Distribusi excess pore pressure
- Pola deformasi vertikal

---

## SLIDE 19: KESIMPULAN
**Kesimpulan Utama:**

1. Sistem CDM–CPM terbukti efektif dalam mengurangi penurunan konsolidasi tanah lunak pada area reklamasi Pelabuhan Patimban Paket 6

2. Kombinasi kolom CDM (rigid inclusion) dan timbunan ringan CPM bekerja secara sinergis:
   - CDM: Meningkatkan kekakuan dan mentransfer beban
   - CPM: Mengurangi beban vertikal pada tanah dasar

3. Pemodelan numerik FEM (Plaxis 2D v8.6) memberikan prediksi yang realistis terhadap perilaku konsolidasi jangka panjang

4. Metode ini lebih efisien dibanding metode konvensional dalam hal waktu konstruksi dan pengendalian settlement

---

## SLIDE 20: REKOMENDASI & PENUTUP
**Rekomendasi:**
- Sistem CDM–CPM dapat diaplikasikan pada proyek reklamasi serupa dengan kondisi tanah lunak tebal
- Perlu monitoring jangka panjang untuk validasi hasil pemodelan
- Studi lanjutan dapat mencakup analisis 3D dan variasi konfigurasi CDM

**Keterbatasan Penelitian:**
- Analisis terbatas pada kondisi plane strain (2D)
- Tidak membahas pengaruh gempa dan likuifaksi
- Fokus pada penurunan vertikal (tanpa deformasi lateral)

**Penutup:**
- Terima kasih atas perhatiannya
- Terbuka untuk diskusi dan pertanyaan

---

## CATATAN UNTUK EDITING PPT:
- Setiap "SLIDE" di atas adalah 1 halaman presentasi
- Total: 20 slide
- Format poin-poin memudahkan copy-paste ke PowerPoint
- Tambahkan gambar/diagram dari file referensi yang ada
- Sesuaikan warna dan template sesuai kebutuhan institusi
- Isi data hasil aktual pada Slide 18 setelah analisis selesai
