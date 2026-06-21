# 📊 Pandas ile Veri Okuma ve Temel Veri Manipülasyonu

Bu bölümde harici bir CSV dosyasını Pandas ortamına aktarmayı, veri setinin yapısını incelemeyi (`shape`, `info`) ve mevcut sütunlar üzerinde ekleme, silme, dönüştürme gibi temel manipülasyon işlemlerini öğreneceğiz.

---

## 1. Veri Okuma (Read CSV)
Öncelikle test edeceğimiz örnek veri setini indirip projemize dahil edelim ve ilk 5 satırına göz atalım:

python
import pandas as pd

# Örnek veri setini indirme (Terminal/Notebook komutu)
# !curl -o simple_data.csv [https://raw.githubusercontent.com/erkansirin78/datasets/master/simple_data.csv](https://raw.githubusercontent.com/erkansirin78/datasets/master/simple_data.csv)

# Veriyi okuma
df = pd.read_csv("simple_data.csv")
df.head()

,sirano,isim,yas,meslek,sehir,aylik_gelir
0,1,Cemal,35,Isci,Ankara,3500
1,2,Ceyda,42,Memur,Kayseri,4200
2,3,Timur,30,Müzisyen,Istanbul,9000
3,4,Burcu,29,Pazarlamaci,Ankara,4200
4,5,Yasemin,23,NaN,Bursa,4800

2. Veri Setini Keşfetme (Shape & Info)
A) Boyut Bilgisi (shape)
Veri setinin kaç satır ve kaç sütundan oluştuğunu tuple formatında döner:
df.shape
# Çıktı: (17, 6) -> 17 satır, 6 sütun
B) Detaylı Özet Bilgi (info())
Bellek kullanımı, veri tipleri ve sütunlardaki eksik (Null/NaN) değer sayıları hakkında bilgi verir:

df.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 17 entries, 0 to 16
Data columns (total 6 columns):
 #   Column       Non-Null Count  Dtype 
---  ------       --------------  ----- 
 0   sirano       17 non-null     int64 
 1   isim         17 non-null     object
 2   yas          17 non-null     int64 
 3   meslek       15 non-null     object --> 2 satırda eksik veri var (NaN)
 4   sehir        17 non-null     object
 5   aylik_gelir  17 non-null     int64 
dtypes: int64(3), object(3)
memory usage: 944.0+ bytes

3. Gelişmiş Veri Okuma Seçenekleri
Büyük veri setlerinde sadece ihtiyacımız olan sütunları okumak performansı artırır. usecols ve sep parametreleri ile okuma işlemini özelleştirebiliriz:
df2 = pd.read_csv("simple_data.csv", sep=",", usecols=['sirano', 'isim'])
df2.head()

,sirano,isim
0,1,Cemal
1,2,Ceyda

4. Sütun İşlemleri ve Manipülasyon
A) Sütun İsimlerini Güncelleme (rename)
DataFrame'deki belirli sütun isimlerini kalıcı olarak (inplace=True) değiştirebiliriz:
df.rename(columns={'isim': 'ad', 'sehir': 'il'}, inplace=True)
df.columns
# Çıktı: Index(['sirano', 'ad', 'yas', 'meslek', 'il', 'aylik_gelir'], dtype='object')
B) Mevcut Sütun Değerlerini Değiştirme (Güncelleme)
Mevcut tüm çalışanların aylık gelirlerine 1000 birim zam uygulayalım:
df['aylik_gelir'] = df['aylik_gelir'] + 1000
C) Yeni Sütun Ekleme (Matematiksel İşlemle)
Mevcut bir sütunu sabit bir katsayı ile çarparak yeni bir sütun üretebiliriz:
D) pd.cut() ile Kategorik Değişken Üretme
Sürekli (sayısal) bir değişkeni, belirli aralıklara (bins) bölerek kategorik bir değişkene dönüştürebiliriz:
op_labels = ['low', 'middle', 'high']
category = [0, 5000, 8000, 100000]

df['income_cat'] = pd.cut(df['aylik_gelir'], bins=category, labels=op_labels, include_lowest=False)
E) Sütunları Metinsel Olarak Birleştirme
Farklı sütunlardaki verileri string veri tipine (astype(str)) dönüştürerek birleştirebiliriz:
df['income_and_cat'] = df['aylik_gelir'].astype(str) + ' - ' + df['income_cat'].astype(str)
F) Benzersiz Kimlik (ID) Sütunu Oluşturma
Veri setindeki satır sayısı (len(df)) kadar benzersiz ardışık sayılardan oluşan bir ID listesi ekleyebiliriz:
df['id'] = list(range(100001, 100001 + len(df)))

5. Sütunların Sırasını Yeniden Düzenleme
Oluşturduğumuz yeni sütunlar varsayılan olarak en sona eklenir. DataFrame'i sadece istediğimiz sütun sırasıyla listeleyerek yeniden organize edebiliriz:
# Yeni sütun sırasını tanımlama
yeni_sira = ['id', 'sirano', 'ad', 'yas', 'meslek', 'il', 'aylik_gelir', 'aylik_gelir_dolar', 'income_cat', 'income_and_cat']

df = df[yeni_sira]
df.head()
,id,sirano,ad,yas,meslek,il,aylik_gelir,aylik_gelir_dolar,income_cat,income_and_cat
0,100001,1,Cemal,35,Isci,Ankara,4500,88650.0,low,4500 - low
1,100002,2,Ceyda,42,Memur,Kayseri,5200,102440.0,middle,5200 - middle
2,100003,3,Timur,30,Müzisyen,Istanbul,10000,197000.0,high,10000 - high
3,100004,4,Burcu,29,Pazarlamaci,Ankara,5200,102440.0,middle,5200 - middle
4,100005,5,Yasemin,23,NaN,Bursa,5800,114260.0,middle,5800 - middle

