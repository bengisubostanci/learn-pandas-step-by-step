# 🐼 Pandas ile Veri Analitiği ve Veri Manipülasyonu Playbook'u

Bu depo (repository), Python'ın en güçlü veri analitiği kütüphanesi olan **Pandas** ile ilgili uçtan uca ders notlarını, iş hayatında sıkça kullanılan pratik kod bloklarını ve veri mühendisliği senaryolarını barındıran Türkçe bir başvuru kılavuzudur.

Ham dökümanlar yapılandırılmış, kod çıktıları Markdown tablolarına dönüştürülmüş ve en güncel Pandas pratiklerine uygun olarak yeniden düzenlenmiştir.

---

## 📌 Neler Bulacaksınız?

Bu playbook içerisinde sıfırdan başlayarak ileri seviye zaman serilerine ve SQL-Pandas dönüşümlerine kadar uzanan 10 modüllük bir müfredat yer almaktadır.

### 📂 İçerik İndeksi

| Sıra | Modül Adı | Öğrenilecek Temel Kavramlar |
| :---: | :--- | :--- |
| **01** | [Pandas Temelleri](./01_pandas_temelleri.md) | `DataFrame`, `Series` farkı, `index` yönetimi ve temel atamalar. |
| **02** | [Veri Okuma ve Temel Manipülasyon](./02_pandas_ile_veri_okuma_ve_manipulasyon.md) | CSV okuma (`read_csv`), `shape`, `info`, `pd.cut()` ile kategori üretme. |
| **03** | [Sıralama ve Filtreleme](./03_pandas_siralama_ve_filtreleme.md) | `sort_values`, mantıksal filtreleme (`&`, `\|`), SQL tarzı `query()` metodu. |
| **04** | [Excel Okuma ve Yazma](./04_pandas_ile_excel_okuma_ve_yazma.md) | `openpyxl` motor bağımlılığı, `read_excel`, optimize edilmiş `to_csv` çıktısı. |
| **05** | [Veri Tipleri ve Eksik Veri Yönetimi](./05_pandas_veri_tipleri_ve_eksik_veri_yonetimi.md) | `astype()` tip dönüşümü, `isnull()`, `dropna()` temizliği ve `iloc` kullanımı. |
| **06** | [Metin (String) İşlemleri](./06_pandas_string_islemleri_ve_veri_manipulasyonu.md) | `.str` aksesuvarı, regex ile toplu `replace`, nokta atışı `at` güncellemesi. |
| **07** | [GroupBy ve Toplulaştırma](./07_pandas_groupby_ve_toplulaştırma_fonksiyonları.md) | `groupby`, `agg()` fonksiyonu esnekliği ve `MultiIndex` yapılarında sıralama. |
| **08** | [Tarih-Saat (Datetime) İşlemleri](./08_pandas_tarih_saat_datetime_islemleri.md) | `DatetimeIndex`, `pd.to_datetime`, `.dt` aksesuvarı ve zaman farkı hesaplama. |
| **09** | [Veri Birleştirme ve Join İşlemleri](./09_pandas_ile_veri_birlestirme_ve_join_islemleri.md) | `pd.concat()` dikey birleşim, `pd.merge()` ile Inner/Outer Join mantığı. |
| **10** | [SQL vs. Pandas Karşılaştırma Rehberi](./10_sql_ve_pandas_karsilastirmali_rehber.md) | SQL komutlarının (`SELECT`, `WHERE`, `UPDATE`, `DELETE`) Pandas karşılıkları. |

---

## 🚀 Nasıl Kullanılır?

Notlar tamamen modüler ve bağımsız şekilde tasarlanmıştır. İlgilendiğiniz konunun üzerindeki bağlantıya tıklayarak doğrudan dökümana gidebilir, kod bloklarını kopyalayarak kendi Jupyter Notebook veya Python script'lerinizde deneyebilirsiniz.

### Kullanılan Örnek Veri Setleri
Ders notlarında kullanılan test veri setleri açık kaynaklı repolardan dinamik olarak çekilmektedir:
* `simple_data.csv` & `simple_data.xlsx` (Perakende ve Çalışan Verisi)
* `OnlineRetail2.csv` (Uluslararası E-Ticaret Verisi)
* `tips.csv` (Restoran Bahşiş Analizi Verisi)

---

## 🛠️ Kurulum ve Bağımlılıklar

Bu notlardaki tüm kodları yerel ortamınızda çalıştırmak için aşağıdaki kütüphanelerin yüklü olması yeterlidir:

```bash
pip
