# Test Case Design
## Open Banking API Entegrasyonu ve Hesap Bilgisi Paylaşım Platformu

> Referans: [`../04-requirements/acceptance-criteria.md`](../04-requirements/acceptance-criteria.md), [`../04-requirements/srs-software-requirements-specification.md`](../04-requirements/srs-software-requirements-specification.md), [`../04-requirements/requirements-prioritization-matrix.md`](../04-requirements/requirements-prioritization-matrix.md)
>

---

## 1. Functional Test Cases (15 Adet)

### TC-F-01
- **Requirement Reference:** FR-01
- **Related User Story:** US-01
- **Test Objective:** Müşterinin TPP'den gelen rıza talebini onaylayabildiğini doğrulamak
- **Preconditions:** Müşteri sistemde kayıtlı, SCA doğrulamasını tamamlamış, geçerli bir TPP rıza talebi mevcut
- **Test Steps:** 1) TPP rıza talebi gönderir 2) Müşteri rıza ekranını görüntüler 3) Müşteri "Onayla" seçer
- **Expected Result:** Rıza "Aktif" durumda kaydedilir, TPP'ye onay bildirimi iletilir
- **Priority:** High
- **Test Type:** Functional (Positive)

### TC-F-02
- **Requirement Reference:** FR-02
- **Related User Story:** US-02
- **Test Objective:** Müşterinin aktif rızalarını listeleyebildiğini doğrulamak
- **Preconditions:** Müşterinin en az bir aktif rızası mevcut
- **Test Steps:** 1) Müşteri oturum açar 2) "Aktif Rızalarım" ekranına gider
- **Expected Result:** TPP adı, veri kapsamı, başlangıç/bitiş tarihi doğru şekilde listelenir
- **Priority:** High
- **Test Type:** Functional (Positive)

### TC-F-03
- **Requirement Reference:** FR-03
- **Related User Story:** US-03
- **Test Objective:** Müşterinin aktif rızasını iptal edebildiğini doğrulamak
- **Preconditions:** Müşterinin aktif bir rızası mevcut
- **Test Steps:** 1) Müşteri rızayı seçer 2) "İptal Et" butonuna basar 3) İşlemi onaylar
- **Expected Result:** Rıza "İptal Edildi" durumuna geçer, ilgili access token geçersiz kılınır
- **Priority:** High
- **Test Type:** Functional (Positive)

### TC-F-04
- **Requirement Reference:** FR-04
- **Related User Story:** US-04
- **Test Objective:** Rıza süresinin dolmasıyla erişimin otomatik kesildiğini doğrulamak
- **Preconditions:** Süresi dolmak üzere olan bir rıza mevcut (test ortamında kısaltılmış)
- **Test Steps:** 1) Süre dolumunu bekle/simüle et 2) Rıza durumunu sorgula
- **Expected Result:** Rıza otomatik olarak "Süresi Dolmuş" durumuna geçer
- **Priority:** High
- **Test Type:** Functional (Positive)

### TC-F-05
- **Requirement Reference:** FR-05
- **Related User Story:** US-05
- **Test Objective:** Rıza onayında müşteriye bildirim gönderildiğini doğrulamak
- **Preconditions:** Müşteri bildirimleri açık, rıza süreci tetiklenmiş
- **Test Steps:** 1) Müşteri rızayı onaylar 2) Bildirim kanalını kontrol et
- **Expected Result:** "Rızanız onaylandı" bildirimi müşteriye iletilir
- **Priority:** Medium
- **Test Type:** Functional (Positive)

### TC-F-06
- **Requirement Reference:** FR-07
- **Related User Story:** US-07
- **Test Objective:** TPP'nin Developer Portal üzerinden self-servis kayıt olabildiğini doğrulamak
- **Preconditions:** Developer Portal erişilebilir durumda
- **Test Steps:** 1) TPP kayıt formunu açar 2) Tüm zorunlu alanları doldurur 3) Formu gönderir
- **Expected Result:** Kayıt oluşturulur, durum "Beklemede - Doğrulama Sürüyor" olarak işaretlenir
- **Priority:** High
- **Test Type:** Functional (Positive)

### TC-F-07
- **Requirement Reference:** FR-08, FR-09
- **Related User Story:** US-08, US-09
- **Test Objective:** Geçerli sertifikanın otomatik doğrulandığını ve onboarding sürecinin ilerlediğini doğrulamak
- **Preconditions:** TPP kayıt sürecinde, geçerli sertifika hazır
- **Test Steps:** 1) TPP sertifikayı yükler 2) Sistem otomatik doğrulamayı çalıştırır
- **Expected Result:** Sertifika "Doğrulandı" durumuna geçer, onboarding bir sonraki aşamaya ilerler
- **Priority:** High
- **Test Type:** Functional (Positive)

### TC-F-08
- **Requirement Reference:** FR-10
- **Related User Story:** US-10
- **Test Objective:** Doğrulanmış TPP'nin sandbox ortamına erişebildiğini doğrulamak
- **Preconditions:** TPP onboarding doğrulama aşamasını geçmiş
- **Test Steps:** 1) TPP sandbox erişim talebinde bulunur 2) Sandbox üzerinden mock API çağrısı yapar
- **Expected Result:** Sistem mock veriyle başarılı yanıt döner
- **Priority:** High
- **Test Type:** Functional (Positive)

### TC-F-09
- **Requirement Reference:** FR-12
- **Related User Story:** US-12
- **Test Objective:** TPP sertifikasının periyodik olarak otomatik yeniden doğrulandığını kontrol etmek
- **Preconditions:** Sertifika yenileme tarihi yaklaşan bir TPP kaydı mevcut
- **Test Steps:** 1) Periyodik kontrol job'ı çalışır 2) Sonucu kontrol et
- **Expected Result:** TPP'ye sertifika yenileme hatırlatma bildirimi gönderilir
- **Priority:** High
- **Test Type:** Functional (Positive)

### TC-F-10
- **Requirement Reference:** FR-13
- **Related User Story:** US-13
- **Test Objective:** TPP'nin yalnızca rıza kapsamındaki veriye erişebildiğini doğrulamak
- **Preconditions:** Yalnızca "bakiye görüntüleme" kapsamlı geçerli bir consent token mevcut
- **Test Steps:** 1) TPP, bu token ile bakiye sorgusu yapar
- **Expected Result:** Yalnızca bakiye bilgisi döner, işlem geçmişi dönmez
- **Priority:** High
- **Test Type:** Functional (Positive)

### TC-F-11
- **Requirement Reference:** FR-14
- **Related User Story:** US-14
- **Test Objective:** Geçerli bir isteğin gerçek zamanlı yanıt aldığını doğrulamak
- **Preconditions:** Core Banking sistemi normal çalışır durumda, geçerli token mevcut
- **Test Steps:** 1) TPP API çağrısı yapar 2) Yanıt süresini ölç
- **Expected Result:** Yanıt, tanımlı SLA (p95 < 500ms) içinde döner
- **Priority:** High
- **Test Type:** Functional / Performance

### TC-F-12
- **Requirement Reference:** FR-21
- **Related User Story:** US-21
- **Test Objective:** Tüm rıza/erişim işlemlerinin audit log'a kaydedildiğini doğrulamak
- **Preconditions:** Sistem normal çalışır durumda
- **Test Steps:** 1) Bir rıza onayı işlemi gerçekleştir 2) Audit log'u sorgula
- **Expected Result:** İşlem; zaman damgası, aktör ve sonuç bilgisiyle log'da görünür
- **Priority:** High
- **Test Type:** Functional (Positive)

### TC-F-13
- **Requirement Reference:** FR-22
- **Related User Story:** US-22
- **Test Objective:** Compliance kullanıcısının belirli tarih aralığı için uyum raporu üretebildiğini doğrulamak
- **Preconditions:** Sistemde ilgili tarih aralığında işlem verisi mevcut
- **Test Steps:** 1) Compliance kullanıcısı tarih aralığı girer 2) Rapor talep eder
- **Expected Result:** Rapor, tanımlı standart formatta (PDF/CSV) eksiksiz üretilir
- **Priority:** High
- **Test Type:** Functional (Positive)

### TC-F-14
- **Requirement Reference:** FR-23
- **Related User Story:** US-23
- **Test Objective:** KPI dashboard'unun güncel metrikleri görüntülediğini doğrulamak
- **Preconditions:** Sistemde ölçülecek KPI verisi mevcut
- **Test Steps:** 1) Product Owner dashboard'a erişir
- **Expected Result:** Süreç süresi, hata oranı, TPP memnuniyeti gibi KPI'lar görüntülenir
- **Priority:** Medium
- **Test Type:** Functional (Positive)

### TC-F-15
- **Requirement Reference:** FR-24
- **Related User Story:** US-24
- **Test Objective:** TPP'nin yalnızca kendi kullanım istatistiklerini görebildiğini doğrulamak
- **Preconditions:** TPP'nin geçmiş API kullanım verisi mevcut
- **Test Steps:** 1) TPP Developer Portal'da "Kullanım İstatistikleri" sekmesine girer
- **Expected Result:** Yalnızca kendi API çağrı sayısı, hata oranı, yanıt süresi metrikleri görüntülenir
- **Priority:** Low
- **Test Type:** Functional (Positive)

---

## 2. Security Test Cases (10 Adet)

### TC-S-01
- **Requirement Reference:** SEC-01
- **Related User Story:** US-17
- **Test Objective:** OAuth2 Authorization Code Flow'un doğru çalıştığını doğrulamak
- **Preconditions:** Yetkilendirilmiş bir TPP mevcut
- **Test Steps:** 1) TPP authorization isteği gönderir 2) Akışı uçtan uca tamamlar
- **Expected Result:** Authorization code ve sonrasında geçerli access token üretilir
- **Priority:** High
- **Test Type:** Security

### TC-S-02
- **Requirement Reference:** SEC-02
- **Related User Story:** US-18
- **Test Objective:** Rıza onayının SCA olmadan tamamlanamadığını doğrulamak
- **Preconditions:** Müşteri rıza ekranında, SCA tamamlanmamış
- **Test Steps:** 1) Müşteri SCA adımını atlayarak doğrudan onay vermeye çalışır
- **Expected Result:** Sistem onayı reddeder, SCA zorunlu adım olarak kalır
- **Priority:** High
- **Test Type:** Security

### TC-S-03
- **Requirement Reference:** SEC-03
- **Related User Story:** US-20
- **Test Objective:** Tüm API trafiğinin TLS ile şifrelendiğini doğrulamak
- **Preconditions:** API Gateway erişilebilir durumda
- **Test Steps:** 1) Network trafiğini izleme aracıyla kontrol et 2) Bağlantı protokolünü doğrula
- **Expected Result:** Tüm trafik minimum TLS 1.2 ile şifrelenmiş olarak gözlemlenir
- **Priority:** High
- **Test Type:** Security

### TC-S-04
- **Requirement Reference:** SEC-04
- **Related User Story:** -
- **Test Objective:** RBAC'ın iç kullanıcı rollerini doğru sınırlandırdığını doğrulamak
- **Preconditions:** Operasyon rolüne sahip bir kullanıcı mevcut
- **Test Steps:** 1) Operasyon kullanıcısı Compliance'a özel bir ekrana erişmeye çalışır
- **Expected Result:** Sistem erişimi reddeder (403 Forbidden)
- **Priority:** High
- **Test Type:** Security

### TC-S-05
- **Requirement Reference:** SEC-05
- **Related User Story:** -
- **Test Objective:** Rate limiting mekanizmasının çalıştığını doğrulamak
- **Preconditions:** Bir TPP için tanımlı rate limit (örn. 100 req/dk) mevcut
- **Test Steps:** 1) TPP, limiti aşacak şekilde ardışık istek gönderir
- **Expected Result:** Limit aşıldığında istekler HTTP 429 ile reddedilir
- **Priority:** High
- **Test Type:** Security

### TC-S-06
- **Requirement Reference:** SEC-06
- **Related User Story:** US-21
- **Test Objective:** Audit log kayıtlarının değiştirilemez olduğunu doğrulamak
- **Preconditions:** Mevcut bir audit log kaydı bulunmakta
- **Test Steps:** 1) Yönetici yetkisine sahip bir kullanıcı log kaydını değiştirmeye/silmeye çalışır
- **Expected Result:** Sistem işlemi reddeder, kayıt değişmeden kalır
- **Priority:** High
- **Test Type:** Security

### TC-S-07
- **Requirement Reference:** SEC-07
- **Related User Story:** US-17
- **Test Objective:** Geçerli olmayan/sahte sertifikanın reddedildiğini doğrulamak
- **Preconditions:** Geçersiz/sahte bir TPP sertifikası hazırlanmış
- **Test Steps:** 1) TPP bu sertifika ile API isteği gönderir
- **Expected Result:** İstek reddedilir, güvenlik olayı InfoSec'e bildirilir
- **Priority:** High
- **Test Type:** Security

### TC-S-08
- **Requirement Reference:** SEC-08
- **Related User Story:** -
- **Test Objective:** Süresi dolmuş access token'ın reddedildiğini doğrulamak
- **Preconditions:** Süresi dolmuş bir access token mevcut
- **Test Steps:** 1) TPP bu token ile API isteği gönderir
- **Expected Result:** İstek HTTP 401 ile reddedilir
- **Priority:** High
- **Test Type:** Security

### TC-S-09
- **Requirement Reference:** SEC-08
- **Related User Story:** -
- **Test Objective:** Refresh token ile yeni access token alınabildiğini doğrulamak
- **Preconditions:** Geçerli bir refresh token mevcut
- **Test Steps:** 1) TPP refresh token ile yeni access token talep eder
- **Expected Result:** Yeni, geçerli bir access token üretilir
- **Priority:** Medium
- **Test Type:** Security

### TC-S-10
- **Requirement Reference:** SEC-02, FR-18
- **Related User Story:** US-18
- **Test Objective:** SCA'da art arda başarısız denemelerde hesabın geçici kilitlendiğini doğrulamak
- **Preconditions:** Müşteri SCA doğrulama ekranında
- **Test Steps:** 1) Müşteri 3 kez yanlış doğrulama bilgisi girer
- **Expected Result:** Hesap geçici olarak kilitlenir, güvenlik ekibine bildirim gönderilir
- **Priority:** High
- **Test Type:** Security

---

## 3. Negative Test Cases (10 Adet)

### TC-N-01
- **Requirement Reference:** ERR-401-01, FR-15
- **Related User Story:** US-15
- **Test Objective:** Geçersiz token ile yapılan isteğin reddedildiğini doğrulamak
- **Preconditions:** Geçersiz/uydurma bir token oluşturulmuş
- **Test Steps:** 1) TPP bu token ile API çağrısı yapar
- **Expected Result:** HTTP 401 döner, olay audit log'a kaydedilir
- **Priority:** High
- **Test Type:** Negative

### TC-N-02
- **Requirement Reference:** ERR-403-02, FR-15
- **Related User Story:** US-03, US-15
- **Test Objective:** İptal edilmiş rızaya ait token ile erişimin reddedildiğini doğrulamak
- **Preconditions:** Müşteri rızasını iptal etmiş, TPP elinde eski token var
- **Test Steps:** 1) TPP eski token ile API çağrısı yapar
- **Expected Result:** HTTP 403 ve "consent revoked" hatası döner
- **Priority:** High
- **Test Type:** Negative

### TC-N-03
- **Requirement Reference:** ERR-403-01
- **Related User Story:** US-04
- **Test Objective:** Süresi dolmuş rıza ile erişimin reddedildiğini doğrulamak
- **Preconditions:** Rıza süresi dolmuş durumda
- **Test Steps:** 1) TPP bu rızaya ait token ile API çağrısı yapar
- **Expected Result:** HTTP 403 ve "consent expired" hatası döner
- **Priority:** High
- **Test Type:** Negative

### TC-N-04
- **Requirement Reference:** BR-02, FR-13
- **Related User Story:** US-13
- **Test Objective:** Token kapsamı dışında veri talebinin reddedildiğini doğrulamak
- **Preconditions:** Yalnızca bakiye kapsamlı token mevcut
- **Test Steps:** 1) TPP, bu token ile işlem geçmişi talep eder
- **Expected Result:** Sistem isteği reddeder, "yetersiz kapsam" hatası döner
- **Priority:** High
- **Test Type:** Negative

### TC-N-05
- **Requirement Reference:** FR-07
- **Related User Story:** US-07
- **Test Objective:** Eksik bilgiyle TPP kayıt formunun reddedildiğini doğrulamak
- **Preconditions:** Developer Portal erişilebilir
- **Test Steps:** 1) TPP, zorunlu bir alanı boş bırakarak formu gönderir
- **Expected Result:** Sistem kaydı reddeder, eksik alan belirtilir
- **Priority:** Medium
- **Test Type:** Negative

### TC-N-06
- **Requirement Reference:** FR-08, FR-09
- **Related User Story:** US-08
- **Test Objective:** Süresi dolmuş sertifika ile TPP kaydının reddedildiğini doğrulamak
- **Preconditions:** Süresi dolmuş bir sertifika hazır
- **Test Steps:** 1) TPP bu sertifikayı yükler
- **Expected Result:** Sistem yüklemeyi reddeder, "sertifika süresi dolmuş" hatası gösterilir
- **Priority:** High
- **Test Type:** Negative

### TC-N-07
- **Requirement Reference:** FR-10
- **Related User Story:** US-10
- **Test Objective:** Onboarding doğrulaması tamamlanmamış TPP'nin sandbox erişiminin reddedildiğini doğrulamak
- **Preconditions:** TPP doğrulama aşamasında, henüz tamamlanmamış
- **Test Steps:** 1) TPP sandbox erişimi talep eder
- **Expected Result:** Sistem erişimi reddeder, "doğrulama tamamlanmadı" mesajı gösterilir
- **Priority:** High
- **Test Type:** Negative

### TC-N-08
- **Requirement Reference:** ERR-429-01, SEC-05
- **Related User Story:** -
- **Test Objective:** Rate limit aşımında doğru hata kodunun döndüğünü doğrulamak
- **Preconditions:** TPP'nin rate limiti az miktarda kalmış
- **Test Steps:** 1) TPP limiti aşacak şekilde istek gönderir
- **Expected Result:** HTTP 429 ve "Retry-After" başlığı ile yanıt döner
- **Priority:** Medium
- **Test Type:** Negative

### TC-N-09
- **Requirement Reference:** ERR-503-01
- **Related User Story:** US-14
- **Test Objective:** Core Banking sistemi erişilemez olduğunda doğru hata davranışının gerçekleştiğini doğrulamak
- **Preconditions:** Core Banking servisi simüle edilmiş kesinti durumunda (test ortamı)
- **Test Steps:** 1) TPP geçerli bir veri talebi gönderir
- **Expected Result:** HTTP 503 döner, sistem çökmez, circuit breaker devreye girer
- **Priority:** High
- **Test Type:** Negative

### TC-N-10
- **Requirement Reference:** ERR-401-03, SEC-07
- **Related User Story:** US-17
- **Test Objective:** Askıya alınmış TPP sertifikasıyla erişimin engellendiğini doğrulamak
- **Preconditions:** TPP sertifikası Compliance tarafından askıya alınmış
- **Test Steps:** 1) TPP bu sertifika ile API isteği gönderir
- **Expected Result:** Sistem isteği reddeder, "erişim askıya alınmış" hatası döner
- **Priority:** High
- **Test Type:** Negative

---

## 4. Integration Test Cases (5 Adet)

### TC-I-01
- **Requirement Reference:** FR-14, FR-13 (API Gateway ↔ Core Banking)
- **Related User Story:** US-13, US-14
- **Test Objective:** API Gateway'in Core Banking sisteminden doğru ve gerçek zamanlı veri çekebildiğini doğrulamak
- **Preconditions:** API Gateway ve Core Banking entegrasyonu aktif, test hesabı mevcut
- **Test Steps:** 1) Geçerli token ile bakiye sorgusu yap 2) Core Banking'deki gerçek değerle karşılaştır
- **Expected Result:** API yanıtı, Core Banking'deki güncel veriyle birebir tutarlıdır
- **Priority:** High
- **Test Type:** Integration

### TC-I-02
- **Requirement Reference:** FR-01, FR-18 (Consent Mgmt ↔ OAuth2 Server)
- **Related User Story:** US-01, US-18
- **Test Objective:** Consent Management System ile OAuth2 Authorization Server arasındaki entegrasyonun doğru çalıştığını doğrulamak
- **Preconditions:** Müşteri SCA doğrulamasını tamamlamış
- **Test Steps:** 1) Müşteri rızayı onaylar 2) OAuth2 Server'ın consent onayını alıp token ürettiğini doğrula
- **Expected Result:** Consent onayı sonrası OAuth2 Server, doğru kapsamlı access token üretir
- **Priority:** High
- **Test Type:** Integration

### TC-I-03
- **Requirement Reference:** FR-21 (Tüm Bileşenler ↔ Audit & Logging Platform)
- **Related User Story:** US-21
- **Test Objective:** Farklı bileşenlerden gelen olayların merkezi audit platformda birleştiğini doğrulamak
- **Preconditions:** Sistem genelinde test işlemleri gerçekleştirilebilir durumda
- **Test Steps:** 1) Her bileşende bir işlem tetikle 2) Audit platformda üç olayı da sorgula
- **Expected Result:** Üç olay da doğru zaman damgası ve aktör bilgisiyle merkezi platformda görünür
- **Priority:** High
- **Test Type:** Integration

### TC-I-04
- **Requirement Reference:** FR-09 (Developer Portal ↔ Compliance Engine)
- **Related User Story:** US-09
- **Test Objective:** Developer Portal'a yüklenen sertifikanın Compliance Engine tarafından doğru işlendiğini doğrulamak
- **Preconditions:** TPP kayıt sürecinde, geçerli sertifika hazır
- **Test Steps:** 1) Sertifikayı Developer Portal'a yükle 2) Compliance Engine'in doğrulama sonucunu kontrol et
- **Expected Result:** Doğrulama sonucu doğru şekilde TPP kaydına yansır
- **Priority:** High
- **Test Type:** Integration

### TC-I-05
- **Requirement Reference:** FR-10 (Developer Portal ↔ Sandbox Environment)
- **Related User Story:** US-10
- **Test Objective:** Developer Portal üzerinden verilen sandbox erişiminin Sandbox Environment'a doğru yansıdığını doğrulamak
- **Preconditions:** TPP onboarding doğrulamasını geçmiş
- **Test Steps:** 1) Developer Portal'dan sandbox erişimi etkinleştir 2) Sandbox'ta mock API çağrısı yap
- **Expected Result:** Sandbox, mock veriyle başarılı yanıt döner; production verisine erişim sağlanmaz
- **Priority:** Medium
- **Test Type:** Integration

---

## 5. UAT (User Acceptance Test) Cases (5 Adet)

### TC-U-01
- **Requirement Reference:** BR-01, BR-02, BR-03
- **Related User Story:** US-01, US-02, US-03
- **Test Objective:** Bir müşterinin rıza verme, görüntüleme ve iptal etme sürecini uçtan uca doğrulamak
- **Preconditions:** Test müşteri hesabı, mobil bankacılık erişimi mevcut
- **Test Steps:** 1) TPP üzerinden rıza talebi başlat 2) Mobil bankacılıkta SCA ile doğrula 3) Rızayı onayla 4) "Aktif Rızalarım" ekranından görüntüle 5) Rızayı iptal et
- **Expected Result:** Süreç sorunsuz tamamlanır, her adımda doğru bilgi/bildirim sunulur, iptal sonrası erişim anında kesilir
- **Priority:** High
- **Test Type:** UAT

### TC-U-02
- **Requirement Reference:** BR-06, BR-07, BR-08
- **Related User Story:** US-07, US-08, US-10
- **Test Objective:** Bir TPP'nin kayıt olmasından sandbox testine kadar olan sürecin gerçek kullanıcı deneyimiyle uyumlu olduğunu doğrulamak
- **Preconditions:** Test TPP hesabı, geçerli test sertifikası
- **Test Steps:** 1) Developer Portal'a kayıt ol 2) Sertifikayı yükle 3) Doğrulama sonucunu bekle 4) Sandbox'ta API testleri yap
- **Expected Result:** Süreç, tanımlanan SLA (≤5 iş günü) içinde ve TPP'nin anlayabileceği netlikte tamamlanır
- **Priority:** High
- **Test Type:** UAT

### TC-U-03
- **Requirement Reference:** BR-11, BR-12
- **Related User Story:** US-13, US-14
- **Test Objective:** Onaylı bir TPP'nin, gerçek bir müşteri rızası kapsamında hesap bilgisine erişim deneyimini doğrulamak
- **Preconditions:** Aktif rıza, yetkilendirilmiş TPP, production benzeri test ortamı
- **Test Steps:** 1) TPP, müşteri adına bakiye sorgusu yapar 2) Yanıtı ve süresini değerlendir
- **Expected Result:** TPP, doğru veriyi, kabul edilebilir sürede, beklenen formatta alır
- **Priority:** High
- **Test Type:** UAT

### TC-U-04
- **Requirement Reference:** BR-19, BR-20
- **Related User Story:** US-21, US-22
- **Test Objective:** Compliance ekibinin gerçek bir denetim talebi senaryosunda gerekli raporu makul sürede üretebildiğini doğrulamak
- **Preconditions:** Sistemde geçmiş dönem işlem verisi mevcut
- **Test Steps:** 1) Compliance kullanıcısı belirli bir dönem için uyum raporu talep eder 2) Raporu inceler
- **Expected Result:** Rapor, denetim için yeterli detayda ve düzenleyici otoritenin beklediği formatta, makul sürede üretilir
- **Priority:** High
- **Test Type:** UAT

### TC-U-05
- **Requirement Reference:** BR-16, BR-15
- **Related User Story:** US-17, US-18
- **Test Objective:** Hem müşterinin hem TPP'nin kimlik doğrulama deneyiminin güvenli ve makul sürede tamamlandığını doğrulamak
- **Preconditions:** Test müşteri ve test TPP hesapları hazır
- **Test Steps:** 1) TPP, OAuth2 akışını başlatır 2) Müşteri SCA ile doğrulanır 3) Süreç tamamlanır
- **Expected Result:** Süreç, NFR-04'te tanımlanan 2 dakika hedefini aşmadan, güvenlik standartlarından ödün vermeden tamamlanır
- **Priority:** High
- **Test Type:** UAT

---

## Test Case Özet Tablosu

| Kategori | Test Case Sayısı | High Priority | Medium Priority | Low Priority |
|---|:---:|:---:|:---:|:---:|
| Functional Test Cases | 15 | 12 | 2 | 1 |
| Security Test Cases | 10 | 9 | 1 | 0 |
| Negative Test Cases | 10 | 7 | 3 | 0 |
| Integration Test Cases | 5 | 4 | 1 | 0 |
| UAT Test Cases | 5 | 5 | 0 | 0 |
| **TOPLAM** | **45** | **37** | **7** | **1** |

> **İzlenebilirlik Notu:** Toplam 45 test case, Acceptance Criteria (78 AC) ile uyumlu şekilde tasarlanmıştır; her test case en az bir FR/NFR/SEC gereksinimi ve ilgili User Story ile ilişkilendirilmiştir.

---

*Bu doküman, test case tasarım çalışmasının konsolide edilmiş halidir.*