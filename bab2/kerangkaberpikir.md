2.3	Kerangka Berpikir
Penelitian konsolidasi tanah lunak pada area reklamasi dilakukan dengan membandingkan dua pendekatan perbaikan: metode konvensional menggunakan Prefabricated Vertical Drain (PVD) + preloading dan teknologi perbaikan tanah dari Jepang berupa Cement Deep Mixing (CDM) serta Cement Pipe Mixing (CPM). Pendekatan analitis menggunakan teori konsolidasi satu dimensi Terzaghi dipakai sebagai estimasi awal, dan perangkat lunak Plaxis 2D digunakan untuk memodelkan proses konsolidasi secara time-dependent, termasuk distribusi tekanan air pori dan perubahan tegangan efektif. Fokus penelitian diarahkan pada kondisi soft–very soft clay yang umum dijumpai di proyek reklamasi pelabuhan di Indonesia, dengan target evaluasi waktu konsolidasi, besaran settlement residual, serta stabilitas konstruksi.

**Alur Logika Penelitian (Percabangan Opsi Perbaikan):**

```
Studi Literatur & Data Lapangan
(Karakteristik Tanah Dasar, Geometri Reklamasi)
             ↓
      ┌──────────────────┐
      │ Analisis Kondisi │
      │   Awal Tanah     │
      │   (Baseline)     │
      └────────┬─────────┘
               ↓
      ┌──────────────────────────┐
      │ Estimasi Konsolidasi     │
      │ Tanpa Perbaikan (1D)     │
      └────────┬─────────────────┘
               ↓
      ┌──────────────────────────┐
      │ Pemodelan Numerik Awal   │
      │ (Plaxis 2D)              │
      └────────┬─────────────────┘
               ↓
      ┌──────────────────────────┐
      │  Verifikasi/Kalibrasi    │
      │  Parameter Model         │
      └───────┬──────────────────┘
              ↓
        ┌─────────────┬─────────────┐
        │             │             │
        │  Opsi A     │  Opsi B     │
        │  PVD +      │  CDM–CPM    │
        │  Preloading │  (Teknologi │
        │             │  Jepang)    │
        └──────┬──────┴──────┬──────┘
               │             │
   ┌───────────▼──────────┐  │  ┌───────────▼──────────┐
   │ Desain PVD (spasi,   │  │  │ Desain Grid Kolom,   │
   │ smear, waktu) +      │  │  │ Diameter, Kuat Tekan │
   │ Preloading/Surcharge │  │  │ Target, CPM piping   │
   └───────────┬──────────┘  │  └───────────┬──────────┘
               ↓             │              ↓
   ┌────────────────────────┐ │  ┌────────────────────────┐
   │ Pemodelan Numerik PVD │ │  │ Pemodelan Numerik CDM– │
   │ (Konsolidasi + drain) │ │  │ CPM (Stiffness &       │
   │ di Plaxis 2D          │ │  │ Percepatan Konsolidasi)│
   └───────────┬───────────┘ │  └───────────┬───────────┘
               ↓             │              ↓
   ┌────────────────────────┐ │  ┌────────────────────────┐
   │ Hasil: Waktu konsolidasi│ │  │ Hasil: Waktu konsolidasi│
   │ & settlement residual,  │ │  │ & settlement residual,  │
   │ stabilitas selama preload│ │ │ stabilitas meningkat     │
   └───────────┬───────────┘ │  └───────────┬───────────┘
               └──────┬──────┴──────┬───────┘
                      ↓             ↓
              ┌──────────────────────────┐
              │  Perbandingan & Evaluasi │
              │  (Waktu, Settlement,     │
              │  Stabilitas, Biaya,      │
              │  Ketersediaan Alat)      │
              └───────────┬──────────────┘
                          ↓
              ┌──────────────────────────┐
              │  Keputusan:              │
              │  Tanah "very soft"      │
              │  dengan target waktu     │
              │  ketat → pilih CDM–CPM  │
              └──────────────────────────┘
```

Gambar 2.1 Kerangka Berpikir (Percabangan PVD vs CDM–CPM)
Sumber: Analisis Penulis 2025

**Ringkasan Kelebihan & Kelemahan**

- PVD + Preloading:
  - Kelebihan: biaya relatif lebih rendah; teknologi umum dan ketersediaan alat tinggi; konsep desain sederhana; efektif pada soft clay dengan kuat geser cukup dan waktu konstruksi longgar.
  - Kelemahan: waktu konsolidasi panjang; risiko smear mengurangi efisiensi drain; membutuhkan preload besar sehingga ada isu stabilitas (lateral spreading) pada very soft clay; kontrol settlement residual sering kurang pada jadwal ketat.

- CDM–CPM:
  - Kelebihan: peningkatan kekuatan geser dan modulus segera; mempercepat konsolidasi dan menurunkan settlement residual; sangat efektif pada very soft clay; fleksibel untuk kombinasi grid kolom dan pipa mixing pada area luas.
  - Kelemahan: biaya awal lebih tinggi; memerlukan peralatan khusus dan kendali mutu; sensitivitas terhadap variasi kualitas pencampuran di lapangan.

Kerangka berpikir ini menempatkan verifikasi model sebagai tahap wajib sebelum simulasi opsi perbaikan. Hasil kedua cabang dibandingkan dengan kriteria proyek (waktu konstruksi, batas settlement, stabilitas, biaya, dan ketersediaan alat). Untuk kondisi very soft clay dan tenggat konstruksi ketat, keputusan mengerucut pada pemilihan sistem CDM–CPM sebagai solusi yang lebih aman dan efektif.
