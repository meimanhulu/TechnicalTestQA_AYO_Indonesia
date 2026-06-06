# Studi Kasus 2 — Bagian 1: QA Test Plan, Entry/Exit Criteria & Device Matrix

> **Project:** AYO Indonesia — Super Sport Community App
> **QA Engineer:** Meiman Ryland Hulu
> **Versi Dokumen:** 1.0
> **Tanggal:** 4 Juni 2026

---

## Daftar Isi

- [A. Test Plan](#a-test-plan)
- [B. Entry & Exit Criteria](#b-entry--exit-criteria)
- [C. Device / Browser / OS Compatibility Matrix](#c-device--browser--os-compatibility-matrix)
- [D. Defect Life Cycle](#d-defect-life-cycle--workflow)
- [E. Test Metrics & Reporting](#e-test-metrics--reporting-template)

---

## A. Test Plan

### 1. Project Overview

AYO Indonesia adalah platform booking lapangan olahraga, komunitas sparring, dan kompetisi amatir. Ekosistem mencakup:

- **Landing Page Web** — `ayo.co.id`
- **Mobile App** — iOS & Android (User/Penyewa)
- **Dashboard Venue Management** — Web-based
- **Backend API & Database**

---

### 2. Testing Scope

| In-Scope | Out-of-Scope |
|----------|--------------|
| Landing Page: `ayo.co.id` | Marketing campaign di luar aplikasi |
| Mobile App: iOS & Android (User/Penyewa) | Hardware POS di venue fisik |
| Venue Management Web | Integrasi bank non-payment-gateway |
| Backend API & Database | Social media management tools |
| Payment Gateway Integration | Third-party logistics |
| Push Notification Service | — |

---

### 3. Testing Approach

| Phase | Aktivitas | Durasi Estimasi |
|-------|-----------|-----------------|
| **Phase 1 — Preparation** | Review requirement, setup environment, prepare test data | 2 hari |
| **Phase 2 — Smoke Test** | Verifikasi build deployable, critical path runnable | 0.5 hari |
| **Phase 3 — Functional Testing** | Execute test cases per modul | 5 hari |
| **Phase 4 — Integration Testing** | Payment gateway, API contract, webhook | 2 hari |
| **Phase 5 — Regression Testing** | Re-run critical path setelah bug fix | 2 hari |
| **Phase 6 — Performance & Security** | Load test, penetration test | 2 hari |
| **Phase 7 — UAT / Sign-off** | Demo ke stakeholder, final approval | 1 hari |

> **Total Estimasi:** 14.5 hari kerja (~3 sprint weeks)

---

### 4. Resource Plan

| Role | Jumlah | Tanggung Jawab |
|------|--------|----------------|
| QA Lead | 1 | Test plan, koordinasi, reporting |
| QA Engineer (Mobile) | 2 | iOS & Android functional testing |
| QA Engineer (Web/API) | 1 | Landing page, venue management, API testing |
| QA Engineer (Automation) | 1 | Script automation, CI/CD integration |
| QA Engineer (Performance) | 1 | Load test, stress test, security scan |

---

### 5. Testing Types & Coverage Target

| Testing Type | Coverage Target | Tools |
|--------------|-----------------|-------|
| Functional Testing | 100% critical path, 80% major features | Manual, Appium, Selenium |
| API Testing | 100% endpoints | Postman, REST Assured |
| Integration Testing | 100% payment flows | Postman, mock server |
| Regression Testing | 100% critical path setiap release | Automation suite |
| Performance Testing | 1000 concurrent users, < 3s response | JMeter, K6 |
| Security Testing | OWASP Top 10 coverage | OWASP ZAP, Burp Suite |
| Accessibility Testing | WCAG 2.1 AA compliance | Axe, Lighthouse |
| Cross-Device Testing | Top 10 devices + 5 browsers | BrowserStack, real devices |

---

### 6. Deliverables

1. Test Plan Document *(dokumen ini)*
2. Test Case Document *(85 test cases)*
3. Bug Report *(dengan template standar)*
4. Test Execution Report *(daily/weekly)*
5. Performance Test Report
6. Security Scan Report
7. Final QA Sign-off Document

---

### 7. Risk & Mitigation

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Payment gateway sandbox tidak stabil | High | Medium | Siapkan mock server untuk testing offline |
| Device fisik tidak tersedia | High | Low | Gunakan BrowserStack / cloud device farm |
| Scope creep dari product team | High | High | Weekly scope review + change request form |
| Developer fix bug terlambat | Medium | Medium | Daily standup + bug triage meeting |
| Data test tidak representatif | Medium | Low | Generate synthetic data mirroring production |

---

## B. Entry & Exit Criteria

### 1. Entry Criteria

> Kondisi yang **wajib terpenuhi** sebelum testing boleh dimulai.

| Kode | Kriteria | Verifikasi |
|------|----------|------------|
| EC-01 | Build/deploy berhasil di staging environment | `[ ]` Smoke test deployment pass |
| EC-02 | API contract documentation finalized & shared | `[ ]` Swagger/Postman collection tersedia |
| EC-03 | Test environment (staging) identik dengan production config | `[ ]` DB schema, server spec, CDN verified |
| EC-04 | Test data (accounts, venues, payment sandbox) ready | `[ ]` 5 user accounts, 3 venue accounts, sandbox CC |
| EC-05 | Requirements/user stories untuk fitur yang dites sudah approved | `[ ]` PRD signed off oleh PM |
| EC-06 | Critical path test cases sudah di-review oleh QA Lead | `[ ]` 100% critical path TC reviewed |
| EC-07 | Tidak ada blocker bug dari previous release (S1 bug = 0) | `[ ]` Previous release bug closed |
| EC-08 | Access credentials untuk semua environment sudah diberikan | `[ ]` VPN, DB access, admin panel access |

---

### 2. Exit Criteria

> Kondisi yang **wajib terpenuhi** sebelum release dapat dilakukan.

| Kode | Kriteria | Verifikasi |
|------|----------|------------|
| XC-01 | 100% Critical Path test cases (S1/P1) status = PASS | `[ ]` All critical path green |
| XC-02 | Zero Critical (S1) bug masih open | `[ ]` S1 bug count = 0 |
| XC-03 | Major (S2) bug open ≤ 2 dan sudah ada workaround/approved | `[ ]` S2 bug count ≤ 2 |
| XC-04 | Test execution rate ≥ 95% dari total test cases | `[ ]` 95%+ TC executed |
| XC-05 | Performance test: response time < 3s untuk 95th percentile | `[ ]` Performance report attached |
| XC-06 | Security scan: no high/critical vulnerabilities | `[ ]` Security report attached |
| XC-07 | Payment end-to-end flow (full payment, DP, refund) 100% PASS | `[ ]` Payment flow verified |
| XC-08 | Accessibility audit: WCAG 2.1 AA compliance ≥ 90% | `[ ]` Axe/Lighthouse report attached |
| XC-09 | Regression test suite 100% PASS setelah final bug fix | `[ ]` Regression report green |
| XC-10 | QA Sign-off document signed oleh QA Lead & PM | `[ ]` Sign-off document ready |

---

### 3. Suspension & Resumption Criteria

| Kondisi Suspension | Kapan Boleh Resume |
|--------------------|--------------------|
| Staging environment down > 4 jam | Environment up + smoke test pass |
| > 30% test cases blocked oleh bug | Bug fix deployed + regression pass |
| Payment gateway sandbox down > 1 hari | Mock server ready atau sandbox up |
| Requirement berubah signifikan mid-testing | New test cases reviewed + approved |

---

## C. Device / Browser / OS Compatibility Matrix

### 1. Mobile — iOS

| Priority | Device | OS Version | Screen Size | Test Focus |
|----------|--------|------------|-------------|------------|
| P1 | iPhone 15 Pro | iOS 17.x | 6.1" | Primary test device |
| P1 | iPhone 14 | iOS 16.x | 6.1" | Most common user device |
| P2 | iPhone SE (3rd Gen) | iOS 17.x | 4.7" | Small screen edge case |
| P2 | iPhone 12 | iOS 16.x | 6.1" | Older hardware performance |
| P3 | iPad Pro 12.9" | iPadOS 17.x | 12.9" | Tablet layout |
| P3 | iPhone 11 | iOS 15.x | 6.1" | Minimum supported OS |

---

### 2. Mobile — Android

| Priority | Device | OS Version | Screen Size | Test Focus |
|----------|--------|------------|-------------|------------|
| P1 | Samsung Galaxy S24 | Android 14 | 6.2" | Primary test device |
| P1 | Samsung Galaxy A54 | Android 14 | 6.4" | Mid-range (most common) |
| P2 | Xiaomi Redmi Note 13 | Android 13 | 6.67" | Popular budget device |
| P2 | Google Pixel 8 | Android 14 | 6.2" | Pure Android reference |
| P3 | Samsung Galaxy S21 | Android 12 | 6.2" | Older OS compatibility |
| P3 | Oppo Reno 11 | Android 14 | 6.7" | Custom UI (ColorOS) |
| P3 | Samsung Tab S9 | Android 14 | 11" | Tablet layout |

---

### 3. Web — Landing Page & Venue Management

| Priority | Browser | Version | OS | Test Focus |
|----------|---------|---------|----|------------|
| P1 | Chrome | Latest | Windows 11 | Primary browser |
| P1 | Chrome | Latest | macOS Sonoma | Cross-OS consistency |
| P1 | Safari | Latest | macOS Sonoma | Apple ecosystem |
| P2 | Firefox | Latest | Windows 11 | Alternative browser |
| P2 | Edge | Latest | Windows 11 | Enterprise users |
| P2 | Safari | Latest | iOS 17 | Mobile web |
| P3 | Chrome | Latest | Android 14 | Mobile web |
| P3 | Samsung Internet | Latest | Android 14 | Samsung users |

---

### 4. Network Conditions

| Kondisi | Kapan Diuji | Tools |
|---------|-------------|-------|
| Wi-Fi (High Speed) | Default testing | — |
| 4G LTE | Mobile app functional | Network Link Conditioner |
| 3G (Slow) | Landing page performance | Chrome DevTools Throttle |
| Offline | Offline handling | Airplane mode |
| Intermittent | Network disconnect mid-payment | Charles Proxy / Clumsy |

---

## D. Defect Life Cycle / Workflow

```
[New] → [Assigned] → [In Progress] → [Fixed] → [Ready for Test] → [Closed]
  ↑                                                     ↓
  └──────────────────────────────────────── [Reopen] ← [Failed Retest]
```

| Status | Definisi | Siapa yang Update |
|--------|----------|-------------------|
| **New** | Bug baru dilaporkan, belum dilihat developer | QA Engineer |
| **Assigned** | Bug sudah dilihat dan di-assign ke developer | QA Lead / Dev Lead |
| **In Progress** | Developer sedang memperbaiki | Developer |
| **Fixed** | Developer selesai fix, belum di-test ulang | Developer |
| **Ready for Test** | Fix sudah deploy ke staging, siap retest | DevOps / Developer |
| **Closed** | Retest PASS, bug fix verified | QA Engineer |
| **Reopen** | Retest FAIL, bug masih ada atau regresi | QA Engineer |
| **Rejected** | Bukan bug (expected behavior / invalid report) | Developer / QA Lead |
| **Duplicate** | Bug sudah dilaporkan sebelumnya | QA Lead |
| **Deferred** | Bug valid tapi fix di-delay ke release berikutnya | Product Manager |

---

## E. Test Metrics & Reporting Template

### Daily Test Execution Report

| Metric | Target | Day 1 | Day 2 | Day 3 | ... | Day N |
|--------|--------|------:|------:|------:|:---:|------:|
| Total Test Cases | 85 | 85 | 85 | 85 | | 85 |
| Executed | — | 20 | 45 | 70 | | 85 |
| Pass | — | 15 | 35 | 60 | | 80 |
| Fail | — | 5 | 8 | 7 | | 3 |
| Blocked | — | 0 | 2 | 3 | | 2 |
| Not Run | — | 65 | 40 | 15 | | 0 |
| Execution Rate | ≥ 95% | 24% | 53% | 82% | | 100% |
| Pass Rate | ≥ 90% | 75% | 78% | 86% | | 94% |

---

### Bug Summary Report

| Severity | Open | Fixed | Closed | Reopen | Total |
|----------|-----:|------:|-------:|-------:|------:|
| Critical (S1) | 0 | 2 | 2 | 0 | 4 |
| Major (S2) | 1 | 5 | 4 | 0 | 10 |
| Medium (S3) | 3 | 8 | 5 | 1 | 17 |
| Minor (S4) | 2 | 4 | 3 | 0 | 9 |
| **Total** | **6** | **19** | **14** | **1** | **40** |

---