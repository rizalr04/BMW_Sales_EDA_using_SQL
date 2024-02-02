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

pada data mobil bmw_sales didapatkan rata rata dari keseluruhan harga mobil ialah ...

- **Mencari nilai harga tertinggi mobil bmw**
```mysql
select *
from bmw_sales
order by price desc
limit 1;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/rata%20rata%20harga%20tertinggi.PNG)

Mobil BMW seri .... dengan harga ... merupakan mobil termahal yang terdapat di data bmw_sales

- **Mencari nilai harga terendah mobil bmw**
```mysql
select *
from bmw_sales
order by price asc
limit 1;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/rata%20rata%20harga%20terendah.PNG)

Mobil BMW seri .... dengan harga ... merupakan mobil termurah yang terdapat di data bmw_sales

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

berdasarkan hasil kueri mobil dengan jarak tempuh ... memiliki harga yang lebih tinggi

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

berdasarkan hasil kueri didapatkan nilai korelasi pearson sebesar ... yang menunjukan ...

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

berdasarkan hasil kueri mobil dengan kategori engine power ... memiliki harga yang lebih tinggi

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

berdasarkan hasil kueri didapatkan nilai korelasi pearson sebesar ... yang menunjukan ...

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

berdasarkan hasil kueri terlihat bahwa mobil dengan bahan bakar ... memiliki harga yang lebih tinggi

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

berdasarkan hasil kueri terlihat bahwa mobil dengan bahan bakar ... menjadi mobil yang paling banyak terjual

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

berdasarkan hasil kueri terlihat bahwa mobil dengan warna ... menjadi mobil yang paling banyak terjual

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

berdasarkan hasil kueri didapatkan nilai korelasi pearson sebesar ... yang menunjukan ...

## 7. Analisis Tipe Mobil
Pada analisis jenis bahan bakan ini akan dicari perbandingan harga mobil berdasarkan jenis bahan bakarnya dan perbandingan penjualan mobil berdasarkan jenis bahan bakarnya

- **Mencari perbandingan harga mobil berdasarkan jenis bahan bakar**
```mysql
select car_type,
round(avg(price)) as avg_price
from bmw_sales
group by car_type
order by avg_price desc;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/avg%20price%20tipe%20mobil.PNG)

berdasarkan hasil kueri terlihat bahwa mobil dengan tipe ... memiliki harga yang lebih tinggi

- **Mencari perbandingan penjualan mobil berdasarkan jenis bahan bakar**
```mysql
select car_type,
count(*) as jumlah_tejual
from bmw_sales
group by car_type
order by jumlah_tejual desc;
```
Dari query diatas didapatkan hasil

   ![image](https://github.com/rizalr04/SQL_EDA/blob/5923dd186adc8cd1d477b36689a465eaa7d40f4a/Asset/penjualan%20berdasarkan%20tipe%20mobil.PNG)

berdasarkan hasil kueri terlihat bahwa mobil dengan tipe ... menjadi mobil yang paling banyak terjual

## 8. Kesimpulan dan Rekomendasi
### Kesimpulan
asadas

### Rekomendasi
asasas


