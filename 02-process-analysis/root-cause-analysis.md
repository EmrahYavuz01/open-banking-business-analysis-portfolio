# Root Cause Analysis
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

> Bu doküman, [`as-is-process-analysis.md`](./as-is-process-analysis.md) içinde tespit edilen 10 probleme (P-01—P-10) yönelik kök neden analizini standalone bir referans dokümanı olarak sunar.

---

## Root Cause Analysis Tablosu

| Problem ID | Problem | Root Cause | Business Impact | Severity |
|---|---|---|---|---|
| P-01 | Standart bir API altyapısı olmadığından her TPP talebi manuel olarak işleniyor | Bankanın açık bankacılık stratejisi henüz API-first mimariye geçmemiş; tarihsel olarak entegrasyonlar nokta-to-nokta (point-to-point) ve özel (custom) olarak yapılandırılmış | Ölçeklenemeyen süreç, artan operasyonel maliyet, TPP sayısı arttıkça yönetilemez hale gelme | High |
| P-02 | Müşteri rızası dijital/merkezi olarak değil, telefon/şube üzerinden alınıyor | Merkezi bir Consent Management (Rıza Yönetimi) sistemi hiç kurulmamış; rıza süreci tarihsel olarak şube/çağrı merkezi operasyon süreçlerinin bir uzantısı olarak tasarlanmış | Hukuki ispat zafiyeti, regülasyon denetiminde "rıza şeffaflığı" maddesinin karşılanamaması, müşteri güven kaybı | High |
| P-03 | Veri paylaşımı CSV/Excel gibi dosya formatlarında, güvenli dosya transferi ile yapılıyor | API tabanlı, alan bazlı (field-level) veri paylaşım standardı tanımlanmamış; mevcut SFTP tabanlı entegrasyon eski (legacy) toplu veri transferi süreçlerinden miras kalmış | Veri sızıntısı/yetkisiz erişim riski, gereğinden fazla veri paylaşımı (over-sharing) riski | High |
| P-04 | Süreçte gerçek zamanlı veri paylaşımı mümkün değil | Core banking sistemi ile dış sistemler arasında gerçek zamanlı API katmanı (API Gateway) bulunmuyor; entegrasyonlar batch/manuel rapor mantığıyla tasarlanmış | TPP deneyiminde rekabet dezavantajı, açık bankacılık pazarında geri kalma riski | Medium |
| P-05 | Compliance ve InfoSec değerlendirmeleri kişiye bağlı manuel değerlendirme ile yapılıyor | TPP değerlendirme süreci için standardize bir kontrol listesi (checklist) veya karar matrisi hiç oluşturulmamış; değerlendirme kriterleri dokümante edilmemiş | Tutarsız onay/red kararları, değerlendirici bağımlılığı (key person risk), denetim sırasında karar gerekçesi sunamama | Medium |
| P-06 | Müşteri, rızasının nereye, ne kadar süreyle verildiği konusunda şeffaf bilgi alamıyor | Müşteriye yönelik bir rıza yönetim arayüzü (self-servis portal/uygulama ekranı) mevcut değil; rıza bilgisi yalnızca operasyon ekibinin iç kayıtlarında tutuluyor | Regülasyonun öngördüğü "şeffaflık ve kontrol" ilkesinin ihlali, müşteri şikayetlerinde artış riski | High |
| P-07 | Süreç durumu TPP tarafından izlenemiyor (status tracking yok) | TPP'lere yönelik bir developer portal veya self-servis durum izleme mekanizması kurulmamış; iletişim yalnızca e-posta üzerinden, talep bazlı yürütülüyor | TPP memnuniyetsizliği, ortaklık ilişkilerinde güven azalması, rekabetçi TPP'lerin alternatif bankalara yönelmesi | Medium |
| P-08 | Rıza iptal (revoke) mekanizması manuel ve yavaş işliyor | Rıza iptal talebi de dahil olmak üzere tüm rıza yaşam döngüsü (verme/güncelleme/iptal) manuel operasyon sürecine bağlı; otomatik tetiklenen bir iptal mekanizması yok | Regülasyonun zorunlu kıldığı "anlık iptal hakkı" ilkesinin karşılanamaması, uyumsuzluk cezası riski | High |
| P-09 | Veri export işlemleri IT ekibinin manuel müdahalesine bağlı, otomasyon yok | Core banking sistemi üzerinde TPP'lere yönelik standart bir sorgu/API servisi tanımlanmamış; her talep özel bir SQL sorgusu/rapor talebi olarak ele alınıyor | Operasyonel darboğaz, insan hatası riski (yanlış müşteri/hesap verisi paylaşımı), IT kaynak israfı | Medium |
| P-10 | Audit trail (denetim izi) parçalı şekilde farklı sistemlerde tutuluyor | Merkezi bir log/audit yönetim sistemi sürece entegre edilmemiş; kayıtlar e-posta, dosya sunucusu ve Excel gibi birbirinden bağımsız kaynaklarda tutuluyor | Düzenleyici denetimlerde kanıt toplama zorluğu, denetim süresinin uzaması, potansiyel uyumsuzluk bulgusu | High |

---

## Özet Değerlendirme

**Tekrarlayan Kök Nedenler (Pattern):**

1. **Sistemsel Otomasyon Eksikliği** — P-01, P-03, P-04, P-09: Bankanın API-first/otomasyon altyapısının olmaması, neredeyse tüm operasyonel problemlerin ortak kök nedeni.
2. **Merkezi Rıza/Veri Yönetimi Sisteminin Yokluğu** — P-02, P-06, P-08: Consent Management sisteminin hiç kurulmamış olması, regülasyon uyum risklerinin temel kaynağı.
3. **Standardizasyon ve Dokümantasyon Eksikliği** — P-05, P-07: Süreç ve karar kriterlerinin dokümante edilmemiş olması, tutarsızlık ve TPP deneyimi sorunlarının kaynağı.
4. **Merkezi İzlenebilirlik (Audit) Altyapısının Olmaması** — P-10: Denetim ve raporlama risklerinin temel nedeni.

> **BA Değerlendirmesi:** Bu dört tekrarlayan kök neden, [`to-be-process-design.md`](./to-be-process-design.md) içindeki **ana mimari bileşenlerini** (API Gateway, Consent Management Sistemi, Standart TPP Değerlendirme Çerçevesi, Merkezi Audit/Logging Sistemi) doğrudan şekillendirmiştir. Bu izlenebilirlik, [`gap-analysis.md`](./gap-analysis.md) çalışmasında referans alınmıştır.

---

*Bu doküman, Aşama 2 (Root Cause Analysis) çalışmasının konsolide edilmiş halidir.*
