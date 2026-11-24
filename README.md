# 🛒 Olist E-Ticaret Veri Analizi ve Performans Raporlaması

![Python](https://img.shields.io/badge/Python-3.13-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-Matrix%20%26%20Simulation-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)

Brezilya'nın en büyük e-ticaret pazar yerlerinden **Olist**'e ait 8 farklı ilişkisel veri tablosunu bir araya getirerek; veri temizleme, özellik mühendisliği (feature engineering), lojistik performansı, müşteri memnuniyeti, satıcı segmentasyonu, ödeme alışkanlıkları ve finansal matris simülasyonlarını kapsayan uçtan uca bir veri analizi projesidir.

---

## 📌 Proje Özeti ve Kapsam

Bu çalışmada ham e-ticaret kayıtları ilişkisel olarak birleştirilmiş, eksik veriler istatistiksel yöntemlerle temizlenmiş ve iş kararlarına doğrudan girdi sağlayabilecek **15 kritik performans sorusu** yanıtlanmıştır.

### Kullanılan Veri Tabloları (8 İlişkisel CSV)
| Dosya Adı | İçerik |
| :--- | :--- |
| `olist_orders_dataset.csv` | Sipariş durumları ve satın alma / kargo / teslimat tarihleri (99.441 kayıt) |
| `olist_order_items_dataset.csv` | Sipariş kalemleri, ürün fiyatları ve kargo ücretleri |
| `olist_customers_dataset.csv` | Müşteri kimlikleri, şehir ve eyalet bilgileri |
| `olist_products_dataset.csv` | Ürün kategorileri, ağırlık ve paket boyutları (cm) |
| `olist_order_payments_dataset.csv` | Ödeme yöntemleri, taksit sayıları ve ödeme tutarları |
| `olist_order_reviews_dataset.csv` | Müşteri değerlendirme puanları (1–5) ve yorum tarihleri |
| `olist_sellers_dataset.csv` | Satıcı kimlikleri ve konum bilgileri |
| `product_category_name_translation.csv` | Portekizce kategori adlarının İngilizce karşılıkları |

---

## ⚙️ Uygulanan Veri İşleme ve Analiz Adımları

1. **Veri Keşfi ve Temizleme:**
   * Teslim tarihi eksik olan 2.965 sipariş çıkarılarak **96.476 teslim edilmiş sipariş** üzerinden analiz kurgulandı.
   * Ürün boyutlarındaki eksik ölçümler (`weight`, `length`, `height`, `width`), ilgili kategorinin ortalaması (`groupby().transform("mean")`) ile dolduruldu.
   * Kategori isimleri İngilizce çeviri tablosuyla eşleştirildi ve eksik kategoriler `unknown` olarak etiketlendi.
2. **İlişkisel Birleştirme (Data Merging):**
   * Siparişler, ürünler, müşteriler, satıcılar, yorumlar ve ödemeler `order_id`, `customer_id`, `product_id` ve `seller_id` anahtarları üzerinden tek bir **ana analitik tabloda (115.722 satır)** birleştirildi.
3. **Özellik Mühendisliği (Feature Engineering):**
   * **Ürün Hacmi & Segmenti:** NumPy dizileri ile `product_volume_cm3` hesaplandı ve ürünler `Küçük`, `Orta`, `Büyük`, `Çok büyük` sınıflarına ayrıldı.
   * **Bölgesel & Satıcı Tipi Sınıflandırması:** Müşteri eyaletleri bölgelere ayrıldı; `np.where` ile `Yerel satıcı` / `Farklı eyaletteki satıcı` ayrımı oluşturuldu.
   * **Lojistik Metrikleri:** Gerçekleşen teslimat süresi (`teslimat_suresi_gun`), tahmini süre (`tahmini_sure_gun`) ve gecikme günü (`gecikme_gun`) türetildi.
   * **Memnuniyet & Satıcı Segmentasyonu:** Müşteri puanlarına göre memnuniyet grupları; sipariş hacmi, puan ve gecikme oranına göre 5 kademeli satıcı performans sınıfları oluşturuldu.
4. **NumPy Vektörel Simülasyon ve Matris İşlemleri:**
   * Fiyatlara **+%10 zam** ve kargo ücretlerine **-%5 indirim** uygulanması senaryosunun toplam gelir etkisi hesaplandı.
   * En popüler 3 kategori × 4 ödeme yöntemi çapraz tablosu `(3, 4)` NumPy matrisine dönüştürülerek transpoze ve ağırlıklı finansal matris çarpımı (`np.dot`) uygulandı.

---

## 📊 Temel Bulgular ve KPI Özeti

### Genel İşletme Metrikleri
| Metrik | Değer |
| :--- | :--- |
| **Teslim Edilen Benzersiz Sipariş** | 96.476 |
| **Benzersiz Satıcı / Ürün Sayısı** | 2.970 Satıcı / 32.214 Ürün |
| **Toplam Ödeme Hacmi (Birleşik Tablo)** | 19.881.454,95 BRL |
| **Toplam Kargo Geliri** | 2.312.280,16 BRL |
| **Ortalama Teslimat Süresi** | 12,0 gün |
| **Sipariş Bazlı Gecikme Oranı** | %6,8 |
| **Genel Müşteri Memnuniyet Puanı** | 4,08 / 5,00 |

### Öne Çıkan Analitik Çıkarımlar
* **Gecikmenin Müşteri Memnuniyetine Etkisi:** Zamanında veya erken teslim edilen siparişlerin ortalama puanı **4,21** iken, geciken siparişlerde bu ortalama **2,25**'e düşmektedir. Erken teslimat oranı **%92,16** seviyesindedir.
* **Lojistik ve Paket Boyutu İlişkisi:** Ürün hacmi büyüdükçe ortalama teslimat süresi düzenli olarak artmaktadır (`Küçük`: 11,0 gün → `Orta`: 11,7 gün → `Büyük`: 11,9 gün → `Çok büyük`: 13,0 gün). Kargo ücreti ile teslimat süresi arasında pozitif ilişki (`r = 0.21`) gözlemlenmiştir.
* **Kategori Performansı:** Ürün geliri bazında ilk 3 kategori `health_beauty` (1,28M), `watches_gifts` (1,21M) ve `bed_bath_table` (1,09M) olurken; en yüksek satış adedine **11.814 adet** ile `bed_bath_table` ulaşmıştır.
* **Bölgesel Dağılım:** Sipariş ve gelir lideri **SP (São Paulo)** eyaletidir (40.495 sipariş, ort. 8,3 gün teslimat, 4,18 puan). En yüksek gecikme oranına sahip eyalet **AL**, geciken siparişlerde en uzun ortalama gecikmeye sahip eyalet ise **AP (96,3 gün)** olmuştur.
* **Ödeme Davranışları:** En yaygın ödeme yöntemi **kredi kartı**dır (85.372 işlem, ort. 3,63 taksit). Tek çekim ortalama ödeme tutarı **131,78 BRL** iken taksitli alışverişlerde bu tutar **211,59 BRL**'ye çıkmaktadır.
* **Satıcı Segmentasyonu:** Aktif satıcıların **751**'i `Çok başarılı`, **382**'si `Başarılı`, **85**'i `Ortalama`, **19**'u `Riskli` ve **1.733**'ü `Yeni/Az Satış Yapan` sınıfında yer almaktadır.
* **Fiyatlandırma Simülasyonu:** +%10 ürün fiyatı artışı ve -%5 kargo indirimi senaryosu toplamda **+1.271.972,87 BRL** ek gelir potansiyeli üretmektedir.

---

## 📁 Proje Klasör Yapısı

```text
olist-ecommerce-data-analysis/
├── data/                         # Kaggle Olist CSV dosyaları (repo boyutunu korumak için .gitignore ile hariç tutulur)
├── olist_ecommerce_analysis.ipynb # Ana Jupyter Notebook analiz dosyası
├── .gitignore                    # Büyük veri dosyalarını ve geçici notebook dosyalarını hariç tutar
└── README.md                     # Proje dokümantasyonu
```

---

## 🚀 Kurulum ve Çalıştırma

1. **Repoyu klonlayın:**
   ```bash
   git clone https://github.com/esmanuryavuz908/olist-ecommerce-data-analysis.git
   cd olist-ecommerce-data-analysis
   ```

2. **Gerekli kütüphaneleri yükleyin:**
   ```bash
   pip install numpy pandas jupyter
   ```

3. **Veri setini indirin:**
   * [Kaggle – Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) sayfasından veri setini indirin.
   * Proje ana dizininde `data/` klasörü oluşturup tüm `.csv` dosyalarını içine yerleştirin.

4. **Notebook'u başlatın:**
   ```bash
   jupyter notebook
   ```

---

## 👩‍💻 Geliştirici

**Esmanur Yavuz**  
Data Science & Analytics Student
