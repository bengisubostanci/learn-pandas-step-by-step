# 🔗 Pandas ile Veri Birleştirme (Concat) ve Tablo Eşleştirme (Merge/Join)

Veri analitiği projelerinde veriler genellikle tek bir tabloda bulunmaz. İlişkisel veri tabanlarında (RDBMS) olduğu gibi farklı kaynaklardaki verileri dikeyde birleştirmek (Union) veya ortak anahtarlar (Keys) üzerinden yatayda eşleştirmek (Join) gerekir. Bu bölümde `pd.concat()` ve `pd.merge()` fonksiyonlarını inceleyeceğiz.

---

## 1. Veri Setlerini Yükleme

Analizimizde bir perakende (retail) veri tabanına ait iki farklı tabloyu kullanacağız:
* **`order_items`**: Sipariş edilen ürünlerin miktarları ve alt toplamları.
* **`products`**: Ürünlerin isimleri, kategorileri ve fiyat listesi.

```python
import pandas as pd

# Veri setlerini indirme (Notebook / Terminal komutu)
# !wget [https://github.com/erkansirin78/datasets/raw/master/retail_db/order_items.csv](https://github.com/erkansirin78/datasets/raw/master/retail_db/order_items.csv)
# !wget [https://raw.githubusercontent.com/erkansirin78/datasets/master/retail_db/products.csv](https://raw.githubusercontent.com/erkansirin78/datasets/master/retail_db/products.csv)

order_items = pd.read_csv("order_items.csv")
products = pd.read_csv("products.csv")

2. Dikey Birleştirme: pd.concat() (SQL UNION)
pd.concat() fonksiyonu, iki veya daha fazla DataFrame'i sütun yapıları aynı olmak kaydıyla dikeyde (alt alta) birleştirmek için kullanılır. Bu işlem veri tabanlarındaki UNION ALL mantığına denk gelir.
# Orijinal tablo boyutu
print(products.shape) # Çıktı: (1345, 6)

# İki products tablosunu alt alta birleştirme
union_products = pd.concat([products, products])

# Yeni tablo boyutu (Satır sayısı tam iki katına çıktı)
print(union_products.shape) # Çıktı: (2690, 6)


3. Yatay Eşleştirme: pd.merge() (SQL JOIN)
Farklı tablolardaki ilişkili verileri ortak sütunlar üzerinden yan yana getirmek için pd.merge() fonksiyonu kullanılır.

A) Inner Join (İç Birleşim)
Her iki tabloda da ortak olan anahtar değerlerini (kesişim kümesini) baz alır. Eşleşmeyen satırlar çıktıda yer almaz.

left_on: Soldaki tablonun (order_items) eşleşme sütunu -> orderItemProductId

right_on: Sağdaki tablonun (products) eşleşme sütunu -> productId
inner_join = pd.merge(order_items, products, 
                      left_on="orderItemProductId", 
                      right_on="productId",
                      how="inner")
inner_join.head(2)

orderItemOrderId,orderItemProductId,orderItemSubTotal,productId,productName,productPrice
1,957,299.98,957,Diamondback Women's Serene Bike,299.98
2,957,299.98,957,Diamondback Women's Serene Bike,299.98

B) Outer Join (Tam Dış Birleşim)
Her iki tablodaki tüm satırları getirir. Sol veya sağ tablonun herhangi birinde eşleşme yoksa, eşleşmeyen kısımlar NaN (Null) değerleri ile doldurulur (Birleşim kümesi).
outer_join = pd.merge(order_items, products, 
                      left_on="orderItemProductId", 
                      right_on="productId",
                      how="outer")
outer_join.head(2)

how Parametresi,SQL Karşılığı,Açıklama
inner,INNER JOIN,Sadece her iki tabloda da ortak olan anahtarları eşleştirir.
outer,FULL OUTER JOIN,"Eşleşsin ya da eşleşmesin iki tablodaki tüm kayıtları birleştirir, boş yerlere NaN yazar."
left,LEFT JOIN,"Sol tablonun tamamını alır, sağ tablodan sadece eşleşenleri getirir."
right,RIGHT JOIN,"Sağ tablonun tamamını alır, sol tablodan sadece eşleşenleri getirir."
