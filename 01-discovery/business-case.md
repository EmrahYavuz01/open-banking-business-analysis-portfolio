# Business Case
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

---

## 1. Problem Statement (Problem Tanımı)

Bankamızın mevcut sistem altyapısı, müşterilerin finansal verilerini yalnızca kendi dijital kanalları (mobil/internet bankacılığı) üzerinden erişilebilir şekilde tutmaktadır. PSD2 ve yerel düzenleyici otoritenin (BDDK / Açık Bankacılık Yönetmeliği) zorunlu kıldığı şekilde, üçüncü taraf sağlayıcıların (TPP - Third Party Provider) müşteri onayıyla hesap bilgilerine erişebilmesi için standardize edilmiş, güvenli ve denetlenebilir bir API altyapısı bulunmamaktadır.

Bu durum aşağıdaki sorunlara yol açmaktadır:
- **Regülasyon Uyum Riski:** Açık Bankacılık düzenlemelerine uyumsuzluk nedeniyle yasal/cezai risk.
- **Rekabet Dezavantajı:** Fintech ve diğer bankaların sunduğu açık bankacılık servislerine kıyasla pazar kaybı.
- **Manuel Süreç Bağımlılığı:** Üçüncü taraflarla veri paylaşımı bugün manuel, dosya bazlı veya ad-hoc entegrasyonlarla yapılmakta; bu da güvenlik açığı ve operasyonel yük yaratmaktadır.
- **Müşteri Rıza Yönetimi Eksikliği:** Müşterinin hangi veriyi, hangi süreyle, hangi üçüncü tarafla paylaştığına dair merkezi bir rıza (consent) yönetim mekanizması yoktur.

---

## 2. Business Need (İş İhtiyacı)

Banka, müşteri hesap bilgilerini (bakiye, işlem geçmişi, hesap detayları) standart, güvenli ve regülasyona uyumlu bir **Account Information Service (AIS)** API'si üzerinden, müşterinin açık rızasıyla, yetkilendirilmiş üçüncü taraf sağlayıcılara sunabilmelidir.

Bu ihtiyaç şu üç temel sürükleyiciden doğmaktadır:
1. **Regülasyon Zorunluluğu** — Açık Bankacılık mevzuatına uyum son tarihi yaklaşmaktadır.
2. **Stratejik Pazar Konumlandırma** — Banka, açık bankacılık ekosisteminde "API-first" bir oyuncu olarak konumlanmak istemektedir.
3. **Operasyonel Verimlilik** — Manuel veri paylaşım süreçlerinin otomasyonu ile operasyonel maliyetin azaltılması.

---

## 3. Business Goals (İş Hedefleri)

| # | Hedef | Açıklama |
|---|-------|----------|
| BG-1 | Regülasyon Uyumu | Açık Bankacılık / PSD2 benzeri yönetmeliklere tam uyumlu AIS API'si devreye almak |
| BG-2 | Güvenli Rıza Yönetimi | Müşterinin veri paylaşım rızasını uçtan uca yönetebileceği bir Consent Management mekanizması kurmak |
| BG-3 | Üçüncü Taraf Entegrasyonu | TPP'lerin standardize, dokümante edilmiş API'ler üzerinden hızlı entegre olabilmesini sağlamak |
| BG-4 | Operasyonel Risk Azaltma | Manuel/ad-hoc veri paylaşımını ortadan kaldırarak denetlenebilirlik ve izlenebilirlik sağlamak |
| BG-5 | Pazar Rekabetçiliği | Açık bankacılık ekosisteminde erken/güçlü konumlanma ile yeni iş ortaklıkları için altyapı oluşturmak |

---

## 4. Project Vision (Proje Vizyonu)

> Müşterilerin finansal verileri üzerinde tam kontrol sahibi olduğu; bu verileri yetkilendirdiği üçüncü taraf sağlayıcılarla güvenli, şeffaf ve regülasyona uyumlu şekilde paylaşabildiği; bankanın ise bu süreci uçtan uca denetleyebildiği, izleyebildiği ve yönetebildiği bir Açık Bankacılık platformu kurmak.

Vizyon, üç temel paydaş ekseninde tanımlanmıştır:
- **Müşteri için:** Kontrol, şeffaflık, kolay rıza yönetimi.
- **TPP için:** Standart, dokümante, kolay entegre olunabilir API.
- **Banka için:** Regülasyon uyumu, denetlenebilirlik, güvenlik, yeni iş modeli fırsatları.

---

## 5. Scope (Kapsam)

- **Account Information Service (AIS) API** tasarımı ve gereksinim analizi
- **Consent Management** süreci ve sistemi gereksinimleri
- **TPP Kimlik Doğrulama ve Yetkilendirme** (OAuth2.0 / eIDAS sertifika tabanlı yaklaşım)
- **Strong Customer Authentication (SCA)** süreç tasarımı
- **TPP Onboarding ve Sandbox** süreci
- **API Güvenlik ve Denetim (Audit Trail)** gereksinimleri
- **Müşteri İletişim/Bildirim** akışları
- İlgili BPMN süreç modelleri, BRD, SRS, User Story'ler

## 6. Out of Scope (Kapsam Dışı)

- Payment Initiation Service (PIS)
- Confirmation of Funds (CAF/PIIS) servisleri
- Bankanın çekirdek bankacılık sistemi (Core Banking) değişiklikleri
- Üçüncü taraf sağlayıcıların kendi iç sistemlerinin tasarımı
- Kredi, mevduat, yatırım ürünü satış süreçleri
- Mobil/internet bankacılığı UI/UX yeniden tasarımı
- Fiziksel şube süreçleri

---

## 7. Business Case

### 7.1 Beklenen Operasyonel Maliyet Tasarrufu

| Kalem | Mevcut Durum | Hedeflenen Durum | Tahmini Yıllık Tasarruf |
|-------|--------------|-------------------|--------------------------|
| Manuel veri paylaşım talepleri | Operasyon ekibi her talebi manuel işliyor (~30-45 dk/işlem) | Otomatik API çağrısı (saniyeler) | ~%90 işgücü tasarrufu |
| Uyumsuzluk/hata kaynaklı düzeltme maliyetleri | Manuel süreçte hata oranı yüksek | Standardize API ile hata oranı minimuma iner | Operasyonel hata maliyetlerinde azalma |
| TPP Onboarding süreci | Ad-hoc, haftalar sürüyor | Sandbox + standart API ile < 5 iş günü | Onboarding başına işgücü tasarrufu |
| Çağrı merkezi yükü | Manuel veri paylaşım talepleri | Self-servis rıza yönetimi | Çağrı merkezi maliyetinde azalma |

> Not: Bu tablo portföy projesi için **varsayımsal/illüstratif** maliyet kategorileridir.

### 7.2 Beklenen Gelir Fırsatları

- **Premium API Erişimi (Tiered API Monetization):** Yüksek hacimli TPP'lere kademeli ücretlendirme modeli
- **Banking-as-a-Service (BaaS) Genişlemesi:** PIS ve değer katan servislerle (analytics, kredi skorlama) genişleme imkânı
- **Yeni İş Ortaklıkları:** Fintech, muhasebe yazılımları, PFM uygulamalarıyla ortaklık geliri
- **Müşteri Sadakati ve Çapraz Satış:** Açık bankacılık deneyimi ile müşteri elde tutma ve çapraz satış fırsatları

### 7.3 Stratejik Faydalar

- Açık bankacılık ekosisteminde **erken hareket eden (first-mover)** konumlanma
- **API-first mimari** ile gelecekteki dijital ürün geliştirme hızının artması
- Düzenleyici otoritelerle **proaktif uyum ilişkisi**
- **Veri yönetişimi olgunluğu** artışı — gelecekteki AI/analitik girişimleri için temel

### 7.4 Regülasyon Uyumsuzluğunun Riskleri

| Risk Alanı | Potansiyel Sonuç |
|------------|-------------------|
| Yasal/İdari Para Cezası | Düzenleyici otorite tarafından uygulanacak yaptırımlar |
| Faaliyet Kısıtlaması | Belirli bankacılık hizmetlerinde geçici kısıtlama riski |
| İtibar Kaybı | Medya ve kamuoyunda güven kaybı |
| Rekabet Kaybı | Uyumlu rakiplerin pazar payını hızla artırması |
| Denetim Yükü Artışı | Düzenleyici otoritenin artan denetim sıklığı |

### 7.5 ROI Varsayımları (İllüstratif)

| Varsayım | Değer (Örnek) |
|----------|----------------|
| Geliştirme + Entegrasyon Maliyeti (1 kerelik) | X birim (proje bütçesi placeholder) |
| Yıllık İşletim/Bakım Maliyeti | Geliştirme maliyetinin ~%15-20'si |
| Operasyonel Tasarruf (Yıllık) | Bölüm 7.1'deki kalemlerin toplamı |
| Yeni Gelir (Yıl 2 itibariyle) | TPP partnerlik/API monetizasyon geliri |
| Geri Ödeme Süresi (Payback Period) | Tahmini 18-24 ay |
| ROI Hesaplama Mantığı | (Toplam Fayda − Toplam Maliyet) / Toplam Maliyet × 100 |

---

## 8. Success Metrics (Başarı Kriterleri)

| Metrik | Hedef Değer |
|--------|-------------|
| API Uptime (Erişilebilirlik) | %99.9 |
| Rıza Onay Süresi | < 2 dakika (uçtan uca) |
| TPP Entegrasyon Süresi | Sandbox üzerinden < 5 iş günü |
| Regülasyon Denetim Uyumu | %100 zorunlu kontrol maddesi karşılanması |
| Yetkisiz Erişim Vakası | 0 (sıfır tolerans) |
| Müşteri Rıza İptal Oranı (Revoke) | İzlenebilir ve < 24 saatte işlenebilir |
| API Hata Oranı (4xx/5xx) | < %1 |

### Ek Başarı KPI'ları

| Metrik | Hedef Değer |
|--------|-------------|
| TPP Memnuniyet Skoru (Developer Experience) | > 8/10 |
| Aktif TPP Sayısı (İlk 6 Ay) | ≥ 5 sertifikalı TPP |
| Rıza İptal/Erişim Talebi İşlem Süresi | < 24 saat |
| API Yanıt Süresi (Latency) | < 500ms (p95) |
| Operasyonel Maliyet Azalma Oranı | İlk yıl içinde ≥ %20 |
| Denetim Bulgusu Sayısı (Kritik) | 0 |

---

## 9. Süreç Akışı (Onaylanan Metodoloji)

```
Business Need
     ↓
As-Is Process Analysis
     ↓
Gap Analysis
     ↓
To-Be Process Design
     ↓
BRD (Business Requirements Document)
     ↓
SRS (Software Requirements Specification)
```

---

*Bu doküman, projenin Aşama 1 çalışmasının (Business Case, Goals, Vision, Scope, Success Metrics) konsolide edilmiş halidir.*
