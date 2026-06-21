# 🔍 Pandas ile Sıralama (Sorting) ve Filtreleme (Filtering)

Veri analizi süreçlerinde ham veriyi belirli kriterlere göre dizmek veya sadece ilgilendiğimiz alt kümeleri (subsets) seçmek en sık yaptığımız işlemlerdir. Bu bölümde `sort_values()`, mantıksal filtreleme operatörleri (`&`, `|`), `query()` metodu ve benzersiz değer analizlerini inceleyeceğiz.

---

## 1. Veriyi Sıralama (`sort_values`)

Pandas'ta verileri belirli bir sütuna göre küçükten büyüğe veya büyükten küçüğe sıralamak için `sort_values()` fonksiyonu kullanılır.

### A) Artan Sıralama (Default)
Varsayılan olarak veriler küçükten büyüğe (ascending) sıralanır. Örnek olarak veri setini `aylik_gelir` sütununa göre sıralayalım:

python
import pandas as pd

df = pd.read_csv("simple_data.csv")

# Aylık gelire göre küçükten büyüğe sıralama
df.sort_values(by="aylik_gelir").head()

,sirano,isim,yas,meslek,sehir,aylik_gelir
0,1,Cemal,35,Isci,Ankara,3500
12,13,Hakkı,33,Memur,Çorum,3750
1,2,Ceyda,42,Memur,Kayseri,4200

B) Azalan Sıralama (ascending=False)
En yüksek gelirden en düşüğe doğru sıralamak için ascending=False parametresi eklenir:
# Aylık gelire göre büyükten küçüğe sıralama
df.sort_values(by="aylik_gelir", ascending=False).head()

C) Birden Fazla Sütuna Göre Sıralama
Önce ilk sütuna göre sıralama yapılır, ilk sütunda aynı değere sahip olan satırlar kendi içinde ikinci sütuna göre sıralanır:
# Önce gelire (artan), gelirleri eşit olanları ise yaşa (artan) göre sıralar
df.sort_values(by=["aylik_gelir", "yas"]).head()

2. Koşullu Filtreleme (Conditional Filtering)
Koşullu filtreleme yaparken Pandas'ta geleneksel Python (and, or) ifadeleri yerine bit düzeyinde (bitwise) operatörler kullanılır:

& : VE (And) - İki koşulun da sağlanması gerekir.

| : VEYA (Or) - Koşullardan en az birinin sağlanması yeterlidir.

A) Tek Koşullu Filtreleme
# Sadece mesleği 'Doktor' olan satırları getir
df[df['meslek'] == 'Doktor']
,sirano,isim,yas,meslek,sehir,aylik_gelir
8,9,Ahmet,33,Doktor,Ankara,18000
13,14,Gülizar,37,Doktor,İzmir,14250

B) Çoklu Koşul: VE (&) Operatörü
# Mesleği Müzisyen OLAN VE aylık geliri 10000'den BÜYÜK olanlar
df[(df['meslek'] == 'Müzisyen') & (df['aylik_gelir'] > 10000)]
C) Çoklu Koşul: VEYA (|) Operatörü
# Mesleği Müzisyen VEYA Berber olanlar
df[(df['meslek'] == 'Müzisyen') | (df['meslek'] == 'Berber')].head()

3. SQL Tarzı Filtreleme: query() Metodu
Özellikle karmaşık ve çok koşullu filtrelemelerde kodun okunabilirliğini artırmak için SQL'deki WHERE ifadesine benzeyen query() metodu tercih edilebilir.
# Mesleği Doktor olanlar VEYA aylık geliri 10000'den büyük olanlar
df.query("meslek == 'Doktor' | aylik_gelir > 10000")
🔗 Filtreleme ve Sıralamayı Zincirleme (Chaining) Kullanmak
Filtrelediğimiz veriyi anında sıralamaya sokmak için metotları ardı ardına ekleyebiliriz:
# query() ile filtrele, ardından sort_values() ile gelire göre sırala
df.query("meslek == 'Doktor' | aylik_gelir > 10000").sort_values('aylik_gelir')

4. Benzersiz (Unique) Değer Analizleri
Kategorik sütunlarda hangi sınıfların bulunduğunu ve kaç farklı kategori olduğunu bulmak için aşağıdaki metotlar kullanılır:

A) Benzersiz Kategorileri Listeleme (unique)
Sütundaki tekil (tekrar etmeyen) değerleri bir NumPy dizisi olarak döner:
df['meslek'].unique()
# Çıktı: array(['Isci', 'Memur', 'Müzisyen', 'Pazarlamaci', nan, 'Doktor', 'Berber', 'Tuhafiyeci', 'Tornacı'], dtype=object)

B) Benzersiz Eleman Sayısı (Distinct Count)
Veri setinde kaç farklı meslek grubu olduğunu öğrenmek için unique() çıktısının uzunluğu alınabilir:
len(df['meslek'].unique())
# Çıktı: 9 (Eksik değer olan 'nan' dahil)
Not: Eğer eksik değerleri (NaN) saymadan doğrudan geçerli kategorilerin sayısını bulmak isterseniz Pandas'ın yerleşik df['meslek'].nunique() fonksiyonunu da kullanabilirsiniz.
