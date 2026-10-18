# Open Banking Hesap Bilgisi Paylaşım Platformu
### İş Analizi Portföy Projesi | Bankacılık ve Dijital Çözümler

[![Domain](https://img.shields.io/badge/domain-Open%20Banking-blue)]()
[![Focus](https://img.shields.io/badge/focus-Business%20Analysis-orange)]()

---

## Proje Hakkında

Bu depo, kurgusal bir bankanın Açık Bankacılık kapsamında müşterilerin hesap bilgilerini yetkilendirilmiş üçüncü taraf sağlayıcılarla güvenli biçimde paylaşmasını konu alan bir iş analizi vaka çalışmasıdır.

Çalışma; iş probleminin tanımlanması, mevcut ve hedef süreçlerin analizi, paydaş ve risk değerlendirmesi, gereksinim dokümantasyonu, çözüm yaklaşımı, API spesifikasyonu ve test senaryosu tasarımını kapsar. Gerçek bir bankanın üretim sistemi değildir; senaryolar, endpoint'ler ve ölçümler proje varsayımlarıdır.

## İş Problemi

Hesap bilgisi paylaşımının manuel ve dosya bazlı süreçlerle yürütülmesi; operasyonel gecikme, izlenebilirlik ve uyum riskleri doğurabilir. Proje, bu süreci standart API'ler, dijital rıza yönetimi ve denetim kayıtlarıyla desteklenen bir hedef işleyişe dönüştürmeyi ele alır.

## İş Hedefleri

- Açık Bankacılık ihtiyaçlarını destekleyen standart bir API yaklaşımı tanımlamak.
- Müşteri rızasının oluşturulması, görüntülenmesi ve iptalini yönetmek.
- Üçüncü taraf sağlayıcıların (TPP) kayıt ve entegrasyon sürecini yapılandırmak.
- Erişim, güvenlik ve denetim gereksinimlerini dokümante etmek.
- Manuel süreçleri azaltmayı hedefleyen bir işleyiş tasarlamak.

## Kapsam

**Kapsam içi:** Account Information Service (AIS), Consent Management, TPP onboarding, erişim/yetkilendirme, audit ve uyum raporlaması.

**Kapsam dışı:** Ödeme başlatma hizmetleri, gerçek core banking geliştirmesi ve üretim ortamında canlı entegrasyon.

## Ana Çıktılar

| Alan | Çıktılar |
|---|---|
| Keşif ve iş gerekçesi | Business case, paydaş analizi, risk kaydı |
| Süreç analizi | As-Is, kök neden analizi, gap analizi, To-Be |
| Çözüm yaklaşımı | Yüksek seviyeli çözüm mimarisi |
| Gereksinimler | BRD, iş kuralları, epic/user story, kabul kriterleri, SRS |
| Önceliklendirme | Gereksinim önceliklendirme matrisi |
| Test tasarımı | Fonksiyonel, güvenlik, negatif, entegrasyon ve UAT senaryoları |
| API dokümantasyonu | OpenAPI 3.0.3 / Swagger YAML |
| Modelleme | UML ve süreç modelleme paketi |

## Klasör Yapısı

```text
01-discovery/          Business case, paydaş analizi ve riskler
02-process-analysis/   As-Is, kök neden, gap ve To-Be analizi
03-architecture/       Çözüm mimarisi özeti
04-requirements/       BRD, user story, kabul kriterleri, SRS ve önceliklendirme
05-testing/            Test case tasarımı
06-api-specification/  OpenAPI / Swagger dokümantasyonu
07-process-modeling/   UML ve süreç modelleme
```

## Nasıl İncelenmeli?

Projeyi sırasıyla incelemek için önerilen yol:

1. [`01-discovery/business-case.md`](01-discovery/business-case.md)
2. [`02-process-analysis/as-is-process-analysis.md`](02-process-analysis/as-is-process-analysis.md)
3. [`02-process-analysis/root-cause-analysis.md`](02-process-analysis/root-cause-analysis.md) ve [`gap-analysis.md`](02-process-analysis/gap-analysis.md)
4. [`02-process-analysis/to-be-process-design.md`](02-process-analysis/to-be-process-design.md)
5. [`04-requirements/brd-business-requirements-document.md`](04-requirements/brd-business-requirements-document.md)
6. [`04-requirements/epic-and-user-story-model.md`](04-requirements/epic-and-user-story-model.md) ve [`acceptance-criteria.md`](04-requirements/acceptance-criteria.md)
7. [`04-requirements/srs-software-requirements-specification.md`](04-requirements/srs-software-requirements-specification.md)
8. [`05-testing/test-case-design.md`](05-testing/test-case-design.md)
9. [`06-api-specification/swagger.yaml`](06-api-specification/swagger.yaml)

## Kullanılan Yaklaşımlar

- BABOK'tan esinlenen iş analizi yaklaşımı
- Paydaş analizi ve risk değerlendirmesi
- As-Is / To-Be ve gap analizi
- BRD, SRS, iş kuralları ve gereksinim izlenebilirliği
- Agile user story ve Given/When/Then kabul kriterleri
- MoSCoW önceliklendirme
- UML / süreç modelleme
- Risk bazlı manuel test senaryosu tasarımı
- OpenAPI / Swagger ile API sözleşmesi dokümantasyonu

## Proje Sınırları

Bu çalışma portföy amaçlı kurgusal bir vaka çalışmasıdır. Gerçek banka sistemlerine bağlanmaz; örnek API adresleri ve veriler temsili niteliktedir. Hedef KPI'lar ölçülmüş sonuçlar değil, değerlendirilmek üzere önerilen varsayımlardır. Test dokümanları test tasarımını ifade eder; canlı sistemde yürütülmüş test sonuçları olarak değerlendirilmemelidir.
