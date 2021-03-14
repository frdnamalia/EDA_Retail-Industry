# EDA (Exploratory Data Analysis)

## Analisa Data Transaksi 2015-2016

## -Ayanghaseyo.co.id-


##### Presented by Fardhina Amalia, Rafif Abdul Aziz

<hr>

#### Introduction


`Ayanghaseyo.co.id` merupakan bagian dari perusahaan Ayanghaseyo Group yang bergerak di Industri Retail. `Ayanghaseyo.co.id` menjual  beberapa kategori produk unggulan seperti `wine, daging, ikan, manisan, buah dan emas`. Memiliki 2 kategori pelanggan, yaitu customer non member dan `member`. Customer non member dapat membeli produk melalui website maupun store dan customer member dapat membeli produk melalui `website,store dan catalog bulanan`.
Privilage yang dimiliki oleh Member adalah adanya `campaign marketing` yang diadakan rutin oleh perusahaan.


#### Rumusan Masalah

```
Bagaimana pola transaksi customer member yang mulai  bergabung sejak 2012-2014 dengan umur, latar belakang, dan status yang berbeda?
```

#### Goals


- Meningkatkan angka penjualan 
- Meningkatkan jumlah member baru yang bergabung
- Mengefisiensikan pengeluaran untuk kebutuhan promosi
- Meningkatkan keberhasilan campaign marketing perusahaan

<hr>

### Dataset

- Customer profiles
    - Year birth : Tahun kelahiran member
    - Education : Pendidikan terakhir member
    - Marital_Status : Status Perkawinan member
    - Income : Pendapatan (per tahun) member
    - Dt_Customer : Tanggal pendaftaran member sebagai member baru
    - Recency : Jumlah hari terakhir kali member melakukan transaksi
    - Complain : Angka 1 apabila member melakukan komplain dalam 2 tahun terakhir
    
    
- Product preferences 
    - MntWines : Jumlah pembelian produk wine
    - MntFruits : Jumlah pembelian produk buah-buahan
    - MntMeatProducts : Jumlah pembelian produk daging
    - MntFishProducts : Jumlah pembelian produk ikan
    - MntSweetProducts : Jumlah pembelian produk manis
    - MntGoldProds : Jumlah pembelian produk emas
    - MNTtotal : Total pembelian dari semua produk 
    
 *Transaksi dalam 2 tahun terakhir*
    
    
- Campaign successes/failures
    - AcceptedCmp1 : Angka 1 apabila member menerima tawaran yang diberikan pada campaign pertama, angka 0 apabila member tidak menerima tawaran
    - AcceptedCmp2 : Angka 1 apabila member menerima tawaran yang diberikan pada campaign kedua, angka 0 apabila member tidak menerima tawaran
    - AcceptedCmp3 : Angka 1 apabila member menerima tawaran yang diberikan pada campaign ketiga, angka 0 apabila member tidak menerima tawaran
    - AcceptedCmp4 : Angka 1 apabila member menerima tawaran yang diberikan pada campaign keempat, angka 0 apabila member tidak menerima tawaran
    - AcceptedCmp5 : Angka 1 apabila member menerima tawaran yang diberikan pada campaign kelima, angka 0 apabila member tidak menerima tawaran
    - AcceptedCmp6 : Angka 1 apabila member menerima tawaran yang diberikan pada campaign keenam, angka 0 apabila member tidak menerima tawaran
    - total_acccmp : Jumlah campaign yang diterima oleh member
    - Purc_Discount : Jumlah transaksi oleh member menggunakan discount
    - cam_success : Persentase keberhasilan Campaign yang telah dilakukan
    
    
- Channel performance
    - NumWebPurchases : Jumlah transaksi yang dilakukan oleh member melalui web
    - NumCatalogPurchases : Jumlah transaksi yang dilakukan oleh member melalui catalogue
    - NumStorePurchases : Jumlah transaksi yang dilakukan oleh member melalui store/toko
    - NumWebVisitsMonth : banyaknya kunjungan ke website yang dilakukan oleh member
    
<hr>
<hr>

### Statistical Analysis & Data Visualization

hal yang pertama dilakukan yaitu import library yang dibutuhkan untuk melakukan analisis

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore')
```
import dataset yang akan di analisis 
dataset yang akan digunakan diambil dari https://www.kaggle.com/rodsaldanha/arketing-campaign

## Explore Dataset
sebelum melakukan analisa data, hal yang perlu dilakukan adalah :

**- Memeriksa tipe data**
```
Untuk melihat tipe data agar memudahkan analisa data
```
**- Melakukan describe (numerik dan object)**
```
Untuk mengetahui statistik deskriptif berupa jumlah data, mean, median, Q1, Q2, std
```
**- Memeriksa keberadaan outliers**
```
Outliers yang kami periksa yaitu dari kolom Year_Birth, ditemuka outliers berupa tahun kelahiran dibawah 1900. kami menduga data ini merupakan kesalahan input, sehingga kami memutuskan untuk menghilangkan data tersebut karena dapat menggangu proses analisa data
```
**- Missing values**
```
Tidak ada missing values pada Dataset
```
**- Mengubah tipe data waktu dari object menjadi datetime**
```
Memudahkan kami untuk melakukan ekstraksi
```



## Ekstraksi Dataset

- Mengambil tahun dari Dt_customer untuk melihat tahun bergabungnya member

## Manipulasi Dataset

**- Update kolom Year_Birth dari tahun kabisat menjadi kolom generasi**

 Update ini bersumber dari *wikipedia* mengenai generasi


```python 
def func(row): 
    if row['Year_Birth'] <= 1945 : #diatas umur 75 tahun
        val = "Silent Generation"
    if row['Year_Birth'] > 1945 and row['Year_Birth'] <= 1965  : # 55-74
        val = "Boomers"
    elif row['Year_Birth'] > 1965 and row['Year_Birth'] <= 1980  : # 40-54
        val = "Gen X"
    elif row['Year_Birth'] > 1980 and row['Year_Birth'] <= 1996  : # 24-39
        val = "Millenials"
    return val
```
 Dari keempat kategori diatas, kami memfokuskan pada generasi Boomers, Gen X dan Millenials
**- Membuat kolom baru yaitu kolom total_acccmp**

 Kolom ini menjelaskan berapa banyak campaign yang diterima oleh member
 
```python
df['Total_AccCmp']=df['AcceptedCmp1']+df['AcceptedCmp2']+df['AcceptedCmp3']+df['AcceptedCmp4']+df['AcceptedCmp5']+df['AcceptedCmp6']
```
**- Membuat kolom baru yaitu kolom MntTotal**

 Kolom ini berisi jumlah transaksi pembelian seluruh kategori barang yang dilakukan oleh member
 
```python 
df['MntTotal']=df['MntFishProducts']+df['MntFruits']+df['MntGoldProds']+df['MntMeatProducts']+df['MntSweetProducts']+df['MntWines']
```

**- Membuat kolom baru yaitu cam_success**
  Kolom ini berisi persentase keberhasilan dari Campaign yang telah dilakukan

```python 
cam_success = pd.DataFrame(df[['AcceptedCmp1', 'AcceptedCmp2', 'AcceptedCmp3', 'AcceptedCmp4', 'AcceptedCmp5', 'AcceptedCmp6']].mean()*100, 
                           columns=['Percent']).reset_index()
```

**- Membuat kolom baru yaitu purchases**
  data ini merupakan rata-rata dari masing-masing kolom 'NumCatalogPurchases','NumStorePurchases','NumWebPurchases','NumWebVisitsMonth','Purc_Discount'.
  
```python
purchases = pd.DataFrame(df[['NumCatalogPurchases','NumStorePurchases','NumWebPurchases','NumWebVisitsMonth','Purc_Discount']].mean(), 
                           columns=['average']).reset_index()
```


## Analisa Dataset

### Customer Profile

- Jumlah Pendaftar Tiap Tahun
    - Member yang kami analisis melakukan pendaftaran pada rentang tahun antara 2012-2014
    - Jumlah pendaftar terbanyak berada pada tahun 2013 dan yang terendah pada 2012
    - terjadi penurunan pendaftar yang sangat signifikan antara 2013-2014

- Jumlah member berdasarkan generasi tahun kelahiran
    - Member yang paling banyak berasal dari generasi Gen X
    - Jumlah member paling sedikit berasal dari generasi Millenials

- Jumlah member berdasarkan latar belakang pendidikan
    - Member yang paling banyak berasal dari latar belakang pendidikan Graduation
    - Member dengan latar belakang basic merupakay yang paling sedikit

- Jumlah member berdasarkan status perkawinan
    - Member yang paling banyak memiliki status perkawinan married atau sudah menikah
    - Status perkawinan dari member yang paling sedikit yaitu widow/er 

- Rata-rata Income member berdasarkan Education
    - Berdasarkan Education, rata-rata income yang tertinggi memiliki Education PhD
    - Income paling rendah berasal dari Education Basic, perbandingan Income antara Education Basic dengan Education lainnya sangat signifikan
    - Income dari Education basic juga satu-satunya yang dibawah rata-rata


#### Analisa Product Preferences berdasarkan Customer Profiles

- Rata-rata penjualan tiap kategori
    - Berdasarkan kategori, penjualan paling tinggi berasal dari produk Wine
    - penjualan tertinggi kedua berasal dari produk Meat
    - adanya korelasi penjualan antara produk wine dan meat, karena berdasarkan Dataset, perbedaan penjualan kedua item ini memiliki perbedaan yang cukup signifikan dibanding item lainnya.
    - penjualan paling rendah yaitu produk buha-buahan, dan memiliki selisih penjualan yang sangat tipis dengan produk manisan

- Rata-rata penjualan semua kategori berdasarkan Education
    - Rata-rata penjualan tertinggi berdasarkan Education, berasal dari latar belakang Education PhD, hal ini berbanding lurus dengan Income per tahun PhD yang paling besar
    - Rata-rata penjualan tertinggi berdasarkan Education, berasal dari latar belakang Education Basic, hal ini juga berbanding lurus dengan Income per tahun Basic yang paling kecil
    
- Rata-rata penjualan semua kategori berdasarkan Generasi
    - Berddasarkan Generasi, penjualan tertinggi berasal dari Generasi Boomer
    - penjualan terendah berasal dari Generasi Gen X
    - Perbedaan penjualan tiap generasi tidak terlalu signifikan
    
- Rata-rata penjualan berdasarkan Marital_Status, Education dan Year_birth (Gen X)
    - Cust berlatar belakang pendidikan Basic rata-rata paling banyak menghabiskan uangnya untuk membeli Emas dibanding kategori lainnya. dan Cenderung lebih memilih makan ikan dibanding daging
    - Cust berlatar belakang pendidikan Graduation,Master,dan PhD rata-rata paling banyak menghabiskan uangnya untuk Wine. dan cenderung lebih memilih makan daging dibanding ikan
        - Rata-rata jumlah pembelian Seluruh kategori dalam 2 tahun terakhir paling banyak adalah 596.83 oleh Cust berlatar belakang pendidikan PhD dan berstatus Single
        - Rata-rata jumlah pembelian Seluruh Kategori dalam 2 tahun terakhir paling dikit adalah 29.00 oleh Cust berlatar belakang pendidikan Basic dan berstatus Widow/er
        
- Rata-rata penjualan berdasarkan Marital_Status, Education dan Year_birth (Boomers)
    - Rata-rata pembelian wine paling tinggi dikalangan boomer dengan latar belakang pendidikan phd.
    - Boomer basic lebih banyak membeli gold prod dibanding produk lain, hal ini bertentangan dengan latar belakang pendidikan lain yang lebih banyak membeli wine,
    - Boomer basic lebih memilih makan ikan dibanding daging, berbanding terbalik denan yang lain yg lebih memilih daging dibanding ikan
    - Selain latar belakang pendidikan basic, perbedaan atara pembelian wine dan daging dengan produk lain sangat signifikan
    - Total dan rata2 pembelian barang paling rendah yaitu boomer basic hal ini berbanding lurus dengan pendapatannya yg merupakan paling rendah
    - Boomer graduation single membeli rata2 barang paling banyak
    - Boomer phd single paling sedikit membeli produk emas
    - Rata-rata membeli barang dari keenam kategori yg paling rendah yaitu boomer vasic single

- Rata-rata penjualan berdasarkan Marital_Status, Education dan Year_birth (Millenials)
    - illenials phd widow paling banyak beli wine sekaligus paling banyak beli barang
    - rata2 pembelian barang basic paling sedikit dibanding yg lain sekaligus paling sedikit pada setiap kategori
    - master single paling banyak beli gold prods
    - pembelian wine paling rendah adalah basic single
    - raat2 pembelian paling rendah dari semua kategori adalah basic single
    - meskpun rata2 paling banyak beli barang, phd widow kedua paling sedikit dalam rata2 membeli buah.

#### Analisa Campaign successes/failures berdasarkan Customer Profiles

- Banyaknya Campaign yang diikuti oleh member
    - masih banyak member yang belum pernah sama sekali mengukuti campaign, dengan selisih yang sangat signifikan
  
- Banyaknya Campaign yang diikuti oleh member berdasarkan Year_birth
    - Yang paling banyak mengikuti campaign adalah dari generasi BOOMERS
    
- Banyaknya Campaign yang diikuti oleh member berdasarkan Education
    - Yang paling banyak mengikuti campaign adalah dari generasi Graduation
    
- Persentase keberhasilan Campaign
    - Campaign 6 memiliki success rate paling tinggi dibanding yg lain dengan presentase 14,8 %
    - campaign 2 memiliki success rate paling rendah dengan prsentase keberhasilan 1,3 %

- Jumlah member yang mengikuti Campaign berdasarkan Education
    - Orang yang memiliki pendidikan terakhir basic lebih sedikit mengikuti campaign karna daya beli yang kecil karna menyangkut rata-rata income yang cukup rendah dibanding yang lain
    - Jumlah orang-orang dengan latar belakang master dan phd, lebih sedikit terpengaruh campaign dibanding yang berlatar belakang graduation. karena berdasarkan edukasinya mereka dapat memilah dan tidak gampang terpengaruh oleh campaign (hipo)

#### Analisa Channel Performance berdasarkan Customer Profiles

- Rata-rata perilaku transaksi dari member
    - member melakukan transaksi paling banyak melalui Store
    - member paling sedikit melakukan transaksi melalui Catalogue
    - jumlah member yang melakukan Transaksi saat diskon lebih rendah dari Catalogue, hal ini mengindikasi bahwa perusahaan ini jarang mengadakan diskon

- Rata-rata perilaku transaksi dari member berdasarkan Year_Birth
    - seluruh generasi paling banyak melakukan transaksi melalui Store
    - Catalogue merupakan cara transaksi yang paling sedikit dilakukan
 
- Jumlah transaksi dengan Menggunakan discount berdasarkan Year_Birth
    - Gen X paling banyak melakukan transaksi saat diskon
    
- Jumlah member yang tidak pernah melakukan transaksi melalui Web berdasarkan Year_Birth dan Marital_Status
    - Gen X juga paling banyak memiliki member yang belum pernah bertransaksi menggunakan web
    - Millenials paling sedikit memiliki member yang belum pernah bertransaksi dengan menggunakan web
    
- Jumlah member yang tidak pernah melakukan transaksi melalui Catalogue berdasarkan Year_Birth dan Marital_Status
    - Gen X juga paling banyak memiliki member yang belum pernah bertransaksi menggunakan catalogue
    - Boomers memiliki jumalah member yang tidak pernah bertransaksi dengan catalogue paling sedikit
    - catalogue merupakan platform yang paling banyak memiliki member yang tidak pernah menggunakannya 
    
- Jumlah member yang tidak pernah melakukan transaksi melalui store berdasarkan Year_Birth dan Marital_Status
    - Generasi paling banyak yang memiliki member yang tidak pernah bertransaksi melalui store adalah generasi Gen X
    - Millenials paling sedikit memiliki member yang belum pernah bertransaksi melalui Store
- Jumlah member yang tidak pernah melakukan transaksi menggunakan Discount berdasarkan Year_Birth dan Marital_Status
    - Generasi paling banyak yang memiliki member yang tidak pernah bertransaksi menggunakan diskon adalah generasi Gen X
    - Millenials paling sedikit memiliki member yang belum pernah bertransaksi menggunakan diskon
    
- Perbandingan transaksi menggunakan Discount dengan total transaksi keseluruhan
    - Dilihat dari DataFrame, perbandingan antara total pembelian dengan pembelian saat diskon sangat jauh
    - Perushaaan ini jarang mengadakan diskon/diskon yang diberikan kurang menarik dalam 2 tahun terakhir (hipo)
    
- Rata-rata lama member sejak terakhir melakukan transaksi (recency)
    - Boomers memiliki rata-rata recency paling lama
    - perbandingan recency tiap generasi hampir sama
    
    
<hr>
<hr>


**Recommendation**

- Berdasarkan data, pembelian wine dan daging adalah yg paling tinggi, sehingga rekomendasi kami pada campaign selanjutnya membuat suatu promo paket bbq yang berisikan produk wine, daging dan kategori lainnya. hal ini sekaligus bertujuan untuk meningkatkan angka penjualan produk lain.
- Membuat mobile apps untuk memudahkan customer untuk membeli barang karena bisa diakses dan bertransaksi 24 jam, hal ini juga bertujuan untuk meningkatkan jumlah member.
- Memberikan paket voucher kepada member di store, website, maupun aplikasi. Voucher yang hanya memiliki masa berlaku dalam 1 bulan.
- Menambah fitur investasi gold untuk meningkatkan penjualan gold products, dimana gold yang dibeli bisa berupa barang fisik maupun investasi tabungan, dan hasil investasi dapat digunakan untuk transaksi pembayaran di toko ini.