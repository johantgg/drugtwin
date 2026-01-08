2.3	Kerangka Berpikir

Kerangka berpikir penelitian ini disusun secara sistematis untuk mengevaluasi efektivitas sistem perbaikan tanah CDM–CPM dalam mengendalikan penurunan konsolidasi pada area reklamasi Pelabuhan Patimban Paket 6. Kerangka ini dimulai dari identifikasi permasalahan geoteknik di lapangan, dilanjutkan dengan pengumpulan data, analisis menggunakan pendekatan analitis dan numerik, hingga evaluasi kinerja sistem perbaikan tanah yang diterapkan.

┌─────────────────────────────────────────────────────────────────────────────────────────────────────┐
│  EVALUASI KINERJA SISTEM PERBAIKAN TANAH CDM–CPM TERHADAP PENURUNAN KONSOLIDASI TANAH LUNAK       │
│                  PADA REKLAMASI PELABUHAN PATIMBAN PAKET 6                                          │
└─────────────────────────────────────────────────────────────────────────────────────────────────────┘
                                                    ↓
┌─────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                    Identifikasi Masalah                                             │
├─────────────────────────────────────────────────────────────────────────────────────────────────────┤
│  Masalah:                                              Wawancara/Observasi Lapangan:                │
│                                                                                                     │
│  • Reklamasi Pelabuhan Patimban dibangun di atas       • Kondisi tanah dasar sangat lunak          │
│    tanah lunak (soft marine clay) dengan               • Data hasil investigasi geoteknik:         │
│    ketebalan >20 meter dan N-SPT 0-2                     - Boring log dan SPT                      │
│  • Potensi penurunan konsolidasi yang sangat             - Uji laboratorium tanah                  │
│    besar dan waktu konsolidasi lama                      - Profil stratigrafi                      │
│  • Metode konvensional (PVD-Preloading) kurang         • Geometri timbunan reklamasi               │
│    efektif untuk kondisi tanah sangat lunak            • Sistem perbaikan tanah yang diterapkan:   │
│  • Perlu evaluasi efektivitas sistem CDM–CPM             - Kolom CDM (D=1.3m, s=4.4m)              │
│    yang telah diterapkan di lapangan                     - Material timbunan CPM (lightweight)     │
│                                                                                                     │
└─────────────────────────────────────────────────────────────────────────────────────────────────────┘
                                                    ↓
                    ┌─────────────────────────────────────────────┐
                    │            Studi Literatur                  │
                    └─────────────────────────────────────────────┘
                                       ↓
        ┌──────────────────────────────┬──────────────────────────────┐
        ↓                              ↓                              ↓
┌──────────────────┐          ┌──────────────────┐          ┌──────────────────┐
│ Teori Konsolidasi│          │ Metode Perbaikan │          │  Metode Elemen   │
│   Tanah Lunak    │          │      Tanah       │          │   Hingga (FEM)   │
└──────────────────┘          └──────────────────┘          └──────────────────┘
                                       ↓
┌─────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                             Tujuan                                                  │
├─────────────────────────────────────────────────────────────────────────────────────────────────────┤
│  1. Menganalisis besarnya penurunan konsolidasi tanah dasar tanpa perbaikan (baseline condition)   │
│  2. Melakukan pemodelan numerik untuk mempelajari perilaku konsolidasi dengan sistem CDM–CPM       │
│  3. Membandingkan penurunan antara kondisi tanpa perbaikan dan dengan sistem CDM–CPM               │
│  4. Mengevaluasi efektivitas sistem CDM–CPM dalam meningkatkan stabilitas dan mengendalikan        │
│     penurunan jangka panjang pada tanah dasar                                                       │
└─────────────────────────────────────────────────────────────────────────────────────────────────────┘
                                                    ↓
┌─────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                          ANALISIS DAN PEMODELAN PENURUNAN KONSOLIDASI                               │
└─────────────────────────────────────────────────────────────────────────────────────────────────────┘
                                                    ↓
                          ┌─────────────────────────────────────────────┐
                          │    Pengumpulan dan Pengolahan Data          │
                          │    Geoteknik Lapangan (Paket 6)             │
                          └────────────────────┬────────────────────────┘
                                               ↓
                ┌──────────────────────────────┴──────────────────────────────┐
                ↓                                                              ↓
┌───────────────────────────────────┐                      ┌───────────────────────────────────────┐
│   KONDISI TANPA PERBAIKAN         │                      │   KONDISI DENGAN SISTEM CDM–CPM       │
│      (Baseline Scenario)          │                      │      (Improved Scenario)              │
└───────────────┬───────────────────┘                      └────────────────┬──────────────────────┘
                ↓                                                            ↓
┌───────────────────────────────────┐                      ┌───────────────────────────────────────┐
│  Analisis Konsolidasi 1D          │                      │  Pemodelan Numerik FEM                │
│  (Metode Terzaghi)                │                      │  (Plaxis 2D versi 8.6)                │
├───────────────────────────────────┤                      ├───────────────────────────────────────┤
│  • Identifikasi lapisan tanah     │                      │  • Pemodelan geometri dan stratigrafi │
│    kompresibel                    │                      │  • Input parameter tanah dan material │
│  • Perhitungan tegangan           │                      │  • Pemodelan kolom CDM sebagai        │
│    overburden (P₀)                │                      │    rigid inclusion                    │
│  • Perhitungan tambahan           │                      │  • Pemodelan timbunan CPM sebagai     │
│    tegangan (ΔP)                  │                      │    lightweight material               │
│  • Estimasi penurunan total       │                      │  • Analisis staged construction:      │
│    menggunakan formula:           │                      │    - Initial stress (K₀ procedure)    │
│    Sc = (Cc×H)/(1+e₀)×log(P₀+ΔP)/P₀│                     │    - Instalasi kolom CDM              │
│  • Perhitungan waktu              │                      │    - Penimbunan bertahap CPM          │
│    konsolidasi (U=90%)            │                      │    - Konsolidasi time-dependent       │
│                                   │                      │  • Model konstitutif: Soft Soil Model │
└───────────────┬───────────────────┘                      └────────────────┬──────────────────────┘
                ↓                                                            ↓
┌───────────────────────────────────┐                      ┌───────────────────────────────────────┐
│  Output Analisis Baseline:        │                      │  Output Analisis Numerik:             │
├───────────────────────────────────┤                      ├───────────────────────────────────────┤
│  • Penurunan total (Sc)           │                      │  • Deformasi vertikal total           │
│  • Waktu konsolidasi (t₉₀)        │                      │  • Distribusi settlement              │
│  • Derajat konsolidasi (U)        │                      │  • Kurva settlement vs waktu          │
│                                   │                      │  • Excess pore pressure (Δu)          │
│                                   │                      │  • Deformasi lateral                  │
│                                   │                      │  • Safety Factor (SF)                 │
└───────────────┬───────────────────┘                      └────────────────┬──────────────────────┘
                ↓                                                            ↓
                └──────────────────────┬───────────────────────────────────┘
                                       ↓
                    ┌─────────────────────────────────────────────┐
                    │  PERBANDINGAN DAN EVALUASI KINERJA          │
                    ├─────────────────────────────────────────────┤
                    │  • Reduksi penurunan total (%)              │
                    │  • Percepatan waktu konsolidasi             │
                    │  • Peningkatan stabilitas (SF)              │
                    │  • Distribusi beban pada kolom CDM          │
                    │  • Kontribusi material ringan CPM           │
                    │  • Efektivitas sistem kombinasi CDM-CPM     │
                    └────────────────────┬────────────────────────┘
                                         ↓
┌─────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                            Hasil                                                    │
├─────────────────────────────────────────────────────────────────────────────────────────────────────┤
│  1. Besarnya penurunan konsolidasi tanah dasar tanpa perbaikan pada area reklamasi                 │
│     Pelabuhan Patimban Paket 6                                                                      │
│                                                                                                     │
│  2. Perilaku konsolidasi dan deformasi tanah setelah dilakukan perbaikan menggunakan               │
│     sistem CDM–CPM berdasarkan hasil pemodelan numerik FEM                                          │
│                                                                                                     │
│  3. Persentase reduksi penurunan tanah antara kondisi tanpa perbaikan dan kondisi dengan           │
│     perbaikan CDM–CPM, serta evaluasi efektivitasnya                                                │
│                                                                                                     │
│  4. Kesimpulan tentang kemampuan sistem CDM–CPM dalam meningkatkan stabilitas dan                  │
│     mengendalikan penurunan jangka panjang pada tanah dasar area reklamasi                         │
│                                                                                                     │
│  5. Rekomendasi teknis untuk penerapan sistem CDM–CPM pada proyek reklamasi serupa                 │
└─────────────────────────────────────────────────────────────────────────────────────────────────────┘

Gambar 2.X Kerangka Berpikir Penelitian
Sumber: Analisis Penulis (2026)

**Penjelasan Alur Kerangka Berpikir:**

Kerangka berpikir penelitian ini menggambarkan alur sistematis dalam mengevaluasi kinerja sistem perbaikan tanah CDM–CPM pada area reklamasi Pelabuhan Patimban Paket 6. Penelitian dimulai dari **tahap identifikasi masalah** yang mengidentifikasi tantangan geoteknik berupa potensi penurunan konsolidasi besar pada tanah lunak dengan ketebalan >20 meter. Identifikasi masalah diperkuat melalui **wawancara dan observasi lapangan** meliputi kondisi tanah dasar, data boring dan SPT, hasil uji laboratorium, serta dokumentasi sistem perbaikan yang telah diterapkan (kolom CDM berdiameter 1,3 m dengan jarak 4,4 m dan timbunan ringan CPM).

**Studi literatur** dilakukan secara menyeluruh mencakup tiga pilar pengetahuan: (1) teori konsolidasi tanah lunak dan Terzaghi untuk pendekatan analitis, (2) metode perbaikan tanah meliputi teknik konvensional dan teknologi mutakhir, dan (3) metode elemen hingga (FEM) untuk analisis numerik. Berdasarkan pemahaman masalah dan fondasi literatur, ditetapkan **tujuan penelitian** yang spesifik yaitu menganalisis penurunan baseline, memodelkan perilaku dengan CDM–CPM, membandingkan kondisi, dan mengevaluasi efektivitas sistem.

Inti penelitian terdapat pada tahap **analisis dan pemodelan penurunan konsolidasi** yang mengikuti alur **percabangan parallel**: (1) **skenario tanpa perbaikan (baseline)** dianalisis menggunakan metode analitis Terzaghi 1D dengan perhitungan sistematis penurunan total, waktu konsolidasi, dan derajat konsolidasi, serta (2) **skenario dengan sistem CDM–CPM** dimodelkan numerik menggunakan Plaxis 2D dengan merepresentasikan geometri aktual, sifat material (tanah lunak, kolom CDM rigid, material ringan CPM), analisis bertahap sesuai konstruksi, dan perhitungan tegangan pori, deformasi, serta faktor keamanan secara time-dependent.

Output dari kedua skenario selanjutnya masuk ke tahap **perbandingan dan evaluasi kriteria**, di mana enam indikator kinerja dievaluasi: reduksi penurunan total (%), percepatan waktu konsolidasi, peningkatan stabilitas (SF), distribusi beban pada kolom CDM, kontribusi material ringan CPM, dan efektivitas sistem kombinasi CDM–CPM. Hasil perbandingan ini membentuk dasar untuk **tahap hasil akhir** penelitian, yang menghasilkan (1) kuantifikasi penurunan baseline area reklamasi, (2) perilaku konsolidasi dengan CDM–CPM berbasis FEM, (3) persentase reduksi dan efektivitas, (4) kesimpulan tentang kemampuan sistem dalam mengendalikan settlement jangka panjang, dan (5) rekomendasi teknis untuk penerapan pada proyek reklamasi serupa. Alur ini memastikan bahwa keputusan teknis berbasis data dan validasi numerik yang ketat.
