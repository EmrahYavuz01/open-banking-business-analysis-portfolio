# Epic and User Story Model
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

> Acceptance Criteria için bkz. [`acceptance-criteria.md`](./acceptance-criteria.md)

---

## Bölüm A: Epic'ler

| Epic ID | Epic Name | Business Goal Mapping | Description |
|---|---|---|---|
| EPIC-01 | Consent Management (Rıza Yönetimi) | BO-2 | Müşterinin veri paylaşım rızasını verme, görüntüleme, iptal etme ve rıza yaşam döngüsünü uçtan uca yönetebileceği yetenekleri kapsar |
| EPIC-02 | TPP Onboarding & Yetkilendirme | BO-3 | Üçüncü taraf sağlayıcıların sisteme kayıt olması, sertifikasyon doğrulaması, sandbox testi ve production aktivasyonu süreçlerini kapsar |
| EPIC-03 | Hesap Bilgisi Erişimi (AIS) | BO-1, BO-4 | TPP'lerin müşteri rızası kapsamında gerçek zamanlı hesap/bakiye/işlem verisine erişimini sağlayan yetenekleri kapsar |
| EPIC-04 | Güvenlik ve Erişim Kontrolü | BO-1, BO-5 | Kimlik doğrulama, yetkilendirme, şifreleme ve anomali tespiti gibi güvenlik yeteneklerini kapsar |
| EPIC-05 | Denetim, Raporlama ve KPI Yönetimi | BO-1, BO-4 | Tüm işlemlerin denetlenebilir şekilde kayıt altına alınması, regülasyon raporlaması ve performans (KPI) izleme yeteneklerini kapsar |

---

## Bölüm B: User Story'ler

### EPIC-01: Consent Management

| Story ID | User Story | Business Value | Priority | Related BR |
|---|---|---|---|---|
| US-01 | Bir müşteri olarak, bir TPP'nin hesap bilgime erişim talebini onaylamak veya reddetmek istiyorum, böylece verimin paylaşımını kontrol edebilirim | Müşteri kontrolü ve regülasyon uyumu sağlar | Must | BR-01 |
| US-02 | Bir müşteri olarak, hangi TPP'ye, hangi veriyi, ne kadar süreyle paylaştığımı görmek istiyorum, böylece şeffaf bir şekilde bilgilendirilmiş olurum | Şeffaflık ve güven artırır | Must | BR-02 |
| US-03 | Bir müşteri olarak, istediğim zaman herhangi bir rızamı iptal etmek istiyorum, böylece veri paylaşımını anında durdurabilirim | Regülasyonun öngördüğü anlık iptal hakkını karşılar | Must | BR-03 |
| US-04 | Bir müşteri olarak, rıza süresi dolduğunda otomatik olarak erişimin kesilmesini istiyorum, böylece süresiz veri paylaşımı riskiyle karşılaşmam | Kontrolsüz veri paylaşımını önler | Must | BR-04 |
| US-05 | Bir müşteri olarak, rıza talebi aldığımda, onayladığımda veya iptal ettiğimde bildirim almak istiyorum, böylece sürecin her aşamasından haberdar olurum | Müşteri deneyimini iyileştirir | Should | BR-05 |
| US-06 | Bir Compliance uzmanı olarak, müşterilerin rıza geçmişini görüntülemek istiyorum, böylece denetim sırasında rıza ispatı sunabilirim | Denetim hazırlığını kolaylaştırır | Should | BR-01, BR-19 |

### EPIC-02: TPP Onboarding & Yetkilendirme

| Story ID | User Story | Business Value | Priority | Related BR |
|---|---|---|---|---|
| US-07 | Bir TPP geliştiricisi olarak, Developer Portal üzerinden self-servis kayıt olmak istiyorum, böylece manuel onboarding sürecini beklemeden başvuru yapabilirim | Onboarding süresini kısaltır, ölçeklenebilirlik sağlar | Must | BR-06 |
| US-08 | Bir TPP geliştiricisi olarak, kayıt sırasında sertifikamı/lisansımı sisteme yüklemek istiyorum, böylece yetkilendirme sürecim başlatılabilir | Yetkisiz erişim riskini azaltır | Must | BR-07 |
| US-09 | Bir Compliance uzmanı olarak, TPP sertifikasyonunun sistem tarafından otomatik doğrulanmasını istiyorum, böylece manuel kontrol yükünden kurtulurum | Operasyonel verimlilik sağlar | Must | BR-07 |
| US-10 | Bir TPP geliştiricisi olarak, production'a geçmeden önce Sandbox ortamında API'leri test etmek istiyorum, böylece entegrasyon hatalarını erken tespit edebilirim | Entegrasyon kalitesini artırır | Must | BR-08 |
| US-11 | Bir Compliance uzmanı olarak, otomatik onboarding sürecinde şüpheli/red durumlarını manuel olarak inceleyebilmek istiyorum, böylece riskli TPP'leri durdurabilirim | Güvenlik ve uyum riskini azaltır | Should | BR-09 |
| US-12 | Bir Compliance uzmanı olarak, TPP sertifikasyon durumunun periyodik olarak yeniden doğrulanmasını istiyorum, böylece süresi dolmuş sertifikalarla erişim engellenir | Sürekli uyum sağlar | Must | BR-10 |

### EPIC-03: Hesap Bilgisi Erişimi (AIS)

| Story ID | User Story | Business Value | Priority | Related BR |
|---|---|---|---|---|
| US-13 | Bir TPP olarak, müşterinin onayladığı kapsamda hesap bilgisine API üzerinden erişmek istiyorum, böylece kendi uygulamamda müşteriye değer sunabilirim | Açık bankacılık ekosistemine katılımı sağlar | Must | BR-11 |
| US-14 | Bir TPP olarak, hesap bilgisi taleplerime gerçek zamanlı yanıt almak istiyorum, böylece kullanıcılarıma anlık deneyim sunabilirim | Rekabetçilik ve TPP memnuniyetini artırır | Must | BR-12 |
| US-15 | Bir sistem (API Gateway) olarak, geçerli rıza/yetkilendirme olmayan istekleri otomatik reddetmek istiyorum, böylece yetkisiz veri paylaşımı engellenir | Temel güvenlik ve regülasyon uyumu sağlar | Must | BR-13 |
| US-16 | Bir TPP olarak, platformun yüksek erişilebilirlikte (uptime) çalışmasını istiyorum, böylece hizmetlerimi kesintisiz sunabilirim | İş sürekliliği ve güven sağlar | Should | BR-14 |

### EPIC-04: Güvenlik ve Erişim Kontrolü

| Story ID | User Story | Business Value | Priority | Related BR |
|---|---|---|---|---|
| US-17 | Bir sistem (API Gateway) olarak, her TPP isteğini güçlü kimlik doğrulama ile doğrulamak istiyorum, böylece yalnızca yetkili TPP'ler erişebilir | Güvenlik açığını minimize eder | Must | BR-15 |
| US-18 | Bir müşteri olarak, rıza onayımı güçlü kimlik doğrulama (SCA) ile vermek istiyorum, böylece kimliğimin güvenliği korunur | Dolandırıcılık riskini azaltır | Must | BR-16 |
| US-19 | Bir InfoSec uzmanı olarak, anormal erişim paternlerinin otomatik tespit edilip bildirilmesini istiyorum, böylece olası tehditlere hızlı müdahale edebilirim | Proaktif güvenlik yönetimi sağlar | Should | BR-17 |
| US-20 | Bir sistem (API Gateway) olarak, tüm veri transferlerini şifreli kanal üzerinden gerçekleştirmek istiyorum, böylece veri gizliliği korunur | Veri güvenliği ve uyum sağlar | Must | BR-18 |

### EPIC-05: Denetim, Raporlama ve KPI Yönetimi

| Story ID | User Story | Business Value | Priority | Related BR |
|---|---|---|---|---|
| US-21 | Bir sistem (Audit Platform) olarak, tüm rıza ve veri paylaşım işlemlerini değiştirilemez şekilde kayıt altına almak istiyorum, böylece denetim sürecinde güvenilir kanıt sunulabilir | Regülasyon uyumu ve denetim hazırlığı sağlar | Must | BR-19 |
| US-22 | Bir Compliance uzmanı olarak, düzenleyici denetim için gerekli raporları sistemden otomatik üretmek istiyorum, böylece manuel rapor hazırlama yükünden kurtulurum | Operasyonel verimlilik ve denetim hızını artırır | Must | BR-20 |
| US-23 | Bir Product Owner olarak, operasyonel KPI'ları dashboard üzerinden izlemek istiyorum, böylece performansı sürekli takip edebilirim | Veri odaklı karar almayı sağlar | Should | BR-21 |
| US-24 | Bir TPP geliştiricisi olarak, kendi API kullanım istatistiklerimi Developer Portal üzerinden görmek istiyorum, böylece entegrasyonumu optimize edebilirim | TPP deneyimini iyileştirir | Could | BR-22 |

---

## Epic – Story Özet Tablosu

| Epic ID | Epic Name | Story Sayısı | Must | Should | Could |
|---|---|:---:|:---:|:---:|:---:|
| EPIC-01 | Consent Management | 6 | 4 | 2 | 0 |
| EPIC-02 | TPP Onboarding & Yetkilendirme | 6 | 4 | 2 | 0 |
| EPIC-03 | Hesap Bilgisi Erişimi (AIS) | 4 | 3 | 1 | 0 |
| EPIC-04 | Güvenlik ve Erişim Kontrolü | 4 | 3 | 1 | 0 |
| EPIC-05 | Denetim, Raporlama ve KPI Yönetimi | 4 | 2 | 1 | 1 |
| **TOPLAM** | | **24** | **16** | **7** | **1** |

> **BA/PO Değerlendirmesi:** "Must" önceliğindeki story'lerin (16 adet) çoğunluğu Consent Management ve TPP Onboarding epic'lerinde yoğunlaşmaktadır — bu da bu iki alanın MVP kapsamında önceliklendirilmesi gerektiğini gösterir.

---

*Bu doküman, Aşama 6 (Epic ve User Story Modeli) çalışmasının konsolide edilmiş halidir.*
