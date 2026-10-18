# Business Requirements Document (BRD)
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

**Doküman Versiyonu:** 1.0

---

## 1. Executive Summary

Bu doküman, bankanın düzenleyici otoritenin Açık Bankacılık (Open Banking) yönetmeliğine uyum sağlaması ve üçüncü taraf sağlayıcılara (TPP) güvenli, standardize ve gerçek zamanlı hesap bilgisi erişimi sunması amacıyla geliştirilecek **Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu**'nun iş gereksinimlerini tanımlamaktadır.

Mevcut süreç analizi (As-Is), platformun manuel, dosya bazlı ve ölçeklenemeyen bir yapıda olduğunu; bu durumun hem regülasyon uyum riski hem de operasyonel verimsizlik yarattığını ortaya koymuştur. Gap Analysis ve To-Be süreç tasarımı doğrultusunda; **Consent Management System, API Gateway, OAuth2 Authorization Server, Developer Portal, Audit & Logging Platform ve Sandbox Environment** bileşenlerinden oluşan bir mimari önerilmektedir.

---

## 2. Business Background

Banka, sektördeki dijital dönüşüm ve düzenleyici gereksinimler doğrultusunda, müşteri hesap bilgilerinin yetkilendirilmiş üçüncü taraf sağlayıcılarla güvenli şekilde paylaşılmasını sağlayacak bir Açık Bankacılık altyapısı kurma ihtiyacı içerisindedir.

Mevcut durumda bu paylaşım, standart bir API olmadan, manuel ve ad-hoc yöntemlerle yürütülmektedir (bkz. [`../02-process-analysis/as-is-process-analysis.md`](../02-process-analysis/as-is-process-analysis.md)). Bu yapı; düzenleyici uyumsuzluk riski, operasyonel maliyet, güvenlik açığı ve düşük TPP/müşteri memnuniyeti gibi önemli sorunlara yol açmaktadır (bkz. [`../02-process-analysis/gap-analysis.md`](../02-process-analysis/gap-analysis.md)).

---

## 3. Business Objectives

| # | Hedef |
|---|-------|
| BO-1 | Açık Bankacılık / ilgili düzenleyici yönetmeliklere tam uyumlu bir hesap bilgisi paylaşım platformu hayata geçirmek |
| BO-2 | Müşterinin rızasını uçtan uca, şeffaf ve denetlenebilir şekilde yönetebileceği bir Consent Management mekanizması kurmak |
| BO-3 | TPP'lerin standart, dokümante API'ler üzerinden hızlı ve güvenli şekilde entegre olabilmesini sağlamak |
| BO-4 | Manuel operasyonel süreçleri otomatikleştirerek işlem süresini ve maliyetini azaltmak |
| BO-5 | Açık bankacılık ekosisteminde bankanın rekabetçi/öncü konumunu güçlendirmek |

---

## 4. Scope

- Account Information Service (AIS) kapsamında hesap bilgisi, bakiye ve işlem geçmişi paylaşımı
- Müşteri rıza yönetimi (verme, görüntüleme, iptal)
- TPP onboarding (kayıt, sertifikasyon doğrulama, sandbox erişimi, production aktivasyonu)
- Güvenlik ve yetkilendirme (yüksek seviye iş kuralları)
- Denetim ve izlenebilirlik (Audit)
- Regülasyon raporlama

## 5. Out of Scope

- Payment Initiation Service (PIS)
- Confirmation of Funds (CAF/PIIS) servisleri
- Core Banking sisteminin yeniden tasarımı
- Üçüncü taraf sağlayıcıların kendi iç sistem tasarımları
- Şube/fiziksel kanal süreç değişiklikleri
- Mobil/internet bankacılığı genel UI/UX yeniden tasarımı
- Teknik mimari detayları, API spesifikasyonları (SRS'de ele alınır)
- Kullanıcı hikayeleri ve kabul kriterleri (ayrı dokümanda)

---

## 6. Stakeholders

> Detaylı analiz için bkz. [`../01-discovery/stakeholder-analysis.md`](../01-discovery/stakeholder-analysis.md)

| Paydaş | Rol |
|---|---|
| GM Yardımcısı (Dijital Bankacılık) | Proje Sponsoru |
| Düzenleyici Otorite | Dış Denetleyici / Uyum Otoritesi |
| Compliance Departmanı | Regülasyon Uyum Onaylayıcısı |
| Bilgi Güvenliği (InfoSec) | Güvenlik Standartları Onaylayıcısı |
| IT Mimarisi | Teknik Mimari Onaylayıcısı |
| API Geliştirme Ekibi | Çözüm Geliştirici |
| Hukuk Departmanı | Sözleşme ve Veri Sorumluluğu |
| Müşteri Deneyimi (CX) | Rıza Akışı ve Müşteri İletişimi |
| Operasyon Departmanı | TPP Onboarding Operasyonel Yürütücüsü |
| TPP'ler | Dış Paydaş / API Tüketicisi |
| Müşteriler | Nihai Veri Sahibi / Kullanıcı |
| Business Analyst / Product Owner | Gereksinim Yönetimi ve Süreç Sahipliği |

---

## 7. Current State Summary

Mevcut süreç (As-Is), TPP taleplerinin e-posta/form ile alınması, Compliance ve InfoSec tarafından manuel değerlendirilmesi, müşteri rızasının telefon/şube üzerinden sözlü teyit edilmesi ve verinin dosya bazlı paylaşılmasından oluşmaktadır. Süreç ortalama **10-15 iş günü** sürmekte, gerçek zamanlı veri paylaşımı mümkün olmamaktadır. Detaylar için bkz. [`../02-process-analysis/as-is-process-analysis.md`](../02-process-analysis/as-is-process-analysis.md).

---

## 8. Future State Summary

To-Be süreci; TPP onboarding'in Developer Portal üzerinden self-servis olarak yapıldığı, müşteri rızasının SCA ile dijital olarak alındığı, veri paylaşımının API Gateway üzerinden gerçek zamanlı gerçekleştirildiği ve tüm işlemlerin merkezi Audit & Logging Platform'da kayıt altına alındığı bir mimariyi öngörmektedir. Detaylar için bkz. [`../02-process-analysis/to-be-process-design.md`](../02-process-analysis/to-be-process-design.md).

---

## 9. Business Requirements

### 9.1 Consent Management (Rıza Yönetimi)

| Requirement ID | Requirement Description | Business Rationale | Priority |
|---|---|---|---|
| BR-01 | Sistem, müşterinin hangi TPP'ye, hangi veri kapsamında, ne kadar süreyle rıza verdiğini dijital olarak kayıt altına almalıdır | Hukuki ispat edilebilirlik ve regülasyon uyumu için zorunludur | Must |
| BR-02 | Müşteri, mobil/internet bankacılığı üzerinden tüm aktif rızalarını görüntüleyebilmelidir | Şeffaflık ilkesi ve müşteri güveni için gereklidir | Must |
| BR-03 | Müşteri, dilediği zaman herhangi bir rızasını anlık olarak iptal edebilmelidir | Regülasyonun öngördüğü "anlık iptal hakkı" ilkesinin karşılanması gerekir | Must |
| BR-04 | Rıza süresi dolduğunda erişim otomatik olarak sona ermelidir | Veri paylaşımının süresiz/kontrolsüz devam etmesini önlemek için gereklidir | Must |
| BR-05 | Müşteri, rıza talebi, onayı ve iptali ile ilgili bildirim almalıdır | Müşteri deneyimi ve şeffaflığın artırılması için önemlidir | Should |

### 9.2 TPP Onboarding & Yetkilendirme

| Requirement ID | Requirement Description | Business Rationale | Priority |
|---|---|---|---|
| BR-06 | TPP'ler, Developer Portal üzerinden self-servis kayıt olabilmelidir | Manuel onboarding sürecinin azaltılması ve ölçeklenebilirlik için gereklidir | Must |
| BR-07 | TPP sertifikasyon/lisans bilgisi sisteme kayıt sırasında doğrulanmalıdır | Yetkisiz/sertifikasız TPP'lerin sisteme erişimini önlemek için zorunludur | Must |
| BR-08 | TPP'ler, production erişimi öncesinde Sandbox ortamında test yapabilmelidir | Entegrasyon hatalarının production öncesinde tespit edilmesi için gereklidir | Must |
| BR-09 | Compliance ekibi, otomatik onboarding sürecinde istisna durumları manuel olarak inceleyebilmelidir | Tam otomasyonun riskli durumlarda insan kontrolüne ihtiyaç duyması nedeniyle gereklidir | Should |
| BR-10 | TPP'lerin sertifikasyon durumu periyodik olarak otomatik yeniden doğrulanmalıdır | Süresi dolmuş/iptal edilmiş sertifikalarla erişimi önlemek için gereklidir | Must |

### 9.3 Veri Erişimi (AIS)

| Requirement ID | Requirement Description | Business Rationale | Priority |
|---|---|---|---|
| BR-11 | TPP'ler, yalnızca müşterinin onayladığı veri kapsamı içinde hesap bilgisine erişebilmelidir | Veri minimizasyonu ve regülasyon uyumu için zorunludur | Must |
| BR-12 | Sistem, hesap bilgisi taleplerine gerçek zamanlı yanıt verebilmelidir | TPP deneyimi ve rekabetçilik için gereklidir | Must |
| BR-13 | Sistem, geçerli rıza/yetkilendirme olmadan herhangi bir veri paylaşımına izin vermemelidir | Temel güvenlik ve uyum ilkesidir | Must |
| BR-14 | Veri paylaşım hizmeti, tanımlanan kullanılabilirlik (uptime) hedefini karşılamalıdır | İş sürekliliği ve TPP güveni için önemlidir | Should |

### 9.4 Güvenlik ve Erişim Kontrolü

| Requirement ID | Requirement Description | Business Rationale | Priority |
|---|---|---|---|
| BR-15 | Tüm TPP istekleri, güçlü kimlik doğrulama mekanizması ile doğrulanmalıdır | Yetkisiz erişimi önlemek için zorunludur | Must |
| BR-16 | Müşteri rıza onayı, güçlü müşteri kimlik doğrulaması (SCA) ile gerçekleştirilmelidir | Regülasyon gereği ve dolandırıcılık riskinin azaltılması için zorunludur | Must |
| BR-17 | Sistem, anormal veya şüpheli erişim paternlerini tespit edip ilgili ekiplere bildirmelidir | Proaktif güvenlik yönetimi için gereklidir | Should |
| BR-18 | Veri transferi, uçtan uca şifrelenmiş kanallar üzerinden gerçekleştirilmelidir | Veri gizliliği ve güvenliği için zorunludur | Must |

### 9.5 Denetim ve Raporlama (Audit & Compliance)

| Requirement ID | Requirement Description | Business Rationale | Priority |
|---|---|---|---|
| BR-19 | Sistem, tüm rıza, erişim ve veri paylaşım işlemlerini değiştirilemez şekilde kayıt altına almalıdır | Denetim ve hukuki ispat edilebilirlik için zorunludur | Must |
| BR-20 | Compliance departmanı, düzenleyici denetimler için gerekli raporları sistemden üretebilmelidir | Denetim sürecinin hızlandırılması için gereklidir | Must |
| BR-21 | Sistem, önceden tanımlanmış operasyonel KPI'ları ölçüp raporlayabilmelidir | Performans yönetimi ve sürekli iyileştirme için gereklidir | Should |
| BR-22 | TPP'ler, kendi API kullanım istatistiklerini Developer Portal üzerinden görüntüleyebilmelidir | TPP deneyiminin iyileştirilmesi için faydalıdır | Could |

---

## 10. Business Rules

| Kural ID | Kural Tanımı |
|---|---|
| BRULE-01 | Hiçbir veri paylaşımı, geçerli ve aktif bir müşteri rızası olmadan gerçekleştirilemez |
| BRULE-02 | Bir rıza, yalnızca müşterinin açıkça onayladığı veri alanlarını kapsayabilir |
| BRULE-03 | Rıza süresi, düzenleyici otoritenin belirlediği azami süreyi aşamaz |
| BRULE-04 | Müşteri, rıza iptalini herhangi bir gerekçe sunma zorunluluğu olmadan gerçekleştirebilir |
| BRULE-05 | Sertifikası geçersiz, askıya alınmış veya iptal edilmiş bir TPP'nin API erişimi otomatik olarak engellenir |
| BRULE-06 | TPP onboarding sürecinde Compliance onayı olmadan hiçbir TPP production ortamına geçemez |
| BRULE-07 | Audit kayıtları, oluşturulduktan sonra hiçbir kullanıcı veya sistem tarafından değiştirilemez veya silinemez |
| BRULE-08 | Müşteri SCA doğrulamasını tamamlamadan rıza onay işlemi geçerli sayılmaz |

---

## 11. Success Criteria

- Platform, düzenleyici otoritenin belirlediği uyum tarihine kadar production ortamında çalışır durumda olmalıdır.
- Müşteri, rıza verme/iptal sürecini herhangi bir şube/çağrı merkezi desteği olmadan, dijital kanal üzerinden tamamlayabilmelidir.
- TPP onboarding süreci, sertifikasyon doğrulamasından production aktivasyonuna kadar 5 iş gününü aşmamalıdır.
- Sistem, tanımlanan tüm Business Requirements (BR-01—BR-22) için kabul testlerini (UAT) başarıyla geçmelidir.
- Audit & Logging Platform, düzenleyici bir denetim talebi durumunda gerekli raporu makul bir sürede üretebilmelidir.
- Proje, tanımlanan Business Case'deki maliyet ve fayda varsayımlarıyla uyumlu şekilde tamamlanmalıdır.

---

## 12. Risks & Dependencies

> Detaylı risk kayıtları için bkz. [`../01-discovery/project-risks.md`](../01-discovery/project-risks.md)

### Riskler
- Regülasyon gereksinimlerinin geliştirme süreci sırasında değişmesi
- Core Banking sisteminin API katmanına yeterli performansta veri sağlayamaması
- TPP'lerin yeni API standardına adaptasyonunun beklenenden uzun sürmesi
- Müşterilerin dijital rıza sürecini benimsememesi (düşük adopsiyon)

### Bağımlılıklar
- Core Banking sisteminin API Gateway'e gerekli veri erişimini sağlaması
- Düzenleyici otoritenin API standartlarını ve onboarding kriterlerini onaylaması
- Bilgi Güvenliği departmanının güvenlik mimarisini onaylaması
- Hukuk departmanının TPP sözleşme şablonlarını hazırlaması/onaylaması

---

*Bu doküman, Aşama 5 (BRD) çalışmasının konsolide edilmiş halidir. Epic ve User Story modeli için bkz. [`epic-and-user-story-model.md`](./epic-and-user-story-model.md).*
