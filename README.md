# __PERBAIKAN JADWAL ROYALTRANS SESUAI POLA DAN KEBUTUHAN PENGGUNA__
# __1. Pendahuluan__
## 1.1 Latar Belakang
TransJakarta adalah layanan sistem transportasi umum baik berbasis bus rapid transit (BRT) maupun bus kecil / angkutan umum yang beroperasi di Jakarta. TransJakarta dikelola oleh Pemerintah Provinsi DKI Jakarta serta di bawah naungan Dinas Perhubungan DKI Jakarta sehingga pendanaan berasal dari anggaran pemerintah termasuk subsidi untuk tarif. TransJakarta bertujuan untuk mengurangi kemacetan atau ketergantungan pada kendaraan pribadi dan memberikan alternatif transportasi yang cepat dan efisien bagi warga Jakarta.

Fitur Utama:
1. Korridor / corridor: Terdapat beberapa koridor yang menghubungkan berbagai wilayah di Jakarta.
2. Halte / stops: Halte-halte TransJakarta dirancang untuk memberikan akses mudah dan nyaman bagi penumpang.
3. Tiket / payment: Sistem pembayaran menggunakan tiket elektronik atau kartu, memudahkan proses naik dan turun.

## 1.2 Permasalahan
Mengembalikan fitur utama TransJakarta yaitu melayanin rute-rute strategis, termasuk area perkantoran, pusat perbelanjaan dan pemukiman untuk mengurangi penggunaan kendaraan pribadi, utamanya pada rute-rute jauh. Mengingat sebagian besar pengguna kendaraan pribadi yang menyumbangkan kemacetan di ibukota dan sekitarnya adalah warga yang berdomisili tinggal di daerah pinggiran kota Jakarta namun bekerja di sekitar Jakarta. 

Tim Data Analis Dinas Perhubungan DKI Jakarta diminta menganalisis data penggunaan TransJakarta pada 1 bulan yaitu April 2023, khususnya pada rute RoyalTrans agar moda ini menjadi pilihan utama bagi warga yang berdomisili di luar wilayah sekitar Jakarta yang bekerja di Jakarta.
Di antaranya:
1. Melihat kebutuhan warga / pengguna melalui traffic per `corridor` / rute, melalui pola kebiasaan pengguna Royaltrans.
2. Perbaikan jadwal per `corridor` / rute agar dapat mengakomodir permintaan / kebutuhan warga pengguna.

# __2. Data__
Untuk menjawab permasalahan di atas, Tim Data Analis akan menganalisis data penggunaan TransJakarta April 2023. Dataset dapat diakses [di sini](https://www.kaggle.com/datasets/hieremiaskevin/pt-transjakarta-dataset-april-2023).

## 2.1 Tinjauan Data
Data set berisi 22 kolom yang menjelaskan informasi:
- transID           : ID transaksi unik untuk setiap transaksi
- payCardID         : Identifikasi utama pelanggan. Kartu yang pelanggan gunakan sebagai tiket masuk dan keluar.
- payCardBank       : Nama bank penerbit kartu pelanggan
- payCardName       : Nama pelanggan yang tercetak di kartu.
- payCardSex        : Jenis kelamin pelanggan yang tercetak di kartu
- payCardBirthDate  : Tahun kelahiran pelanggan
- corridorID        : ID Koridor / ID Rute sebagai kunci untuk pengelompokan rute.
- corridorName      : Nama Koridor / Nama Rute yang berisi Titik Awal dan Titik Akhir untuk setiap rute.
- direction         : 0 untuk Pergi, 1 untuk Kembali. Arah rute.
- tapInStops        : ID Halte Tap In (masuk) untuk mengidentifikasi nama halte
- tapInStopsName    : Nama Halte Tap In (masuk) di mana pelanggan melakukan tap in.
- tapInStopsLat     : Garis lintang (Latitude) Halte Tap In
- tapInStopsLon     : Garis bujur (Longitude) Halte Tap In
- stopStartSeq      : Urutan dari halte, halte pertama, halte kedua, dan seterusnya. Terkait dengan arah.
- tapInTime         : Waktu tap in. Tanggal dan waktu
- tapOutStops       : ID Halte Tap Out (keluar) untuk mengidentifikasi nama halte
- tapOutStopsName   : Nama Halte Tap Out (keluar) di mana pelanggan melakukan tap out.
- tapOutStopsLat    : Garis lintang (Latitude) Halte Tap Out
- tapOutStopsLon    : Garis bujur (Longitude) Halte Tap Out
- stopEndSeq        : Urutan dari halte, halte pertama, halte kedua, dan seterusnya. Terkait dengan arah.
- tapOutTime        : Waktu tap out. Tanggal dan waktu
- payAmount         : Jumlah yang dibayarkan oleh pelanggan. Ada yang gratis. Ada yang tidak.

# __3. Analisis Data moda Royaltrans__

![Gambar Contoh](https://transjakarta.co.id/aset/gambar/armada/royal.png)

__Tinjauan Singkat__
- Royaltrans merupakan layanan bus premium yang menawarkan kenyamanan dan fasilitas lengkap untuk menunjang mobilitas di Jakarta.
- Memiliki kapasitas 30 orang per bus (seluruh penumpang duduk, tidak berdiri)
- Harga tiket bus Rp. 20.000
- Pemesanan tiket dilakukan melalui aplikasi TiJe, H-1 dari jadwal keberangkatan (maksimal pukul 22.00) untuk keberangkatan di pagi hari, dan di hari yang sama (H+0) maksimal pukul 10.00 untuk keberangkatan di sore hari. 

referensi: https://oto.detik.com/oto-galeri/d-6485643/ini-lho-fasilitas-nyaman-bus-royaltrans/1


Sesuai dengan pernyataan permasalahan, untuk tugas objektif dilakukan limitasi pada studi kasus __Royaltrans__.

Filter kolom spesifik `Moda`: __Royaltrans__

## __3.1 Analisis Penggunaan__
#### __3.1.1 Demografi Pengguna Royaltrans__

Demografi pengguna royal trans berdasarkan kolom `payCardSex`, `payCardBirthDate`, `payCardBank`.
Demografi pengguna __Royaltrans__
1. Penumpang dengan jenis kelamin perempuan lebih banyak (54.1%) daripada penumpang dengan jenis kelamin laki-laki (45.9%)
2. Penumpang dengan rentang usia 35-44 tahun paling mendominasi (631 penumpang) dan rentang usia 25-34 sebanyak 542 orang. selanjutnya rentang usia 0-17 tahun sebanyak 304 orang (penumpang usia sekolah) dan 18-24 tahun (diasumsikan penumpang yang merupakan mahasiswa / kuliah). terakhir, usia 45 ke atas sampai dengan lansia sebanyak 98 orang.
3. Bank (prepaid card / kartu prabayar) yang paling banyak digunakan adalah Bank DKI, kemudian Bank Mandiri (emoney), dan bank-bank lainnya juga online payment.

#### __3.1.2 Penggunaan Royaltrans pada weekday (working day) dan weekend (non working day)__
__Penggunaan Royaltrans per hari, hari kerja dan hari libur__

1. Berdasarkan data di bulan April 2023, 91.7% (sebanyak 1.558 pengguna) Royaltrans di hari kerja (Senin s.d Jumat) dan hanya 8.3% (sebanyak 141 pengguna) yang menggunakan Royaltrans di hari libur (Sabtu dan Minggu) dari total sebanyak 1.699 pengguna Royaltrans pada bulan April 2023. Hal ini mengindikasikan bahwa mobilitas warga yang berdomisili di sekitar Jakarta (Cibubur, Bekasi, Cinere/Depok, Bintaro/BSD/Serpong/Tangerang ) bekerja di Jakarta.
2. Perlu diperhatikan kondisi pada Bulan April 2023, terdapat beberapa hari libur di hari selain sabtu dan minggu, yaitu tanggal 7 (Jumat) sebagai Jumat Agung, libur dan cuti bersama Hari Raya Idul Fitri pada 21 (Jumat), 24 (Senin), 25 (Selasa), dan 26 (Rabu). Namun dari visualisasi Lineplot di atas, untuk tanggal-tanggal libur tersebut tidak berpengaruh / menurun. hal ini mengindikasikan bahwa pengguna tetap menggunakan Royaltrans untuk ke tempat ibadah dan berkunjung ke keluarga yang berada di Jakarta atau sekitar Jakarta.
3. Rata-rata pengguna Royaltrans harian di hari kerja sebanyak 311 pengguna, dan pengguna Royaltrans di hari libur sebanyak 70 pengguna. Perbandingan rata-rata pengguna di hari kerja dan hari libur rasionya adalah 22.5%.

#### __3.1.3 Penggunaan Royaltrans per jam (jam-jam sibuk / sepi)__
__Penggunaan Royaltrans di tiap jam selama April 2023__
1. Penggunaan tertinggi ada di pukul 06.00 WIB (225 pengguna) dan 17.00 WIB (234 pengguna).
2. Penggunaan terendah ada di rentang pukul 10.00 WIB - 15.00 WIB dengan total gabungan selama 5 jam hanya 70 pengguna (hanya 4.1% dari keseluruhan). Jadwal mulai operasional jam 5.00 WIB sampai dengan jadwal operasional terakhir yaitu jam 21.00.
3. Pada hari libur penggunaan mayoritas pada jam 15 dan jam 17, namun tidak banyak, mengingat total pengguna pada hari libur (Sabtu dan Minggu) di bulan April 2023 totalnya adalah 141 atau hanya 8.3% dari total pengguna selama April 2023.

## __3.2 Analisis Rute__
__Dari 3 Rute yang paling banyak diminati yaitu:__
- 1T : Cibubur - Balai Kota (400 pengguna)
- B14 : Bekasi Barat - Kuningan (222 pengguna)
- D32 : Cinere - Bundaran Senayan (180 pengguna)

__Dari 3 Rute yang kurang diminati yaitu:__
- D31 : Cinere - Kuningan (88 pengguna)
- 6P : Cibubur - Kuningan (94 pengguna)
- S12 : Terminal BSD - Fatmawati (94 pengguna)

1. Dari data per rute per harinya ada keunikan pada tanggal 1 (hari Minggu), 8 (hari Minggu) dan tanggal 22 (hari Minggu) yaitu rute S12 terminal BDS - Fatmawati, mengindikasikan ada warga yang berangkat dari terminal BSD menuju Fatmawati yang rutin naik Royaltrans pada hari Minggu. 
2. Dari rute yang paling diminati dan paling kurang diminati terdapat suatu pola yaitu Cibubur - Balai Kota yang paling diminati, sedangkan untuk Cibubur - Kuningan kurang diminati. Ini tidak dapat disimpulkan bahwa mobilitas terpadat adalah warga dari Cibubur.

Namun lebih jelasnya dapat dianalisis kembali sesuai jadwal, apakah ada jadwal yang anomali atau kurang tepat sehingga tidak maksimal dan efektif dalam operasional Royaltrans.

## __3.3 Analisis Jadwal__
__Mengidentifikasi jadwal Keberangkatan dan Kepulangan (`direction` 0 / berangkat atau 1 / pulang) pada tiap `corridorID` atau `corridorName` dari data df_royaltrans__.
1. Mengambil kolom yang dibutuhkan yaitu `corridorID`, `corridorName`, `direction`, `tapInStopsName`, `stopStartSeq` `tapInTime`. 
2. Kolom `tapInTime` diubah menjadi only_hour atau jam saja hh:mm.
3. Memilih / filter kolom `stopStartSeq` yang bernilai paling kecil (0 atau 1 atau 2) dengan asumsi bus tersebut memulai rutenya pada pemberhentian maks 3 (yaitu seq 0, 1, atau 2).
4. Mengurutkan berdasarkan `corridorID`, `direction`, dan `tapInTime` paling awal.
5. Limitasi hh:mm (pembulatan jam) dan untuk setiap `corridorID` dan `direction` pada jam yang sudah dibulatkan dan bernilai sama, maka dilakukan penghapusan sehingga tidak duplikasi.

Membuat jadwal per corridor dengan dictionary hasil dari dataframe `sorted_df`

Diasumsikan:
1. Untuk setiap rute Bus Royaltrans memiliki pool / tempat parkir di titik pertama rute pergi (`direction` = 0)
2. Untuk setiap satu armada bus yang berangkat (`direction`= 0) dapat langsung memiliki jadwal pulang/kembali (`direction` = 1) dengan kurun maksimal 2 jam setelah jadwal berangkat.

| No | corridorID | corridorName                      | poolBus             |
|----|------------|-----------------------------------|---------------------|
| 1  | 1K         | Cibubur Junction - Blok M        | Cibubur    |
| 2  | 1T         | Cibubur - Balai Kota             | Cibubur             |
| 3  | B13        | Bekasi Barat - Blok M            | Bekasi Barat        |
| 4  | B14        | Bekasi Barat - Kuningan          | Bekasi Barat        |
| 5  | D31        | Cinere - Kuningan                | Cinere              |
| 6  | D32        | Cinere - Bundaran Senayan        | Cinere              |
| 7  | S12        | BSD Serpong - Fatmawati          | BSD Serpong         |
| 8  | S31        | Bintaro - Fatmawati              | BSD Serpong             |
| 9  | T21        | Palem Semi - Bundaran Senayan     | Palem Semi          |
| 10 | 6P         | Cibubur - Kuningan               | Cibubur             |

Selain pool di atas, TransJakarta secara khusus mengakomodir pool yang berada di dalam kota Jakarta untuk armada yang memiliki rute di dalam kota, namun bisa dipakai bus-bus Royaltrans. Adapun poolnya sebagai berikut:
- Pool Cawang: Berada di Cawang, Jakarta Timur.
- Pool Pulogebang: Lokasi ini juga berada di Jakarta Timur.
- Pool Tanjung Priok: Terletak dekat pelabuhan, mendukung layanan transportasi ke daerah tersebut.
- Pool Grogol: Terletak di Grogol, Jakarta Barat.

__Rute 1K : Cibubur Junction - Blok M__ 
1. Jadwal mulai beroperasi dengan `direction` pergi / berangkat  terlalu siang (kurang pagi), bisa diatur jadwal lebih pagi lagi misalnya di jam 05.30 atau jam 06.30 dengan asumsi pola pengguna berangkat dari rumah dan tiba di kantor sebelum pukul 08.00 atau 09.00 WIB. 
2. Jadwal pertama pada `direction` pulang / kembali dari Blok M, cukup pagi yaitu pukul 06.00 untuk mengakomodir penumpang yang memiliki tujuan berkebalikan arah dengan pengguna pada umumnya (para pekerja di Jakarta yang berdomisili di luar atau pinggiran Jakarta). Armada yang sama seharusnya dapat mulai beroperasi di jam 05.00 berangkat (`direction` 0) dari parkir bus di Cibubur sehingga mengefektifkan armada agar tetap bisa mengangkut penumpang yang berangkat dan pulang.
3. Armada yang mengangkut penumpang pada jadwal pulang / kembali pukul 09.30 seharusnya dapat mengangkut penumpang yang berangkat dari Cibubur (sesuai pool bus) pada pukul 07.00 atau pukul 07.30 (dapat disesuaikan).
4. Jadwal berangkat pukul 16.00 sudah tepat, agar armada dapat mengangkut penumpang yang memiliki kebalikan arah dengan penumpang pada umumnya (Cibubur ke Blok M) dan setelahnya armada yang sama dapat mengangkut penumpang yang kembali ke Cibubur dari Blok M di pukul 17.00 atau pukul 17.30. 
5. Armada yang bersiap untuk jadwal pertama kepulangan di sore hari dapat dimulai pukul 15.30 berangkat dari pool terdekat (pool Cawang) ke pemberhentian Blok M agar tiba tepat waktu yaitu pukul 16.15 atau 16.30.

__Rute 1T : Cibubur - Balai Kota__

1. Jadwal dengan `direction` pergi / berangkat tidak terdapat jadwal di pagi hari. Seluruh jadwal berangkat dari Cibubur menuju Balai Kota ada di sore hari yaitu mulai dari pukul 16.30. Perlu ada jadwal pagi hari agar pengguna sebagai warga Cibubur dapat terakomodir menuju tengah kota Jakarta. Setidaknya 2 atau 3 jadwal keberangkatan di pagi hari.
2. Jadwal dengan `direction` kembali / pulang cukup banyak, mulai di pagi hari sampai dengan malam, yaitu sebanyak 21 keberangkatan. Bisa dioptimalkan dan dibuat lebih efektif dengan melihat traffic / kebutuhan dari pengguna rute tersebut.
Sehingga armada yang ada dapat lebih efektif pada corridor ini. Jika permintaan dari pengguna hanya sedikit, dapat dibuat jadwal per 1 jam sekali saja, tidak perlu per 30 menit sekali.

__Rute B13 : Bekasi Barat - Blok M__

1. Jadwal paling pagi dengan `direction` pergi / berangkat cukup siang yaitu mulai pukul 06.30, dan tetap mengakomodir jadwal di siang hari yaitu 11.30 dan 12.00.
2. Tidak terdapat jadwal dengan arah berangkat di sore hari. Untuk kebutuhan menambah jadwal arah berangkat / pergi di sore hari perlu dipertimbangkan, namun tetap melihat dari permintaan pengguna / warga.
3. Terdapat 8 jadwal dengan arah / `direction` pulang yang dimulai pada pukul 14.30, cukup awal untuk asumsi pengguna yang pulang kerja dari kantornya di tengah kota.

__Rute B14 : Bekasi Barat - Kuningan__

1. Jadwal dengan `direction` berangkat dari Bekasi Barat ke Kuningan hanya 1 dan dimulai pada siang hari menjelang sore, yaitu pukul 14.00 WIB. Perlu dipertimbangan dengan menambah jadwal keberangkatan di pagi hari dengan mempertimbangkan pola pengguna corridor ini.
2. Terdapat 9 jadwal dengan `direction` pulang atau kembali dari Kuningan ke Bekasi Barat, di mana seluruhnya pagi hingga siang hari yaitu mulai pukul 05.00 WIB hingga pukul 12.30 WIB, dengan rentang 30 menit hingga 1 jam.
3. Tidak terdapatnya jadwal kepulangan di sore hari perlu dipertimbangkan.

__Rute D31 : Cinere - Kuningan__

1. Jadwal keberangkatan pada corridor ini hanya 2x dalam sehari dan keduanya adalah arah pergi atau berangkat. Satu di waktu siang pagi menjelang siang 10.30 dan cukup malam yaitu pukul 20.30.
2. Perlu dipertimbangkan untuk memperbanyak jadwal namun tergantung dari pola pengguna pada corridor ini.

__Rute D32 : Cinere - Bundaran Senayan__

1. Terdapat 12 jadwal dengan arah berangkat dari Cinere ke Bundaran Senayan, dimulai cukup siang yaitu pukul 07.30 WIB dan jadwal terakhir pukul 21.30
2. Terdapat 13 jadwal dengan arah pulang / kembali dari Bundaran Senayan kembali ke Cinere. Jadwal kembali ini dimulai cukup pagi yaitu pukul 05.00 WIB, dan selanjutnya pukul 08.30, selebihnya jadwal di sore hari dimulai pukul 16.00 hingga 21.30 dengan jeda tiap jam adalah 30 menit. Jadwal ini cukup banyak dan ideal apabila kebutuhan atau permintaan pengguna cukup banyak.
4. Untuk menambah atau mengurangi jadwal keberangkatan dan kepulangan perlu ditinjau kembali tergantung traffic pengguna agar dapat mengefektifkan armada pada corridor ini.

__Rute S12 : BSD Serpong - Fatmawati__

1. Terdapat 12 jadwal dengan arah keberangkatan dari BSD Serpong ke Fatmawati, dimulai pukul 06.00 WIB sampai dengan pukul 10.00 di pagi hari, kemudian dilanjutkan kembali jadwal di sore hari pukul 16.30, pukul 18.00 dan pukul 19.30.
2. Sebanyak 14 jadwal dengan arah pulang dari Fatmawati kembali ke BSD Serpong, di mulai pada pagi menjelang siang yaitu pukul 10.30 WIB, lalu pukul 11.00, 13.00, 14.00, kemudian sore hari pukul 17.00 sampai dengan pukul 22.00 dengan rentang tiap 30 menit antar jadwal.

__Rute S31 : Bintaro - Fatmawati__

1. Jadwal pada corridor ini cukup banyak, untuk arah berangkat dari Bintaro ke Fatmawati ada 19 jadwal, dan jadwal ini bisa dikatakan sepanjang hari, karena di pagi hari, siang dan sore hari dengan rentang 30 menit sampai 1 jam dan 1 jam 30 menit sekali. Namun, jadwal paling pagi di pukul 07.00 (tidak cukup pagi untuk jadwal pertama) dan selanjutnya di pukul 08.30 (cukup jauh di rentang 1 jam 30 menit).
2. Arah kembali dari Fatmawati ke Bintaro terdapat 17 jadwal. Dimulai pukul 05.00 WIB di pagi hari hingga pukul 10.00 WIB dengan rentang 30 menit sekali. Jadwal di sore hari mulai pukul 16.00 sampai dengan pukul 20.30, dengan rentang yang variatif, 1 jam, 30 menit, 1 jam 30 menit.

__Rute T21 : Palem Semi - Bundaran Senayan__

1. Jadwal pada corridor ini hanya `direction` pulang, sebanyak 15 jadwal, dengan jadwal sepanjang hari, mulai pukul 05.00 sampai dengan pukul 22.00.
2. Perlu melihat pola pengguna pada corridor ini untuk melakukan improvisasi apakah perlu menambah direction keberangkatan atau cukup jadwal yang sudah ada saat ini.

__Rute 6P : Cibubur - Kuningan__

1. Pada rute ini hanya terdapat 3 jadwal dengan `direction` berangkat sebanyak 2 jadwal, pagi pukul 06.30 dan sore pukul 15.00 WIB.
2. Pada arah kembali dari Kuningan ke Cibubur hanya 1 jadwal, pukul 20.30, cukup malam untuk jadwal ini.
3. Perlu dilakukan tinjauan ulang untuk jadwal pada rute ini yang terbilang sedikit.

# __4. Kesimpulan Analisis Rute dan Jadwal__

## __4.1. Pola Pengguna per Corridor Royaltrans__

Setelah melihat analisis penggunaan Royaltrans pada sub-bab __Analisis Penggunaan Royaltrans secara harian dan jam__, dapat ditarik kesimpulan bahwa:
1. Penggunaan Royaltrans mayoritas pada hari kerja / weekday (Senin sd Jumat)
2. Penggunaan Royaltrans pada jam sibuk / rush hour berada pada pagi hari pukul 06.00 - 09.00 dan pada sore hari pukul 17.00 - 20.00.

Selanjutnya, traffic per jam di masing-masing rute atau corridor dapat dilihat dari data sebagai berikut.

__Pada waktu/jam pagi hari jika diasumsikan seluruh direction 0 atau berangkat lebih ramai, terdapat pada corridor berikut:__
1. Cibubur - Balai Kota
2. Bekasi Barat - Blok M
3. Cinere - Bundaran Senayan
4. BSD Serpong - Fatmawati

namun data anomali lebih banyak, yaitu direction 1 atau pulang lebih ramai, yaitu pada corridor berikut:
1. Cibubur Junction - Blok M
2. Cibubur - Kuningan
3. Bekasi Barat - Kuningan
4. Cinere - Kuningan (walaupun datanya hampir mirip direction 0 dan direction 1)
5. Bintaro - Fatmawati
6. Palem Semi - Bundaran Senayan


__Pada waktu/jam sore hari jika diasumsikan seluruh direction 1 atau pulang lebih ramai, terdapat pada corridor berikut:__
1. Cibubur - Balai Kota
2. Bekasi Barat - Blok M
3. Cinere - Bundaran Senayan
4. BSD Serpong - Fatmawati

namun data anomali lebih banyak, yaitu direction 0 atau berangkat lebih ramai, yaitu pada corridor berikut:
1. Cibubur Junction - Blok M
2. Cibubur - Kuningan
3. Bekasi Barat - Kuningan
4. Cinere - Kuningan (walaupun datanya hampir mirip direction 0 dan direction 1)
5. Bintaro - Fatmawati
6. Palem Semi - Bundaran Senayan

Melihat data ini, ada fakta menarik bahwa tiap corridor memiliki karakteristik pengguna masing-masing:
- Kelompok Pekerja Pagi ditemukan pada corridor:
1. Cibubur - Balai Kota
2. Bekasi Barat - Blok M
3. Cinere - Bundaran Senayan
4. BSD Serpong - Fatmawati

- Kelompok Pekerja Malam ditemukan pada corridor:
1. Cibubur Junction - Blok M
2. Cibubur - Kuningan
3. Bekasi Barat - Kuningan
4. Cinere - Kuningan (walaupun datanya hampir mirip direction 0 dan direction 1)
5. Bintaro - Fatmawati
6. Palem Semi - Bundaran Senayan

## __4.2 Perbaikan Jadwal per Corridor__

Sesuai dengan customer behaviour atau pola kebiasaan pengguna masing-masing corridor, maka berikut ini perbaikan jadwalnya agar dapat menyesuaikan kebutuhan pengguna / customer.
#### __4.2.1 Rekomendasi Jadwal Corridor dengan Pola Pengguna Kerja Pagi__

1. 1T: Cibubur - Balai Kota
2. B13: Bekasi Barat - Blok M
3. D32: Cinere - Bundaran Senayan
4. S12: BSD Serpong - Fatmawati

__Asumsi__
1. untuk mengurai kepadatan pengguna pada 1 peak hours, maka jadwal keberangkatan per 15 menit.
2. untuk lebih efektif, maka mengeliminasi jadwal apabila traffic kurang dari 10 orang pada 1 waktu.

#### __4.2.2 Rekomendasi Jadwal Corridor dengan Pola Pengguna Kerja Malam__

1. 1K: Cibubur Junction - Blok M
2. 6P: Cibubur - Kuningan
3. B14: Bekasi Barat - Kuningan
4. D31: Cinere - Kuningan
5. S31: Bintaro - Fatmawati
6. T21: Palem Semi - Bundaran Senayan

__Asumsi__
1. untuk mengurai kepadatan pengguna pada 1 peak hours, maka jadwal keberangkatan per 15 menit.
2. untuk lebih efektif, maka mengeliminasi jadwal apabila traffic kurang dari 10 orang pada 1 waktu.

# 5. Penutup

Sesuai pola dan kebutuhan dari pengguna moda Royaltrans per `corridorID` / `corridorName`, berikut perbaikan jadwal yang dapat digunakan untuk mengakomodir traffic pengguna.

| corridorID | corridorName                  | direction | Jumlah Jadwal Sebelum | Jumlah Jadwal Sesudah |
|------------|--------------------------------|-----------|------------------------|------------------------|
| 1K         | Cibubur Junction - Blok M     | 0         | 2                      | 13                     |
| 1K         | Cibubur Junction - Blok M     | 1         | 10                     | 13                     |
| 1T         | Cibubur - Balai Kota          | 0         | 10                     | 15                     |
| 1T         | Cibubur - Balai Kota          | 1         | 22                     | 15                     |
| 6P         | Cibubur - Kuningan            | 0         | 5                      | 8                      |
| 6P         | Cibubur - Kuningan            | 1         | 3                      | 8                      |
| B13        | Bekasi Barat - Blok M         | 0         | 10                     | 12                     |
| B13        | Bekasi Barat - Blok M         | 1         | 9                      | 12                     |
| B14        | Bekasi Barat - Kuningan       | 0         | 1                      | 14                      |
| B14        | Bekasi Barat - Kuningan       | 1         | 9                      | 14                      |
| D31        | Cinere - Kuningan             | 0         | 2                      | 6                      |
| D31        | Cinere - Kuningan             | 1         | 2                      | 0                      |
| D32        | Cinere - Bundaran Senayan     | 0         | 12                     | 15                     |
| D32        | Cinere - Bundaran Senayan     | 1         | 13                     | 15                     |
| S12        | BSD Serpong - Fatmawati       | 0         | 12                     | 8                      |
| S12        | BSD Serpong - Fatmawati       | 1         | 14                     | 8                      |
| S31        | Bintaro - Fatmawati           | 0         | 19                     | 11                     |
| S31        | Bintaro - Fatmawati           | 1         | 17                     | 11                     |
| T21        | Palem Semi - Bundaran Senayan | 0         | 0                     | 13                     |
| T21        | Palem Semi - Bundaran Senayan | 1         | 15                     | 13                     |


Jadwal ini dapat dievaluasi pada masa yang akan datang apabila terjadi perubahan kembali pada pola pengguna Royaltrans per corridor.
