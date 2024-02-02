# BMW Sales EDA with SQL
Melakukan eksplorasi data menggunakan SQL untuk mendapatkan wawasan tentang faktor-faktor yang memengaruhi harga mobil BMW.

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/BMW-SSE21-Footer-desktop.jpg)

Dengan kemajuan teknologi dan inovasi yang terus berlanjut, memiliki basis data yang terstruktur dan departemen yang mampu memberikan makna pada data akan menjadi kunci kesuksesan dalam menjalankan proyek eksplorasi data penjualan BMW ini. Eksekusi proyek ini menjadi penting, namun kita memerlukan input yang kuat untuk menghasilkan hasil yang optimal.

Adapun proyek eksplorasi data penjualan BMW ini akan melibatkan dua tahap utama menggunakan alat dan teknologi yang berbeda. Pertama, data penjualan akan disimpan menggunakan Excel sebagai repositori awal untuk data. Kedua, SQL akan digunakan untuk memberikan makna pada data, melibatkan analisis lebih mendalam dan penggalian informasi dari basis data BMW Sales.

## Project Outline
1. Mengimpor Data dan Explorasi Awal
2. Statistik sederhana
3. Analisis Mileage
4. Analisis Engine Power
5. Analisis Jenis Bahan Bakar
6. Analisis Warna Cat
7. Analisis Tipe Mobil
8. Kesimpulan dan Rekomendasi

## 1. Mengimpor Data dan Explorasi Awal
Dataset yang digunakan merupakan data BMW Pricing Challange yang diambil dari website kaggle.com
Link Dataset [BMW Pricing Challange](https://www.kaggle.com/datasets/danielkyrka/bmw-pricing-challenge)

Data yang diberikan terdiri dari hampir 5000 mobil BMW asli yang terjual melalui lelang b2b pada tahun 2018. Harga yang tertera pada tabel merupakan penawaran tertinggi yang dicapai selama lelang.

### Load Data
```mysql
select * 
from bmw_pricing 
limit 10;
```
   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/load%20data%20bmw.PNG)

pada eksporasi data ini saya hanya akan menggunakan kolom marker key, model key, mileage, engine power, registrasion date, fuel, paint colour, car type, price, dan sold at.
```mysql
create table bmw_sales as
select maker_key, model_key, mileage, engine_power, registration_date, fuel, paint_color, car_type, price, sold_at
from bmw_pricing;
```
```mysql
describe bmw_sales;
```
   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/describe%20bmw%20sales.PNG)
```mysql
select * from bmw_sales limit 10;
```
   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/membuat%20table%20bmw%20sales%20dari%20kolom%20bmw%20pricing.PNG)

Selanjutnya, mari kita melakukan Analisis Data Eksploratif (Exploratory Data Analysis atau EDA) pada data bmw_sales.

## 2. Statistik Sederhana
- **Mencari nilai rata rata dari harga mobil bmw**
```mysql
select round(avg(price)) as rata_rata_harga
from bmw_sales;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/rata%20rata%20harga.PNG)

pada data mobil bmw_sales didapatkan rata rata harga dari keseluruhan harga mobil ialah $15.828

- **Mencari nilai harga tertinggi mobil bmw**
```mysql
select *
from bmw_sales
order by price desc
limit 1;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/rata%20rata%20harga%20tertinggi.PNG)

Mobil BMW seri X3 type SUV dengan harga $178.500 merupakan mobil termahal yang terdapat di data bmw_sales

- **Mencari nilai harga terendah mobil bmw**
```mysql
select *
from bmw_sales
order by price asc
limit 1;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/BMW_Sales_EDA_using_SQL/blob/06829426f3cc1f43ab2cb10af2f47777ec907ca1/Asset/harga%20terendah.PNG)

Mobil BMW seri 320 type Estate dengan harga $100 merupakan mobil termurah yang terdapat di data bmw_sales

## 3. Analisis Mileage
Pada analisis mileage ini ingin dicari rata rata harga mobil bmw berdasarkan kategori mileage atau jarak tempuh yang sudah digunakan oleh mobil tersebut, dan ingin dicari apakah terdapat korelasi antara jarak tempuh yang sudah digunakan oleh mobil dengan harga mobil

- **Mencari nilai rata rata harga mobil bmw berdasarkan kategori mileage**
```mysql
select 
case
  when mileage <= 50000 then "LOW Mileage"
  when mileage > 50000 and mileage < 500000 then "MEDIUM Mileage"
  else "HIGH Mileage"
end as category_mileage,
round(avg(price)) as avg_price
from bmw_sales
group by category_mileage
order by avg_price desc;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/mileage%20price.PNG)

berdasarkan hasil kueri mobil dengan jarak tempuh kategori LOW Mileage memiliki rata rata harga yang lebih tinggi dibanding kategori mileage lainnya

- **Analisis korelasi antara jarak tempuh yang sudah digunakan oleh mobil dengan harga mobil**
```mysql
select (
count(*) * sum(mileage * price) - sum(mileage) * sum(price)) / sqrt(
(count(*) * sum(mileage * mileage) - pow(sum(mileage), 2)) * 
(count(*) * sum(price * price) - pow(sum(price), 2))) as korelasi_mileage_price
from bmw_sales;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/korelasi%20mileage%20price.PNG)

berdasarkan hasil kueri didapatkan nilai korelasi pearson sebesar -0,4095 yang menunjukan adanya korelasi negatif antara mileage terhadap harga mobil, yang berarti terdapat kecenderungan bahwa semakin tinggi nilai mileage atau semakin banyak kendaraan digunakan (jarak tempuh lebih tinggi), harga mobil cenderung menurun.

## 4. Analisis Engine Power
Pada analisis Engine Power ini ingin dicari rata rata harga mobil bmw berdasarkan kategori Engine Power atau kekuatan yang dihasilkan oleh mesin kendaraan, dan ingin dicari apakah terdapat korelasi antara Engine Power dengan harga mobil

- **Mencari nilai rata rata harga mobil bmw berdasarkan kategori Engine Power**
```mysql
select 
case
  when engine_power < 100 then "LOW Power"
  when engine_power >= 100 and engine_power < 200 then "MEDIUM Power"
  else "HIGH Power"
end as category_power,
round(avg(price)) as avg_price
from bmw_sales
group by category_power
order by avg_price desc;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/engine%20power%20price.PNG)

berdasarkan hasil kueri mobil dengan kategori engine power HIGH memiliki rata rata harga yang lebih tinggi dibanding kategori power lainnya

- **Analisis korelasi antara Engine Power dengan harga mobil**
```mysql
select
((sum(engine_power * price) - (sum(engine_power) * sum(price)) /
count(*))) / (sqrt(sum(engine_power * engine_power) - (sum(engine_power) * sum(engine_power)) /
count(*)) *  sqrt(sum(price * price) - (sum(price) * sum(price)) / count(*))) as korelasi_engine_power_price
from bmw_sales;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/korelasi%20engine%20power%20price.PNG)

berdasarkan hasil kueri didapatkan nilai korelasi pearson sebesar 0,6389 yang menunjukan adanya korelasi positif antara engine power terhadap harga mobil, yang berarti terdapat kecenderungan bahwa semakin tinggi nilai engine power, harga mobil juga cenderung meningkat.


## 5. Analisis Jenis Bahan Bakar
Pada analisis jenis bahan bakan ini akan dicari perbandingan harga mobil berdasarkan jenis bahan bakarnya dan perbandingan penjualan mobil berdasarkan jenis bahan bakarnya

- **Mencari perbandingan harga mobil berdasarkan jenis bahan bakar**
```mysql
select fuel,
round(avg(price)) as avg_price
from bmw_sales
group by fuel
order by fuel;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/avg%20price%20fuel.PNG)

berdasarkan hasil kueri terlihat bahwa mobil dengan bahan bakar Hybrid Petrol memiliki rata rata harga yang lebih tinggi dibanding jenis bahan bakar lainnya

- **Mencari perbandingan penjualan mobil berdasarkan jenis bahan bakar**
```mysql
select fuel,
count(*) as jumlah_terjual
from bmw_sales
group by fuel
order by jumlah_terjual desc;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/penjualan%20berdasarkan%20bahan%20bakar.PNG)

berdasarkan hasil kueri terlihat bahwa mobil dengan bahan bakar Diesel menjadi mobil yang paling banyak terjual

## 6. Analisis Warna Cat
Pada analisis jenis bahan bakan ini akan dicari mobil dengan warna cat apa yang paling banyak terjual dan ingin mengetahui apakah terdapat korelasi antara warna cat mobil terhadap penjualan mobil

- **Mencari perbandingan penjualan mobil berdasarkan warna cat**
```mysql
select paint_color,
count(*) as jumlah_terjual
from bmw_sales
group by paint_color
order by jumlah_terjual;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/jumlah%20mobil%20berdasarkan%20warna.PNG)

berdasarkan hasil kueri terlihat bahwa mobil dengan warna Black menjadi mobil yang paling banyak terjual diikuti oleh warna Grey

- **Analisis korelasi antara warna cat dengan jumlah penjualan**
```mysql
with paint_color_onehot as (
select 
case
  when paint_color = 'beige' then 1
  when paint_color = 'black' then 2
  when paint_color = 'blue' then 3
  when paint_color = 'brown' then 4
  when paint_color = 'green' then 5
  when paint_color = 'grey' then 6
  when paint_color = 'orange' then 7
  when paint_color = 'red' then 8
  when paint_color = 'silver' then 9
  when paint_color = 'white' then 10
end as paint_color,
count(*) as jumlah_terjual
from bmw_sales
group by paint_color
)
select
((sum(paint_color * jumlah_terjual) - (sum(paint_color) * sum(jumlah_terjual)) /
count(*))) / (sqrt(sum(paint_color * paint_color) - (sum(paint_color) * sum(paint_color)) /
count(*)) *  sqrt(sum(jumlah_terjual * jumlah_terjual) - (sum(jumlah_terjual) * sum(jumlah_terjual)) / count(*))) as korelasi_color_penjualan
from paint_color_onehot;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/korelasi%20color%20price.PNG)

berdasarkan hasil kueri didapatkan nilai korelasi pearson sebesar -0,2598 yang menunjukan adanya korelasi negatif lemah antara warna cat mobil terhadap jumlah penjualan mobil, yang berarti terdapat kecenderungan bahwa penjualan mobil cenderung meningkat untuk mobil dengan warna tertentu

## 7. Analisis Tipe Mobil
Pada analisis jenis bahan bakan ini akan dicari perbandingan harga mobil berdasarkan jenis bahan bakarnya dan perbandingan penjualan mobil berdasarkan jenis bahan bakarnya

- **Mencari perbandingan harga mobil berdasarkan type mobil**
```mysql
select car_type,
round(avg(price)) as avg_price
from bmw_sales
group by car_type
order by avg_price desc;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/avg%20price%20tipe%20mobil.PNG)

berdasarkan hasil kueri terlihat bahwa mobil dengan tipe Coupe memiliki harga yang lebih tinggi dibanding type lainnya

- **Mencari perbandingan penjualan mobil berdasarkan type mobil**
```mysql
select car_type,
count(*) as jumlah_tejual
from bmw_sales
group by car_type
order by jumlah_tejual desc;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/penjualan%20berdasarkan%20tipe%20mobil.PNG)

berdasarkan hasil kueri terlihat bahwa mobil dengan tipe Estate menjadi mobil yang paling banyak terjual diikuti oleh tipe sedan.

## 8. Kesimpulan dan Rekomendasi
### Kesimpulan
Setelah melakukan eksplorasi data pada dataset bmw_sales, beberapa temuan dapat diidentifikasi :
1. Pengaruh Mileage

   Berdasarkan analisis mileage terhadap harga mobil, ditemukan korelasi negatif yang moderat sebesar -0,4095, mengindikasikan bahwa semakin tinggi mileage, harga mobil cenderung menurun. Differensiasi harga yang signifikan terlihat pada kategori Low Mileage dengan rata-rata harga $25,640, Medium Mileage $15,227, dan High Mileage $1,400.
2. Pengaruh Engine Power

   Dari hasil analisis, ditemukan bahwa kendaraan dengan engine power tinggi memiliki rata-rata harga yang lebih tinggi, seiring dengan nilai korelasi engine power terhadap harga mobil yang mencapai 0,6389. Korelasi positif yang cukup kuat menunjukkan bahwa semakin tinggi nilai engine power, harga mobil cenderung meningkat.
3. Pengaruh Jenis Bahan Bakar

   Hasil analisis menunjukkan bahwa kendaraan dengan bahan bakar hybrid_petrol memiliki harga yang tinggi, sementara penjualan terbanyak terjadi pada mobil dengan bahan bakar diesel. Korelasi positif antara harga dan bahan bakar hybrid_petrol mengindikasikan adanya permintaan pasar untuk kendaraan ramah lingkungan dengan teknologi hibrida. Meskipun demikian, popularitas kendaraan diesel menyoroti kebutuhan untuk mempertahankan opsi bahan bakar yang lebih konvensional dan terjangkau.
4. Pengaruh Warna Cat

   Dari hasil analisis, dapat disimpulkan bahwa penjualan mobil dengan warna hitam menunjukkan keunggulan dengan menjadi penjualan terbanyak, diikuti oleh warna grey. Korelasi negatif sebesar -0,2598 mengindikasikan bahwa preferensi konsumen cenderung pada warna hitam ketika memilih mobil. Kesimpulan ini mengarah kepada kesan estetika dan elegan yang diberikan oleh mobil berwarna hitam, yang sering kali diidentifikasi dengan daya tarik yang klasik dan mewah.
5. Pengaruh Tipe Mobil

   Dari hasil analisis, terungkap bahwa tipe mobil "coupe" memiliki harga tertinggi, sementara tipe "estate" memimpin dalam jumlah penjualan, diikuti oleh tipe "sedan." Kesimpulan ini menyoroti perbedaan preferensi konsumen antara keinginan akan mobil dengan harga premium seperti "coupe" dan kebutuhan akan mobilitas praktis yang tercermin dalam popularitas tipe "estate" dan "sedan."

### Rekomendasi
Berdasarkan temuan di atas, beberapa rekomendasi strategis yang dapat diajukan:
1. Pengelompokan berdasarkan kondisi mileage

   Dalam strategi pemasaran, disarankan untuk menyesuaikan penetapan harga proporsional terhadap mileage. Fokus promosi pada keunggulan kendaraan dengan Low Mileage, sambil mempertimbangkan strategi khusus untuk kendaraan High Mileage dengan menekankan harga yang lebih terjangkau.
2. Penekanan pada mobil dengan performa tinggi

   Disarankan untuk melakukan penyesuaian harga yang mempertimbangkan nilai tambah dari engine power tersebut dalam strategi penetapan harga. Strategi pemasaran dapat difokuskan pada keunggulan performa kendaraan dengan engine power tinggi, dengan komunikasi yang jelas mengenai manfaat performa superior kepada calon pembeli.
3. Optimalisasi Penjualan Mobil Diesel

   Peningkatan dalam kampanye pemasaran yang lebih agresif dan terarah untuk mempromosikan keunggulan kendaraan diesel, seperti efisiensi bahan bakar yang tinggi dan daya tahan yang handal. Dalam hal ini, penawaran program promosi eksklusif untuk mobil diesel, seperti diskon atau paket servis yang menguntungkan, dapat meningkatkan daya tarik konsumen. Selain itu, edukasi konsumen tentang teknologi diesel yang terkini dan berinovasi, termasuk peningkatan dalam aspek lingkungan, dapat membantu mengubah persepsi positif terhadap kendaraan diesel. 
4. Penyesuaian strategi warna mobil

   Berdasarkan temuan ini, perusahaan dapat mempertimbangkan strategi yang lebih terfokus pada pemasaran dan produksi mobil berwarna hitam. Dalam pengembangan produk, upaya dapat difokuskan pada peningkatan kualitas dan varian warna hitam yang lebih menarik. Kampanye pemasaran dapat ditingkatkan untuk menekankan kesan estetika dan elegan yang dapat diberikan oleh mobil berwarna hitam, dengan memanfaatkan elemen-elemen desain yang menonjol. Menggabungkan kesan estetika ini dalam strategi pemasaran dapat meningkatkan daya tarik produk dan meningkatkan pangsa pasar.
5. Penyesuaian strategi tipe mobil

   Mempertahankan strategi pemasaran yang efektif untuk tipe "estate" dan "sedan," sekaligus menjelajahi inovasi pada tipe "coupe" untuk meningkatkan daya tariknya. Perusahaan dapat memanfaatkan keberhasilan penjualan tipe "estate" dan "sedan" sebagai fondasi untuk pertumbuhan lebih lanjut, sambil terus memonitor dinamika pasar dan merespons perubahan preferensi konsumen dengan fleksibilitas.

   

