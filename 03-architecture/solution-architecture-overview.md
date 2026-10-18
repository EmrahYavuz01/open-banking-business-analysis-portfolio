# Solution Architecture Overview
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

> Bu doküman, Business Analyst / Product Owner perspektifinden, çözümün iş gereksinimlerini teknik mimariyle ilişkilendirmek amacıyla hazırlanmıştır. Solution Architect seviyesinde detaylı teknik tasarım bu dokümanın kapsamı dışındadır.

---

## 1. High Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     DIŞ KATMAN (External Layer)                  │
│              TPP Uygulamaları  │  Müşteri Mobil/Web Uygulaması    │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                  ERİŞİM KATMANI (Access Layer)                   │
│         API Gateway   │   OAuth2 Authorization Server             │
│         Developer Portal   │   Sandbox Environment                │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│              İŞ MANTIĞI KATMANI (Business Logic Layer)           │
│        Consent Management System   │   Compliance Engine          │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                  ÇEKİRDEK KATMAN (Core Layer)                    │
│                    Core Banking Sistemi                          │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│            İZLENEBİLİRLİK KATMANI (Observability Layer)          │
│                  Audit & Logging Platform                        │
└─────────────────────────────────────────────────────────────────┘
```

**Mimari Yaklaşım:** "API-first" ve "Security by Design" prensiplerine dayanır. Hiçbir dış sistem (TPP), Core Banking sistemine doğrudan erişemez — tüm trafik API Gateway üzerinden geçer.

---

## 2. System Components

| Bileşen | İş Fonksiyonu (BA Perspektifi) |
|---|---|
| **Developer Portal** | TPP'lerin sisteme giriş kapısı; kayıt, dokümantasyon, sertifika yükleme ve kullanım takibi |
| **Sandbox Environment** | TPP'lerin production'a geçmeden risk almadan test yapabileceği izole alan |
| **OAuth2 Authorization Server** | "Kim, neye, ne kadar süreyle erişebilir" sorusunun cevabını yöneten kimlik/yetki motoru |
| **API Gateway** | Tüm dış-iç trafiğin geçtiği, kuralları uygulayan tek giriş/çıkış noktası |
| **Consent Management System** | Müşteri rızasının "iş kuralı" merkezi |
| **Compliance Engine** | TPP onboarding ve düzenli kontrol süreçlerinde regülasyon kurallarının otomatik uygulanması |
| **Core Banking Sistemi** | Gerçek hesap/bakiye/işlem verisinin kaynağı (mevcut sistem, değiştirilmiyor) |
| **Audit & Logging Platform** | "Kim, ne zaman, neyi, neden yaptı" sorusunu her zaman cevaplanabilir kılan merkezi kayıt sistemi |

---

## 3. Component Interaction Diagram

```
   ┌──────────┐         ┌──────────────────┐
   │  TPP     │────1───►│ Developer Portal  │
   │ Uygulama │         └──────────────────┘
   └────┬─────┘                  │
        │                        │ (Onboarding Onayı)
        │ 2. Kimlik Doğrulama    ▼
        │    Talebi      ┌──────────────────┐
        ├───────────────►│ OAuth2 Auth Server│
        │                └─────────┬────────┘
        │                          │ 3. Yönlendirme
        │                          ▼
        │                ┌──────────────────┐
        │                │ Mobile/Internet   │◄──── Müşteri SCA ile
        │                │ Banking (Müşteri) │      Kimlik Doğrular
        │                └─────────┬────────┘
        │                          │ 4. Rıza Onayı
        │                          ▼
        │                ┌──────────────────┐
        │                │ Consent Mgmt Sys  │──── 5. Consent Token
        │                └─────────┬────────┘      Üretir
        │                          │
        │  6. Token ile API Çağrısı│
        ▼                          ▼
   ┌─────────────────────────────────────┐
   │           API Gateway               │
   │  (Token Doğrulama + Rate Limiting)  │
   └──────────────────┬──────────────────┘
                       │ 7. Veri Sorgusu
                       ▼
              ┌──────────────────┐
              │  Core Banking     │
              └─────────┬────────┘
                       │ 8. Veri Yanıtı
                       ▼
              ┌──────────────────┐
              │  TPP'ye Döner     │
              └──────────────────┘

   [Her adım, paralel olarak Audit & Logging Platform'a kaydedilir]
```

---

## 4. Data Flow Overview

| Veri Akışı | Kaynak | Hedef | İçerik | Tetikleyici |
|---|---|---|---|---|
| Onboarding Verisi | TPP | Developer Portal → Compliance Engine | Sertifika, lisans, şirket bilgisi | TPP kaydı |
| Kimlik Doğrulama Verisi | Müşteri | Mobile/Internet Banking → OAuth2 Server | SCA bilgisi | Rıza talebi |
| Rıza Verisi | Müşteri | Consent Management System | Onaylanan veri kapsamı, süre, TPP bilgisi | Müşteri onayı |
| Hesap Verisi | Core Banking | API Gateway → TPP | Bakiye, işlem geçmişi (rıza kapsamına göre filtrelenmiş) | TPP API çağrısı |
| Audit Verisi | Tüm Bileşenler | Audit & Logging Platform | İşlem zaman damgası, aktör, sonuç | Her sistem olayı |

**Önemli İlke:** Core Banking'den TPP'ye giden veri, **yalnızca müşterinin onayladığı kapsamla sınırlı** olacak şekilde API Gateway tarafından filtrelenir (data minimization).

---

## 5. Security Architecture

Güvenlik mimarisi, **"Zero Trust"** yaklaşımı temelinde tasarlanmıştır.

| Katman | Sağladığı Koruma |
|---|---|
| **TPP Kimlik Doğrulama** | Sertifika tabanlı (eIDAS benzeri QWAC/QSEAL) güçlü kimlik doğrulama |
| **Müşteri Kimlik Doğrulama (SCA)** | İki faktörlü/güçlü müşteri kimlik doğrulama |
| **Yetkilendirme (Authorization)** | OAuth2 tabanlı, kapsam (scope) sınırlı erişim token'ları |
| **Şifreleme** | Veri aktarımında uçtan uca TLS şifrelemesi |
| **Erişim Kontrolü** | RBAC ile yalnızca yetkili sistemlerin Core Banking'e erişimi |
| **Anomali Tespiti** | Rate limiting, anormal işlem paterni tespiti, otomatik alarm |
| **Veri Minimizasyonu** | API yanıtlarının yalnızca rıza kapsamındaki alanlarla sınırlandırılması |

---

## 6. Authentication & Authorization Flow

```
1. TPP → OAuth2 Authorization Server: "Bu müşterinin hesap bilgisine
   erişmek istiyorum" (Authorization Request)
        │
        ▼
2. OAuth2 Server → Müşteri (Mobile/Internet Banking'e yönlendirme):
   "Kimliğini doğrula"
        │
        ▼
3. Müşteri → SCA ile kimlik doğrulama
        │
        ▼
4. Müşteri → Consent Management System: Rıza kapsamını onaylar
        │
        ▼
5. Consent Management System → OAuth2 Server: Rıza onayı bildirimi
        │
        ▼
6. OAuth2 Server → TPP: Access Token + Refresh Token üretilir ve iletilir
        │
        ▼
7. TPP → API Gateway: Access Token ile API çağrısı yapar
        │
        ▼
8. API Gateway: Token geçerliliğini, kapsamını ve rıza durumunu
   doğrular → Erişime izin verir/reddeder
```

**Kritik İş Kuralı:** Access Token, yalnızca müşterinin onayladığı kapsam (scope) ile sınırlıdır.

---

## 7. API Gateway Responsibilities

| Sorumluluk | Açıklama |
|---|---|
| Tek Giriş Noktası | Tüm TPP trafiği tek bir kontrollü noktadan geçer |
| Token Doğrulama | Her API çağrısında access token'ın geçerliliği ve kapsamı kontrol edilir |
| Rate Limiting / Throttling | Bir TPP'nin sistemi aşırı yüklemesini önler |
| Veri Filtreleme | Yanıtların yalnızca rıza kapsamındaki verilerle sınırlandırılması |
| Versiyonlama | API'lerin geriye dönük uyumluluk bozulmadan güncellenebilmesi |
| İzleme ve Loglama Entegrasyonu | Her işlemin Audit Platform'a otomatik aktarılması |
| Hata Yönetimi | Standart, tutarlı hata mesajları |

---

## 8. Consent Management Responsibilities

| Sorumluluk | Açıklama |
|---|---|
| Rıza Oluşturma | Müşterinin onayladığı veri kapsamı, süre ve TPP bilgisini kayıt altına alır |
| Rıza Doğrulama | Her API çağrısında, ilgili rızanın geçerliliğini kontrol eder |
| Rıza Süresi Yönetimi | Rızanın belirlenen sürede otomatik sona ermesini sağlar |
| Anlık İptal (Revoke) | Müşterinin istediği an rızasını iptal edebilmesini sağlar |
| Rıza Geçmişi | Geçmiş rızaların izlenebilir kaydını tutar |
| Şeffaflık Arayüzü Desteği | Consent Dashboard için gerekli veriyi sağlar |

---

## 9. Audit & Logging Architecture

```
┌───────────────┐   ┌──────────────────┐   ┌───────────────┐
│ API Gateway    │   │ Consent Mgmt Sys │   │ OAuth2 Server  │
└───────┬───────┘   └────────┬─────────┘   └───────┬───────┘
        │                    │                      │
        └────────────────────┼──────────────────────┘
                              ▼
                ┌─────────────────────────────┐
                │   Audit & Logging Platform   │
                │  (Merkezi, Immutable Kayıt)  │
                └─────────────┬───────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
       [Denetim Raporu] [Anomali Tespiti] [KPI Dashboard]
```

**Temel Tasarım İlkeleri:** Değiştirilemezlik (Immutability), Merkezi Toplama, Gerçek Zamanlı İzleme, Raporlama Hazırlığı.

---

## 10. Non-Functional Architecture Considerations

| Kategori | Gereksinim (Yüksek Seviye) |
|---|---|
| Performans | API yanıt süresi p95 < 500ms; yatay ölçeklenebilirlik |
| Erişilebilirlik (Availability) | %99.9 uptime hedefi |
| Ölçeklenebilirlik | Yeni TPP'lerin ve servislerin kolayca eklenebilmesi |
| Güvenlik | Zero Trust, uçtan uca şifreleme, düzenli penetrasyon testi |
| Uyumluluk (Compliance) | Düzenleyici otoritenin teknik standartlarına tam uyum |
| İzlenebilirlik (Auditability) | Her işlemin geriye dönük izlenebilir olması |
| Felaket Kurtarma (DR) | Kritik bileşenler için yedekleme/kurtarma planı |
| Veri Gizliliği (Privacy) | Kişisel verilerin yalnızca rıza kapsamında işlenmesi |
| Bakım Kolaylığı | API versiyonlama stratejisi |
| Interoperabilite | Açık bankacılık endüstri standartlarına uyumlu API tasarımı |

---

*Bu doküman, Aşama 4.5 (Solution Architecture Overview) çalışmasının konsolide edilmiş halidir. Teknik gereksinimler için bkz. [`../04-requirements/srs-software-requirements-specification.md`](../04-requirements/srs-software-requirements-specification.md).*
