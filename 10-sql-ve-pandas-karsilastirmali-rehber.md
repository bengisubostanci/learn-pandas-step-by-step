# 📊 SQL ile Pandas Karşılaştırmalı Veri Analizi Rehberi

Veri analitiği dünyasında en sık kullanılan iki araç SQL ve Pandas'tır. Bu bölümde, veri tabanlarında sıklıkla yazdığımız temel SQL sorgularının (`SELECT`, `WHERE`, `GROUP BY`, `UPDATE`, `DELETE`) Pandas kütüphanesindeki tam karşılıklarını veri analitiğinin klasikleşmiş veri seti **Tips (Bahşişler)** üzerinden inceleyeceğiz.

---

## 1. Veri Setini Yükleme

python
import pandas as pd
import numpy as np

# Örnek veri setini indirme
# !wget [https://raw.githubusercontent.com/erkansirin78/datasets/master/tips.csv](https://raw.githubusercontent.com/erkansirin78/datasets/master/tips.csv)

df = pd.read_csv("tips.csv")
df.head(2)

,total_bill,tip,sex,smoker,day,time,size
0,16.99,1.01,Female,No,Sun,Dinner,2
1,10.34,1.66,Male,No,Sun,Dinner,3

2. Belirli Sütunları Seçme (SELECT & LIMIT)
Sadece belirli sütunları listelemek ve ilk 5 satırını görmek istediğimizde:

=== "SQL"
sql SELECT total_bill, tip, smoker, time FROM tips LIMIT 5; 

=== "Pandas"
python df[['total_bill', 'tip', 'smoker', 'time']].head(5) 

3. Koşullu Filtreleme (WHERE)
A) Tek Koşullu Filtreleme
=== "SQL"
sql SELECT * FROM tips WHERE time = 'Dinner' LIMIT 5; 

=== "Pandas"
python df[df['time'] == 'Dinner'].head(5) 

B) Çoklu Koşullu Filtreleme (AND)
=== "SQL"
sql SELECT * FROM tips WHERE time = 'Dinner' AND tip > 5.00; 

=== "Pandas"
python df[(df['time'] == 'Dinner') & (df['tip'] > 5.0)] 

4. Gruplama ve Özet İstatistikler (GROUP BY)
A) Tek Sütun Gruplama ve Ortalama Hesaplama
=== "SQL"
sql SELECT day, AVG(total_bill) AS total_bill, AVG(tip) AS tip FROM tips GROUP BY day; 

=== "Pandas"
python # Not: İki köşeli parantez kullanarak list halinde sütun seçmek modern sürümlerde zorunludur. df.groupby(['day'])[['total_bill', 'tip']].mean().reset_index() 

B) Farklı Sütunlara Farklı Agregasyonlar Uygulama
Gün bazında ortalama bahşişi ve o gün gelen toplam sipariş (satır) sayısını bulalım:

=== "SQL"
sql SELECT day, AVG(tip), COUNT(*) FROM tips GROUP BY day; 

=== "Pandas"
python # Sözlük yapısı kullanarak hangi sütuna hangi fonksiyonun uygulanacağını belirtiriz df.groupby(['day']).agg({'tip': 'mean', 'day': 'size'}).rename(columns={'day': 'count'}) 

C) Çoklu Sütun ile Gruplama ve Sıralama
Hem sigara içme durumuna hem de güne göre kırılım yapıp sıralayalım:

=== "SQL"
sql SELECT smoker, day, COUNT(*), AVG(tip) FROM tips GROUP BY smoker, day; 

=== "Pandas"
python # MultiIndex çıktısında 'day' indeksine göre sıralama df.groupby(['smoker', 'day']).agg({'tip': ['size', 'mean']}).sort_values('day') 

5. Veri Güncelleme (UPDATE)
Belirli bir şartı sağlayan hücrelerin değerlerini kalıcı olarak güncellemek için Pandas'ta en güvenli yöntem .loc yapısıdır.

=== "SQL"
sql UPDATE tips SET tip = tip * 1000 WHERE tip < 2; 

=== "Pandas"
python # Bahşişi 2 dolardan az olanların değerini 1000 ile çarpıyoruz df.loc[df['tip'] < 2, 'tip'] *= 1000 

6. Veri Silme (DELETE / DROP)
Veri tabanlarında satır silmek için DELETE kullanılırken, Pandas ortamında genellikle koşulun tersini seçerek veriyi filtreleme veya .drop() yöntemini kullanma mantığı işletilir.

=== "SQL"
sql DELETE FROM tips WHERE tip > 1000; 

=== "Pandas"
python # Bahşişi 1000'den küçük olanları seçerek, 1000'den büyük olanları dolaylı olarak siliyoruz df = df.loc[df['tip'] < 1000]

