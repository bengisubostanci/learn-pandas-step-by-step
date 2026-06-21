# 🐼 Pandas Temelleri: DataFrame ve Series Yapısı

Bu bölümde Pandas kütüphanesinin temel yapı taşları olan **DataFrame** ve **Series** kavramlarını inceleyecek, temel indeksleme (indexing) işlemlerine göz atacağız.

## 1. İlk DataFrame'i Oluşturma
Bir Python sözlüğü (dictionary) kullanarak ilk veri çerçevemizi (DataFrame) oluşturalım:

python
import pandas as pd

# Sözlük yapısı ile veri tanımlama
data = {
    'country': ['Kazakhstan', 'Russia', 'Belarus', 'Ukraine'],
    'population': [17.04, 143.5, 9.5, 45.5],
    'square': [2724902, 17125191, 207600, 603628]
}

df = pd.DataFrame(data)
df.head()

,country,population,square
0,Kazakhstan,17.04,2724902
1,Russia,143.50,17125191
2,Belarus,9.50,207600
3,Ukraine,45.50,603628

type(df)
# Çıktı: pandas.core.frame.DataFrame
2. Series (Seri) ve DataFrame Farkı
Pandas'ta tek bir sütun seçtiğimizde elde ettiğimiz yapı ile iki köşeli parantez kullandığımızda elde ettiğimiz yapı birbirinden veri tipi olarak farklıdır.

A) Tek Köşeli Parantez Yapısı df['column'] -> Series
Tek parantez kullandığımızda veri tipi Series (Seri) olur. Seriler tek boyutlu etiketli dizilerdir.

df['country'].head()
Çıktı:
0    Kazakhstan
1        Russia
2       Belarus
3       Ukraine
Name: country, dtype: object

type(df['country'])
# Çıktı: pandas.core.series.Series

B) Çift Köşeli Parantez Yapısı df[['column']] -> DataFrame
Eğer tek bir sütun seçiyor olsak bile çift köşeli parantez kullanırsak, Pandas bize iki boyutlu bir DataFrame nesnesi döner.
type(df[['country']])
# Çıktı: pandas.core.frame.DataFrame

3. İndeks (Index) Yönetimi
Varsayılan olarak Pandas satırlara 0, 1, 2... şeklinde ardışık sayılardan oluşan bir indeks atar. Bu indeksleri özelleştirmek mümkündür.

Yöntem A: Veriyi Oluştururken İndeks Tanımlama
DataFrame oluşturulurken index parametresi ile özel etiketler atanabilir:
df2 = pd.DataFrame(data, index=['KZ', 'RU', 'BY', 'UA'])
df2.head()
İndeks,country,population,square
KZ,Kazakhstan,17.04,2724902
RU,Russia,143.50,17125191
BY,Belarus,9.50,207600
UA,Ukraine,45.50,603628

Yöntem B: Mevcut DataFrame'in İndeksini Sonradan Değiştirme
Mevcut bir DataFrame nesnesinin .index özniteliğine (property) doğrudan yeni bir liste atanarak indeksler güncellenebilir:
df.index = ['KZ1', 'RU1', 'BY1', 'UA1']
df.head()
İndeks,country,population,square
KZ1,Kazakhstan,17.04,2724902
RU1,Russia,143.50,17125191
BY1,Belarus,9.50,207600
UA1,Ukraine,45.50,603628
