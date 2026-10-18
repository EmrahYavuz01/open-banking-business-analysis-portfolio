# AŞAMA 12: UML ve Process Modeling Package
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

> Bu paket, Aşama 5 (BRD), Aşama 6 (User Stories), Aşama 7 (SRS) ve Aşama 4.5 (Solution Architecture Overview) ile tam tutarlı olacak şekilde hazırlanmıştır. Yeni gereksinim veya test case içermez.

---

## 1. Use Case Diagram

### Amaç
Sistemin sunduğu temel işlevleri ve bu işlevlerle etkileşen aktörleri üst seviyede göstermek. BRD (Bölüm 9) ve User Story (Aşama 6) kapsamındaki tüm Epic'lerin (Consent Management, TPP Onboarding, AIS, Security, Audit) use case karşılıklarını sunar.

### Aktörler / Bileşenler
- **Customer** (Müşteri)
- **TPP** (Third Party Provider)
- **Compliance Officer**
- **InfoSec Officer**
- **System: API Gateway** (otomatik aktör)

### Mermaid Formatı

```mermaid
flowchart LR
    Customer((Customer))
    TPP((TPP))
    Compliance((Compliance Officer))
    InfoSec((InfoSec Officer))

    subgraph System["Open Banking Platform"]
        UC1([Grant Consent])
        UC2([View Active Consents])
        UC3([Revoke Consent])
        UC4([Register as TPP])
        UC5([Upload Certificate])
        UC6([Test in Sandbox])
        UC7([Request Account Information])
        UC8([Review TPP Onboarding Exception])
        UC9([Generate Compliance Report])
        UC10([Monitor Security Anomalies])
    end

    Customer --> UC1
    Customer --> UC2
    Customer --> UC3
    TPP --> UC4
    TPP --> UC5
    TPP --> UC6
    TPP --> UC7
    Compliance --> UC8
    Compliance --> UC9
    InfoSec --> UC10

    UC1 -.includes.-> UC11([Perform SCA])
    UC7 -.includes.-> UC12([Validate Consent Token])
```

### Draw.io Uyumlu Açıklama
- **Şekil Önerisi:** Standart UML Use Case notasyonu (oval = use case, çubuk adam = aktör, dikdörtgen = sistem sınırı "Open Banking Platform")
- **Konumlandırma:** Customer ve TPP soldan, Compliance/InfoSec sağdan sistem kutusuna bağlanır
- **İlişki Türleri:** `<<include>>` ilişkisi, "Grant Consent" → "Perform SCA" ve "Request Account Information" → "Validate Consent Token" arasında kullanılmalı (drawio'da kesikli ok + include etiketi)
- **Draw.io Şablonu:** "UML > Use Case" kategorisindeki hazır şekil kütüphanesi kullanılabilir

### PlantUML Formatı

```plantuml
@startuml UseCaseDiagram
left to right direction
actor Customer
actor TPP
actor "Compliance Officer" as Compliance
actor "InfoSec Officer" as InfoSec

rectangle "Open Banking Platform" {
  usecase "Grant Consent" as UC1
  usecase "View Active Consents" as UC2
  usecase "Revoke Consent" as UC3
  usecase "Register as TPP" as UC4
  usecase "Upload Certificate" as UC5
  usecase "Test in Sandbox" as UC6
  usecase "Request Account Information" as UC7
  usecase "Review TPP Onboarding Exception" as UC8
  usecase "Generate Compliance Report" as UC9
  usecase "Monitor Security Anomalies" as UC10
  usecase "Perform SCA" as UC11
  usecase "Validate Consent Token" as UC12
}

Customer --> UC1
Customer --> UC2
Customer --> UC3
TPP --> UC4
TPP --> UC5
TPP --> UC6
TPP --> UC7
Compliance --> UC8
Compliance --> UC9
InfoSec --> UC10

UC1 .> UC11 : <<include>>
UC7 .> UC12 : <<include>>
@enduml
```

---

## 2. Activity Diagram — Consent Management Flow

### Amaç
Müşterinin rıza verme sürecindeki adımları ve karar noktalarını göstermek (FR-01, FR-04, FR-18, SEC-02 ile uyumlu).

### Aktörler / Bileşenler
Customer, Mobile/Internet Banking, OAuth2 Authorization Server, Consent Management System

### Mermaid Formatı

```mermaid
flowchart TD
    Start([Başlangıç: TPP Rıza Talebi Gönderir]) --> A[Müşteriye Bildirim Gönderilir]
    A --> B{Müşteri Talebi Görüntüler}
    B -->|Zaman Aşımı 5dk| Timeout[Talep Otomatik İptal Edilir]
    Timeout --> EndCancel([Son: Talep İptal])
    B --> C[SCA Doğrulaması Başlatılır]
    C --> D{SCA Başarılı mı?}
    D -->|Hayır - 3 Deneme| Lock[Hesap Geçici Kilitlenir + InfoSec Bildirilir]
    Lock --> EndLock([Son: Erişim Reddedildi])
    D -->|Evet| E[Rıza Kapsamı Ekranı Gösterilir]
    E --> F{Müşteri Kararı}
    F -->|Reddet| G[Rıza Oluşturulmaz, TPP'ye Red Bildirimi]
    G --> EndReject([Son: Red])
    F -->|Onayla| H[Consent Kaydı Oluşturulur - ACTIVE]
    H --> I[Consent Token Üretilir]
    I --> J[TPP'ye Token İletilir]
    J --> K[Audit Log Kaydı Oluşturulur]
    K --> EndApprove([Son: Rıza Aktif])
```

### Draw.io Uyumlu Açıklama
- **Şekil Önerisi:** Standart UML Activity notasyonu — yuvarlak köşeli dikdörtgen = aktivite, elmas = karar noktası, dolu daire = başlangıç, çevrili dolu daire = bitiş
- **Swimlane Önerisi:** Customer / Mobile-Internet Banking / Consent Management System olmak üzere 3 dikey swimlane kullanılabilir; SCA adımı "Mobile/Internet Banking" lane'inde, Token üretimi "Consent Management System" lane'inde gösterilmeli
- **Renk Kodlama:** Karar noktaları sarı, hata/red çıkışları kırmızı, başarı çıkışı yeşil

### PlantUML Formatı

```plantuml
@startuml ConsentManagementActivity
start
:TPP Rıza Talebi Gönderir;
:Müşteriye Bildirim Gönderilir;

if (Müşteri 5 dk içinde işlem yapar mı?) then (hayır - zaman aşımı)
  :Talep Otomatik İptal Edilir;
  stop
else (evet)
endif

:SCA Doğrulaması Başlatılır;
if (SCA Başarılı mı?) then (hayır - 3 deneme)
  :Hesap Geçici Kilitlenir;
  :InfoSec'e Bildirim Gönderilir;
  stop
else (evet)
endif

:Rıza Kapsamı Ekranı Gösterilir;
if (Müşteri Kararı) then (Reddet)
  :Rıza Oluşturulmaz;
  :TPP'ye Red Bildirimi Gönderilir;
  stop
else (Onayla)
  :Consent Kaydı Oluşturulur (ACTIVE);
  :Consent Token Üretilir;
  :TPP'ye Token İletilir;
  :Audit Log Kaydı Oluşturulur;
  stop
endif
@enduml
```

---

## 3. Activity Diagram — TPP Onboarding Flow

### Amaç
TPP'nin kayıt olmasından production erişimine kadar olan sürecin adım ve karar noktalarını göstermek (FR-07—FR-12 ile uyumlu).

### Aktörler / Bileşenler
TPP, Developer Portal, Compliance Engine, Compliance Officer (manuel istisna durumu), Sandbox Environment

### Mermaid Formatı

```mermaid
flowchart TD
    Start([Başlangıç: TPP Kayıt Formu]) --> A[TPP Kayıt Bilgilerini Girer]
    A --> B{Zorunlu Alanlar Eksiksiz mi?}
    B -->|Hayır| C[Hata Mesajı Gösterilir]
    C --> A
    B -->|Evet| D[Sertifika Yüklenir]
    D --> E[Otomatik Sertifika Doğrulama Çalışır]
    E --> F{Doğrulama Sonucu}
    F -->|Geçersiz| G[Otomatik Red + Bildirim]
    G --> EndReject([Son: Red])
    F -->|Belirsiz/Şüpheli| H[Manuel İnceleme Kuyruğuna Yönlendirilir]
    H --> I{Compliance Kararı}
    I -->|Red| G
    I -->|Onay| J[Sandbox Erişimi Verilir]
    F -->|Geçerli| J
    J --> K[TPP Sandbox'ta Test Eder]
    K --> L{Testler Başarılı mı?}
    L -->|Hayır| K
    L -->|Evet| M[Compliance Production Onayı Verir]
    M --> N[Production Erişimi Aktive Edilir]
    N --> EndApprove([Son: TPP Aktif])
```

### Draw.io Uyumlu Açıklama
- **Şekil Önerisi:** UML Activity notasyonu, "Manuel İnceleme Kuyruğu" adımı için fork/join veya basit dikdörtgen kullanılabilir
- **Swimlane Önerisi:** TPP / Developer Portal / Compliance Engine / Compliance Officer olmak üzere 4 swimlane
- **Vurgu Notu:** "Otomatik Sertifika Doğrulama" ile "Manuel İnceleme" arasındaki dallanma, otomasyon + istisna yönetimi prensibini (FR-11) görsel olarak vurgulamalı

### PlantUML Formatı

```plantuml
@startuml TppOnboardingActivity
start
:TPP Kayıt Bilgilerini Girer;
if (Zorunlu Alanlar Eksiksiz mi?) then (hayır)
  :Hata Mesajı Gösterilir;
  stop
else (evet)
endif

:Sertifika Yüklenir;
:Otomatik Sertifika Doğrulama Çalışır;

if (Doğrulama Sonucu) then (Geçersiz)
  :Otomatik Red + Bildirim;
  stop
elseif (Belirsiz/Şüpheli) then
  :Manuel İnceleme Kuyruğuna Yönlendirilir;
  if (Compliance Kararı) then (Red)
    :Otomatik Red + Bildirim;
    stop
  else (Onay)
    :Sandbox Erişimi Verilir;
  endif
else (Geçerli)
  :Sandbox Erişimi Verilir;
endif

:TPP Sandbox'ta Test Eder;
repeat
  :Test Sonuçları Değerlendirilir;
repeat while (Testler Başarılı mı?) is (hayır)
->evet;
:Compliance Production Onayı Verir;
:Production Erişimi Aktive Edilir;
stop
@enduml
```

---

## 4. Sequence Diagram — Account Information Request

### Amaç
TPP'nin geçerli bir rıza kapsamında hesap bilgisine gerçek zamanlı erişim sürecindeki bileşen etkileşimlerini göstermek (FR-13, FR-14, FR-15 ile uyumlu).

### Aktörler / Bileşenler
TPP, API Gateway, Consent Management System, Core Banking, Audit & Logging Platform

### Mermaid Formatı

```mermaid
sequenceDiagram
    participant TPP
    participant Gateway as API Gateway
    participant Consent as Consent Mgmt System
    participant Core as Core Banking
    participant Audit as Audit & Logging Platform

    TPP->>Gateway: GET /accounts/balance (Bearer Token)
    Gateway->>Consent: Token / Consent Doğrulama İsteği
    alt Token Geçersiz veya Rıza İptal/Süresi Dolmuş
        Consent-->>Gateway: Geçersiz Sonuç
        Gateway-->>TPP: 401/403 Hata Yanıtı
        Gateway->>Audit: Başarısız Erişim Olayını Kaydet
    else Token ve Rıza Geçerli
        Consent-->>Gateway: Geçerli + Kapsam Bilgisi (örn. "balance")
        Gateway->>Core: Hesap Bakiyesi Sorgusu (Kapsam Sınırlı)
        Core-->>Gateway: Bakiye Verisi
        Gateway-->>TPP: 200 OK + JSON Yanıt
        Gateway->>Audit: Başarılı Erişim Olayını Kaydet
    end
```

### Draw.io Uyumlu Açıklama
- **Şekil Önerisi:** UML Sequence Diagram notasyonu — dikey yaşam çizgileri (lifeline), yatay mesaj okları
- **Alt/Else Bloğu:** Draw.io'da "Combined Fragment" (alt) kutusu kullanılarak "Token Geçersiz" / "Token Geçerli" senaryoları tek diyagramda gösterilmeli
- **Renk Kodlama:** Hata yolu (alt bloğun ilk dalı) kırmızı çerçeve, başarı yolu yeşil çerçeve ile vurgulanabilir

### PlantUML Formatı

```plantuml
@startuml AccountInformationSequence
actor TPP
participant "API Gateway" as Gateway
participant "Consent Mgmt System" as Consent
participant "Core Banking" as Core
participant "Audit & Logging Platform" as Audit

TPP -> Gateway: GET /accounts/balance (Bearer Token)
Gateway -> Consent: Token / Consent Doğrulama İsteği

alt Token Geçersiz veya Rıza İptal/Süresi Dolmuş
  Consent --> Gateway: Geçersiz Sonuç
  Gateway --> TPP: 401/403 Hata Yanıtı
  Gateway -> Audit: Başarısız Erişim Olayını Kaydet
else Token ve Rıza Geçerli
  Consent --> Gateway: Geçerli + Kapsam Bilgisi
  Gateway -> Core: Hesap Bakiyesi Sorgusu (Kapsam Sınırlı)
  Core --> Gateway: Bakiye Verisi
  Gateway --> TPP: 200 OK + JSON Yanıt
  Gateway -> Audit: Başarılı Erişim Olayını Kaydet
end
@enduml
```

---

## 5. Sequence Diagram — Consent Creation Flow

### Amaç
Müşteri rızasının oluşturulma sürecindeki bileşen etkileşimlerini, SCA ve token üretimi dahil göstermek (FR-01, FR-18, SEC-01, SEC-02 ile uyumlu).

### Aktörler / Bileşenler
TPP, Customer, Mobile/Internet Banking, OAuth2 Authorization Server, Consent Management System, Audit & Logging Platform

### Mermaid Formatı

```mermaid
sequenceDiagram
    participant TPP
    participant OAuth as OAuth2 Authorization Server
    participant Customer
    participant Banking as Mobile/Internet Banking
    participant Consent as Consent Mgmt System
    participant Audit as Audit & Logging Platform

    TPP->>OAuth: Authorization Request (client_id, scope)
    OAuth->>Banking: Müşteriyi Kimlik Doğrulamaya Yönlendir
    Banking->>Customer: SCA Ekranını Göster
    Customer->>Banking: SCA Bilgisi Girer (Biyometrik/OTP)
    Banking->>OAuth: SCA Sonucu (Başarılı)
    OAuth->>Consent: Rıza Talebi Oluştur (scope, süre)
    Consent->>Customer: Rıza Kapsamı Onay Ekranı
    Customer->>Consent: Rızayı Onaylar
    Consent->>Consent: Consent Kaydı ACTIVE Olarak Güncellenir
    Consent->>OAuth: Rıza Onay Bildirimi
    OAuth->>OAuth: Access Token + Refresh Token Üretilir
    OAuth-->>TPP: Access Token Döner
    OAuth->>Audit: Token Üretim Olayını Kaydet
    Consent->>Audit: Rıza Oluşturma Olayını Kaydet
```

### Draw.io Uyumlu Açıklama
- **Şekil Önerisi:** UML Sequence Diagram, 6 yaşam çizgisi (TPP, OAuth2 Server, Customer, Mobile/Internet Banking, Consent Mgmt System, Audit Platform)
- **Aktivasyon Kutuları (Activation Bars):** "Consent Kaydı ACTIVE Olarak Güncellenir" gibi iç işlem adımları, Consent Mgmt System'in kendi üzerine dönen (self-message) ok ile gösterilmeli
- **Not Kutusu Önerisi:** SCA adımının yanına "Regülasyon Zorunluluğu (SEC-02)" notu eklenmesi, dokümanın iş gerekçesini güçlendirir

### PlantUML Formatı

```plantuml
@startuml ConsentCreationSequence
actor TPP
participant "OAuth2 Authorization Server" as OAuth
actor Customer
participant "Mobile/Internet Banking" as Banking
participant "Consent Mgmt System" as Consent
participant "Audit & Logging Platform" as Audit

TPP -> OAuth: Authorization Request (client_id, scope)
OAuth -> Banking: Müşteriyi Kimlik Doğrulamaya Yönlendir
Banking -> Customer: SCA Ekranını Göster
Customer -> Banking: SCA Bilgisi Girer
Banking -> OAuth: SCA Sonucu (Başarılı)
OAuth -> Consent: Rıza Talebi Oluştur (scope, süre)
Consent -> Customer: Rıza Kapsamı Onay Ekranı
Customer -> Consent: Rızayı Onaylar
Consent -> Consent: Consent Kaydı ACTIVE Olarak Güncellenir
Consent -> OAuth: Rıza Onay Bildirimi
OAuth -> OAuth: Access Token + Refresh Token Üretilir
OAuth --> TPP: Access Token Döner
OAuth -> Audit: Token Üretim Olayını Kaydet
Consent -> Audit: Rıza Oluşturma Olayını Kaydet
@enduml
```

---

## 6. Domain Model / Class Diagram

### Amaç
SRS Bölüm 7'de (Data Requirements) tanımlanan veri varlıklarının (Customer, Consent, TPP, Account, Transaction, Access Token, Audit Log) ilişkilerini kavramsal düzeyde göstermek.

### Aktörler / Bileşenler
Veri varlıkları: Customer, Consent, TPP, Account, Transaction, AccessToken, AuditLog

### Mermaid Formatı

```mermaid
classDiagram
    class Customer {
        +String customerId
        +String fullName
        +String contactInfo
        +List~Account~ accounts
    }

    class Account {
        +String accountId
        +String accountType
        +Decimal balance
        +String currency
    }

    class Transaction {
        +String transactionId
        +Date transactionDate
        +Decimal amount
        +String transactionType
    }

    class Consent {
        +String consentId
        +List~String~ dataScope
        +Date startDate
        +Date endDate
        +String status
    }

    class TPP {
        +String tppId
        +String companyName
        +String certificateInfo
        +String onboardingStatus
    }

    class AccessToken {
        +String tokenId
        +String tokenType
        +Date expiryDate
        +String relatedConsentId
    }

    class AuditLog {
        +String logId
        +DateTime timestamp
        +String actor
        +String eventType
        +String result
    }

    Customer "1" --> "many" Account : owns
    Account "1" --> "many" Transaction : has
    Customer "1" --> "many" Consent : grants
    TPP "1" --> "many" Consent : receives
    Consent "1" --> "1" AccessToken : generates
    Consent "1" --> "many" AuditLog : logged_in
    TPP "1" --> "many" AuditLog : logged_in
    AccessToken "1" --> "many" AuditLog : logged_in
```

### Draw.io Uyumlu Açıklama
- **Şekil Önerisi:** UML Class Diagram notasyonu — 3 bölmeli kutular (sınıf adı / öznitelikler / metotlar — bu domain modelinde metot bölümü boş bırakılabilir, kavramsal model olduğu için)
- **İlişki Çizgileri:** "owns", "has", "grants", "receives", "generates", "logged_in" etiketli düz çizgiler; çokluk (multiplicity) değerleri (1, many) çizgi uçlarına yazılmalı
- **Renk Kodlama:** Düzenleyici/denetim açısından kritik varlıklar (Consent, AuditLog) farklı bir renkle (örn. açık kırmızı çerçeve) vurgulanabilir

### PlantUML Formatı

```plantuml
@startuml DomainModel
class Customer {
  +customerId: String
  +fullName: String
  +contactInfo: String
}

class Account {
  +accountId: String
  +accountType: String
  +balance: Decimal
  +currency: String
}

class Transaction {
  +transactionId: String
  +transactionDate: Date
  +amount: Decimal
  +transactionType: String
}

class Consent {
  +consentId: String
  +dataScope: List<String>
  +startDate: Date
  +endDate: Date
  +status: String
}

class TPP {
  +tppId: String
  +companyName: String
  +certificateInfo: String
  +onboardingStatus: String
}

class AccessToken {
  +tokenId: String
  +tokenType: String
  +expiryDate: Date
  +relatedConsentId: String
}

class AuditLog {
  +logId: String
  +timestamp: DateTime
  +actor: String
  +eventType: String
  +result: String
}

Customer "1" -- "many" Account : owns
Account "1" -- "many" Transaction : has
Customer "1" -- "many" Consent : grants
TPP "1" -- "many" Consent : receives
Consent "1" -- "1" AccessToken : generates
Consent "1" -- "many" AuditLog : logged_in
TPP "1" -- "many" AuditLog : logged_in
AccessToken "1" -- "many" AuditLog : logged_in
@enduml
```

---

## 7. System Context Diagram

### Amaç
Platformun, dış aktörler ve mevcut bankacılık sistemleriyle olan sınırlarını ve ilişkilerini en üst seviyede göstermek (Solution Architecture Overview, Bölüm 1-2 ile uyumlu).

### Aktörler / Bileşenler
Customer, TPP, Düzenleyici Otorite, Open Banking Platform (sistem sınırı), Core Banking System (mevcut sistem), IAM Sistemi (mevcut sistem)

### Mermaid Formatı

```mermaid
flowchart TB
    Customer((Customer))
    TPP((TPP))
    Regulator((Düzenleyici Otorite))

    subgraph Platform["Open Banking API Platform (Yeni Sistem)"]
        direction TB
        P1[Consent Management System]
        P2[API Gateway]
        P3[OAuth2 Authorization Server]
        P4[Developer Portal]
        P5[Sandbox Environment]
        P6[Audit & Logging Platform]
    end

    CoreBanking[(Core Banking System - Mevcut)]
    IAM[(IAM Sistemi - Mevcut)]

    Customer -->|Rıza Verir/İptal Eder| Platform
    TPP -->|API Çağrısı Yapar| Platform
    Platform -->|Salt Okunur Veri Sorgusu| CoreBanking
    Platform -->|Kimlik Doğrulama Entegrasyonu| IAM
    Platform -->|Uyum Raporları Sunar| Regulator
    Regulator -->|Denetim Talebi| Platform
```

### Draw.io Uyumlu Açıklama
- **Şekil Önerisi:** C4 Model "System Context" notasyonu (veya basit UML Deployment/Component karışımı) — dış aktörler daire, sistem sınırı büyük dikdörtgen, mevcut sistemler silindir (veritabanı) şekliyle gösterilebilir
- **Sınır Vurgusu:** "Open Banking API Platform" kutusu kalın/kesikli çerçeve ile diğer mevcut sistemlerden (Core Banking, IAM) görsel olarak ayrılmalı — bu, Solution Architecture Overview'daki "mevcut sistemi değiştirmiyoruz, üzerine ekliyoruz" mesajını pekiştirir
- **Renk Kodlama:** Yeni geliştirilen bileşenler (Platform içi) mavi tonlarda, mevcut sistemler (Core Banking, IAM) gri tonlarda gösterilmeli

### PlantUML Formatı

```plantuml
@startuml SystemContext
actor Customer
actor TPP
actor "Düzenleyici Otorite" as Regulator

package "Open Banking API Platform (Yeni Sistem)" as Platform {
  component "Consent Management System" as P1
  component "API Gateway" as P2
  component "OAuth2 Authorization Server" as P3
  component "Developer Portal" as P4
  component "Sandbox Environment" as P5
  component "Audit & Logging Platform" as P6
}

database "Core Banking System\n(Mevcut)" as CoreBanking
database "IAM Sistemi\n(Mevcut)" as IAM

Customer --> Platform : Rıza Verir / İptal Eder
TPP --> Platform : API Çağrısı Yapar
Platform --> CoreBanking : Salt Okunur Veri Sorgusu
Platform --> IAM : Kimlik Doğrulama Entegrasyonu
Platform --> Regulator : Uyum Raporları Sunar
Regulator --> Platform : Denetim Talebi
@enduml
```

---

## Diyagram - Artefakt İzlenebilirlik Tablosu

| Diyagram | İlgili BRD/SRS/Architecture Referansı |
|---|---|
| Use Case Diagram | BRD Bölüm 9 (tüm BR'ler), Epic Model (Aşama 6) |
| Activity Diagram - Consent Management | FR-01, FR-04, FR-18, SEC-02 |
| Activity Diagram - TPP Onboarding | FR-07, FR-08, FR-09, FR-10, FR-11, FR-12 |
| Sequence Diagram - Account Information Request | FR-13, FR-14, FR-15, SEC-08 |
| Sequence Diagram - Consent Creation Flow | FR-01, FR-18, SEC-01, SEC-02 |
| Domain Model / Class Diagram | SRS Bölüm 7 (Data Requirements) |
| System Context Diagram | Solution Architecture Overview (Aşama 4.5, Bölüm 1-3) |

> **Modelleme Notu:** Tüm diyagramlar, gerçek bir bankacılık projesinde Solution Architect ve Geliştirme Ekibi'nin BA/PO dokümanlarından teknik tasarıma geçişte kullanacağı **köprü artefaktlardır**. Mermaid formatı GitHub'da doğrudan render edilebilir; PlantUML formatı kurumsal araçlarla (Confluence, IntelliJ plugin) entegrasyon için sunulmuştur; draw.io açıklamaları ise görsel/sunum amaçlı manuel diyagram oluşturma ihtiyacını karşılar.

---

Aşama 12 (UML ve Process Modeling Package) tamamlanmıştır. Yeni gereksinim veya test case eklenmemiştir.
