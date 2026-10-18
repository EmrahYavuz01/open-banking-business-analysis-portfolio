# Open Banking AIS Platform — API Specification

## swagger.yaml — OpenAPI 3.0.3

---

## Izlenebilirlik Zinciri

```
Business Goals (BG-1..BG-5)
         ↓
Business Requirements / BRD (BR-01..BR-22)
         ↓
SRS Functional Requirements (FR-01..FR-24)
SRS Security Requirements (SEC-01..SEC-08)
         ↓
API Specification — swagger.yaml (bu dosya)
         ↓
Test Case Design
```

---

## Endpoint Envanteri

| Method | Path | Tag | SRS FR | BRD BR |
|--------|------|-----|--------|--------|
| POST | /oauth/token | OAuth2 | SEC-01, SEC-08 | BR-15 |
| POST | /oauth/revoke | OAuth2 | SEC-08 | BR-03 |
| POST | /consents | Consent Management | FR-01, FR-18 | BR-01 |
| GET | /consents/{id} | Consent Management | FR-02 | BR-02 |
| POST | /consents/{id}/revoke | Consent Management | FR-03 | BR-03 |
| POST | /consents/{id}/approve | Consent Management | FR-01, FR-18 | BR-01, BR-16 |
| GET | /customers/{id}/consents | Consent Management | FR-02, FR-06 | BR-02 |
| GET | /accounts | AIS | FR-13, FR-14, FR-15 | BR-11, BR-12, BR-13 |
| GET | /accounts/{id} | AIS | FR-13 | BR-11 |
| GET | /accounts/{id}/balances | AIS | FR-13, FR-14 | BR-11, BR-12 |
| GET | /accounts/{id}/transactions | AIS | FR-13, FR-14 | BR-11, BR-12 |
| POST | /tpp/register | TPP Management | FR-07, FR-08 | BR-06, BR-07 |
| GET | /tpp/{id} | TPP Management | FR-07, FR-09 | BR-06, BR-07 |
| PUT | /tpp/{id}/certificate | TPP Management | FR-08, FR-09 | BR-07 |
| GET | /tpp/{id}/status | TPP Management | FR-09, FR-12 | BR-07, BR-10 |
| POST | /tpp/{id}/sandbox | TPP Management | FR-10 | BR-08 |
| GET | /tpp/{id}/usage-stats | TPP Management | FR-24 | BR-22 |
| GET | /audit-logs | Audit & Logging | FR-21, FR-22 | BR-19, BR-20 |
| GET | /audit-logs/{id} | Audit & Logging | FR-21 | BR-19 |
| POST | /audit-logs/reports | Audit & Logging | FR-22 | BR-20 |
| GET | /health | Health | NFR-01 | — |

---

## HTTP Durum Kodları Özeti

| Kod | Anlamı | SRS Error Code |
|-----|--------|----------------|
| 200 | Başarılı | — |
| 201 | Kaynak oluşturuldu | — |
| 202 | Async işlem başlatıldı | — |
| 400 | Validasyon hatası | ERR-400-VALIDATION |
| 401 | Geçersiz/süresi dolmuş token | ERR-401-01, ERR-401-02, ERR-401-03 |
| 403 | Yetki yok / rıza geçersiz | ERR-403-01, ERR-403-02, ERR-403-03 |
| 404 | Kaynak bulunamadı | ERR-404-NOT-FOUND |
| 409 | Çakışma (zaten kayıtlı/iptal) | ERR-409-* |
| 422 | SCA tamamlanmamış | ERR-401-SCA-REQUIRED |
| 429 | Rate limit aşıldı | ERR-429-01 |
| 503 | Core Banking erişilemez | ERR-503-01 |

---

## OAuth2 Scope Tanımları

| Scope | Açıklama | İzin Verdiği Endpoint |
|-------|----------|----------------------|
| `accounts` | Hesap listesi ve detayı | GET /accounts, GET /accounts/{id} |
| `balance` | Bakiye görüntüleme | GET /accounts/{id}/balances |
| `transactions` | İşlem geçmişi | GET /accounts/{id}/transactions |
| `consents:read` | Rıza görüntüleme | GET /consents, GET /customers/{id}/consents |
| `consents:write` | Rıza oluşturma/iptal | POST /consents, POST /revoke |
| `tpp:read` | TPP bilgisi görüntüleme | GET /tpp/{id} |
| `audit:read` | Audit log görüntüleme (Compliance rolü) | GET /audit-logs |

---

## Reusable Schemas (31 adet)

| Schema | Kullanıldığı Yerler |
|--------|---------------------|
| `TokenRequest` | POST /oauth/token request |
| `TokenResponse` | POST /oauth/token response |
| `ConsentRequest` | POST /consents request |
| `ConsentResponse` | Consent CRUD responses |
| `ConsentRevokeResponse` | POST /revoke response |
| `ConsentListResponse` | GET /customers/{id}/consents |
| `ConsentStatus` | ConsentResponse içinde (enum) |
| `AccountListResponse` | GET /accounts |
| `AccountSummary` | AccountListResponse item |
| `AccountDetailResponse` | GET /accounts/{id} |
| `BalanceResponse` | GET /accounts/{id}/balances |
| `Balance` | BalanceResponse item |
| `Amount` | Balance, Transaction |
| `TransactionListResponse` | GET /accounts/{id}/transactions |
| `Transaction` | TransactionListResponse item |
| `TppRegistrationRequest` | POST /tpp/register request |
| `TppRegistrationResponse` | POST /tpp/register response |
| `TppDetailResponse` | GET /tpp/{id} |
| `TppStatusResponse` | GET /tpp/{id}/status |
| `TppStatus` | TPP response'larında (enum) |
| `CertificateUploadRequest` | PUT /tpp/{id}/certificate request |
| `CertificateUploadResponse` | PUT /tpp/{id}/certificate response |
| `TppUsageStatsResponse` | GET /tpp/{id}/usage-stats |
| `AuditLogListResponse` | GET /audit-logs |
| `AuditLogEntry` | AuditLogListResponse item, GET /audit-logs/{id} |
| `ComplianceReportRequest` | POST /audit-logs/reports request |
| `HealthResponse` | GET /health |
| `ErrorResponse` | Tüm hata yanıtları |
| `Pagination` | Liste yanıtları |
| `Links` | HATEOAS linkleri |
| `SuccessMessage` | Basit başarı mesajları |

---

## Swagger UI Kullanımı

### Swagger UI'ı yerelde çalıştırma

```bash
# Docker ile
docker run -p 8080:8080 \
  -e SWAGGER_JSON=/swagger/swagger.yaml \
  -v $(pwd):/swagger \
  swaggerapi/swagger-ui

# Tarayıcıda aç
open http://localhost:8080
```

### Alternatif: swagger-editor online
1. https://editor.swagger.io adresine git
2. swagger.yaml içeriğini yapıştır
3. Sağ panelde Swagger UI görünür

---

## Test Tasarımıyla İlişki

Bu API spesifikasyonu, BRD ve SRS içinde tanımlanan gereksinimleri teknik arayüz düzeyinde örneklemek amacıyla hazırlanmıştır. İlgili manuel test senaryoları `../05-testing/test-case-design.md` dosyasında yer alır.
