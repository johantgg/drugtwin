2.1.3	Penurunan Tanah (Settlement)
	Pemberian beban di atas permukaan tanah akan memicu peningkatan tegangan pada partikel tanah yang berujung pada fenomena penurunan (settlement). Secara teknis, penurunan ini terjadi akibat deformasi struktur tanah serta keluarnya air atau udara dari rongga pori. Dalam proyek konstruksi di atas tanah lunak, besarnya penurunan konsolidasi menjadi tantangan utama.
	Menurut Terzaghi (1943) dan Das (2021), penurunan total (St) yang terjadi pada tanah akibat pembebanan dapat dibagi menjadi tiga komponen utama yang memiliki mekanisme dan waktu kejadian yang berbeda-beda, yaitu: (1) penurunan segera atau penurunan elastis (immediate settlement, Si), (2) penurunan konsolidasi primer (primary consolidation settlement, Sc), dan (3) penurunan konsolidasi sekunder atau creep (secondary compression, Ss). Ketiga komponen penurunan ini dapat dinyatakan dalam persamaan total sebagai berikut (Craig, 2004):

St = Si + Sc + Ss							(2.1)

	Ilustrasi hubungan antara ketiga komponen penurunan tersebut terhadap waktu ditunjukkan pada Gambar 2.5. Grafik ini menggambarkan karakteristik temporal dari setiap komponen penurunan, mulai dari fase konstruksi hingga operasional jangka panjang, yang menjadi dasar dalam perencanaan dan analisis geoteknik proyek reklamasi.

[GAMBAR 2.5: Diagram Komponen Penurunan Total]
Kurva settlement vs time menunjukkan tiga komponen penurunan: immediate settlement (Si), primary consolidation settlement (Sc), dan secondary compression (Ss). Fase konstruksi ditandai dengan penurunan segera yang terjadi instan, diikuti penurunan konsolidasi primer yang berkembang secara logaritmik, dan penurunan sekunder yang berlanjut dalam jangka panjang pada fase operasional.
(Sumber: Adaptasi dari Terzaghi, 1943; Das, 2021)

	Gambar 2.5 dengan jelas memvisualisasikan bahwa penurunan segera (Si) terjadi secara instan pada saat beban diterapkan, yang direpresentasikan oleh lompatan vertikal awal pada kurva. Penurunan ini merupakan respons elastis tanah terhadap pembebanan dan tidak melibatkan perubahan kadar air. Selanjutnya, penurunan konsolidasi primer (Sc) berkembang secara bertahap mengikuti pola logaritmik terhadap waktu, mencerminkan proses disipasi tekanan air pori berlebih dan pengalihan beban secara progresif dari air pori ke kerangka butiran tanah. Fase ini merupakan komponen penurunan terbesar pada tanah lempung jenuh dan dapat berlangsung dari beberapa bulan hingga puluhan tahun tergantung pada ketebalan lapisan kompresibel dan kondisi drainase. Komponen terakhir adalah penurunan sekunder (Ss), yang terus berlanjut bahkan setelah tekanan air pori berlebih telah terdisipasi sepenuhnya, disebabkan oleh fenomena creep atau penataan ulang partikel tanah pada tegangan efektif konstan (Holtz & Kovacs, 1981; Lambe & Whitman, 1969).

2.1.3.1 Penurunan Konsolidasi Primer (Consolidation Settlement)
	Pemberian beban tambahan pada lapisan tanah jenuh air akan memicu peningkatan tekanan air pori. Fenomena ini memaksa air keluar dari rongga pori-pori tanah, sehingga terjadi reduksi volume yang dikenal sebagai penurunan konsolidasi primer. Besarnya penurunan pada tanah lunak sangat dipengaruhi oleh riwayat geologisnya, khususnya terkait tegangan pra-konsolidasi (beban maksimum historis). Apabila tegangan overburden efektif saat ini merupakan beban tertinggi yang pernah diterima tanah tersebut, maka tanah berada dalam kondisi normally consolidated. Dalam konteks proyek reklamasi pelabuhan, pemahaman terhadap proses konsolidasi primer sangat penting untuk memprediksi besarnya penurunan jangka panjang yang akan terjadi akibat beban timbunan (Arafianto et al., 2022). Estimasi penurunan untuk kondisi ini dapat dihitung menggunakan Persamaan 2.2.

Sc = (Cc × H)/(1 + e₀) × log((P₀ + ΔP)/P₀)				(2.2)

dengan :
Cc	= Indeks kompresi tanah,
P₀	= Tekanan overburden efektif,
ΔP	= Tambahan tegangan vertikal, 
e₀	= Angka pori awal,
H	= Tebal lapisan tanah, dan 
Sc	= Penurunan konsolidasi primer.

2.1.3.2 Parameter Perhitungan Penurunan
	Untuk menghitung penurunan konsolidasi primer, diperlukan parameter Tegangan Overburden Efektif (P₀). Parameter ini didefinisikan sebagai tegangan vertikal efektif tanah asli yang berasal dari beban lapisan tanah yang berada di atas titik pengamatan. Menurut Kristiadi et al. (2023), tegangan overburden efektif merupakan parameter kunci dalam perhitungan penurunan konsolidasi yang diperoleh dari hasil interpretasi data pengujian tanah di laboratorium maupun di lapangan. Nilai P₀ tersebut dapat ditentukan melalui kalkulasi dengan Persamaan 2.3.

P₀ = γ′ × H								(2.3)

dengan :
P₀	= Tegangan overburden efektif,
γ′	= Berat volume tanah efektif, dan 
H	= Tebal lapisan tanah.

	Parameter kritis lainnya dalam perhitungan penurunan konsolidasi adalah Indeks Kompresi (Cc), yang merupakan gradien kemiringan dari bagian lurus kurva virgin compression pada grafik hubungan angka pori (e) terhadap logaritma tekanan efektif (log p). Perilaku kompresibilitas tanah lempung lunak dalam merespons pembebanan dapat divisualisasikan secara komprehensif melalui kurva e-log p yang diperoleh dari uji konsolidasi satu dimensi (oedometer test) di laboratorium (Das, 2021; Craig, 2004). Kurva ini tidak hanya memberikan informasi mengenai besarnya kompresi yang akan terjadi, tetapi juga mengungkapkan riwayat tegangan geologis tanah, yang tercermin dari posisi tegangan pra-konsolidasi (preconsolidation pressure, Pc). Karakteristik kurva e-log p untuk tanah lempung lunak ditunjukkan pada Gambar 2.6.

[GAMBAR 2.6: Kurva e-log p (Compression Curve)]
Hubungan antara angka pori (e) dan logaritma tekanan efektif (log p) menunjukkan perilaku kompresibilitas tanah lempung. Kurva ini memperlihatkan garis recompression/rebound dengan kemiringan Cs pada daerah overconsolidated, titik kritis tegangan pra-konsolidasi (Pc), dan garis virgin compression dengan kemiringan Cc pada daerah normally consolidated. Nilai e₀ menunjukkan angka pori awal tanah sebelum pembebanan.
(Sumber: Das, 2021; Craig, 2004)

	Gambar 2.6 mengilustrasikan karakteristik perilaku tanah lempung terhadap pembebanan bertahap dalam kondisi konsolidasi satu dimensi. Kurva ini dibagi menjadi dua zona utama yang dipisahkan oleh tegangan pra-konsolidasi (Pc). Pada zona overconsolidated (σ' < Pc), tanah menunjukkan kekakuan yang relatif tinggi karena pernah mengalami tegangan yang lebih besar di masa lampau, sehingga kompresi yang terjadi mengikuti garis recompression atau rebound dengan kemiringan yang landai, dinyatakan oleh indeks rekompresi (Cs atau Cr). Nilai Cs umumnya berkisar antara 1/5 hingga 1/10 dari nilai Cc, mengindikasikan bahwa tanah overconsolidated mengalami penurunan yang jauh lebih kecil dibandingkan tanah normally consolidated pada kenaikan tegangan yang sama (Lambe & Whitman, 1969).
	Sebaliknya, pada zona normally consolidated (σ' ≥ Pc), tanah mengalami kompresi virgin yang signifikan karena tanah tersebut belum pernah menerima beban sebesar ini sebelumnya. Pada kondisi ini, kurva mengikuti garis virgin compression dengan kemiringan yang curam, dinyatakan oleh indeks kompresi (Cc). Parameter Cc merupakan faktor dominan yang mengendalikan besarnya penurunan konsolidasi primer untuk tanah normally consolidated, dan nilainya sangat dipengaruhi oleh karakteristik intrinsik tanah seperti batas cair (Liquid Limit), kadar air alami, dan kandungan mineral lempung (Das, 2021). Untuk tanah lempung lunak marine seperti yang ditemukan di lokasi reklamasi Pelabuhan Patimban, nilai Cc umumnya berkisar antara 0,3 hingga 0,6, mencerminkan kompresibilitas yang tinggi (Yoon & Kim, 2006; Samui et al., 2012).
	Penentuan posisi tegangan pra-konsolidasi (Pc) pada kurva e-log p merupakan aspek kritis dalam analisis penurunan, karena parameter ini menentukan apakah tanah dalam kondisi normally consolidated (OCR = 1), overconsolidated (OCR > 1), atau underconsolidated (OCR < 1). Over-Consolidation Ratio (OCR) didefinisikan sebagai rasio antara tegangan pra-konsolidasi dengan tegangan overburden efektif saat ini (OCR = Pc/P₀). Metode Casagrande merupakan prosedur standar yang paling umum digunakan untuk menentukan nilai Pc secara grafis dari kurva e-log p (ASTM D2435). Pemahaman yang akurat terhadap nilai OCR tanah dasar sangat penting dalam desain reklamasi, karena akan menentukan formula perhitungan penurunan yang tepat dan besarnya penurunan total yang akan terjadi (Holtz & Kovacs, 1981; Bowles, 1996).
