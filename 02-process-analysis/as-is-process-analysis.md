# As-Is Process Analysis
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

---

## 1. Süreç Özeti

Mevcut durumda banka, üçüncü taraf sağlayıcılar (TPP) ile müşteri hesap bilgisi paylaşımını **standardize bir API altyapısı olmadan**, büyük ölçüde **manuel, ad-hoc ve dosya/e-posta tabanlı** yöntemlerle gerçekleştirmektedir. Süreç ortalama **10-15 iş günü** sürmekte olup, herhangi bir gerçek zamanlı (real-time) veri paylaşımı imkânı bulunmamaktadır.

---

## 2. BPMN Seviyesinde Mevcut Süreç Akışı (Üst Seviye)

```
[Başlangıç: TPP Talebi]
        │
        ▼
[TPP Banka ile İletişime Geçer (E-posta/Form)]
        │
        ▼
[Operasyon Ekibi Talebi Kayıt Altına Alır]
        │
        ▼
[Hukuk & Compliance Ön Değerlendirmesi] ──(Red)──► [TPP'ye Red Bildirimi] ──► [Son]
        │ (Onay)
        ▼
[Güvenlik (InfoSec) Değerlendirmesi] ──(Red)──► [TPP'ye Red Bildirimi] ──► [Son]
        │ (Onay)
        ▼
[Sözleşme Hazırlığı (Hukuk)]
        │
        ▼
[Müşteri Rızasının Manuel Teyidi]
   (Telefon/Şube/Form ile onay alınır)
        │
        ▼
[IT Ekibi Manuel Veri Çıkarımı]
   (Core Banking sisteminden rapor/export)
        │
        ▼
[Veri Güvenli Dosya Transferi ile TPP'ye İletilir]
        │
        ▼
[Operasyon Kaydı Kapatılır / Arşivleme]
        │
        ▼
[Son: Süreç Tamamlandı]
```

---

## 3. Swimlane Bazlı Süreç Analizi

```
┌──────────────┬─────────────────────────────────────────────────────────────────────┐
│ CUSTOMER     │  [Rıza Talebi Bildirimi Alır] → [Telefon/Şubede Sözlü/Yazılı Onay   │
│              │   Verir] → [Onay Sürecinin Sonucu Hakkında Bilgilendirilmez]         │
├──────────────┼─────────────────────────────────────────────────────────────────────┤
│ BANK         │  [TPP Talebini Kayıt Eder] → [Süreci Takip Eder (Excel/Manuel       │
│ OPERATIONS   │   Liste)] → [Müşteriyle İletişime Geçer] → [Dosyayı TPP'ye İletir]  │
│              │   → [Kaydı Kapatır]                                                  │
├──────────────┼─────────────────────────────────────────────────────────────────────┤
│ COMPLIANCE   │  [TPP'nin Yasal Uygunluğunu Değerlendirir] → [Regülasyon Kontrol    │
│              │   Listesini Manuel Doldurur] → [Onay/Red Kararı Verir]               │
├──────────────┼─────────────────────────────────────────────────────────────────────┤
│ IT           │  [Core Banking'den Veri Sorgusu Yapar] → [Veriyi Manuel Export      │
│              │   Eder (CSV/Excel)] → [Güvenli Dosya Transfer Sistemine Yükler]      │
├──────────────┼─────────────────────────────────────────────────────────────────────┤
│ TPP          │  [Talebi Banka'ya İletir] → [Bekler (SLA Belirsiz)] → [Veriyi       │
│              │   Dosya Olarak Alır] → [Kendi Sistemine Manuel İşler]                │
└──────────────┴─────────────────────────────────────────────────────────────────────┘
```

**Gözlem:** "Customer" lane'i sürecin çoğunda pasif konumdadır ve sürecin sonucu hakkında geri bildirim alamaz. IT ve Compliance arasında otomatik bir entegrasyon/onay mekanizması bulunmamaktadır.

---

## 4. Süreç Adımları

| # | Adım | Sorumlu | Ortalama Süre |
|---|------|---------|----------------|
| 1 | TPP, banka ile iletişime geçer (e-posta/form) | TPP | 1 gün |
| 2 | Operasyon ekibi talebi kayıt altına alır | Bank Operations | 1 gün |
| 3 | Hukuk & Compliance ön değerlendirmesi yapılır | Compliance | 2-3 gün |
| 4 | Bilgi güvenliği değerlendirmesi yapılır | InfoSec | 2-3 gün |
| 5 | Sözleşme hazırlanır ve TPP ile imzalanır | Hukuk | 3-5 gün |
| 6 | Müşteri rızası telefon/şube üzerinden manuel teyit edilir | Bank Operations / Customer | 1-2 gün |
| 7 | IT ekibi core banking sisteminden veriyi manuel export eder | IT | 1 gün |
| 8 | Veri güvenli dosya transfer sistemi ile TPP'ye iletilir | IT | 1 gün |
| 9 | Operasyon kaydı kapatılır, arşivlenir | Bank Operations | 1 gün |

**Toplam Ortalama Süreç Süresi: ~10-15 iş günü**

---

## 5. Süreçteki Problemler

| Problem ID | Açıklama | Etki |
|---|---|---|
| P-01 | Standart bir API altyapısı olmadığından her TPP talebi manuel olarak işleniyor | Yüksek operasyonel maliyet, ölçeklenemeyen süreç |
| P-02 | Müşteri rızası dijital/merkezi olarak değil, telefon/şube üzerinden sözlü ya da kağıt bazlı alınıyor | Denetlenebilirlik eksikliği, hukuki ispat riski |
| P-03 | Veri paylaşımı CSV/Excel gibi dosya formatlarında, güvenli dosya transferi ile yapılıyor | Veri sızıntısı/yetkisiz erişim riski |
| P-04 | Süreçte gerçek zamanlı (real-time) veri paylaşımı mümkün değil | TPP deneyimi düşük, rekabet dezavantajı |
| P-05 | Compliance ve InfoSec değerlendirmeleri kişiye bağlı manuel değerlendirme ile yapılıyor | Tutarsız karar süreçleri, denetim riski |
| P-06 | Müşteri, rızasının nereye, ne kadar süreyle verildiği konusunda şeffaf bilgi alamıyor | Müşteri güveni ve regülasyon uyum riski |
| P-07 | Süreç durumu TPP tarafından izlenemiyor (status tracking yok) | TPP memnuniyetsizliği, belirsiz SLA |
| P-08 | Rıza iptal (revoke) mekanizması manuel ve yavaş işliyor | Regülasyon uyum riski (anlık iptal hakkı) |
| P-09 | Veri export işlemleri IT ekibinin manuel müdahalesine bağlı, otomasyon yok | Operasyonel darboğaz, insan hatası riski |
| P-10 | Audit trail (denetim izi) parçalı şekilde farklı sistemlerde tutuluyor | Denetim sırasında kanıt toplama zorluğu |

---

## 6. Darboğazlar (Bottlenecks)

| Darboğaz | Konum (Süreç Adımı) | Neden |
|---|---|---|
| Compliance & InfoSec Onay Süresi | Adım 3-4 | Manuel değerlendirme, kişiye bağlı kapasite, standart kontrol listesi eksikliği |
| Sözleşme Hazırlama Süreci | Adım 5 | Her TPP için özel sözleşme hazırlığı, şablon eksikliği |
| Müşteri Rızası Teyidi | Adım 6 | Şube/çağrı merkezi kapasitesine bağımlılık |
| IT Manuel Veri Export | Adım 7 | Veri çekme işleminin otomasyona bağlı olmaması |

---

## 7. Operasyonel Riskler

| Risk | Açıklama |
|------|----------|
| Veri Sızıntısı Riski | Dosya bazlı veri transferinde şifreleme/erişim kontrolü standardizasyonu eksik |
| İnsan Hatası Riski | Manuel veri export ve dosya hazırlama süreçlerinde yanlış veri paylaşılma riski |
| Yetkisiz Erişim Riski | Rıza teyidinin sözlü/manuel olması nedeniyle yetkisiz veri paylaşımı riski |
| SLA İhlali Riski | Standart bir SLA tanımı olmaması |
| Denetim Bulgusu Riski | Parçalı audit trail nedeniyle kanıt sunma zorluğu |
| Süreklilik Riski (Key Person Dependency) | Belirli çalışanların bilgisine bağımlılık |

---

## 8. Mevcut KPI'lar

| KPI | Mevcut Durum |
|-----|----------------|
| Ortalama Süreç Tamamlanma Süresi | ~10-15 iş günü |
| TPP Onboarding Süresi | Standart değil, 1-3 hafta |
| Veri Paylaşım Hata Oranı | Sistematik ölçülmüyor |
| Müşteri Rıza İptal İşlem Süresi | Ölçülmüyor, ~3-5 iş günü (tahmini) |
| Denetim Bulgusu Sayısı | Geçmiş denetimlerde kayıt eksikliği bulguları mevcut |
| TPP Memnuniyeti | Sistematik olarak ölçülmüyor |

---

## 9. Süreç Ağrı Noktaları (Pain Points)

| Paydaş | Pain Point |
|--------|------------|
| Müşteri | Rızasının takibinin yapılamaması; iptal sürecinin yavaş olması |
| TPP | Belirsiz SLA, dokümantasyon eksikliği, manuel onboarding |
| Bank Operations | Yüksek manuel iş yükü |
| Compliance | Standart kontrol listesi eksikliği |
| InfoSec | Dosya bazlı transferin güvenlik standardizasyonu sağlamaması |
| IT | Manuel veri export işlemlerinin tekrarlayan, hataya açık olması |
| Üst Yönetim | Sürecin ölçeklenemez olması |

---

## 10. As-Is Sürecinin Metinsel BPMN Açıklaması

**Süreç Adı:** Mevcut Hesap Bilgisi Paylaşım Süreci (Manuel)

**Başlangıç Olayı:** TPP, bankaya e-posta veya form aracılığıyla hesap bilgisi paylaşım talebinde bulunur.

1. **Bank Operations Lane:** Talep alınır ve manuel takip listesine kaydedilir (Task).
2. **Gateway (XOR):** Compliance, TPP'nin yasal uygunluğunu değerlendirir. Red yolunda süreç sonlanır.
3. **Gateway (XOR):** InfoSec, teknik/güvenlik kriterlerini değerlendirir. Red yolunda süreç sonlanır.
4. **Hukuk Lane:** Sözleşme hazırlanır, imzalanır (Manual Task).
5. **Customer Lane (Intermediate Event - Message):** Operasyon, müşteriyle telefon/şube üzerinden iletişime geçer; müşteri sözlü/kağıt rıza beyan eder (sistemde otomatik kayıt yok — kritik zayıf nokta).
6. **IT Lane:** Core banking'den veri manuel export edilir (Manual Task, otomasyon yok).
7. **IT Lane → TPP Lane (Message Flow):** Veri, SFTP ile iletilir.
8. **Bank Operations Lane:** Kayıt kapatılır, dokümanlar farklı sistemlerde arşivlenir.

**Bitiş Olayı:** Veri TPP'ye iletildikten ve kayıt kapatıldıktan sonra süreç sonlanır.

**Modelleme Notu:** Sürecin en dikkat çekici özelliği, **otomatik sistem görevi (Service Task) bulunmaması** ve neredeyse tüm adımların **Manual/User Task** olarak modellenmesidir — bu, To-Be sürecinin temel otomasyon fırsatlarını işaret eder.

---

## 11. Root Cause Analysis (Kök Neden Analizi)

| Problem ID | Problem | Root Cause | Business Impact | Severity |
|---|---|---|---|---|
| P-01 | Manuel TPP talep işleme | Bankanın API-first mimariye geçmemiş olması; entegrasyonlar nokta-to-nokta yapılandırılmış | Ölçeklenemeyen süreç, artan operasyonel maliyet | High |
| P-02 | Manuel/telefon bazlı rıza | Merkezi Consent Management sistemi hiç kurulmamış | Hukuki ispat zafiyeti, regülasyon riski | High |
| P-03 | Dosya bazlı veri paylaşımı | API tabanlı, alan bazlı veri paylaşım standardı tanımlanmamış | Veri sızıntısı/over-sharing riski | High |
| P-04 | Gerçek zamanlı paylaşım yok | Core banking ile dış sistemler arası real-time API katmanı yok | TPP deneyiminde rekabet dezavantajı | Medium |
| P-05 | Standart değerlendirme kriteri yok | TPP değerlendirme checklist'i hiç oluşturulmamış | Tutarsız kararlar, denetim riski | Medium |
| P-06 | Müşteri şeffaflığı eksik | Müşteri rıza yönetim arayüzü mevcut değil | Şeffaflık ilkesi ihlali, şikayet riski | High |
| P-07 | TPP süreç izleyemiyor | Developer portal/self-servis durum izleme yok | TPP memnuniyetsizliği | Medium |
| P-08 | Yavaş rıza iptali | Rıza yaşam döngüsü manuel operasyona bağlı | Anlık iptal hakkı ihlali riski | High |
| P-09 | Manuel veri export | Standart sorgu/API servisi tanımlanmamış | Operasyonel darboğaz, insan hatası | Medium |
| P-10 | Parçalı audit trail | Merkezi log/audit sistemi entegre edilmemiş | Denetim kanıtı toplama zorluğu | High |

### Root Cause Analysis - Özet Değerlendirme

**Tekrarlayan Kök Nedenler:**
1. **Sistemsel Otomasyon Eksikliği** — P-01, P-03, P-04, P-09
2. **Merkezi Rıza/Veri Yönetimi Sisteminin Yokluğu** — P-02, P-06, P-08
3. **Standardizasyon ve Dokümantasyon Eksikliği** — P-05, P-07
4. **Merkezi İzlenebilirlik (Audit) Altyapısının Olmaması** — P-10

> Bu dört tekrarlayan kök neden, To-Be tasarımının ana mimari bileşenlerini (API Gateway, Consent Management Sistemi, Standart TPP Değerlendirme Çerçevesi, Merkezi Audit/Logging Sistemi) doğrudan şekillendirmiştir.

---

*Bu doküman, Aşama 2 (As-Is Process Analysis ve Root Cause Analysis) çalışmasının konsolide edilmiş halidir.*
