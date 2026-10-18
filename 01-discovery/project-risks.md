# Project Risks, Assumptions, Constraints & Dependencies
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

---

## 1. Project Risks (Proje Riskleri)

| Risk ID | Risk Tanımı | Olasılık | Etki | Mitigation Planı |
|---------|-------------|----------|------|--------------------|
| R-01 | Regülasyon gereksinimlerinin proje sırasında değişmesi/güncellenmesi | Orta | Yüksek | Compliance departmanıyla düzenli senkronizasyon toplantıları; regülasyon takip mekanizması kurulması |
| R-02 | TPP kimlik doğrulama/yetkilendirme mekanizmasında güvenlik açığı | Düşük | Çok Yüksek | Penetrasyon testleri, güvenlik mimarisi review'ı, eIDAS/sertifika tabanlı doğrulama |
| R-03 | Müşterilerin rıza (consent) sürecini anlamaması/yanlış onay vermesi | Orta | Orta | Açık, sade UX dili; rıza ekranlarında zorunlu bilgilendirme; kullanıcı testi |
| R-04 | Çekirdek bankacılık sistemiyle entegrasyon karmaşıklığı / performans sorunları | Yüksek | Yüksek | Erken aşamada teknik fizibilite çalışması, API gateway/caching katmanı planlaması |
| R-05 | TPP'lerin API dokümantasyonunu yetersiz bulması, entegrasyon süresinin uzaması | Orta | Orta | Sandbox ortamı + kapsamlı Swagger/OpenAPI dokümantasyonu + developer portal |
| R-06 | Veri ihlali / yetkisiz erişim vakası | Düşük | Çok Yüksek | Audit trail, anomali tespiti, rate limiting, sıfır güven (zero-trust) mimarisi |
| R-07 | Proje kapsamının zamanla genişlemesi (scope creep) | Orta | Orta | Net kapsam dokümantasyonu, değişiklik yönetim süreci (change control) |
| R-08 | İç paydaşlar arası önceliklendirme çatışması (IT vs Compliance vs Business) | Orta | Orta | RACI matrisi, düzenli steering committee toplantıları |
| R-09 | Düzenleyici denetim sırasında uyumsuzluk tespiti | Düşük | Çok Yüksek | İç denetim simülasyonu, uyum kontrol listesi, bağımsız güvenlik/uyum review'ı |
| R-10 | Üçüncü taraf (TPP) kaynaklı veri kötüye kullanımı | Düşük | Yüksek | TPP sözleşme şartları, sertifikasyon zorunluluğu, sürekli izleme |

---

## 2. Assumptions (Varsayımlar)

- Banka, gerekli düzenleyici lisans/yetkilendirmeye (TPP'lerin sertifikasyonunu kabul etme yetkisi dahil) sahiptir.
- Çekirdek bankacılık sistemi, API katmanına gerekli veri erişimini sağlayabilecek teknik kapasiteye sahiptir.
- Müşteriler dijital kanallar üzerinden kimlik doğrulama yapabilmektedir (SCA için gerekli altyapı mevcuttur).
- Üçüncü taraf sağlayıcılar (TPP), ilgili düzenleyici otorite tarafından sertifikalandırılmış/lisanslı olacaktır.
- Proje için gerekli bütçe ve kaynak (IT, Compliance, Security) üst yönetim tarafından onaylanmıştır.
- Regülasyon gereksinimleri proje süresince büyük ölçüde sabit kalacaktır.

---

## 3. Constraints (Kısıtlar)

- **Regülasyon Takvimi:** Projenin, düzenleyici otorite tarafından belirlenen zorunlu uyum tarihine kadar tamamlanması gerekmektedir.
- **Mevcut Sistem Mimarisi:** Çekirdek bankacılık sisteminin değiştirilmemesi, entegrasyonun yalnızca API katmanı üzerinden yapılması gerekliliği.
- **Güvenlik Standartları:** Bankanın iç bilgi güvenliği politikaları ve sektör standartları (ISO 27001, PCI-DSS vb.) ile uyumluluk zorunluluğu.
- **Bütçe Sınırlaması:** Proje, onaylanmış bütçe çerçevesinde tamamlanmalıdır.
- **Veri Yerel Saklama (Data Residency):** Müşteri verilerinin yasal düzenlemeler gereği yurt içinde saklanması zorunluluğu.

---

## 4. Dependencies (Bağımlılıklar)

| Bağımlılık | Açıklama | Bağımlılık Türü |
|------------|----------|-------------------|
| Çekirdek Bankacılık Sistemi (Core Banking) Veri Erişimi | API katmanının hesap/bakiye verisine erişimi için gerekli | Teknik |
| Kimlik ve Erişim Yönetimi (IAM) Sistemi | SCA ve TPP yetkilendirmesi için mevcut IAM altyapısıyla entegrasyon | Teknik |
| Regülasyon Onayı / Lisanslama | Düzenleyici otoritenin proje kapsamını ve API standartlarını onaylaması | Yasal/Regülasyon |
| Bilgi Güvenliği Politika Onayı | InfoSec departmanının güvenlik mimarisini onaylaması | Organizasyonel |
| TPP Sertifikasyon Otoritesi | Üçüncü tarafların sertifikalandırılması için dış kurum bağımlılığı | Dış (3. Taraf) |
| Hukuk Departmanı Sözleşme Onayı | TPP'lerle yapılacak veri paylaşım sözleşmelerinin hukuki onayı | Organizasyonel |

---

*Bu doküman, Aşama 1 kapsamında üretilen Project Risks, Assumptions, Constraints ve Dependencies bölümlerinin konsolide edilmiş halidir.*
