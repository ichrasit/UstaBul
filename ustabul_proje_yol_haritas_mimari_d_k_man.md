# 🚀 UstaBul - Ürün & Mühendislik Yol Haritası (Roadmap)

## 1. Yönetici Özeti & Vizyon

**UstaBul**, hizmet sektöründe asimetrik iş modeliyle (Martı TAG ilhamıyla) çalışan modern, konum tabanlı bir hizmet pazar yeridir:

* **Müşteriler (Ücretsiz Erişim):** Hizmet talebi oluşturur (tesisat, boya, elektrik vb.), gelen teklifleri inceler ve onaylı ustalarla sıfır platform ücreti ödeyerek anlaşır.

* **Hizmet Verenler / Ustalar (Abonelik Modeli):** Müşteri taleplerine ulaşmak, teklif sunmak ve onaylı profillerini öne çıkarmak için düzenli abonelik ücreti (aylık/yıllık) öder.

* **Büyüme Motoru:** Müşteri hacmini artırmak ve arz-talep dengesini hızlıca kurmak için performans pazarlaması ve marka kampanyaları profesyonel bir **reklam ajansı** tarafından yönetilir.

## 2. İş Modeli & Operasyon

```
                   +------------------------------+
                   |     Harici Reklam Ajansı     |
                   +--------------+---------------+
                                  |
                   +--------------v---------------+
                   | Hedefli Büyüme Kampanyaları  |
                   +--------------+---------------+
                                  |
        +-------------------------+-------------------------+
        |                                                   |
        v                                                   v
+------------------+                              +-------------------+
|   MÜŞTERİLER     |                              |     USTALAR       |
|  (Son Kullanıcı) |                              | (Hizmet Verenler) |
+--------+---------+                              +---------+---------+
         |                                                  |
         | %100 Ücretsiz Hizmet Talebi                      | Ücretli Abonelik
         v                                                  v
+---------------------------------------------------------------------+
|                          UstaBul Platformu                          |
+---------------------------------------------------------------------+
```

* **Gelir Modeli (Monetization):**
  * Ustalar için Başlangıç, Pro ve Sınırsız aylık/yıllık abonelik paketleri.
  * Müşterilerden hiçbir komisyon veya gizli işlem ücreti alınmaz.

* **Sosyal Kanıt & Kalite Kontrol:**
  * Onaylı Usta Rozetleri (Kimlik ve Vergi Levhası kontrolü).
  * Hizmet sonrası puanlama, yorumlama ve dinamik değerlendirme algoritması.

## 3. Mühendislik Mimarisi (Microservices & ELK Stack)

Backend mimarisi, yüksek lead hacmi altında sorunsuz ölçeklenecek ve **ELK Stack** ile tam izlenebilirlik sağlayacak şekilde mikroservisler halinde tasarlanmıştır.

```
                           +------------------------+
                           |   Kullanıcı Uygulama   |
                           |   (iOS / Android / Web) |
                           +-----------+------------+
                                       |
                                       v
                           +------------------------+
                           |      API Gateway       |
                           | (Auth Guard / Rate Lim)|
                           +-----------+------------+
                                       |
     +-----------------+---------------+-----------------+-----------------+
     |                 |                                 |                 |
     v                 v                                 v                 v
+----------+     +-----------+                     +-----------+     +-----------+
| Kullanıcı|     | Talep &   |                     | Abonelik  |     | Değerlen- |
| & Auth   |     | Teklif    |                     | & Ödeme   |     | dirme     |
| Servisi  |     | Servisi   |                     | Servisi   |     | Servisi   |
+----+-----+     +-----+-----+                     +-----+-----+     +-----+-----+
     |                 |                                 |                 |
     +-----------------+---------------+-----------------+-----------------+
                                       | (Asenkron Etkinlik / Yapılandırılmış Log)
                                       v
                         +---------------------------+
                         |        Logstash           |
                         +-------------+-------------+
                                       |
                                       v
                         +---------------------------+
                         |      Elasticsearch        |
                         +-------------+-------------+
                                       |
                                       v
                         +---------------------------+
                         |     Kibana Dashboard      |
                         +---------------------------+
```

### Temel Mikroservisler

1. **Kullanıcı & Auth Servisi:** Kullanıcı profilleri, OTP/JWT kimlik doğrulama, Rol Tabanlı Erişim (Müşteri vs. Usta).
2. **Talep & Eşleşme Servisi:** İş ilanlarının coğrafi indekslenmesi, kategori etiketleme, yakındaki aktif ustalara anlık bildirim.
3. **Abonelik Servisi:** Paket yönetimi, tekrarlayan faturalandırma motoru, aboneliği olmayan ustalar için özellik kısıtlama.
4. **Değerlendirme & Sosyal Kanıt Servisi:** Onaylı yorum toplama, usta güven skoru, şikayet ve bildirim yönetimi.
5. **Bildirim Servisi:** Anlık bildirimler (Push Notifications), SMS ve e-posta gönderimleri (Message Queue yapısı ile).

### ELK Stack Entegrasyonu

* **Elasticsearch:** Konum/kategori bazlı hızlı arama ve yüksek performanslı merkezi log depolama.
* **Logstash:** Tüm mikroservislerden gelen log verilerini toplayan, filtreleyen ve dönüştüren ara hat.
* **Kibana:** Sistem sağlığı takibi, hata analizi ve ürün analitiği (aktif talepler, eşleşme süreleri, abonelik dönüşüm oranları) için canlı paneller.

## 4. Aşamalı Uygulama Yol Haritası (Roadmap)

```
+---------------------------------------------------------------------------------------------------+
| AŞAMA 1: Sistem Tasarımı & Temel Kurulumlar                                                       |
+---------------------------------------------------------------------------------------------------+
| * Servis sınırlarının ve veri protokollerinin belirlenmesi                                        |
| * Dockerized mikroservis şablonu ve API Gateway kurulumu                                          |
| * ELK Stack mimarisinin hazırlanması (Elasticsearch + Logstash + Kibana)                           |
+---------------------------------------------------------------------------------------------------+
                                                 |
                                                 v
+---------------------------------------------------------------------------------------------------+
| AŞAMA 2: Çekirdek Mikroservislerin Geliştirilmesi                                                 |
+---------------------------------------------------------------------------------------------------+
| * Auth Servisi: Kayıt, Giriş, JWT doğrulamaları, Rol doğrulama                                    |
| * Talep Servisi: İş ilanı oluşturma, konum bazlı sorgulama mantığı                               |
| * Abonelik Servisi: Paket limitleri, ödeme altyapısı entegrasyonu                                 |
| * Tüm servislerin ELK yapısına log akışının sağlanması                                           |
+---------------------------------------------------------------------------------------------------+
                                                 |
                                                 v
+---------------------------------------------------------------------------------------------------+
| AŞAMA 3: Eşleşme Motoru & Anlık Bildirimler                                                       |
+---------------------------------------------------------------------------------------------------+
| * Konum yarıçapına göre eşleşme algoritması (İş konumu ile yakındaki aboneli ustalara erişim)     |
| * Push bildirim kuyruğu (Firebase/WebPush entegrasyonu)                                          |
| * Müşteri ve Usta arasındaki teklif verme ve iletişim akışı                                       |
+---------------------------------------------------------------------------------------------------+
                                                 |
                                                 v
+---------------------------------------------------------------------------------------------------+
| AŞAMA 4: Kalite, Sosyal Kanıt & Analitik                                                          |
+---------------------------------------------------------------------------------------------------+
| * Onaylı işlemler için Değerlendirme & Yorum mekanizması                                          |
| * Kibana üzerinde iş analitiği panelleri (Talep sayıları, dönüşüm oranları, sistem gecikmeleri)   |
| * Yük testleri (Load testing) ve API Gateway hız limitleri (Rate-limiting)                        |
+---------------------------------------------------------------------------------------------------+
                                                 |
                                                 v
+---------------------------------------------------------------------------------------------------+
| AŞAMA 5: Ajans El Sıkışması & Canlıya Geçiş                                                        |
+---------------------------------------------------------------------------------------------------+
| * Ajans brief'inin netleştirilmesi, kreatif varlıklar, takip pikselleri/olayları                  |
| * Pilot şehirde soft launch (Ön lansman)                                                          |
| * Reklam kampanyalarının başlatılması ve usta alımının ölçeklenmesi                               |
+---------------------------------------------------------------------------------------------------+
```

## 5. Sonraki Adımlar Kontrol Listesi

* \[ \] Backend yazılım dili/framework seçimi (Node.js / Go / Java Spring Boot).
* \[ \] Ortak GitHub deposunun ve mikroservis klasör yapısının oluşturulması.
* \[ \] Yerel Docker Compose dosyasının yapılandırılması (PostgreSQL/MongoDB ve ELK Stack).
* \[ \] Kullanıcı, Talep ve Abonelik alanları için veri şemalarının tasarımı.