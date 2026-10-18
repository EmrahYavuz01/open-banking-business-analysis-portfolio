# Software Requirements Specification (SRS)
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

**Standart Uyumu:** IEEE 830-1998

---

## 1. Introduction

### 1.1 Purpose
Bu doküman, platformun fonksiyonel, fonksiyonel olmayan, güvenlik, arayüz, veri ve hata yönetimi gereksinimlerini IEEE 830 standardına uygun şekilde tanımlamaktadır.

### 1.2 Scope
Bu SRS, [`brd-business-requirements-document.md`](./brd-business-requirements-document.md) (BR-01—BR-22) ve [`epic-and-user-story-model.md`](./epic-and-user-story-model.md) (US-01—US-24) kapsamındaki tüm iş gereksinimlerinin teknik gereksinimlere dönüştürülmüş halini kapsar.

### 1.3 Definitions & Abbreviations

| Terim | Açıklama |
|---|---|
| AIS | Account Information Service |
| TPP | Third Party Provider |
| SCA | Strong Customer Authentication |
| RBAC | Role Based Access Control |
| OAuth2 | Open Authorization 2.0 |
| TLS | Transport Layer Security |
| Consent | Müşterinin veri paylaşımına verdiği dijital rıza |
| QWAC/QSEAL | TPP kimlik doğrulamasında kullanılan nitelikli sertifika türleri |
| SLA | Service Level Agreement |
| FR / NFR / SEC | Functional / Non-Functional / Security Requirement |

### 1.4 References

| Referans | Doküman |
|---|---|
| REF-01 | `01-discovery/` — Business Goals, Business Case, Stakeholder Analysis, RACI |
| REF-02 | `02-process-analysis/as-is-process-analysis.md`, `root-cause-analysis.md` |
| REF-03 | `02-process-analysis/gap-analysis.md` |
| REF-04 | `02-process-analysis/to-be-process-design.md` |
| REF-05 | `03-architecture/solution-architecture-overview.md` |
| REF-06 | `04-requirements/brd-business-requirements-document.md` |
| REF-07 | `04-requirements/epic-and-user-story-model.md` |
| REF-08 | `04-requirements/acceptance-criteria.md` |
| REF-09 | IEEE 830-1998 Standardı |

---

## 2. Overall Description

### 2.1 Product Perspective
Sistem, bankanın mevcut Core Banking altyapısının üzerine konumlanan, **API-first** mimaride yeni bir entegrasyon ve hizmet katmanıdır.

### 2.2 Product Functions
- Müşteri rızasının dijital olarak verilmesi, görüntülenmesi ve iptal edilmesi
- TPP'lerin self-servis kayıt, sertifikasyon doğrulama ve sandbox testi
- Müşteri rızası kapsamında gerçek zamanlı hesap bilgisi paylaşımı
- Güçlü kimlik doğrulama ve yetkilendirme kontrolü
- Tüm işlemlerin denetlenebilir şekilde kayıt altına alınması ve raporlanması

### 2.3 User Classes

| Kullanıcı Sınıfı | Açıklama | Teknik Yetkinlik |
|---|---|---|
| Müşteri (Customer) | Hesap sahibi, rıza veren/iptal eden nihai kullanıcı | Düşük-Orta |
| TPP Geliştirici | Developer Portal üzerinden entegre olan dış geliştirici | Yüksek |
| Compliance Kullanıcısı | TPP onboarding onayı, denetim raporları görüntüleme | Orta |
| InfoSec Kullanıcısı | Güvenlik politikaları tanımlama, anomali izleme | Yüksek |
| Operasyon Kullanıcısı | İstisna durum yönetimi, destek | Orta |
| Sistem (Otomatik Aktör) | API Gateway, Consent Mgmt, OAuth2 Server gibi otomatik bileşenler | N/A |

### 2.4 Operating Environment
Sistem, bankanın mevcut kurumsal IT altyapısı içinde, bulut veya on-premise hibrit ortamda çalışacak şekilde tasarlanır.

### 2.5 Constraints
- Düzenleyici otoritenin belirlediği uyum tarihine kadar production'a alınma zorunluluğu
- Core Banking sisteminin değiştirilmemesi
- Veri yerel saklama (data residency) yasal zorunluluğu
- Mevcut bilgi güvenliği politikalarına (ISO 27001, PCI-DSS) tam uyum

### 2.6 Assumptions & Dependencies
Bölüm 9'da detaylandırılmıştır.

---

## 3. Functional Requirements

### 3.1 Consent Management

| Req ID | Requirement Name | Description | Related BR | Related US | Priority |
|---|---|---|---|---|---|
| FR-01 | Rıza Oluşturma | Sistem, müşterinin onayladığı TPP, veri kapsamı ve süre bilgisini içeren bir rıza kaydı oluşturmalıdır | BR-01 | US-01 | Must |
| FR-02 | Rıza Görüntüleme | Sistem, müşterinin tüm aktif ve geçmiş rızalarını TPP adı, kapsam ve tarih bilgisiyle listelemelidir | BR-02 | US-02 | Must |
| FR-03 | Rıza İptali | Sistem, müşterinin aktif bir rızayı anlık olarak iptal edebilmesini ve ilgili erişim token'ını geçersiz kılmasını sağlamalıdır | BR-03 | US-03 | Must |
| FR-04 | Rıza Süre Yönetimi | Sistem, tanımlanan süre dolduğunda rızayı otomatik olarak "süresi dolmuş" durumuna almalı ve erişimi kesmelidir | BR-04 | US-04 | Must |
| FR-05 | Rıza Bildirimleri | Sistem, rıza talebi, onay ve iptal işlemlerinde müşteriye otomatik bildirim göndermelidir | BR-05 | US-05 | Should |
| FR-06 | Rıza Geçmişi Raporlama | Sistem, Compliance kullanıcısının belirli bir müşterinin tüm rıza geçmişini sorgulayabilmesini sağlamalıdır | BR-01, BR-19 | US-06 | Should |

### 3.2 TPP Onboarding & Yetkilendirme

| Req ID | Requirement Name | Description | Related BR | Related US | Priority |
|---|---|---|---|---|---|
| FR-07 | TPP Self-Servis Kayıt | Sistem, TPP'lerin Developer Portal üzerinden kayıt formu doldurarak başvuru yapabilmesini sağlamalıdır | BR-06 | US-07 | Must |
| FR-08 | Sertifika Yükleme | Sistem, TPP'nin kayıt sırasında sertifika/lisans dosyasını yükleyebilmesini sağlamalıdır | BR-07 | US-08 | Must |
| FR-09 | Otomatik Sertifikasyon Doğrulama | Sistem, yüklenen sertifikayı otomatik olarak doğrulamalı ve sonucu kayıt durumuna yansıtmalıdır | BR-07 | US-09 | Must |
| FR-10 | Sandbox Erişimi | Sistem, doğrulama sürecini geçen TPP'lere mock veri içeren sandbox ortamına erişim sağlamalıdır | BR-08 | US-10 | Must |
| FR-11 | Manuel İnceleme Kuyruğu | Sistem, otomatik doğrulamada belirsiz/şüpheli sonuç alınan TPP başvurularını Compliance manuel inceleme kuyruğuna yönlendirmelidir | BR-09 | US-11 | Should |
| FR-12 | Periyodik Sertifikasyon Kontrolü | Sistem, TPP sertifikalarını periyodik olarak otomatik yeniden doğrulamalı ve süresi dolanları askıya almalıdır | BR-10 | US-12 | Must |

### 3.3 Hesap Bilgisi Erişimi (AIS)

| Req ID | Requirement Name | Description | Related BR | Related US | Priority |
|---|---|---|---|---|---|
| FR-13 | Kapsam Bazlı Veri Erişimi | Sistem, TPP'ye yalnızca consent token'ın kapsadığı veri alanlarını döndürmelidir | BR-11 | US-13 | Must |
| FR-14 | Gerçek Zamanlı Veri Sorgulama | Sistem, geçerli istekler için Core Banking'den gerçek zamanlı veri sorgulayıp yanıt döndürmelidir | BR-12 | US-14 | Must |
| FR-15 | Rıza Doğrulamasız Erişim Engelleme | Sistem, geçerli rıza/token olmayan hiçbir isteğe veri paylaşmamalıdır | BR-13 | US-15 | Must |
| FR-16 | Hizmet Sürekliliği | Sistem, tanımlanan kullanılabilirlik (uptime) hedefini karşılayacak şekilde tasarlanmalıdır | BR-14 | US-16 | Should |

### 3.4 Güvenlik ve Erişim Kontrolü (Fonksiyonel Boyut)

| Req ID | Requirement Name | Description | Related BR | Related US | Priority |
|---|---|---|---|---|---|
| FR-17 | TPP Kimlik Doğrulama Kontrolü | Sistem, her TPP isteğinde sertifika/OAuth2 tabanlı kimlik doğrulaması yapmalıdır | BR-15 | US-17 | Must |
| FR-18 | Müşteri SCA Doğrulaması | Sistem, rıza onayı öncesinde müşteriden güçlü kimlik doğrulama talep etmelidir | BR-16 | US-18 | Must |
| FR-19 | Anomali Tespiti ve Bildirimi | Sistem, anormal erişim paternlerini tespit edip ilgili ekiplere otomatik bildirim göndermelidir | BR-17 | US-19 | Should |
| FR-20 | Şifreli Veri Transferi | Sistem, tüm veri transferlerini şifrelenmiş kanal üzerinden gerçekleştirmelidir | BR-18 | US-20 | Must |

### 3.5 Denetim, Raporlama ve KPI Yönetimi

| Req ID | Requirement Name | Description | Related BR | Related US | Priority |
|---|---|---|---|---|---|
| FR-21 | Değiştirilemez İşlem Kaydı | Sistem, tüm rıza/erişim/veri paylaşım olaylarını değiştirilemez (immutable) şekilde kaydetmelidir | BR-19 | US-21 | Must |
| FR-22 | Otomatik Regülasyon Raporu Üretimi | Sistem, Compliance kullanıcısının belirli tarih aralığı için uyum raporu üretebilmesini sağlamalıdır | BR-20 | US-22 | Must |
| FR-23 | KPI Dashboard | Sistem, tanımlı operasyonel KPI'ları bir dashboard üzerinden görselleştirmelidir | BR-21 | US-23 | Should |
| FR-24 | TPP Kullanım İstatistikleri | Sistem, TPP'nin yalnızca kendi API kullanım istatistiklerini Developer Portal üzerinden görüntülemesini sağlamalıdır | BR-22 | US-24 | Could |

---

## 4. Non-Functional Requirements

| Req ID | Kategori | Requirement | Ölçüm Kriteri |
|---|---|---|---|
| NFR-01 | Availability | Sistem, aylık bazda %99.9 erişilebilirlik sağlamalıdır | Aylık uptime ≥ %99.9 |
| NFR-02 | Availability | Planlı bakımlar, TPP'lere en az 48 saat önceden bildirilmelidir | Bakım bildirim logları ile doğrulanır |
| NFR-03 | Performance | API yanıt süresi, normal yük altında p95 değerinde 500ms'i aşmamalıdır | API Gateway performans metrikleri |
| NFR-04 | Performance | Rıza onay süreci (SCA dahil), müşteri için 2 dakikayı aşmamalıdır | Uçtan uca süreç zamanlama logları |
| NFR-05 | Scalability | Sistem, TPP sayısının %100 artışında performans bozulması olmadan ölçeklenebilmelidir | Yük testi sonuçları |
| NFR-06 | Scalability | Sistem, saniyede en az 1000 eşzamanlı API isteğini karşılayabilmelidir | Performans/stres testi |
| NFR-07 | Reliability | API Gateway, herhangi bir bileşen arızasında otomatik failover sağlamalıdır | Failover testi (RTO < 5 dk) |
| NFR-08 | Reliability | Sistem hata oranı (4xx/5xx), toplam isteklerin %1'ini aşmamalıdır | API Gateway hata logları |
| NFR-09 | Maintainability | API'ler, geriye dönük uyumluluğu bozmadan versiyonlanabilmelidir | Versiyon değişikliği sonrası regresyon testleri |
| NFR-10 | Maintainability | Yeni bir TPP servis tipi (örn. PIS) eklenmesi, mevcut AIS mimarisini değiştirmeden gerçekleştirilebilmelidir | Mimari review |
| NFR-11 | Auditability | Sistem, her işlem için en az 7 yıl boyunca audit kaydı saklamalıdır | Log saklama politikası denetimi |
| NFR-12 | Auditability | Audit kayıtları, oluşturulduktan sonra hiçbir şekilde değiştirilemez veya silinemez olmalıdır | Immutability testi |

---

## 5. Security Requirements

| Req ID | Security Domain | Requirement |
|---|---|---|
| SEC-01 | OAuth2 Authorization | Sistem, TPP yetkilendirmesini OAuth2 Authorization Code Flow ile gerçekleştirmeli; client credentials doğrudan paylaşılmamalıdır |
| SEC-02 | Strong Customer Authentication (SCA) | Sistem, her rıza onayında en az iki bağımsız faktör ile müşteri kimlik doğrulaması yapmalıdır |
| SEC-03 | TLS Encryption | Sistem, tüm dış/iç API trafiğini minimum TLS 1.2 (tercihen TLS 1.3) ile şifrelemelidir |
| SEC-04 | Role Based Access Control (RBAC) | Sistem, her kullanıcı/sistem rolüne yalnızca yetkili olduğu işlevlere erişim vermelidir |
| SEC-05 | Rate Limiting | Sistem, her TPP için tanımlanan istek limitini aşan trafiği otomatik olarak sınırlamalı (throttle) ve loglamalıdır |
| SEC-06 | Audit Logging | Sistem, kimlik doğrulama, yetkilendirme, rıza ve veri erişim olaylarının tamamını merkezi, değiştirilemez audit log'a kaydetmelidir |
| SEC-07 | Certificate Validation | Sistem, TPP sertifikalarının geçerliliğini her erişimde doğrulamalıdır |
| SEC-08 | Session & Token Management | Sistem, access token ve refresh token'ların tanımlı bir yaşam süresine sahip olmasını sağlamalıdır |

---

## 6. External Interface Requirements

| Entegrasyon | Açıklama (Yüksek Seviye) |
|---|---|
| API Gateway | Tüm dış TPP trafiğinin geçtiği tek giriş noktası |
| OAuth2 Authorization Server | TPP ve müşteri yetkilendirme/token yaşam döngüsü işlemleri |
| Consent Management System | Müşteri rıza verilerinin merkezi kaynağı |
| Core Banking System | Hesap, bakiye ve işlem verisinin kaynağı; salt-okunur sorgular |
| Audit & Logging Platform | Tüm bileşenlerden gelen olayların toplandığı merkezi log platformu |
| Developer Portal | TPP'lerin kayıt, dokümantasyon, sertifika yükleme arayüzü |
| Sandbox Environment | TPP'lerin production öncesi mock veriyle test yaptığı izole ortam |

> Not: Bu bölümde API endpoint, request/response şema detayları **kapsam dışıdır**.

---

## 7. Data Requirements

| Veri Varlığı | Description | Key Attributes | Retention Requirement |
|---|---|---|---|
| Customer | Banka müşterisini temsil eden veri varlığı | Müşteri ID, Ad-Soyad, İletişim Bilgisi, Hesap Referansları | Müşteri ilişkisi süresince + yasal saklama süresi |
| Consent | Müşterinin TPP'ye verdiği rıza kaydı | Consent ID, Müşteri ID, TPP ID, Veri Kapsamı, Başlangıç/Bitiş Tarihi, Durum | Rıza süresi + minimum 7 yıl |
| TPP | Yetkilendirilmiş üçüncü taraf sağlayıcı kaydı | TPP ID, Şirket Bilgisi, Sertifika Bilgisi, Onboarding Durumu | TPP ilişkisi süresince + yasal saklama süresi |
| Account | Müşteri hesap bilgisi (Core Banking'den salt okunur referans) | Hesap No, Hesap Tipi, Bakiye, Para Birimi | Core Banking politikasına tabidir |
| Transaction | Hesap işlem geçmişi (salt okunur referans) | İşlem ID, Tarih, Tutar, İşlem Tipi | Core Banking politikasına tabidir |
| Access Token | TPP'nin API erişimi için kullandığı OAuth2 token | Token ID, Token Tipi, İlişkili Consent ID, Geçerlilik Süresi | Token süresi dolduktan sonra ~90 gün |
| Audit Log | Tüm sistem olaylarının değiştirilemez kaydı | Log ID, Zaman Damgası, Aktör, Olay Tipi, Sonuç | Minimum 7 yıl, immutable |

---

## 8. Error Handling Requirements

| Error Code | Error Description | Expected System Behaviour |
|---|---|---|
| ERR-401-01 | Invalid Token | API Gateway isteği reddeder, HTTP 401 döner, olay audit log'a kaydedilir |
| ERR-401-02 | Expired Token | HTTP 401 ve "token expired" hata kodu döner |
| ERR-403-01 | Expired Consent | HTTP 403 döner |
| ERR-403-02 | Revoked Consent | HTTP 403 döner, "consent revoked" hata mesajı iletilir |
| ERR-401-03 | Invalid Certificate | HTTP 401 döner, güvenlik olayı InfoSec'e bildirilir |
| ERR-403-03 | Unauthorized Access | HTTP 403 döner |
| ERR-429-01 | API Rate Limit Exceeded | HTTP 429 döner, "Retry-After" başlığı belirtilir |
| ERR-503-01 | Core Banking Unavailable | HTTP 503 döner, circuit breaker devreye girer |
| ERR-500-01 | Internal Server Error | HTTP 500 döner, teknik detay TPP'ye sızdırılmaz |

---

## 9. Assumptions & Dependencies

### Assumptions
- Düzenleyici gereksinimler, geliştirme süresince büyük ölçüde sabit kalacaktır.
- Core Banking sistemi gerekli salt-okunur veri erişimini sağlayabilecek kapasiteye sahiptir.
- Müşteriler dijital kanallar üzerinden SCA doğrulaması yapabilmektedir.
- TPP'ler ilgili düzenleyici otorite tarafından sertifikalandırılmış olacaktır.

### Dependencies
- Core Banking sisteminin API katmanına gerekli veri erişimini sağlaması
- IAM sisteminin SCA ve TPP yetkilendirmesi için mevcut altyapıyla entegre olması
- Düzenleyici otoritenin API standartlarını ve TPP onboarding kriterlerini onaylaması
- Bilgi Güvenliği departmanının güvenlik mimarisini onaylaması
- Hukuk departmanının TPP sözleşme şartlarını hazırlaması

---

## 10. Traceability Matrix

| Business Goal | Business Requirement | User Story | Functional Requirement |
|---|---|---|---|
| BO-2 | BR-01 | US-01 | FR-01 |
| BO-2 | BR-02 | US-02 | FR-02 |
| BO-2 | BR-03 | US-03 | FR-03 |
| BO-2 | BR-04 | US-04 | FR-04 |
| BO-2 | BR-05 | US-05 | FR-05 |
| BO-2, BO-1 | BR-01, BR-19 | US-06 | FR-06 |
| BO-3 | BR-06 | US-07 | FR-07 |
| BO-3 | BR-07 | US-08 | FR-08 |
| BO-3 | BR-07 | US-09 | FR-09 |
| BO-3 | BR-08 | US-10 | FR-10 |
| BO-3 | BR-09 | US-11 | FR-11 |
| BO-3 | BR-10 | US-12 | FR-12 |
| BO-1, BO-4 | BR-11 | US-13 | FR-13 |
| BO-1, BO-4 | BR-12 | US-14 | FR-14 |
| BO-1 | BR-13 | US-15 | FR-15 |
| BO-4 | BR-14 | US-16 | FR-16 |
| BO-1, BO-5 | BR-15 | US-17 | FR-17 |
| BO-1, BO-5 | BR-16 | US-18 | FR-18 |
| BO-5 | BR-17 | US-19 | FR-19 |
| BO-1, BO-5 | BR-18 | US-20 | FR-20 |
| BO-1 | BR-19 | US-21 | FR-21 |
| BO-1 | BR-20 | US-22 | FR-22 |
| BO-4 | BR-21 | US-23 | FR-23 |
| BO-3 | BR-22 | US-24 | FR-24 |

---

## 11. Appendix

### 11.1 Acronyms
AIS, TPP, SCA, RBAC, TLS, OAuth2, KPI, SLA, FR, NFR, SEC, RTO, BRD, SRS — bkz. Bölüm 1.3 Definitions.

### 11.2 Business Rules Referansları
Tüm fonksiyonel gereksinimler, BRD Bölüm 10'daki **BRULE-01—BRULE-08** iş kurallarıyla uyumludur.

### 11.3 Kullanılan Standartlar
IEEE 830-1998, OAuth2 (RFC 6749), TLS 1.2/1.3, OpenAPI/Swagger, ISO 27001, Open Banking / Berlin Group benzeri standartlar.

---

*Bu doküman, Aşama 7 (SRS) çalışmasının konsolide edilmiş halidir. Önceliklendirme için bkz. [`requirements-prioritization-matrix.md`](./requirements-prioritization-matrix.md).*
