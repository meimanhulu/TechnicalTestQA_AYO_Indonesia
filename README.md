# 🏸 Technical Test — QA Engineer · AYO Indonesia

> **Kandidat:** Meiman Ryland Hulu
> **Posisi:** QA Engineer / SDET
> **Platform:** AYO Indonesia — Super Sport Community App
> **Tanggal Submission:** Juni 2026

---

## 📋 Tentang Repository Ini

Repository ini berisi jawaban lengkap untuk **Technical Test QA** dari AYO Indonesia, yang mencakup:

- Pembuatan **QA Test Plan** komprehensif
- Pendefinisian **Entry & Exit Criteria** pengujian
- Penyusunan **Device / Browser Compatibility Matrix**
- **Analisis mendalam** terhadap aplikasi mobile AYO (iOS)
- Identifikasi **bug** hasil eksplorasi langsung di aplikasi
- **Rekomendasi improvement** dari sisi UX, Performance, dan Accessibility

---

## 📁 Struktur Dokumen

```
TechnicalTestQA_AYO_Indonesia/
│
├── README.md                   ← Anda di sini
├── StudiCase2_Bagian1.md       ← Test Plan, Entry/Exit Criteria & Device Matrix
└── StudiCase2_Bagian2.md       ← Analisis Aplikasi Mobile AYO (iOS)
```

---

## 🗂️ Panduan Membaca

Dokumen dibagi menjadi **2 bagian** sesuai struktur studi kasus. Disarankan membaca secara berurutan:

### 📄 [Bagian 1 — QA Test Plan, Entry/Exit Criteria & Device Matrix](./StudiCase2_Bagian1.md)

Dokumen ini menjawab kebutuhan perencanaan pengujian secara menyeluruh:

| Section | Isi |
|---------|-----|
| **A. Test Plan** | Project overview, scope, approach (7 phase), resource plan, testing types & coverage target, deliverables, risk & mitigation |
| **B. Entry & Exit Criteria** | 8 entry criteria, 10 exit criteria, suspension & resumption criteria |
| **C. Device Matrix** | iOS (6 device), Android (7 device), Web browser (8 kombinasi), Network conditions |
| **D. Defect Life Cycle** | Workflow diagram, definisi setiap status bug, penanggung jawab |
| **E. Test Metrics** | Template daily execution report & bug summary report |

---

### 📄 [Bagian 2 — Analisis Aplikasi Mobile AYO (iOS)](./StudiCase2_Bagian2.md)

Dokumen ini berisi hasil eksplorasi langsung terhadap aplikasi AYO di iOS:

| Section | Isi |
|---------|-----|
| **1. Ringkasan Ekosistem** | Pemetaan 15+ modul, 25+ cabang olahraga, sub-fitur tiap modul |
| **2. Prioritas Pengujian** | Critical path (P1–P3), area berisiko tinggi (6 kategori) |
| **3. Mekanisme Pengujian** | Functional, non-functional, specialized testing untuk AYO |
| **4. Test Case Spesifik** | 114 test cases terdistribusi di 17 modul |
| **5. Bug yang Ditemukan** | 2 bug hasil eksplorasi langsung (1 Critical, 1 Major) dengan hypothesis teknis |
| **6. Rekomendasi** | UX, Performance, dan Accessibility enhancement |
| **7. Kesimpulan** | Fokus testing utama & mekanisme yang direkomendasikan |

---

## 🐛 Bug Highlight — Ditemukan Saat Eksplorasi

Dua bug ditemukan secara langsung saat melakukan exploratory testing pada aplikasi AYO iOS:

| Bug ID | Modul | Severity | Deskripsi Singkat |
|--------|-------|:--------:|-------------------|
| BUG-001 | Profil › Tentukan Skill | 🔴 **Critical (S1)** | Blank screen setelah konfirmasi skill level — app harus force close, no workaround |
| BUG-002 | Home › Notifikasi | 🟠 **Major (S2)** | Badge notifikasi menampilkan angka "4" padahal tidak ada notifikasi sama sekali |

> Detail lengkap termasuk steps to reproduce, expected/actual result, dan hypothesis teknis tersedia di [Bagian 2 → Section 5](./StudiCase2_Bagian2.md#5-bug-yang-ditemukan-selama-eksplorasi).

---

## 🧪 Snapshot Cakupan Pengujian

| Dimensi | Detail |
|---------|--------|
| **Total Test Cases** | 114 TC (17 modul) |
| **Critical Path Coverage** | 100% (P1 modules) |
| **Device Coverage** | 13 device (iOS + Android) + 8 browser config |
| **Testing Types** | Functional, API, Integration, Performance, Security, Accessibility, Compatibility |
| **Bug Ditemukan** | 2 (1 Critical · 1 Major) |

---

## 👤 Tentang Kandidat

**Meiman Ryland Hulu**
QA Engineer dengan pengalaman di pengujian aplikasi mobile (iOS & Android), API testing, dan automation. Terbiasa bekerja dalam ekosistem agile dengan pendekatan risk-based testing dan shift-left quality mindset.

---

*Terima kasih telah meluangkan waktu untuk mereview submission ini. Untuk pertanyaan atau diskusi lebih lanjut, silakan hubungi saya langsung.*
