# Acceptance Criteria
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

**Format:** Given / When / Then (Gherkin Stili)
> User Story tanımları için bkz. [`epic-and-user-story-model.md`](./epic-and-user-story-model.md)

---

## EPIC-01: Consent Management

### US-01 — Müşteri, TPP'nin erişim talebini onaylar/reddeder

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-01.1 | Müşteri, geçerli bir TPP'den gelen rıza talep ekranını görüntülüyorken | müşteri talebi "Onayla" seçeneğiyle onaylarsa | sistem rızayı aktif olarak kaydeder ve TPP'ye onay bildirimi gönderir |
| AC-01.2 | Müşteri, geçerli bir TPP'den gelen rıza talep ekranını görüntülüyorken | müşteri talebi "Reddet" seçeneğiyle reddederse | sistem rızayı oluşturmaz ve TPP'ye red bildirimi gönderir |
| AC-01.3 (Negatif) | Müşteri rıza onay ekranındayken | müşteri 5 dakika içinde herhangi bir işlem yapmazsa (zaman aşımı) | sistem talebi otomatik olarak iptal eder ve TPP'ye "zaman aşımı" bildirimi gönderir |
| AC-01.4 (Negatif) | Müşteri SCA doğrulamasını tamamlamamışken | müşteri rıza onayı vermeye çalışırsa | sistem onayı kabul etmez ve müşteriden önce kimlik doğrulamasını tamamlamasını ister |

### US-02 — Müşteri, verdiği rızaları görüntüler

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-02.1 | Müşteri, mobil/internet bankacılığında oturum açmışken | müşteri "Aktif Rızalarım" ekranına giderse | sistem tüm aktif rızaları (TPP adı, veri kapsamı, başlangıç/bitiş tarihi) listeler |
| AC-02.2 | Müşterinin hiçbir aktif rızası yokken | müşteri "Aktif Rızalarım" ekranına giderse | sistem "Aktif rızanız bulunmamaktadır" mesajını gösterir |
| AC-02.3 (Negatif) | Müşteri oturum açmamışken | müşteri rıza ekranına erişmeye çalışırsa | sistem erişimi reddeder ve kimlik doğrulama ekranına yönlendirir |
| AC-02.4 | Müşteri "Aktif Rızalarım" ekranındayken | müşteri süresi dolmuş bir rızayı görüntülemek isterse | sistem geçmiş rızaları "Süresi Dolmuş" etiketiyle ayrı bir listede gösterir |

### US-03 — Müşteri, rızasını iptal eder

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-03.1 | Müşteri, aktif bir rızayı görüntülüyorken | müşteri "İptal Et" butonuna basıp onaylarsa | sistem rızayı anında pasif hale getirir ve ilgili TPP'nin erişim token'ını geçersiz kılar |
| AC-03.2 | Müşteri rıza iptal işlemini tamamladığında | sistem işlemi kaydederken | audit log'a iptal zaman damgası ve müşteri kimliği ile birlikte kaydedilir |
| AC-03.3 (Negatif) | Müşteri, daha önce zaten iptal edilmiş bir rızayı tekrar iptal etmeye çalışırsa | sistem bu işlemi algıladığında | sistem "Bu rıza zaten iptal edilmiştir" mesajını gösterir ve tekrar işlem yapmaz |
| AC-03.4 | Müşteri bir rızayı iptal ettiğinde | ilgili TPP aynı rızayla yeni bir API çağrısı yaparsa | API Gateway isteği reddeder ve "rıza geçersiz" hata kodu döner |

### US-04 — Rıza süresi dolduğunda otomatik erişim kesilir

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-04.1 | Bir rızanın süresi (örn. 90 gün) dolmak üzereyken | belirlenen süre sona erdiğinde | sistem rızayı otomatik olarak "süresi dolmuş" durumuna alır |
| AC-04.2 | Bir rızanın süresi dolduğunda | ilgili TPP aynı token ile API çağrısı yaparsa | API Gateway isteği reddeder ve "rıza süresi dolmuş" hata kodu döner |
| AC-04.3 (Negatif) | Bir rızanın süresi dolmadan TPP erişim talep ederse | sistem rıza durumunu kontrol ettiğinde | hâlâ aktif olduğu için erişime izin verilir (yanlış pozitif red oluşmaz) |
| AC-04.4 | Rıza süresinin dolmasına 7 gün kala | sistem periyodik kontrolü çalıştırdığında | müşteriye "rıza süreniz yakında dolacak" bildirimi gönderilir |

### US-05 — Müşteri rıza süreci bildirimlerini alır

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-05.1 | Bir TPP rıza talebinde bulunduğunda | sistem talebi aldığında | müşteriye anlık bildirim (push/SMS/uygulama içi) gönderilir |
| AC-05.2 | Müşteri rıza onayını tamamladığında | sistem onayı işlediğinde | müşteriye "rızanız onaylandı" bildirimi gönderilir |
| AC-05.3 (Negatif) | Müşterinin bildirim tercihi kapalıyken (opt-out) | rıza süreci tetiklendiğinde | sistem bildirim göndermez ancak işlemi normal şekilde işler ve loglar |
| AC-05.4 | Müşteri bir rızasını iptal ettiğinde | sistem iptali işlediğinde | müşteriye "rızanız iptal edildi" bildirimi gönderilir |

### US-06 — Compliance, rıza geçmişini görüntüler

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-06.1 | Compliance kullanıcısı yetkili rolle oturum açmışken | belirli bir müşterinin rıza geçmişini sorgularsa | sistem tüm geçmiş rızaları (verilen, iptal edilen, süresi dolan) zaman sırasıyla listeler |
| AC-06.2 | Compliance kullanıcısı rıza geçmişi raporunu talep ettiğinde | sistem raporu oluştururken | rapor, denetim için gerekli tüm alanları (zaman damgası, TPP, kapsam, durum) içerir |
| AC-06.3 (Negatif) | Yetkisiz bir kullanıcı (Compliance rolüne sahip olmayan) | rıza geçmişi ekranına erişmeye çalışırsa | sistem erişimi reddeder ve "yetkisiz erişim" olayını audit log'a kaydeder |

---

## EPIC-02: TPP Onboarding & Yetkilendirme

### US-07 — TPP self-servis kayıt olur

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-07.1 | Bir TPP, Developer Portal kayıt formunu dolduruyorken | tüm zorunlu alanları eksiksiz girip gönderirse | sistem kaydı oluşturur ve TPP'ye onay e-postası gönderir |
| AC-07.2 (Negatif) | Bir TPP kayıt formunu doldururken | zorunlu bir alanı (örn. sertifika) boş bırakırsa | sistem kaydı reddeder ve eksik alanı belirten hata mesajı gösterir |
| AC-07.3 (Negatif) | Bir TPP, daha önce kayıtlı bir e-posta/şirket bilgisiyle tekrar kayıt olmaya çalışırsa | sistem mükerrer kaydı tespit ettiğinde | sistem kaydı reddeder ve "bu TPP zaten kayıtlı" mesajı gösterir |
| AC-07.4 | Bir TPP kayıt işlemini başarıyla tamamladığında | sistem kaydı oluşturduğunda | TPP'nin durumu "Beklemede - Doğrulama Sürüyor" olarak işaretlenir |

### US-08 — TPP sertifika/lisans yükler

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-08.1 | TPP, kayıt sürecinde sertifika dosyasını yüklerken | sertifika geçerli formatta ve süresi dolmamışsa | sistem sertifikayı kabul eder ve doğrulama sürecine alır |
| AC-08.2 (Negatif) | TPP, süresi dolmuş bir sertifika yüklerse | sistem doğrulama yaptığında | sistem yüklemeyi reddeder ve "sertifika süresi dolmuş" hatası gösterir |
| AC-08.3 (Negatif) | TPP, desteklenmeyen bir dosya formatında sertifika yüklerse | sistem dosyayı işlemeye çalıştığında | sistem yüklemeyi reddeder ve desteklenen formatları belirtir |
| AC-08.4 | TPP sertifikası başarıyla yüklendiğinde | sistem doğrulamayı tamamladığında | TPP'nin onboarding durumu bir sonraki aşamaya (Compliance kontrolü) ilerler |

### US-09 — Compliance, sertifikasyonu otomatik doğrular

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-09.1 | Bir TPP sertifikası yüklendiğinde | sistem otomatik doğrulama sürecini çalıştırdığında | geçerli sertifikalar otomatik olarak "Doğrulandı" durumuna geçer |
| AC-09.2 (Negatif) | Bir TPP sertifikası geçersiz/sahte olarak tespit edildiğinde | sistem doğrulamayı tamamladığında | TPP kaydı otomatik olarak reddedilir ve Compliance ekibine bildirim gönderilir |
| AC-09.3 | Otomatik doğrulama sonucu belirsiz/sınırda olduğunda | sistem sonucu işlediğinde | kayıt manuel inceleme kuyruğuna otomatik olarak yönlendirilir |

### US-10 — TPP, Sandbox'ta test eder

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-10.1 | TPP onboarding sürecinin doğrulama aşamasını geçtiğinde | TPP Sandbox erişim talebinde bulunduğunda | sistem TPP'ye test (mock) verilerle sandbox erişimi sağlar |
| AC-10.2 | TPP, sandbox ortamında API çağrısı yaptığında | istek doğru formatta gönderildiğinde | sistem örnek (mock) hesap verisini başarılı yanıt olarak döner |
| AC-10.3 (Negatif) | TPP, sandbox ortamında gerçek müşteri verisine erişmeye çalıştığında | sistem isteği aldığında | sistem isteği reddeder; sandbox yalnızca mock veriyle çalışır |
| AC-10.4 (Negatif) | TPP, onboarding doğrulaması tamamlanmadan sandbox erişimi talep ederse | sistem talebi kontrol ettiğinde | sistem erişimi reddeder ve "doğrulama tamamlanmadı" mesajı gösterir |

### US-11 — Compliance, istisna durumları manuel inceler

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-11.1 | Bir TPP başvurusu otomatik kontrolde "şüpheli" olarak işaretlendiğinde | Compliance uzmanı kaydı incelediğinde | sistem ilgili tüm dokümanları ve doğrulama sonuçlarını inceleme ekranında gösterir |
| AC-11.2 | Compliance uzmanı manuel incelemeyi tamamladığında | "Onayla" kararını verirse | TPP'nin durumu "Onaylandı" olarak güncellenir ve süreç devam eder |
| AC-11.3 (Negatif) | Compliance uzmanı manuel incelemeyi tamamladığında | "Reddet" kararını verirse | TPP kaydı kalıcı olarak reddedilir ve TPP'ye gerekçeli red bildirimi gönderilir |

### US-12 — Sertifikasyon periyodik yeniden doğrulanır

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-12.1 | Bir TPP'nin sertifikası belirlenen yenileme tarihine yaklaştığında | sistem periyodik kontrolü çalıştırdığında | TPP'ye sertifika yenileme hatırlatması gönderilir |
| AC-12.2 (Negatif) | Bir TPP'nin sertifikası süresi dolmasına rağmen yenilenmediğinde | sistem kontrolü çalıştırdığında | TPP'nin production erişimi otomatik olarak askıya alınır |
| AC-12.3 | Askıya alınmış bir TPP, geçerli yeni bir sertifika yüklediğinde | sistem doğrulamayı tamamladığında | TPP'nin erişimi otomatik olarak yeniden aktive edilir |

---

## EPIC-03: Hesap Bilgisi Erişimi (AIS)

### US-13 — TPP, onaylı kapsamda hesap bilgisine erişir

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-13.1 | TPP, geçerli bir consent token ile API çağrısı yaptığında | token'ın kapsamı "bakiye görüntüleme" içeriyorsa | sistem yalnızca bakiye bilgisini döner |
| AC-13.2 (Negatif) | TPP, yalnızca "bakiye görüntüleme" kapsamına sahip token ile işlem geçmişi talep ettiğinde | sistem kapsamı kontrol ettiğinde | sistem isteği reddeder ve "yetersiz kapsam" hata kodu döner |
| AC-13.3 (Negatif) | TPP, geçersiz veya süresi dolmuş bir token ile istek gönderdiğinde | API Gateway token'ı doğruladığında | sistem isteği reddeder ve 401 yetkisiz erişim hatası döner |

### US-14 — TPP, gerçek zamanlı yanıt alır

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-14.1 | TPP geçerli bir istek gönderdiğinde | Core Banking sistemi normal çalışır durumdayken | sistem 500ms (p95) içinde yanıt döner |
| AC-14.2 (Negatif) | Core Banking sistemi geçici olarak erişilemez durumdayken | TPP istek gönderdiğinde | sistem anlamlı bir hata mesajı döner, sistem çökmez |
| AC-14.3 | Yüksek trafik altında çoklu TPP istekleri eş zamanlı geldiğinde | sistem isteği işlediğinde | her istek tanımlanan SLA içinde, performans bozulması olmadan yanıtlanır |

### US-15 — Geçersiz istekler otomatik reddedilir

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-15.1 | Bir TPP, rıza/token olmadan doğrudan API'ye istek gönderdiğinde | API Gateway isteği aldığında | sistem isteği reddeder ve 401 hata kodu döner |
| AC-15.2 | Bir TPP, iptal edilmiş bir rızaya ait token ile istek gönderdiğinde | API Gateway token durumunu kontrol ettiğinde | sistem isteği reddeder ve "rıza iptal edilmiş" hatası döner |
| AC-15.3 (Negatif) | Geçerli bir token ile gönderilen istekte | tüm doğrulamalar başarılı olduğunda | sistem isteği normal şekilde işler (yanlış red oluşmaz) |

### US-16 — Platform yüksek erişilebilirlikte çalışır

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-16.1 | Sistem normal operasyon koşullarında çalışırken | aylık erişilebilirlik ölçümü yapıldığında | uptime oranı %99.9 veya üzerinde gerçekleşir |
| AC-16.2 (Negatif) | Bir bileşen arızalandığında | sistem otomatik failover mekanizmasını tetiklediğinde | hizmet kesintisi olmadan trafiğin yedek bileşene yönlendirildiği doğrulanır |
| AC-16.3 | Planlı bakım gerçekleştirileceğinde | bakım penceresi başlamadan önce | TPP'lere önceden bildirim yapılır |

---

## EPIC-04: Güvenlik ve Erişim Kontrolü

### US-17 — TPP istekleri güçlü kimlik doğrulama ile doğrulanır

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-17.1 | TPP geçerli sertifika ile API isteği gönderdiğinde | sistem kimlik doğrulamasını yaptığında | istek kabul edilir ve işleme devam eder |
| AC-17.2 (Negatif) | TPP geçersiz veya sahte sertifika ile istek gönderdiğinde | sistem kimlik doğrulamasını yaptığında | istek reddedilir ve güvenlik olayı audit log'a kaydedilir |
| AC-17.3 (Negatif) | Sertifikası askıya alınmış bir TPP istek gönderdiğinde | sistem kontrolü yaptığında | istek reddedilir ve "erişim askıya alınmış" hatası döner |

### US-18 — Müşteri rıza onayını SCA ile verir

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-18.1 | Müşteri rıza onay ekranındayken | müşteri biyometrik/OTP doğrulamasını başarıyla tamamladığında | sistem rıza onayını kabul eder |
| AC-18.2 (Negatif) | Müşteri SCA doğrulamasında 3 kez başarısız olduğunda | sistem başarısız denemeleri tespit ettiğinde | sistem hesabı geçici olarak kilitler ve güvenlik ekibine bildirim gönderir |
| AC-18.3 (Negatif) | Müşteri SCA adımını atlamaya çalıştığında | sistem isteği kontrol ettiğinde | sistem işlemi reddeder, SCA zorunlu adım olarak kalır |

### US-19 — Anormal erişim paternleri tespit edilir

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-19.1 | Bir TPP, tanımlanan rate limit'in üzerinde istek gönderdiğinde | sistem trafiği izlediğinde | sistem fazla istekleri otomatik olarak engeller (throttling) ve InfoSec'e bildirim gönderir |
| AC-19.2 | Bir hesaba kısa sürede olağandışı sayıda erişim talebi geldiğinde | anomali tespit motoru bu paterni tanımladığında | sistem otomatik alarm üretir ve ilgili erişimi geçici olarak işaretler |
| AC-19.3 (Negatif) | Normal kullanım paterni içinde istek geldiğinde | sistem trafiği izlediğinde | herhangi bir yanlış pozitif alarm üretilmez |

### US-20 — Veri transferi şifreli kanal üzerinden yapılır

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-20.1 | Bir TPP API'ye istek gönderdiğinde | bağlantı TLS üzerinden kurulduğunda | veri şifrelenmiş kanal üzerinden iletilir |
| AC-20.2 (Negatif) | Bir istemci, şifrelenmemiş (HTTP) bağlantı ile istek göndermeye çalıştığında | sistem bağlantı türünü kontrol ettiğinde | sistem isteği reddeder ve yalnızca HTTPS/TLS bağlantısını kabul eder |
| AC-20.3 | Şifreleme sertifikası süresi dolmaya yaklaştığında | sistem periyodik kontrolü çalıştırdığında | ilgili teknik ekibe otomatik uyarı gönderilir |

---

## EPIC-05: Denetim, Raporlama ve KPI Yönetimi

### US-21 — Tüm işlemler değiştirilemez şekilde kaydedilir

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-21.1 | Herhangi bir rıza, erişim veya veri paylaşım işlemi gerçekleştiğinde | sistem işlemi tamamladığında | ilgili olay zaman damgası, aktör ve sonuç bilgisiyle audit log'a kaydedilir |
| AC-21.2 (Negatif) | Bir kullanıcı (yönetici dahil) mevcut bir audit kaydını değiştirmeye/silmeye çalıştığında | sistem isteği aldığında | sistem işlemi reddeder; audit kayıtları immutable olarak korunur |
| AC-21.3 | Audit log sistemine erişim talep edildiğinde | yalnızca yetkili roller (Compliance, InfoSec) erişim talep ettiğinde | erişim sağlanır; yetkisiz roller için erişim reddedilir |

### US-22 — Compliance, regülasyon raporlarını otomatik üretir

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-22.1 | Compliance uzmanı belirli bir tarih aralığı için rapor talep ettiğinde | sistem talebi işlediğinde | sistem ilgili dönem için eksiksiz bir uyum raporu üretir |
| AC-22.2 (Negatif) | Talep edilen tarih aralığında veri bulunmadığında | sistem raporu oluşturmaya çalıştığında | sistem "ilgili dönemde kayıt bulunamadı" mesajını gösterir, hata vermez |
| AC-22.3 | Rapor üretim süreci tamamlandığında | sistem raporu hazırladığında | rapor, düzenleyici otoritenin talep ettiği standart formatta (örn. PDF/CSV) sunulur |

### US-23 — KPI'lar dashboard üzerinden izlenir

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-23.1 | Product Owner dashboard ekranına eriştiğinde | sistem güncel verileri yüklediğinde | süreç süresi, hata oranı, TPP memnuniyeti gibi tanımlı KPI'lar görüntülenir |
| AC-23.2 | Bir KPI, tanımlanan eşik değerin altına indiğinde | sistem eşik kontrolünü yaptığında | ilgili sorumlulara otomatik uyarı bildirimi gönderilir |
| AC-23.3 (Negatif) | Dashboard'a yetkisiz bir kullanıcı erişmeye çalıştığında | sistem erişim kontrolünü yaptığında | erişim reddedilir |

### US-24 — TPP, kendi API kullanım istatistiklerini görür

| AC ID | Given | When | Then |
|---|---|---|---|
| AC-24.1 | TPP geliştiricisi Developer Portal'da oturum açtığında | "Kullanım İstatistikleri" sekmesine girdiğinde | sistem TPP'ye ait API çağrı sayısı, hata oranı ve yanıt süresi metriklerini gösterir |
| AC-24.2 (Negatif) | Bir TPP, başka bir TPP'nin istatistiklerini görüntülemeye çalıştığında | sistem erişim kontrolünü yaptığında | sistem erişimi reddeder; TPP yalnızca kendi verisini görebilir |
| AC-24.3 | TPP'nin henüz hiç API çağrısı yapmadığı bir dönem söz konusu olduğunda | TPP istatistik ekranını görüntülediğinde | sistem "bu dönemde kullanım verisi bulunmamaktadır" mesajını gösterir |

---

## Özet

| Epic | Story Sayısı | Toplam AC Sayısı |
|---|:---:|:---:|
| EPIC-01 | 6 | 23 |
| EPIC-02 | 6 | 20 |
| EPIC-03 | 4 | 11 |
| EPIC-04 | 4 | 12 |
| EPIC-05 | 4 | 12 |
| **TOPLAM** | **24** | **78** |

---

*Bu doküman, Aşama 6.5 (Acceptance Criteria) çalışmasının konsolide edilmiş halidir. Test Case tasarımı için bkz. [`../05-testing/test-case-design.md`](../05-testing/test-case-design.md).*
