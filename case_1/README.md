# Case pertama
## Abstrak
Case ini menggunakan *library* Selenium yang di *Python* untuk *scraping website* Shopee indonesia. Produk yang menjadi pengamatan adalah Rockbros, Matoa dan Konichiwa serta informasi yang dicari adalah nama SKU, harga, jumlah yang terjual, halaman. Untuk produk Rockbros dilakukan *scraping* di halaman satu, Matoa halaman satu sampai lima dan Konichiwa halaman satu sampai dua (maksimal halaman pencarian hanya sampai dua halaman). Setelah semua informasi didapatkan, data-data tersebut digabung menjadi satu *dataframe* dan disesuaikan jenis-jenis tipe datanya. Setelah itu dilanjut dengan perhitungan GMV. GMV didapat dengan cara mengalikan harga dengan jumlah penjualan. Terakhir, data disimpan dalam format CSV.
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

