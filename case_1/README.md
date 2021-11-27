# Case pertama
## Abstrak
Case ini menggunakan *library* Selenium yang di *Python* untuk *scraping website* Shopee Indonesia. Produk yang menjadi pengamatan adalah Rockbros, Matoa dan Konichiwa serta informasi yang dicari adalah nama SKU, harga, jumlah yang terjual, halaman. Untuk produk Rockbros dilakukan *scraping* di halaman satu, Matoa halaman satu sampai lima dan Konichiwa halaman satu sampai dua (maksimal halaman pencarian hanya sampai dua halaman). Setelah semua informasi didapatkan, data-data tersebut digabung menjadi satu *dataframe* dan disesuaikan jenis-jenis tipe datanya. Setelah itu dilanjut dengan perhitungan GMV. GMV didapat dengan cara mengalikan harga dengan jumlah penjualan. Terakhir, data disimpan dalam format CSV.
## Alur Program
<p align="center">
  <img width="250" src="https://github.com/alfianpr/study-case/blob/main/case_1/pict/diagram%20alir%20case%201.png?raw=true" alt="Diagram Alir">
</p>
Program ini menggunakan Seenium untuk *scraping* dan pandas untuk manipulasi *dataframe*

```Python
from selenium import webdriver
import pandas as pd
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
```
Menginput alamat *website* yang akan diambil informasinya beserta keywordnya yang berupa nama produk
```Python
for pg in range(1,2): #Halaman yang mau di scrape
  page_num = str(pg)
  brand = "konichiwa" #Nama brand yang mau dicari informasinya
  shopee_url = 'https://shopee.co.id/search?keyword={}&page='.format(brand)+page_num
  driver.get(shopee_url)
  ```
 Membuat parameter untuk mengidentifikasi komponen yang diambil informasinya
 ```Python
  nama = driver.find_elements(By.XPATH, '//div[@class="_10Wbs- _5SSWfi UjjMrh"]')
  harga = driver.find_elements(By.XPATH, '//span[@class="_1d9_77"]')
  penjualan = driver.find_elements(By.XPATH, '//div[@class="_2VIlt8"]')
  ```
Membuat list untuk menampung data hasil scraping
```Python
nama_list = []
  for p in range(len(nama)):
    try : nama_list.append(nama[p].text)
    except : nama_list.append("0")

harga_list = []
  for x in range(len(harga)):
    try : harga_list.append(harga[x].text.replace('.', ''))
    except : harga_list.append("0")

penjualan_list = []
  for z in range(len(penjualan)):
    try : penjualan_list.append(penjualan[z].text.split()[0].replace('RB','000').replace(',',''))
    except : penjualan_list.append("0")
 ```
Menggabungkan semua list menjadi satu kesatuan dataframe
```Python
data_tuples = list(zip(nama_list[1:], harga_list[1:], penjualan_list[1:]))
  temp_data = pd.DataFrame(data_tuples, columns=['SKU', 'Harga', 'Jumlah_Penjualan'])
  temp_data['Halaman'] = pg+1
  dataset = dataset.append(temp_data)
```
Mengubah tipe data harga dan jumlah penjualan menjadi *integer* supaya bisa dikalkulasi untuk menghasilkan nilai GMV
```Python
df = pd.DataFrame(dataset)

#merubah tipe data harga dan jumlah penjualan menjadi integer
df['Harga'] = df['Harga'].astype('int')
df['Jumlah_Penjualan'] = df['Jumlah_Penjualan'].astype('int')

#Estimasi GMV (harga * jumlah penjualan)
df['GMV'] = df['Harga'] * df['Jumlah_Penjualan']

df.head()
```
Lima data teratas dari *dataframe* Konichiwa
<p align="center">
  <img width="700" src="https://github.com/alfianpr/study-case/blob/main/case_1/pict/dataframe.PNG?raw=true" alt="Dataframe">
</p>

Selanjutnya *dataframe* disimpan dalam format CSV
```Python
df.to_csv ("{}1_3.csv".format(brand))
```
## Tantangan
Dari pengalaman *scraping* ini, proses *scraping* perlu dilakukan berulang-ulang karena kadang tidak semua data yang ada di halaman shopee *terscrape* semua. Selain itu, ada beberapa situs yang lebih susah untuk *discraping*. Hal ini membuat metode *scraping* sangat beragam dan terus berkembang dari waktu ke waktu karena perubahan struktur *website* yang dinamis dan kebutuhan data yang semakin banyak.
