# SIG_Creating-Heatmaps

*Tutorial Creating Heatmaps

1. Download terlebih dahulu file 2019-02-surrey-streeet.csv

2. Pertama kita akan memuat layer peta dasar dari OpenStreetmap dan kemudian mengimpor data CSV. Pada tab browser, gulir ke bawah dan temukan bagian XYZ tiles (*Gambar 1*)

3. Bentangkan untk melihat layer petak OpenStreetMap. Seret dan lepas e kanvas utama. Selanjutnya kita akan memuat file CSV. Klik tombol Open Data Source Manager (*Gambar 2)

4. Beralih pada tab Delimited Text. Di sini kita akan mengimpor data kejahatan yang datang dalam file teks format CSV. Klik tombol ... di sebelah File name dan telusuri file 2019-01-surrey-street.csv yang didownload. Bidang X dan bidang Y di bagian Geometry Definition harus dibiarakan ke standar EPSG:4326 - WGS 84 Definition. Pastikan data terlihat benar di panel data sampel dan klik tambah diikuti dengn close (*Gambar 3*)

5. Kita akan melihat 2 lapisan OpenStreetMap dan 2019-02-surrey-street dimuat di panel lapisan QGIS. Klik kanan layer 2019-02-surrey-street dan pilih Zoom to Layer (*Gambar 4*)

6. Kita akan melihat layer pin insiden kejahatan dihamparkan pada peta dasar OpenStreetMap. Zoom dan Pan untuk menjelajahi data. Datany cukup padat dan sulit untuk mengetahui di mana terdapat konsentrasi kejahatan yang tinggi. Di sinilah visualisasi peta panas akan berguna. Pilih layer 2019-02-surrey-street dan klik tombol panel Open the Layer Styling (*Gambar 5*)

7. Pilih Heatmap sebagai perender di menu dropbox. Panel Layer Styling bersifat interaktif dan kita dapat segera melihat efek perubahan Kita tercermin di kanvas. Lapisan sekarang akan ditampilkan di jalan warna skala abu-abu default (*Gambar 6*)

8. Peta panas biasanya dirender menggunakan jalur kuning ke merah atau putih ke merah di mana konsentrasi titik yang lebih tinggi menghasilkan lebih banyak panas. Klik menu dropdown Color ramp dan pilih Reds color-ramp (*Gambar 7*)

9. Selanjutnya kita harus memilih Radius. Parameter ini menentukan lingkungan melingkar di sekitar setiap titik di mana titik tersebut akan memiliki pengaruh. Nilai ini akan sangat bergantung pada jenis data masukan kita. Untuk data anggap saja insiden kejahatan akan berdampak hingga 5 Kilometer dari lokasi. Perhatikan bahwa CRS proyek saat ini diatur ke EPSG: 3857 di pojok kanan bawah. CRS ini memiliki satuan meter, jadi kita harus menentukan 5000 meter sebagai radiusnya. Parameter lain yang disembunyikan dari menu ini adalah bentuk KErnel. Ini adalah fungsi yang menentukan bagaimana pengaruh suatu titik harus tersebar pada radius yang diberikan. Perender Heatmap menggunakan fungsi Quartic untuk perhitungan ini. Ada jenis kernel lain seperti Triangular, Uniform, Triweight, dan Epanechnikov yang dapat ditentukan saat menggunakan metode pembuatan peta panas berbeda ynag dijelaskan nanti di tutorial ini (*Gambar 8*)

10. Visualisai heatmap sudah siap. Kita bisa mengatur Opacity dari heatmap di bagian Layer Rendering di bagian bawah. Atur opacity menjadi 60% agar kita dapat melihat peta dasar beserta heatmap (*Gambar 9*)

11. Untuk banyak jenis analisis, hanya mempertimbangkan kerapatan poin sudah cukup baik. Namun terkadang kita mungkin ingin memberikan kepentingan yang berbeda untuk setiap poin. Kejahatan yang lebih kejam  seharusnta memiliki pengaruh lebih besar pada heatmap keluaran daripada perampokan. Demikian pula, kadang-kadang suatu titik dapat mewakili beberapa pengamatan di satu lokasi yang perlu diperhitungkan dalam analisis. Untuk melakukannya, kita dapat menyediakan kolom bobot numerik opsional yang menentukan nilai untuk setiap titik. Tambahkan bidan bobot dan gunakan untuk menyempurnakan heatmap. Klik kanan layer 2019-02-surrey-street dan pilih Open Attribute Table (*Gambar 10*)

12. Kita akan melihat bidang teks bernama Crime type di data input yang menjelaskan jenis kejahatan. Kita dapat menggunakan ini untuk mengkategorikan berbagai jenis kejahatan dan menetapkan bobot yang lebih tinggi untuk kejahatan yang lebih kejam (*Gambar 11*)

13. Klik Open field Calculator (*Gambar 12*)

14. Kita sekarang akan memasukkan rumus yang menggunakan jenis kejahatan dan menentukan nilai bobotnya. QGIS memiliki cara praktis untuk menambahkan bidang yang dihitung menggunakan Bidang virtual. Bidang virtual disimpan dalam proyek QGIS dan tidak mengubah data sumber. Ini juga dihitung secara dinamis dan dapat digunakan di mana saja di QGIS seperti halnya atribut lainnya. Masukkan bobot sebagai nama bidang keluaran dan atur jeni bidang keluaran ke bilangan bulat. Masukkan ekspresiberikut di editor ekspresi. Disini kita menggunakan pernyataan CASE untuk menetapkan nilai yang berbeda berdasarkan kondisi yang berbeda. CASE
WHEN "Crime type" LIKE 'Violence%' THEN 10
WHEN "Crime type" LIKE 'Criminal%' THEN 5
ELSE 1
END
lalu klik OK (*Gambar 13*)

15. Atribut baru akan ditambahkan untuk seyiap fitur dengan nilai bobot yang sesuai (*Gambar 14*) 

16. Kemablai ke Layer Styling panel, klik menu drop-down untuk weight points by dan pilih bidang bobot yang baru ditambahkan (*Gambar 15*)

17. Kita kan melihat perubahan rendering heatmap untuk memperhitungkan parameter bobot. Tutup Layer Styling panel 

18. Jika kita memerlukan visualisai heatmap untuk disimpan sebagai lapisan raster permanen atau ingin menyesuaikan heatmap dengan opsi lanjutan seperti kernel yang berbeda atau radius dinamis, Kita dapat menggunakan Heatmap (Estimasi kepadatan kernel) Processing Toolbox. (*Gambar 16*)

19. Sebelum kita membuat heatmap, kita perlu memproyeksikan ulang data sumber ke CRS yang diproyeksikan. Karen jarak memainkan peran penting dalam perhitungan heatmap, tidak benar menggunakan CRS geografis. Cari dan temukan algoritma vector general lalu pilih Reproject layer (*Gambar 17*)

20. Pada Reproject layer, klik tombol Select CRS untuk Target CRS. Cari dan pilih EPSG:27700 OSGB 1936 / British National Grid CRS. CRS yang diproyeksikan ini adalah pilihan yang baik untuk data di Inggris. Klik Run (*Gambar 18*)

21. Sebuah layer baru bernama Reprojected akan ditambahkan ke panel Layers. Hapus centang pada kotak di sebelah layer lama 2019-02-surrey-street untuk menyembunyikannya (*Gambar 19*)

22. Cari dan temukan Interpolation lalu heatmap (Kernel Density Estimation) (*Gambar 20*)

23. Pada Heatmap(Kernel Density Estimation), kita akan menggunakan parameter yang sama seperti sebelumnya. Pilih Radius sebagai 5000 meter dan weight from field sebagai weight from field sebagai weight. Atur ukuran pixel X dan ukuran pixel Y menjadi 50 meter. Biarkan Kernel membentuk nilai default Quartic. Klik Run (*Gambar 21*)

24. Setelag pemrosesan selesai, lapisan rater baru bernama OUTPUT akan dimuat. Visualisasi default jelek karena menggunakan penyaji abu-abu Singleband. KLik panel Open the Layer Styling (*Gambar 22*)

25. Ubah render menjadi Singleband Pseudocolor dan pilih jalur warna merah, Lapisan sekarang erlihat seperti visualisasiheatmap yang telah kita buat (*Gambar 23* *Gambar 24*)


