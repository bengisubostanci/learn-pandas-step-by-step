# 🔤 Pandas ile Metin (String) İşlemleri ve Hücre Bazlı Güncelleme

Gerçek dünya veri setlerindeki metinsel veriler genellikle yapısal düzeltmelere ihtiyaç duyar. Bu bölümde Pandas'ın `.str` aksesuvarı ile metinleri büyütmeyi (`upper`), alt metin aramayı (`contains`), regex ile toplu kelime değiştirmeyi (`replace`), hücre bazlı nokta atışı güncellemeyi (`at`) ve `apply(lambda)` kullanarak metin kırpmayı (truncate) öğreneceğiz.

---

## 1. Veriyi Yükleme ve Metinleri Standartlaştırma (`upper`)

Veri setindeki kategorik metinlerin (örneğin ülke isimleri) farklı yazım tarzlarından (United Kingdom, united kingdom vb.) etkilenmemesi için hepsini büyük harfe dönüştürerek standartlaştırabiliriz:

python
import pandas as pd

df = pd.read_csv("OnlineRetail2.csv")

# Country sütunundaki tüm metinleri büyük harfe çevirme
df['Country'] = df['Country'].str.upper()
df['Country'].head(2)
# Çıktı: UNITED KINGDOM

2. Metin İçinde Örüntü Arama (contains)
.str.contains() metodu, bir sütun içerisinde belirli bir kelimenin veya karakterin geçip geçmediğini kontrol eder.

⚠️ Kritik Not: Sütunda eksik veri (NaN) varsa bu metot hata verebilir. Bunun önüne geçmek için na=False parametresi eklenerek hata alması engellenir.

A) Kelime Bazlı Arama
Açıklamasında "COFFEE" geçen ürünleri filtreleyelim:
df[df['Description'].str.contains("COFFEE", na=False)].head(2)

B) İş Mantığına Göre Filtreleme (Örnek: İptal Edilen Siparişler)
E-ticaret veri setlerinde fatura numarasının (InvoiceNo) başında "C" (Cancel) harfi olması siparişin iptal edildiğini gösterir. Bu örüntüyü filtreleyerek iptal edilen satışları analiz edebiliriz:
# Fatura numarasında 'C' harfi geçen iptal siparişleri filtreleme
iptal_siparisler = df[df['InvoiceNo'].str.contains("C", na=False)]
iptal_siparisler.head(2)

3. Regex ile Toplu Metin Değiştirme (replace)
Sütun içindeki belirli ifadeleri sözlük (dictionary) yapısı kullanarak ve düzenli ifadeleri (regex=True) aktif ederek toplu bir şekilde güncelleyebiliriz.
# Tek seferde 'RETRO' kelimelerini 'COSTA', 'MUGS' kelimelerini ise 'MOCHA' yapalım
df['Description'] = df['Description'].replace({'RETRO': 'COSTA', 'MUGS': 'MOCHA'}, regex=True)

4. Hücre Bazlı Nokta Atışı Güncelleme (at)
Eğer tüm sütunu veya satırı değil de, sadece belirli bir satır ve sütunun kesiştiği tek bir hücreyi güncellemek istiyorsak, en performanslı yöntem df.at[indeks, sütun_adı] fonksiyonunu kullanmaktır.
# 53. indeksteki satırın 'StockCode' değerini nokta atışı güncelleme
df.at[53, 'StockCode'] = '37371'

5. apply() ve lambda ile Metin Kırpma (Truncate)
Bazen tarih formatları string olarak gelir ve saat bilgisini atmak, sadece gün/ay/yıl bilgisini tutmak isteyebiliriz. apply(lambda ...) yapısı yardımıyla her satıra özel string dilimleme (slicing) uygulayabiliriz:
# '2010-12-01 08:26:00' formatındaki metnin son 9 karakterini (saat kısmını) kırpma
df['InvoiceDate'] = df['InvoiceDate'].apply(lambda x: x[:-9])
df['InvoiceDate'].head(2)
# Çıktı: 2010-12-01

🎯 Manipülasyon Sonrası Örnek Görünüm
Yaptığımız tüm string dönüşümleri, hücre güncellemeleri ve tarih kırpma işlemlerinin ardından verinin son hali şu şekildedir:
,InvoiceNo,StockCode,Description,Quantity,InvoiceDate,UnitPrice,CustomerID,Country
53,536373,37371,COSTA COFFEE MOCHA ASSORTED,6,2010-12-01,1.06,17850.0,UNITED KINGDOM
70,536375,37370,COSTA COFFEE MOCHA ASSORTED,6,2010-12-01,1.06,17850.0,UNITED KINGDOM

