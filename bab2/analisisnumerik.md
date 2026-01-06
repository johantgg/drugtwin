2.1.9 	Analisis Numerik dengan FEM  (Plaxis 2D versi 8.6)
2.1.9.1 Finite Element Method dan Plaxis 2D
Metode Elemen Hingga (Finite Element Method/FEM) merupakan pendekatan numerik berbasis komputer yang sangat berdaya guna digunakan untuk menganalisis perilaku kompleks tanah dan struktur dalam teknik geoteknik. Dalam praktiknya, dengan menggunakan software Plaxis 2D versi 8.6 karena kemampuannya dalam mensimulasikan kondisi plane strain (regangan bidang) untuk deformasi dan stabilitas.
Metode ini bekerja dengan membagi domain analisis (geometri tanah dan struktur) menjadi elemen-elemen kecil (mesh) berhingga yang saling terhubung melalui titik nodal. Perilaku setiap elemen diatur oleh persamaan konstitutif material dan persamaan keseimbangan. Penyelesaian sistem persamaan linier atau non-linier global menghasilkan estimasi perpindahan (displacement), tegangan (stress), regangan (strain), dan tekanan air pori di seluruh domain analisis (Brinkgreve et al., 2019). Keunggulan utama penggunaan Plaxis 2D dalam analisis perbaikan tanah adalah kemampuan untuk melakukan analisis terkopel (coupled analysis) yang menggabungkan deformasi tanah dan aliran air pori secara simultan selama proses konsolidasi.

2.1.9.2 Pemodelan Tanah Lunak “Mohr-Coulomb vs Soft Soil Model”
Untuk pemodelan numerik, akurasi parameter input sangat menentukan hasil analisis. Parameter dasar yang dibutuhkan adalah Modulus Elastisitas (E) dan Angka Poisson (v). Menurut Bowles (1977), nilai modulus elastisitas tanah (E) dan angka poisson (v) dapat ditaksir berdasarkan jenis tanah dan konsistensinya. Misalnya, untuk lempung lunak (N-SPT < 4), nilai E berkisar antara 2000–4000 kN/m2 dan v antara 0.40–0.50. Selain itu, nilai Angka Pori (e) lempung lunak berkisar antara 0.90–1.40.
Namun, dalam simulasi numerik tingkat lanjut untuk tanah lempung yang sangat lunak dan kompresibel, model konstitutif Mohr-Coulomb (MC) seringkali dianggap kurang memadai karena mengasumsikan kekakuan elastis yang konstan (linier). Oleh karena itu, literatur ilmiah menyarankan penggunaan Soft Soil Model (SSM) atau Soft Soil Creep Model. Model ini lebih realistis karena memperhitungkan kekakuan yang bergantung pada tegangan (stress-dependent stiffness) dan membedakan antara kekakuan pembebanan (loading) serta pembongkaran beban (unloading/reloading).
Dalam Plaxis, parameter kekakuan untuk Soft Soil Model diturunkan dari uji konsolidasi laboratorium, yaitu:
1.	Modified compression index (۸* )
2.	Modified swelling index (* )
Hubungan parameter ini dengan indeks kompresi konvensional dijelaskan oleh Brinkgreve et al. (2019) sebagai berikut:
۸* = Cc / 2.3 ( 1 + e0 )
*  = 2Cs / 2.3 (1 + e0 )

2.1.9.3 Pemodelan CDM sebagai Rigid Inclusion dalam 2D
Kolom Cement Deep Mixing (CDM) dimodelkan sebagai material kaku (rigid inclusion atau embedded pile) yang memiliki perilaku elastis linier atau elasto-plastis dengan modulus elastisitas (Ecdm) dan kuat tekan yang jauh lebih besar dibandingkan tanah sekitarnya. Interaksi antara kolom CDM dan tanah lunak (gesekan selimut dan tahanan ujung) perlu dimodelkan secara cermat menggunakan elemen antarmuka (interface elements) untuk memungkinkan adanya slip dan gapping antara struktur dan tanah.
Tantangan utama dalam pemodelan 2D adalah representasi elemen kolom yang berbentuk silinder terpisah menjadi elemen bidang (plane strain). Untuk mengatasi hal ini, teknik Metode Kekakuan Ekuivalen (Equivalent Stiffness Method) sering diaplikasikan, di mana barisan kolom dan tanah di sekitarnya dimodelkan sebagai dinding komposit dengan permeabilitas dan kekakuan yang disesuaikan (Hamdhan et al., 2024). Parameter ekuivalen (Eeq dan ceq) dihitung berdasarkan rasio luas area perbaikan untuk menghasilkan perilaku deformasi yang representatif.

2.1.9.4 Pemodelan CPM sebagai Timbunan Ringan
Timbunan material CPM dimodelkan sebagai material timbunan dengan properti berat isi () rendah dan model konstitutif yang sesuai (misalnya elastis linier atau Mohr-Coulomb) untuk merepresentasikan kondisi aktual di lapangan, yaitu beban urugan yang relatif ringan namun stabil. Dalam simulasi Plaxis, material timbunan ini umumnya didefinisikan sebagai material drained (terdrainase) karena permeabilitasnya yang tinggi, sehingga tidak memicu tekanan air pori berlebih pada tubuh timbunan itu sendiri.

2.1.9.5 Tahapan Konstruksi (Staged Construction)
Proses analisis dalam Plaxis 2D dilakukan menggunakan fitur Staged Construction untuk merepresentasikan urutan kejadian di lapangan secara nyata. Tahapan analisis yang umum dilakukan meliputi:
1.	Fase Inisialisasi (K0 Procedure) yakni membangkitan tegangan in-situ akibat berat sendiri tanah.
2.	Fase Instalasi CDM yaitu aktivasi elemen kolom dan elemen antarmuka.
3.	Fase Penimbunan Bertahap (Consolidation Calculation) yang mana beban timbunan diaktifkan secara bertahap dalam kurun waktu tertentu. Tipe kalkulasi Consolidation dipilih untuk menghitung excess pore pressure yang timbul akibat beban.
4.	Fase Konsolidasi/Tunggu merupakan periode didiamkan tanpa penambahan beban untuk memungkinkan disipasi tekanan air pori hingga mencapai derajat konsolidasi yang diinginkan.

2.1.9.6 Output Analisis Numerik
Output utama dari analisis numerik FEM dengan menggunakan software Plaxis 2D versi 8.6 ini meliputi profil penurunan total (Stotal), deformasi vertikal dan lateral pada berbagai waktu (selama konstruksi dan operasional jangka panjang), serta distribusi tekanan air pori berlebih (e) dari waktu ke waktu yang menunjukkan kemajuan konsolidasi (Surbakti, 2021). Penurunan konsolidasi akhir dapat dievaluasi ketika kurva excess pore pressure telah meluruh mendekati nol.
Selain deformasi, analisis ini juga memberikan nilai Faktor Keamanan (FK) atau Safety Factor (SF) global menggunakan metode reduksi parameter kuat geser (Phi-c reduction). Metode ini secara bertahap mereduksi kohesi (c) dan sudut geser dalam (ϕ) tanah hingga terjadi keruntuhan numerik, memberikan indikasi stabilitas timbunan pada berbagai tahap konstruksi.
