# 📑 Pandas ile Excel Dosyalarını Okuma ve Yazma

Veri analitiği süreçlerinde veriler her zaman CSV formatında gelmez; Excel (`.xlsx`, `.xls`) dosyaları kurumsal dünyada oldukça yaygındır. Bu bölümde Pandas ile Excel dosyalarını nasıl okuyacağımızı, gerekli motor (engine) bağımlılıklarını nasıl çözeceğimizi ve işlenmiş veriyi tekrar diske nasıl kaydedeceğimizi öğreneceğiz.

---

## 1. Gerekli Kütüphanelerin Kurulumu

Pandas, arka planda Excel dosyalarını anlamlandırmak için özel motorlara (engine) ihtiyaç duyar. Modern `.xlsx` dosyaları için en popüler motor **`openpyxl`** kütüphanesidir.

Eğer sisteminizde Excel okuma hatası alıyorsanız, terminalinizde veya Jupyter Notebook üzerinde şu kurulumları yapmanız gerekir:

bash
# Excel motorunu kurun
pip install openpyxl

Eğer üzerinde çalıştığınız Linux tabanlı sunucuda (örn: CentOS/RHEL) veri indirmek için wget kurulu değilse, paket yöneticisiyle kurulabilir:
sudo yum -y install wget

2. Excel Dosyasını İndirme ve Okuma
Örnek Excel veri setimizi yerel çalışma ortamımıza indirelim ve read_excel fonksiyonu yardımıyla Pandas DataFrame yapısına dönüştürelim:
import pandas as pd

# Örnek veri setini indirme
# !wget [https://github.com/erkansirin78/datasets/raw/master/simple_data.xlsx](https://github.com/erkansirin78/datasets/raw/master/simple_data.xlsx)

# Excel dosyasını okuma (openpyxl motoru ile)
df = pd.read_excel("simple_data.xlsx", engine='openpyxl')
df.head()

,sirano,isim,yas,meslek,sehir,aylik_gelir
0,1,Cemal,35,Isci,Ankara,3500
1,2,Ceyda,42,Memur,Kayseri,4200
2,3,Timur,30,Müzisyen,Istanbul,9000
3,4,Burcu,29,Pazarlamaci,Ankara,4200
4,5,Yasemin,23,Pazarlamaci,Bursa,4800

💡 Karakter Kodlaması (Encoding) Notu: Excel dosyaları okunurken Türkçe karakterlerin (ş, ç, ö, ü, ı, ğ) MÃ¼zisyen gibi bozuk görünmemesi için arka planda doğru encoding altyapısının kullanılması önemlidir. read_excel bu işlemi modern Excel dosyalarında otomatik olarak optimize eder.

3. DataFrame'i Diske Yazma ve Kaydetme (to_csv)
Üzerinde çalıştığımız, temizlediğimiz veya manipüle ettiğimiz verileri daha sonra kullanmak üzere farklı formatlarda diske kaydedebiliriz. En sık kullanılan yöntem veriyi CSV formatına aktarmaktır.
# Veriyi özelleştirilmiş parametrelerle CSV olarak kaydetme
df.to_csv("simple_data_from_excel.csv", 
          sep=";", 
          header=True, 
          encoding="utf-8", 
          index=False)

⚙️ Kullanılan Parametrelerin Açıklamaları:
sep=";": Değerlerin birbirlerinden ne ile ayrılacağını belirler. Varsayılan olarak virgüldür (,) ancak Excel ve Türkçe işletim sistemleriyle tam uyumluluk için noktalı virgül (;) sıkça tercih edilir.

header=True: Sütun isimlerinin dosyanın ilk satırına yazılmasını sağlar.

encoding="utf-8": Türkçe karakterlerin (ş, ç, ğ vb.) bozulmadan, evrensel standartta kaydedilmesini garanti altına alır.

index=False: Pandas'ın otomatik oluşturduğu satır indeks numaralarının (0, 1, 2...) dosyaya yeni bir sütunmuş gibi kaydedilmesini engeller. Temiz bir çıktı için kritik bir parametredir.

