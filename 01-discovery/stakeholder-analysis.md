# Stakeholder Analysis
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

---

## 1. Stakeholder Listesi (Paydaş Listesi)

| Paydaş | Rol/Departman | İlgi Alanı / Etki Düzeyi |
|--------|----------------|---------------------------|
| Genel Müdür Yardımcısı (Dijital Bankacılık) | Üst Yönetim - Sponsor | Stratejik karar verici, yüksek etki |
| Regülasyon ve Uyum Departmanı (Compliance) | İç Paydaş | Mevzuat uyumu onaylayıcısı, yüksek etki |
| Bilgi Güvenliği Departmanı (InfoSec) | İç Paydaş | Güvenlik standartları ve denetim, yüksek etki |
| IT Mimarisi / Enterprise Architecture | İç Paydaş | Teknik mimari onayı, yüksek etki |
| API Geliştirme Ekibi | Uygulama Ekibi | Teknik geliştirme, orta-yüksek etki |
| Hukuk Departmanı | İç Paydaş | Sözleşme ve veri sorumluluğu, orta etki |
| Müşteri Deneyimi (CX) Departmanı | İç Paydaş | Rıza akışı ve müşteri iletişimi, orta etki |
| Operasyon Departmanı | İç Paydaş | TPP onboarding süreç yönetimi, orta etki |
| Üçüncü Taraf Sağlayıcılar (TPP) | Dış Paydaş | API tüketicisi, orta-yüksek etki |
| Müşteriler (Hesap Sahipleri) | Dış Paydaş - Nihai Kullanıcı | Veri sahibi, çok yüksek etki |
| Düzenleyici Otorite (BDDK vb.) | Dış Paydaş - Denetleyici | Uyum denetimi, çok yüksek etki |
| Proje Yöneticisi / Product Owner | Proje Ekibi | Süreç yönetimi ve önceliklendirme |

---

## 2. Stakeholder Power / Interest Matrix

| Paydaş | Power (Güç) | Interest (İlgi) | Konumlandırma | Engagement Strategy |
|--------|:---:|:---:|---|---|
| Genel Müdür Yardımcısı (Dijital Bankacılık) | Yüksek | Yüksek | Manage Closely | Düzenli steering committee raporlaması, stratejik kararlarda doğrudan onay mekanizması |
| Düzenleyici Otorite (BDDK vb.) | Yüksek | Yüksek | Manage Closely | Proaktif uyum raporlaması, denetim öncesi ön bilgilendirme |
| Müşteriler (Hesap Sahipleri) | Düşük (bireysel) / Yüksek (kolektif) | Yüksek | Keep Informed / Manage Closely (kolektif) | Şeffaf iletişim, sade rıza ekranları, geri bildirim kanalları |
| Regülasyon ve Uyum Departmanı (Compliance) | Yüksek | Yüksek | Manage Closely | Her aşamada onay noktası, BRD/SRS review sürecine doğrudan dahil etme |
| Bilgi Güvenliği Departmanı (InfoSec) | Yüksek | Yüksek | Manage Closely | Güvenlik mimarisi tasarım toplantılarına erken dahil etme |
| IT Mimarisi / Enterprise Architecture | Yüksek | Orta | Keep Satisfied | Mimari kararlarda onay alma, teknik tasarım dokümanlarının paylaşımı |
| API Geliştirme Ekibi | Orta | Yüksek | Keep Informed / Manage Closely | Sprint planlama ve backlog refinement toplantılarına aktif katılım |
| Hukuk Departmanı | Orta | Orta | Keep Satisfied | TPP sözleşme şablonları üzerinde dönemsel review |
| Müşteri Deneyimi (CX) Departmanı | Orta | Yüksek | Manage Closely | Rıza akışı tasarımında ortak çalışma |
| Operasyon Departmanı | Düşük | Orta | Keep Informed | TPP onboarding süreç değişikliklerinde düzenli bilgilendirme |
| Üçüncü Taraf Sağlayıcılar (TPP) | Düşük (bireysel) / Orta (kolektif) | Yüksek | Keep Informed | Developer portal, sandbox erişimi, API dokümantasyonu |
| Proje Yöneticisi / Product Owner | Yüksek | Yüksek | Manage Closely | Günlük/haftalık proje takibi, risk ve kapsam yönetimi |

### Power/Interest Matrisi (Görsel Konumlandırma)

```
                    YÜKSEK İLGİ (Interest)
                            │
   Keep Satisfied           │           Manage Closely
   ─────────────────────────┼─────────────────────────────
   • IT Mimarisi             │  • GM Yrd. (Dijital Bankacılık)
   • Hukuk Departmanı         │  • Düzenleyici Otorite
                            │  • Compliance
YÜKSEK ─────────────────────┼───────────────────────────── YÜKSEK
GÜÇ                         │  • InfoSec                    İLGİ
                            │  • CX Departmanı
                            │  • Proje Yöneticisi/PO
   ─────────────────────────┼─────────────────────────────
   Monitor (Minimum Effort)  │           Keep Informed
                            │
                            │  • Operasyon Departmanı
                            │  • TPP'ler (kolektif)
                            │  • API Geliştirme Ekibi
                            │  • Müşteriler (bireysel ölçekte)
                            │
                     DÜŞÜK GÜÇ (Power)
```

> Müşteriler bireysel bazda düşük güce sahip olsa da, kolektif olarak (itibar riski, kamuoyu algısı üzerinden) yüksek etki yaratabileceğinden "Manage Closely" yaklaşımıyla da değerlendirilmelidir.

---

## 3. RACI Matrix

**Lejant:** R = Responsible | A = Accountable | C = Consulted | I = Informed

| Aktivite | Business Analyst | Product Owner / PM | Compliance | InfoSec | IT Architecture | Geliştirme Ekibi | Hukuk | CX Departmanı | Operasyon | GM Yardımcısı (Sponsor) |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Business Case Hazırlama | R | A | C | I | I | I | C | I | I | C |
| Stakeholder Analizi | R | A | C | C | I | I | I | C | I | I |
| As-Is Süreç Analizi | R | A | C | C | C | C | I | C | C | I |
| Gap Analysis | R | A | C | C | C | C | I | C | I | I |
| To-Be Süreç Tasarımı | R | A | C | C | C | C | I | C | C | I |
| BPMN Süreç Modelleme | R | A | I | I | C | C | I | I | I | I |
| BRD Hazırlama | R | A | C | C | C | C | C | C | I | I |
| SRS Hazırlama | R/C | A | C | C | R | R | I | I | I | I |
| Regülasyon Uyum Kontrolü | C | I | A/R | C | I | I | C | I | I | I |
| Güvenlik Mimarisi Tasarımı | C | I | C | A/R | C | C | I | I | I | I |
| API Tasarımı (Teknik Spesifikasyon) | C | I | C | C | A | R | I | I | I | I |
| Consent (Rıza) Akışı Tasarımı | R | A | C | C | C | C | C | R | I | I |
| TPP Onboarding Süreci Tasarımı | R | A | C | C | I | C | C | I | R | I |
| Sözleşme/Hukuki Şartların Belirlenmesi | C | I | C | I | I | I | A/R | I | C | I |
| Risk Kayıt Defteri Yönetimi | R | A | C | C | I | I | I | I | I | I |
| Test Senaryoları (UAT) Hazırlama | R | A | C | C | I | C | I | C | C | I |
| UAT Yürütme | C | A | C | C | I | C | I | C | R | I |
| Sandbox / Developer Portal Yayınlama | C | A | I | C | C | R | I | I | I | I |
| Go-Live Onayı | I | A | R | R | C | C | C | I | I | A |
| Proje İlerleme Raporlama | R | A | I | I | I | I | I | I | I | I |
| Düzenleyici Otoriteyle İletişim | I | C | A/R | C | I | I | C | I | I | I |

> **Not (BA Perspektifinden):** Business Analyst rolü, projede ağırlıklı olarak **Responsible (R)** pozisyonunda yer alır — özellikle gereksinim toplama, süreç analizi ve dokümantasyon aktivitelerinde. Teknik güvenlik ve mimari kararlarda ise **Consulted (C)** rolündedir.

---

*Bu doküman, Aşama 1 kapsamında üretilen Stakeholder Listesi, Power/Interest Matrix ve RACI Matrix çalışmalarının konsolide edilmiş halidir.*
