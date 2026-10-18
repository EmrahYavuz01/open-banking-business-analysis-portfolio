# To-Be Process Design
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

---

## 1. To-Be Process Vision

> Müşterinin tek bir dokunuşla, şeffaf ve güvenli şekilde rıza verebildiği; yetkilendirilmiş üçüncü taraf sağlayıcıların standart API'ler üzerinden saniyeler içinde hesap bilgisine erişebildiği; her işlemin uçtan uca denetlenebilir, güvenli ve regülasyona tam uyumlu olduğu, **API-first** bir Açık Bankacılık platformu.

Vizyonun üç temel ilkesi:
- **Otomasyon Önceliği:** İnsan müdahalesi yalnızca istisna durumlarında gereklidir.
- **Güvenlik ve Uyum by Design:** Güvenlik ve regülasyon uyumu, mimarinin temel taşıdır.
- **Müşteri Kontrolü:** Müşteri, verisinin nerede, kiminle, ne kadar süreyle paylaşıldığını her an görebilir ve kontrol edebilir.

---

## 2. Future State Process Overview

To-Be sürecinde, TPP'lerin hesap bilgisine erişimi üç ana faza ayrılır:

1. **TPP Onboarding Fazı (Bir Kerelik):** TPP, Developer Portal üzerinden kayıt olur, sertifikasyonunu sunar, Sandbox'ta test eder, Compliance/InfoSec otomatik + manuel onay sürecinden geçer.
2. **Müşteri Rıza Fazı (Her Kullanım Öncesi):** Müşteri, TPP üzerinden tetiklenen rıza talebini, bankanın mobil/internet bankacılığı üzerinden SCA ile onaylar.
3. **Veri Erişim Fazı (Operasyonel, Sürekli):** Onaylı rıza kapsamında, TPP, API Gateway üzerinden AIS API'sini çağırarak gerçek zamanlı hesap bilgisine erişir.

Bu üç faz, As-Is sürecindeki **10-15 iş günlük** süreyi; TPP onboarding için **birkaç iş günü**, müşteri rıza süreci için **birkaç dakika**, veri erişimi için ise **saniyeler** seviyesine indirir.

---

## 3. BPMN Seviyesinde To-Be Süreç Akışı (Üst Seviye)

```
[Başlangıç: TPP Onboarding Talebi]
        │
        ▼
[TPP Developer Portal'a Kayıt Olur]
        │
        ▼
[Sertifika/Lisans Otomatik Doğrulama] ──(Geçersiz)──► [Otomatik Red] ──► [Son]
        │ (Geçerli)
        ▼
[Compliance & InfoSec Otomatik Ön Kontrol] ──(Red)──► [Manuel İnceleme] ──┐
        │ (Onay)                                                          │
        ▼                                                                  │
[Sandbox Erişimi Verilir — TPP Test Eder]                                 │
        │                                                                  │
        ▼                                                                  │
[Compliance Onayı — Production Erişimi Aktive Edilir] ◄────────────────────┘
        │
        ▼
[TPP, OAuth2 Authorization Server'a Yönlendirir]
        │
        ▼
[Müşteri Mobile/Internet Banking Üzerinden SCA ile Kimlik Doğrular]
        │
        ▼
[Müşteri Consent Management System Üzerinden Rıza Kapsamını Onaylar]
        │
        ▼
[Consent Token Üretilir ve TPP'ye İletilir]
        │
        ▼
[TPP, API Gateway Üzerinden AIS API'sini Çağırır]
        │
        ▼
[API Gateway, Consent Token Doğrulaması Yapar]
        │
        ▼
[API Gateway, Core Banking'den Gerçek Zamanlı Veri Sorgular]
        │
        ▼
[Veri, Standart JSON Formatında TPP'ye Döndürülür]
        │
        ▼
[Audit & Logging Platform İşlemi Kayıt Altına Alır]
        │
        ▼
[Son: Veri Paylaşımı Tamamlandı]
```

---

## 4. Swimlane Bazlı To-Be Süreç Modeli

```
┌──────────────────┬───────────────────────────────────────────────────────────────────┐
│ CUSTOMER          │ [Rıza Talebi Bildirimi Alır] → [SCA ile Kimlik Doğrular] →        │
│                   │ [Rıza Kapsamını İnceler ve Onaylar/Reddeder] →                     │
│                   │ [Consent Dashboard'dan Aktif Rızaları Görüntüler/İptal Eder]       │
├──────────────────┼───────────────────────────────────────────────────────────────────┤
│ MOBILE/INTERNET   │ [Rıza Onay Ekranını Gösterir] → [SCA Doğrulamasını Tetikler] →    │
│ BANKING           │ [Onay Sonucunu Consent Mgmt'a İletir]                              │
├──────────────────┼───────────────────────────────────────────────────────────────────┤
│ CONSENT           │ [Rıza Talebini Oluşturur] → [Onay/Red Durumunu Kaydeder] →        │
│ MANAGEMENT SYSTEM │ [Consent Token Üretir] → [Rıza Yaşam Döngüsünü Yönetir]           │
├──────────────────┼───────────────────────────────────────────────────────────────────┤
│ API GATEWAY       │ [TPP API Çağrısını Alır] → [Consent Token Doğrular] →             │
│                   │ [Rate Limiting/Throttling Uygular] → [Core Banking'e Yönlendirir]  │
│                   │ → [Yanıtı Standart Formatta TPP'ye Döner]                          │
├──────────────────┼───────────────────────────────────────────────────────────────────┤
│ CORE BANKING      │ [API Gateway'den Gelen Sorguyu İşler] → [Hesap/Bakiye/İşlem        │
│                   │ Verisini Gerçek Zamanlı Döner]                                      │
├──────────────────┼───────────────────────────────────────────────────────────────────┤
│ COMPLIANCE        │ [TPP Sertifikasyonunu Otomatik Doğrulama Sonucunu Review Eder] →   │
│                   │ [Periyodik Uyum Raporlarını İnceler]                                │
├──────────────────┼───────────────────────────────────────────────────────────────────┤
│ INFOSEC           │ [Güvenlik Politikalarını API Gateway'e Tanımlar] →                 │
│                   │ [Anomali/Tehdit İzleme Alarmlarını Yönetir]                         │
├──────────────────┼───────────────────────────────────────────────────────────────────┤
│ TPP               │ [Developer Portal'a Kayıt Olur] → [Sandbox'ta Test Eder] →         │
│                   │ [OAuth2 Akışını Tetikler] → [API Gateway'i Çağırır] →              │
│                   │ [Veriyi Alır ve Kendi Servisinde Kullanır]                          │
└──────────────────┴───────────────────────────────────────────────────────────────────┘
```

---

## 5. Süreç Adımları (To-Be)

### Faz 1: TPP Onboarding (Bir Kerelik)
| # | Adım | Sorumlu/Sistem | Süre |
|---|------|------------------|------|
| 1 | TPP, Developer Portal'a kayıt olur, sertifikasını yükler | TPP / Developer Portal | Dakikalar |
| 2 | Sertifika/lisans otomatik doğrulanır | Compliance Engine | Saniyeler |
| 3 | Compliance & InfoSec ön kontrol checklist'i işletilir | Otomasyon + istisna bazlı manuel review | < 1 gün |
| 4 | TPP, Sandbox ortamında API'leri test eder | TPP / Sandbox | 1-3 gün |
| 5 | Production erişimi aktive edilir | Compliance (Onay) | < 1 gün |

### Faz 2: Müşteri Rıza Süreci (Her Kullanım Öncesi)
| # | Adım | Sorumlu/Sistem | Süre |
|---|------|------------------|------|
| 6 | TPP, OAuth2 Authorization Server'a yönlendirme yapar | TPP / OAuth2 Server | Anlık |
| 7 | Müşteri, SCA ile kimlik doğrular | Customer / Mobile-Internet Banking | < 1 dakika |
| 8 | Müşteri, rıza kapsamını görür ve onaylar | Customer / Consent Mgmt System | < 1 dakika |
| 9 | Consent Token üretilir, TPP'ye iletilir | Consent Mgmt System | Saniyeler |

### Faz 3: Veri Erişim Süreci (Operasyonel, Sürekli)
| # | Adım | Sorumlu/Sistem | Süre |
|---|------|------------------|------|
| 10 | TPP, API Gateway üzerinden AIS API'sini çağırır | TPP / API Gateway | Saniyeler |
| 11 | API Gateway, consent token ve yetkilendirmeyi doğrular | API Gateway | Milisaniyeler |
| 12 | Core Banking'den gerçek zamanlı veri sorgulanır | Core Banking | Milisaniyeler |
| 13 | Veri, standart JSON formatında TPP'ye döndürülür | API Gateway | Saniyeler |
| 14 | İşlem, Audit & Logging Platform'a kaydedilir | Audit Platform | Anlık |

---

## 6. Yeni Mimari Bileşenler

| Bileşen | Amaç |
|---|---|
| **Consent Management System** | Müşteri rızasının uçtan uca yaşam döngüsünü yönetir |
| **API Gateway** | TPP ile Core Banking arasındaki tüm API trafiğinin merkezi giriş noktası |
| **OAuth2 Authorization Server** | TPP ve müşteri kimlik doğrulama/yetkilendirme protokolünü yönetir |
| **Developer Portal** | TPP'lerin self-servis kayıt, dokümantasyon ve test erişim noktası |
| **Audit & Logging Platform** | Tüm sistem genelindeki işlemlerin değiştirilemez kaydı |
| **Sandbox Environment** | TPP'lerin production'a geçmeden önce test yapabileceği izole ortam |

---

## 7. Otomatikleşen Süreçler

| As-Is Manuel Süreç | To-Be Otomatik Süreç |
|---|---|
| TPP talep kaydı (Excel/e-posta) | Developer Portal üzerinden self-servis kayıt |
| Compliance/InfoSec manuel değerlendirme | Otomatik checklist engine + istisna bazlı manuel review |
| Müşteri rızasının telefon/şube ile teyidi | Dijital SCA tabanlı self-servis onay |
| IT'nin core banking'den manuel veri export'u | API Gateway üzerinden gerçek zamanlı otomatik veri çağrısı |
| Dosya bazlı (SFTP/CSV) veri transferi | API tabanlı, alan bazlı veri paylaşımı |
| Rıza iptal sürecinin manuel işlenmesi | Consent Management System üzerinden anlık otomatik iptal |
| Parçalı, manuel audit kaydı | Audit & Logging Platform ile merkezi otomatik kayıt |
| TPP sertifikasyon takibinin manuel kontrolü | Otomatik periyodik sertifikasyon doğrulama |

---

## 8. KPI Hedefleri (As-Is vs To-Be Karşılaştırması)

| KPI | As-Is | To-Be Hedefi |
|---|---|---|
| Uçtan Uca Veri Erişim Süresi | 10-15 iş günü | < 5 saniye (rıza sonrası) |
| Müşteri Rıza Onay Süresi | 1-2 iş günü | < 2 dakika |
| TPP Onboarding Süresi | 1-3 hafta | < 5 iş günü |
| Rıza İptal İşlem Süresi | 3-5 iş günü | < 24 saat |
| API Uptime | Ölçülmüyor | %99.9 |
| API Yanıt Süresi (p95) | N/A | < 500ms |
| Audit Kaydı Tamlığı | Parçalı (%~60 tahmini) | %100 |
| Yetkisiz Erişim Vakası | İzlenemiyor | 0 |
| TPP Memnuniyet Skoru | Ölçülmüyor | > 8/10 |
| Veri Paylaşım Hata Oranı | Sistematik ölçülmüyor | < %1 |

---

## 9. Risk Azaltım Haritası (Gap → Çözüm Eşleştirmesi)

| Gap ID | Gap Özeti | To-Be Çözümü | Kapatan Mimari Bileşen |
|---|---|---|---|
| GAP-01 | Manuel talep takibi | Self-servis dijital kayıt | Developer Portal |
| GAP-02 | Standart değerlendirme kriteri yok | Otomatik checklist engine | Compliance Engine |
| GAP-03 | Manuel sözleşme süreci | Standart şablon + otomatik onay akışı | Developer Portal + Workflow Engine |
| GAP-04 | Parçalı süreç izleme | Merkezi workflow takibi | Developer Portal / Audit Platform |
| GAP-05 | API katmanı yok | Gerçek zamanlı AIS API | API Gateway |
| GAP-06 | Toplu dosya transferi | Alan bazlı API çağrısı | API Gateway |
| GAP-07 | Standart yetkilendirme yok | OAuth2 + SCA | OAuth2 Authorization Server |
| GAP-08 | Dokümantasyon/sandbox yok | Swagger dokümantasyon + test ortamı | Developer Portal + Sandbox |
| GAP-09 | Point-to-point entegrasyon | Merkezi API Gateway | API Gateway |
| GAP-10 | Standart veri güvenliği yok | TLS + RBAC | API Gateway + InfoSec Politikaları |
| GAP-11 | Anomali tespiti yok | Gerçek zamanlı izleme/alarm | Audit & Logging Platform |
| GAP-12 | Parçalı audit trail | Merkezi immutable log | Audit & Logging Platform |
| GAP-13 | Zayıf rıza ispatı | Dijital, zaman damgalı rıza kaydı | Consent Management System |
| GAP-14 | Yavaş rıza iptali | Anlık iptal mekanizması | Consent Management System |
| GAP-15 | Manuel sertifikasyon takibi | Otomatik doğrulama | Compliance Engine |
| GAP-16 | Manuel regülasyon raporlama | Otomatik rapor üretimi | Audit & Logging Platform |
| GAP-17 | Şeffaflık eksikliği | Consent Dashboard | Consent Management System + Mobile/Internet Banking |
| GAP-18 | Müşteri bilgilendirme yok | Otomatik bildirimler | Consent Management System |
| GAP-19 | Şube bağımlı rıza süreci | Dijital self-servis SCA akışı | Mobile/Internet Banking + OAuth2 Server |
| GAP-20 | KPI ölçülmüyor | Otomatik KPI/dashboard raporlama | Audit & Logging Platform |
| GAP-21 | IT manuel veri export yükü | Otomatik API çağrısı | API Gateway + Core Banking Entegrasyonu |

---

## 10. To-Be Süreç Faydaları

**Operasyonel:** Süreç süresinde radikal iyileşme, manuel iş yükünden kurtulma, sıfır operasyonel müdahale ile veri paylaşımı.

**Güvenlik ve Uyum:** Şeffaflık, rıza ispatı ve anlık iptal ilkelerinin tam karşılanması; dakikalar içinde denetim kanıtı sunabilme.

**Müşteri Deneyimi:** Tek ekrandan rıza yönetimi, sürtünmesiz dijital rıza süreci, gerçek zamanlı şeffaflık.

**Stratejik/Ticari:** TPP ekosisteminin büyümesi, gelecekteki PIS gibi servislerin kolayca eklenebilmesi, açık bankacılık pazarında öncü konumlanma.

---

*Bu doküman, Aşama 4 (To-Be Process Design) çalışmasının konsolide edilmiş halidir. Çözüm mimarisi detayları için bkz. [`../03-architecture/solution-architecture-overview.md`](../03-architecture/solution-architecture-overview.md).*
