# Requirements Prioritization Matrix
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

---

## Bölüm A: Functional Requirements (FR) Sınıflandırması

| Req ID | Requirement Name | Release Category | Business Value | Complexity | Justification |
|---|---|---|---|---|---|
| FR-01 | Rıza Oluşturma | MVP | High | Medium | Tüm açık bankacılık akışının temel ön koşulu |
| FR-02 | Rıza Görüntüleme | MVP | High | Low | Regülasyonun zorunlu kıldığı şeffaflık ilkesi |
| FR-03 | Rıza İptali | MVP | High | Medium | "Anlık iptal hakkı"; token geçersiz kılma mekanizması |
| FR-04 | Rıza Süre Yönetimi | MVP | High | Medium | Kontrolsüz veri paylaşımını önleyen kritik kontrol |
| FR-05 | Rıza Bildirimleri | Release 1 | Medium | Low | MVP'de dashboard üzerinden görülebilir |
| FR-06 | Rıza Geçmişi Raporlama | Release 1 | Medium | Low | MVP'de temel audit log sorgusu ile karşılanabilir |
| FR-07 | TPP Self-Servis Kayıt | MVP | High | Medium | Manuel onboarding'i ortadan kaldıran temel otomasyon |
| FR-08 | Sertifika Yükleme | MVP | High | Low | TPP doğrulamasının ön koşulu |
| FR-09 | Otomatik Sertifikasyon Doğrulama | MVP | High | High | Güvenlik açısından kritik, teknik karmaşıklığı yüksek |
| FR-10 | Sandbox Erişimi | MVP | High | Medium | Production öncesi güvenli test, risk azaltımı |
| FR-11 | Manuel İnceleme Kuyruğu | Release 1 | Medium | Low | MVP'de manuel süreçle geçici karşılanabilir |
| FR-12 | Periyodik Sertifikasyon Kontrolü | MVP | High | Medium | Süresi dolmuş sertifikalarla erişimi önlemek zorunludur |
| FR-13 | Kapsam Bazlı Veri Erişimi | MVP | High | High | Veri minimizasyonu ilkesinin teknik uygulaması |
| FR-14 | Gerçek Zamanlı Veri Sorgulama | MVP | High | High | Projenin temel değer önermesi |
| FR-15 | Rıza Doğrulamasız Erişim Engelleme | MVP | High | Medium | Temel güvenlik kontrolü, regülasyon uyumu |
| FR-16 | Hizmet Sürekliliği | Release 1 | Medium | Medium | MVP'de temel SLA ile başlanabilir |
| FR-17 | TPP Kimlik Doğrulama Kontrolü | MVP | High | High | Sistemin güvenlik temeli |
| FR-18 | Müşteri SCA Doğrulaması | MVP | High | High | Regülasyon zorunluluğu |
| FR-19 | Anomali Tespiti ve Bildirimi | Release 1 | Medium | High | MVP'de temel rate limiting yeterli |
| FR-20 | Şifreli Veri Transferi | MVP | High | Low | Standart TLS uygulaması |
| FR-21 | Değiştirilemez İşlem Kaydı | MVP | High | Medium | Denetim ve hukuki ispat için zorunlu |
| FR-22 | Otomatik Regülasyon Raporu Üretimi | MVP | High | Medium | Denetim hazırlığı için kritik |
| FR-23 | KPI Dashboard | Release 1 | Medium | Medium | Performans yönetimi, MVP launch'ını engellemez |
| FR-24 | TPP Kullanım İstatistikleri | Future Release | Low | Low | "Nice-to-have", iş kritikliği düşük |

---

## Bölüm B: Non-Functional Requirements (NFR) Sınıflandırması

| Req ID | Requirement Name | Release Category | Business Value | Complexity | Justification |
|---|---|---|---|---|---|
| NFR-01 | Availability (%99.9 Uptime) | MVP | High | High | TPP güveni ve regülasyon beklentisi |
| NFR-02 | Planlı Bakım Bildirimi | Release 1 | Medium | Low | MVP'de manuel bildirimle karşılanabilir |
| NFR-03 | API Yanıt Süresi (p95 < 500ms) | MVP | High | High | Temel değer önermesi: gerçek zamanlılık |
| NFR-04 | Rıza Onay Süresi (< 2 dk) | MVP | High | Medium | Müşteri deneyimi ve adopsiyon için kritik |
| NFR-05 | Ölçeklenebilirlik (TPP artışı) | Release 1 | Medium | High | MVP'de sınırlı TPP sayısıyla başlanabilir |
| NFR-06 | Eşzamanlı İstek Kapasitesi (1000 req/sn) | Release 1 | Medium | High | Düşük kapasite ile başlanıp aşamalı artırılabilir |
| NFR-07 | Otomatik Failover | Release 1 | Medium | High | MVP'de temel redundancy yeterli olabilir |
| NFR-08 | Hata Oranı (< %1) | MVP | High | Medium | Temel kalite ve güvenilirlik beklentisi |
| NFR-09 | API Versiyonlama | Release 1 | Medium | Medium | MVP'de tek versiyon yeterlidir |
| NFR-10 | Yeni Servis Tipi Genişletilebilirliği (PIS vb.) | Future Release | Medium | High | Gelecekteki servisleri ilgilendirir |
| NFR-11 | Audit Kaydı Saklama (≥7 yıl) | MVP | High | Medium | Regülasyon zorunluluğu |
| NFR-12 | Audit Kaydı Değiştirilemezliği | MVP | High | Medium | Denetim güvenilirliği için temel gereklilik |

---

## Bölüm C: Security Requirements (SEC) Sınıflandırması

| Req ID | Requirement Name | Release Category | Business Value | Complexity | Justification |
|---|---|---|---|---|---|
| SEC-01 | OAuth2 Authorization | MVP | High | High | Yetkilendirme mimarisinin temeli |
| SEC-02 | Strong Customer Authentication (SCA) | MVP | High | High | Regülasyon zorunluluğu |
| SEC-03 | TLS Encryption | MVP | High | Low | Endüstri standart, temel gereklilik |
| SEC-04 | Role Based Access Control (RBAC) | MVP | High | Medium | İç kullanıcı/sistem rollerinin güvenliği |
| SEC-05 | Rate Limiting | MVP | High | Medium | Temel DDoS/aşırı kullanım koruması |
| SEC-06 | Audit Logging | MVP | High | Medium | Denetim ve regülasyon uyumunun temel taşı |
| SEC-07 | Certificate Validation | MVP | High | High | TPP güvenliğinin temel kontrolü |
| SEC-08 | Session & Token Management | MVP | High | Medium | Token yaşam döngüsü kontrolü |

---

## 1. MVP Scope Summary

**MVP Kapsamındaki Gereksinimler:**
- **FR:** FR-01, FR-02, FR-03, FR-04, FR-07, FR-08, FR-09, FR-10, FR-12, FR-13, FR-14, FR-15, FR-17, FR-18, FR-20, FR-21, FR-22 (17 adet)
- **NFR:** NFR-01, NFR-03, NFR-04, NFR-08, NFR-11, NFR-12 (6 adet)
- **SEC:** SEC-01—SEC-08 (8 adet — tüm güvenlik gereksinimleri)

MVP, **"regülasyon uyumu olmadan hiçbir versiyon canlıya çıkamaz"** prensibiyle belirlenmiştir. Tüm Consent Management çekirdek işlevleri, tüm Security Requirements, TPP onboarding'in çekirdek akışı ve gerçek zamanlı veri erişimi MVP'dedir.

> **PO Perspektifi:** MVP, "küçük ama eksiksiz" bir platform olmalıdır — güvenlik ve regülasyon uyumu açısından tam donanımlı.

---

## 2. Release 1 Scope Summary

**Release 1 Kapsamındaki Gereksinimler:**
- **FR:** FR-05, FR-06, FR-11, FR-16, FR-19, FR-23 (6 adet)
- **NFR:** NFR-02, NFR-05, NFR-06, NFR-07, NFR-09 (5 adet)

Release 1, MVP'nin **operasyonel olgunlaşması ve kullanıcı deneyiminin iyileştirilmesi** aşamasıdır. Bildirim mekanizmaları, gelişmiş anomali tespiti, ölçeklenebilirlik ve otomatik failover bu aşamada ele alınır.

> **PO Perspektifi:** Release 1, "MVP'nin gerçek kullanım verisiyle olgunlaştırılması" mantığıyla önceliklendirilmiştir.

---

## 3. Future Release Opportunities

**Future Release Kapsamındaki Gereksinimler:**
- **FR:** FR-24 (TPP Kullanım İstatistikleri)
- **NFR:** NFR-10 (Yeni Servis Tipi Genişletilebilirliği)

**Gelecekte Eklenebilecek Geliştirmeler:**
- TPP Analitik Paneli (Gelişmiş)
- Payment Initiation Service (PIS) Entegrasyonu
- Confirmation of Funds (CAF) Servisi
- AI Destekli Anomali Tespiti
- Çoklu Bölge (Multi-Region) Ölçeklenebilirlik

Bu fırsatlar, platformun tek seferlik bir regülasyon uyum projesi olmaktan çıkıp, sürekli gelişen bir Banking-as-a-Service (BaaS) platformuna dönüşmesini sağlar.

---

## 4. Prioritization Analysis

### 4.1 Business Value vs Complexity

```
                    YÜKSEK İŞ DEĞERİ (Business Value)
                            │
   Strategic Investments     │      Quick Wins
   ───────────────────────────┼─────────────────────────────────────
   • FR-09, FR-13, FR-14      │  • FR-02, FR-08, FR-20, SEC-03
YÜKSEK ──────────────────────┼───────────────────────────────────── DÜŞÜK
KARMAŞIKLIK • FR-17, FR-18,   │                                       KARMAŞIKLIK
   SEC-01, SEC-02, SEC-07     │
   ───────────────────────────┼─────────────────────────────────────
   Long-Term Enhancements    │      
   • NFR-10, NFR-07           │  • FR-24
                            │
                     DÜŞÜK İŞ DEĞERİ
```

### 4.2 Quick Wins (Yüksek Değer / Düşük-Orta Karmaşıklık)
FR-02 (Rıza Görüntüleme), FR-08 (Sertifika Yükleme), FR-20/SEC-03 (TLS) — erken sprint'lere alınmalı.

### 4.3 Strategic Investments (Yüksek Değer / Yüksek Karmaşıklık)
FR-13, FR-14 (Kapsam Bazlı + Gerçek Zamanlı Erişim), FR-09, SEC-01/SEC-02/SEC-07 — yeterli süre ve uzman kaynak planlanmalı.

### 4.4 Long-Term Enhancements
NFR-10 (Genişletilebilirlik), NFR-07 (Failover), FR-24 — roadmap'te "gelecek çeyrekler" olarak işaretlenmeli.

---

## Özet Tablo: Release Dağılımı

| Kategori | FR | NFR | SEC | Toplam |
|---|:---:|:---:|:---:|:---:|
| MVP | 17 | 6 | 8 | 31 |
| Release 1 | 6 | 5 | 0 | 11 |
| Future Release | 1 | 1 | 0 | 2 |
| **TOPLAM** | **24** | **12** | **8** | **44** |

> **PO/BA Genel Değerlendirmesi:** Toplam 44 gereksinimin %70'i (31 adet) MVP kapsamına alınmıştır — bankacılıkta "güvenlik ve regülasyon uyumu MVP'den itibaren tam olmalıdır" prensibinin doğal sonucudur.

---

*Bu doküman, Aşama 7.1 (Requirements Prioritization Matrix) çalışmasının konsolide edilmiş halidir.*
