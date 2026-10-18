# Gap Analysis
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

> Bu aşamada, [`as-is-process-analysis.md`](./as-is-process-analysis.md) ve [`root-cause-analysis.md`](./root-cause-analysis.md) referans alınarak, mevcut durum ile hedeflenen gelecek durumu (To-Be) arasındaki boşluklar sistematik olarak analiz edilmiştir.

---

## 1. Process Gaps (Süreç Boşlukları)

| Gap ID | Current State (As-Is) | Future State (To-Be) | Gap Description | Root Cause Reference | Business Impact | Priority |
|---|---|---|---|---|---|---|
| GAP-01 | TPP talepleri e-posta/form ile manuel alınıyor ve Excel'de takip ediliyor | TPP talepleri standart bir API/portal üzerinden dijital olarak alınıyor ve otomatik takip ediliyor | Talep alma ve takip sürecinde dijitalleşme eksikliği | P-01 | Süreç süresinin uzaması, ölçeklenemezlik | High |
| GAP-02 | TPP değerlendirme süreci standart kriterler olmadan, kişiye bağlı yapılıyor | Standart, dokümante edilmiş bir TPP değerlendirme/karar matrisi üzerinden otomatikleştirilmiş ön değerlendirme | Karar kriterlerinde standardizasyon eksikliği | P-05 | Tutarsız kararlar, denetim riski | Medium |
| GAP-03 | Sözleşme hazırlama süreci her TPP için özel olarak, hukuk departmanı tarafından manuel yürütülüyor | Standart TPP sözleşme şablonları ve otomatikleştirilmiş onay akışı | Şablonlaştırma ve süreç otomasyonu eksikliği | P-01 | Onboarding süresinin uzaması | Medium |
| GAP-04 | Süreç durumu uçtan uca tek bir sistemde izlenemiyor | Uçtan uca süreç tek bir workflow yönetim sistemi üzerinden izlenebiliyor | Merkezi süreç izleme/workflow yönetim sisteminin yokluğu | P-01, P-07 | Şeffaflık eksikliği, raporlama zorluğu | Medium |

## 2. Technology Gaps (Teknoloji Boşlukları)

| Gap ID | Current State (As-Is) | Future State (To-Be) | Gap Description | Root Cause Reference | Business Impact | Priority |
|---|---|---|---|---|---|---|
| GAP-05 | Core banking sistemine TPP'lerin doğrudan erişimi yok; veri manuel export ediliyor | Standart, dokümante edilmiş AIS API'si üzerinden gerçek zamanlı veri erişimi | API katmanı (API Gateway) eksikliği | P-01, P-04, P-09 | Gerçek zamanlı veri paylaşımının imkânsız olması | High |
| GAP-06 | Veri paylaşımı toplu (batch) dosya transferi (CSV/SFTP) ile yapılıyor | Veri paylaşımı alan bazlı, talep odaklı API çağrıları ile yapılıyor | Granüler veri paylaşım mekanizmasının yokluğu | P-03 | Gereğinden fazla veri paylaşımı riski | High |
| GAP-07 | TPP kimlik doğrulaması standart bir protokole (OAuth2.0/eIDAS) dayanmıyor | TPP'ler standart OAuth2.0 + sertifika tabanlı güçlü kimlik doğrulamasıyla yetkilendiriliyor | Standart yetkilendirme protokolü eksikliği | P-01, P-03 | Güvenlik açığı, regülasyon uyumsuzluğu | High |
| GAP-08 | TPP'lere yönelik API dokümantasyonu, sandbox ortamı veya developer portal bulunmuyor | Kapsamlı API dokümantasyonu ve sandbox test ortamı içeren bir developer portal | Developer deneyimi altyapısının yokluğu | P-07 | TPP onboarding süresinin uzaması | Medium |
| GAP-09 | Sistemler arası entegrasyon nokta-to-nokta yapılıyor | Merkezi bir API Gateway/entegrasyon katmanı üzerinden standardize entegrasyon | Entegrasyon mimarisinde merkezi katman eksikliği | P-01 | Bakım maliyeti, ölçeklenememe | Medium |

## 3. Security Gaps (Güvenlik Boşlukları)

| Gap ID | Current State (As-Is) | Future State (To-Be) | Gap Description | Root Cause Reference | Business Impact | Priority |
|---|---|---|---|---|---|---|
| GAP-10 | Veri transferi SFTP/dosya bazlı yapılıyor; uçtan uca şifreleme standardize değil | Veri transferi TLS ile şifrelenmiş API çağrıları üzerinden, RBAC ile yapılıyor | Standart veri güvenliği protokolünün yokluğu | P-03 | Veri sızıntısı riski | High |
| GAP-11 | Yetkisiz erişim/anormal işlem tespiti için otomatik bir izleme mekanizması yok | Gerçek zamanlı anomali tespiti ve otomatik alarm mekanizması | Güvenlik izleme/monitoring altyapısının eksikliği | P-10 | Yetkisiz erişim riskinin geç tespiti | High |
| GAP-12 | Audit trail parçalı şekilde farklı sistemlerde tutuluyor | Tüm işlemler merkezi, değiştirilemez (immutable) audit log sisteminde tutuluyor | Merkezi audit/logging sisteminin yokluğu | P-10 | Denetimde kanıt sunamama | High |

## 4. Compliance Gaps (Uyum Boşlukları)

| Gap ID | Current State (As-Is) | Future State (To-Be) | Gap Description | Root Cause Reference | Business Impact | Priority |
|---|---|---|---|---|---|---|
| GAP-13 | Müşteri rızası sözlü/kağıt bazlı alınıyor, hukuki ispatı zayıf | Müşteri rızası dijital, zaman damgalı ve değiştirilemez şekilde kayıt altına alınıyor | Hukuki geçerli rıza kaydı mekanizmasının yokluğu | P-02 | Rıza ispatı maddesinin karşılanamaması | High |
| GAP-14 | Rıza iptali manuel süreçle 3-5 iş günü sürüyor | Rıza iptali, müşteri talebiyle anlık olarak işleniyor | Otomatik rıza yaşam döngüsü yönetiminin yokluğu | P-08 | Anlık iptal hakkı ihlali | High |
| GAP-15 | TPP'lerin sertifikasyon durumu manuel, periyodik kontrol mekanizması yok | TPP sertifikasyon durumu otomatik doğrulanıyor ve sürekli izleniyor | Otomatik TPP sertifikasyon doğrulama mekanizmasının yokluğu | P-05 | Geçersiz TPP ile veri paylaşımı riski | Medium |
| GAP-16 | Regülasyon raporlama süreçleri manuel olarak hazırlanıyor | Regülasyon raporları merkezi sistem verilerinden otomatik/yarı otomatik üretiliyor | Otomatik regülasyon raporlama mekanizmasının yokluğu | P-10 | Raporlama süresinin uzaması, hata riski | Medium |

## 5. Customer Experience Gaps (Müşteri Deneyimi Boşlukları)

| Gap ID | Current State (As-Is) | Future State (To-Be) | Gap Description | Root Cause Reference | Business Impact | Priority |
|---|---|---|---|---|---|---|
| GAP-17 | Müşteri, paylaşılan veriyi göremiyor | Müşteri, mobil/internet bankacılığı üzerinden tüm aktif rızalarını görüntüleyebiliyor | Self-servis rıza yönetim arayüzünün yokluğu | P-06 | Şeffaflık eksikliği, güven kaybı | High |
| GAP-18 | Müşteri, rıza sürecinin sonucu hakkında bilgilendirilmiyor | Müşteri, her aşamada otomatik bildirim alıyor | Müşteri bildirim/iletişim mekanizmasının yokluğu | P-06 | Müşteri deneyiminde belirsizlik | Medium |
| GAP-19 | Müşterinin rıza verme süreci şube/çağrı merkezi ziyaretini gerektiriyor | Müşteri, dijital kanal üzerinden (SCA ile) birkaç dakikada rıza verebiliyor | Dijital self-servis rıza verme akışının yokluğu | P-02 | Müşteri sürtünmesi, düşük adopsiyon | High |

## 6. Operational Gaps (Operasyonel Boşluklar)

| Gap ID | Current State (As-Is) | Future State (To-Be) | Gap Description | Root Cause Reference | Business Impact | Priority |
|---|---|---|---|---|---|---|
| GAP-20 | Hiçbir süreç KPI'sı sistematik olarak ölçülmüyor | Tüm kritik KPI'lar otomatik olarak ölçülüyor ve raporlanıyor | KPI ölçüm ve raporlama altyapısının yokluğu | P-01, P-10 | Performans körlüğü | Medium |
| GAP-21 | IT ekibi manuel veri export işlemlerine zaman ayırıyor | IT ekibi yalnızca platform bakım/geliştirmesine odaklanıyor; veri export otomatik | Manuel operasyonel yükün IT kaynaklarını meşgul etmesi | P-09 | Verimsiz kaynak kullanımı | Medium |

---

## Gap Analysis - Özet Tablo

| Kategori | Toplam Gap Sayısı | High Priority | Medium Priority | Low Priority |
|---|:---:|:---:|:---:|:---:|
| Process Gaps | 4 | 1 | 3 | 0 |
| Technology Gaps | 5 | 3 | 2 | 0 |
| Security Gaps | 3 | 3 | 0 | 0 |
| Compliance Gaps | 4 | 2 | 2 | 0 |
| Customer Experience Gaps | 3 | 2 | 1 | 0 |
| Operational Gaps | 2 | 0 | 2 | 0 |
| **TOPLAM** | **21** | **11** | **10** | **0** |

> **BA Değerlendirmesi:** Security ve Compliance kategorilerindeki tüm gap'lerin "High Priority" olması, To-Be tasarımında bu iki alanın mimari önceliklendirmede en üst sırada yer alması gerektiğini gösterir. GAP-05, GAP-07, GAP-13 ve GAP-14, To-Be sürecinin çekirdek mimari bileşenlerini doğrudan tetikleyen kritik boşluklardır.

---

*Bu doküman, Aşama 3 (Gap Analysis) çalışmasının konsolide edilmiş halidir. To-Be Process Design için bkz. [`to-be-process-design.md`](./to-be-process-design.md).*
