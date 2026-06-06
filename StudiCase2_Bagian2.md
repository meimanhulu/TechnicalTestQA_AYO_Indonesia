# Studi Kasus 2 — Bagian 2: Analisis Aplikasi Mobile AYO (iOS)

> **Project:** AYO Indonesia — Super Sport Community App
> **QA Engineer:** Meiman Ryland Hulu
> **Device:** iPhone (iOS)
> **Versi Dokumen:** 1.0
> **Tanggal:** 5 Juni 2026

---

## Daftar Isi

- [1. Ringkasan Ekosistem Aplikasi](#1-ringkasan-ekosistem-aplikasi)
- [2. Area yang Perlu Diuji (Prioritas)](#2-area-yang-perlu-diuji-prioritas)
- [3. Mekanisme Pengujian](#3-mekanisme-pengujian)
- [4. Test Case Spesifik](#4-test-case-spesifik)
- [5. Bug yang Ditemukan Selama Eksplorasi](#5-bug-yang-ditemukan-selama-eksplorasi)
- [6. Rekomendasi Improvement](#6-rekomendasi-improvement)
- [7. Kesimpulan](#7-kesimpulan)

---

## 1. Ringkasan Ekosistem Aplikasi

Berdasarkan eksplorasi mendalam aplikasi AYO di iOS, aplikasi ini memiliki arsitektur fitur yang sangat kompleks dan modular:

### 1.1 Fitur Utama yang Teridentifikasi

| Modul | Sub-Fitur | Keterangan |
|-------|-----------|------------|
| **Home** | Search, Booking Venue, Main Bareng, Create GAMETIME, Sparring, Komunitas, Kompetisi, Leaderboard, Banner Carousel, Venue Listing, Sparring Team Matching, Challenge Popup | Entry point utama user |
| **Dashboard** | Saldo, Main Bareng, GameTime (Baru), Absensi, Kelola Komunitas, Kompetisi, Booking Lapangan, Membership, MatchCam Hub (beta), Ajak Teman | Central hub aktivitas user |
| **Saldo / Wallet** | Saldo saat ini, Cairkan ke Rekening, Riwayat Saldo (filter periode & jenis transaksi: Pemasukan Event, Refund, Pencairan) | Financial management |
| **GameTime** | Buat GameTime, Cari GameTime, Urutkan, Filter Cabang Olahraga | Event management |
| **Pertandingan** | Cari Pertandingan, Filter Cabang Olahraga, Kategori (Individual: Tunggal/Ganda, Komunitas), Status (Menunggu Lawan, Siap Tanding, Selesai, Dibatalkan), Buat Sparring | Match making |
| **Kompetisi** | Cari Kompetisi, Tab Peserta/Host, Filter Status (Draft, Published, Scheduled, Berjalan, Selesai, Dibatalkan), Filter Cabang Olahraga, Buat Kompetisi (Tunggal/Grup) | Tournament management |
| **Kelola Komunitas** | Sebagai Pemain/Admin, Cari & Gabung Komunitas, Buat Komunitas Baru | Community management |
| **Absensi** | Cari Absensi, Buat Absensi Baru, Integrasi Pertandingan | Attendance tracking |
| **Booking Lapangan** | Cari Venue, Quick Filter (sport/lokasi/tanggal/waktu/coaching/membership), Filter Status Booking, Venue Card (rating, favorite, harga per sesi), 200+ venue | Core booking feature |
| **Membership** | Cari Paket, Filter Tipe Paket (Bundling/Langganan), Filter Status (Belum Lunas, Sudah Lunas, Selesai), Beli Paket | Subscription model |
| **MatchCam Hub** | Cari Venue/Event, My Clips, Filter Jadwal & Tanggal & Olahraga, Urutkan | Video/clip marketplace *(beta)* |
| **Ajak Teman** | Kode Referral, Salin, Bagikan Link, Cara Kerja Referral, Saldo Reward, Cara Penggunaan Saldo Reward, Tab Berhasil/Pending | Referral system |
| **Chat** | Tab Direct, Komunitas, Aktivitas, Search Chat | Messaging system |
| **Profil** | Username, Stats (Aktivitas/Tanding/Main Bareng), Skill Level Cards (carousel), Komunitas, Achievement, Aktivitas Terdekat, Ulasan Peserta (Ketepatan Waktu, Komunikasi, Sportivitas), Cabang Olahraga (Edit/Reorder/Delete/Tambah), Tentukan Skill (Newbie/Beginner/Intermediate/Advanced/Pro) | User profile & gamification |
| **Notifikasi** | Tab Update, Tab Admin, Mark All Read, Pengaturan Notifikasi | Notification center |
| **Cabang Olahraga** | 25+ olahraga: Fitness, Running, Games, Badminton, Padel, Tennis, Mini Soccer, Sepak Bola, Basketball, Pickleball, Futsal, Billiard, Tenis Meja, Squash, Volley, Golf, Baseball, Softball, Hockey, Recovery, Pilates, Yoga, Airsoft Gun, Fishing, Beach Tennis | Multi-sport support |

---

## 2. Area yang Perlu Diuji (Prioritas)

### 2.1 Critical Path — Wajib 100% Pass Sebelum Release

| Priority | Modul | Alasan |
|----------|-------|--------|
| **P1** | **Booking Lapangan** | Core revenue stream — user utama datang untuk booking |
| **P1** | **Payment & Saldo / Wallet** | Money path: DP, full payment, refund, cairkan ke rekening |
| **P1** | **Profil & Skill Level** | Matching algorithm (sparring/kompetisi) bergantung pada skill level; bug blank screen di sini = feature unusable |
| **P1** | **Authentication** | Gatekeeper untuk semua fitur |
| **P1** | **Referral (Ajak Teman)** | Growth engine — reward system harus akurat |
| **P2** | **Chat (Direct/Komunitas/Aktivitas)** | Communication core untuk koordinasi event |
| **P2** | **Pertandingan & Kompetisi** | Community engagement, tournament lifecycle |
| **P2** | **Main Bareng & GameTime** | Social feature, event creation |
| **P2** | **MatchCam Hub** | Beta feature — perlu stability testing |
| **P3** | **Membership** | Subscription revenue, package management |
| **P3** | **Absensi** | Operational feature untuk komunitas |
| **P3** | **Notifikasi** | User engagement, tidak critical untuk revenue |

---

### 2.2 Area Berisiko Tinggi (High Risk)

Berdasarkan kompleksitas UI yang terlihat dari eksplorasi:

| # | Area | Risiko |
|---|------|--------|
| 1 | **Filter & Search Multi-Dimensi** | Hampir setiap halaman punya filter kombinasi (Cabang Olahraga + Kategori + Status + Periode + Tanggal). Risiko: filter stacking error, query tidak valid. |
| 2 | **State Synchronization** | Badge notifikasi "4" tapi halaman kosong. Risiko: state inconsistency antara client dan server. |
| 3 | **Bottom Sheet & Modal Stack** | Banyak fitur menggunakan bottom sheet yang bisa stack (filter di atas filter). Risiko: memory leak, navigation stack corrupt. |
| 4 | **Dynamic Island / Safe Area** | App berjalan di iPhone dengan Dynamic Island. Risiko: content tertutup, touch area tidak responsif. |
| 5 | **Carousel & Scroll Performance** | Skill cards carousel, venue list (200+ item). Risiko: frame drop, memory pressure. |
| 6 | **Beta Feature Stability** | MatchCam Hub masih beta. Risiko: crash, data corruption. |

---

## 3. Mekanisme Pengujian

### 3.1 Functional Testing (Manual + Automation)

| Mekanisme | Scope | Tools | Coverage Target |
|-----------|-------|-------|-----------------|
| **Manual Exploratory Testing** | Semua fitur, edge case, UX judgment | iPhone device fisik | 100% fitur mayor |
| **Automated UI Testing** | Critical path, regression | XCUITest / Appium | Login, Booking, Payment, Profile update |
| **API Contract Testing** | Backend integration | Postman / REST Assured | 100% endpoints |
| **Integration Testing** | Payment gateway, push notif, referral tracking | Manual + mock server | End-to-end flow |

---

### 3.2 Non-Functional Testing

| Mekanisme | Scope | Tools | Target |
|-----------|-------|-------|--------|
| **Performance Testing** | Scroll 200+ venue, load image, carousel swipe | Xcode Instruments, Firebase Performance | FPS > 55, cold start < 3s |
| **Memory Testing** | Bottom sheet stack, image cache, long session | Xcode Memory Graph, LeakSanitizer | No memory leak setelah 30 menit |
| **Network Testing** | Offline mode, slow 3G, network switch | Network Link Conditioner, Charles Proxy | Graceful degradation |
| **Security Testing** | API auth, referral tampering, payment | OWASP ZAP, Burp Suite, SSL pinning check | No PII leak, secure payment |
| **Accessibility Testing** | VoiceOver, Dynamic Type, color contrast | Xcode Accessibility Inspector | WCAG 2.1 AA |
| **Compatibility Testing** | iOS 15–17, iPhone SE s/d 15 Pro Max | BrowserStack, real devices | 95% functional parity |

---

### 3.3 Specialized Testing untuk AYO

| Mekanisme | Khusus untuk Fitur | Alasan |
|-----------|-------------------|--------|
| **Concurrency Testing** | Booking lapangan (slot terakhir), join event penuh | Race condition di slot booking |
| **State Sync Testing** | Badge notifikasi vs actual data, saldo vs riwayat | Konsistensi real-time data |
| **Deep Link Testing** | Referral link, share link event | Tracking attribution |
| **Background Mode Testing** | Chat, notifikasi booking reminder | Push notification delivery |
| **Interrupt Testing** | Incoming call, low battery, permission dialog | App stability saat interrupt |

---

## 4. Test Case Spesifik

Telah disusun **114 test cases** spesifik untuk aplikasi iOS AYO, dengan distribusi sebagai berikut:

| Modul | Jumlah TC | Fokus |
|-------|:---------:|-------|
| Home | 12 | Navigation, search, quick menu, venue listing |
| Dashboard | 3 | Menu list, saldo card, tap navigation |
| Saldo / Wallet | 6 | Filter, cairkan, empty state |
| GameTime | 5 | Create, search, filter, empty state |
| Pertandingan | 7 | Search, filter, create sparring, FAB |
| Kompetisi | 7 | Search, tab, filter, create |
| Komunitas | 4 | Tab, join, create, empty state |
| Absensi | 3 | Search, create, integration |
| Booking Lapangan | 5 | Search, quick filter, status filter, venue card |
| Membership | 4 | Search, filter, buy package |
| MatchCam Hub | 7 | Search, filter jadwal/tanggal/olahraga, My Clips |
| Referral | 9 | Kode, salin, share, reward, accordion, tab |
| Chat | 7 | Tab, search, send message, empty state |
| Profil | 13 | Stats, skill cards, cabang olahraga, skill level, rating |
| Notifikasi | 6 | Tab, mark read, settings, badge consistency |
| UI/UX | 6 | Dynamic Island, bottom sheet, loading, empty state, scroll, dark mode |
| Edge Cases | 10 | Badge inconsistency, blank screen, multi-filter, beta stability, dll. |
| **Total** | **114** | |

---

## 5. Bug yang Ditemukan Selama Eksplorasi

### 5.1 BUG-001 — Blank Screen After Skill Confirmation

> **Severity:** 🔴 Critical (S1) | **Priority:** P1

| Detail | Keterangan |
|--------|------------|
| **Bug ID** | BUG-2026-06-AYO-001 |
| **Modul** | Profil › Cabang Olahraga › Tentukan Skill |
| **Steps to Reproduce** | Profil → Tambah Cabang Olahraga → Pilih Badminton → Selanjutnya → Pilih Intermediate → Konfirmasi |
| **Expected Result** | Redirect ke halaman Profil; Badminton • Intermediate muncul di daftar |
| **Actual Result** | Layar putih kosong (blank screen), tidak ada UI, harus force close |
| **Reproducibility** | 100% — Always reproducible |
| **Workaround** | ❌ Tidak ada |
| **Impact** | User tidak bisa menambah cabang olahraga baru; fitur skill-based matching tidak akurat |

**Hypothesis Teknis:**
- Unhandled exception saat API `POST` update skill level
- State management failure setelah mutation (Redux/Bloc corrupt)
- Navigation error — route tujuan tidak valid setelah POST success
- Missing null check saat render updated profile data

---

### 5.2 BUG-002 — Notifikasi Badge Inconsistency

> **Severity:** 🟠 Major (S2) | **Priority:** P1

| Detail | Keterangan |
|--------|------------|
| **Modul** | Home › Notifikasi |
| **Observasi** | Badge icon notifikasi menampilkan angka **"4"**, tetapi saat dibuka — tab Update dan Admin kosong semua |
| **Expected Result** | Badge count = jumlah unread notifikasi yang benar-benar ada |
| **Actual Result** | Badge "4" tetapi tidak ada notifikasi sama sekali |
| **Reproducibility** | Perlu investigasi lebih lanjut |
| **Hypothesis** | Badge count tidak direset setelah notifikasi dibaca di device lain, atau stale cache pada client |

---

## 6. Rekomendasi Improvement

### 6.1 UX Enhancement

| Area | Issue yang Ditemukan | Rekomendasi |
|------|----------------------|-------------|
| **Empty State** | Banyak halaman kosong hanya menampilkan teks | Tambahkan CTA yang lebih prominent (contoh: Chat kosong → *"Mulai Chat dengan Teman Sparringmu"*) |
| **Loading State** | Transisi ke blank putih saat loading | Implementasikan skeleton loader atau shimmer effect |
| **Error State** | Tidak ada error message saat blank screen terjadi | Tambahkan error boundary dengan tombol retry |
| **Filter UX** | Banyak filter modal yang stack bertumpuk | Tampilkan "Applied Filters" chip di atas list untuk visibility |
| **Skill Level** | "Belum Atur Level" tidak ada CTA langsung | Tap pada cabang yang belum diatur langsung redirect ke halaman Tentukan Skill |

---

### 6.2 Performance Enhancement

| Area | Issue yang Ditemukan | Rekomendasi |
|------|----------------------|-------------|
| **Venue List** | 200+ venue ditampilkan tanpa pagination indicator | Implementasikan infinite scroll dengan loading indicator yang jelas |
| **Image Loading** | Venue photos berpotensi lambat di koneksi lemah | Implement lazy loading + image compression (WebP) |
| **Bottom Sheet Stack** | Stack bottom sheet bisa membingungkan user | Batasi stack depth + tambahkan breadcrumb/filter summary |

---

### 6.3 Accessibility Enhancement

| Area | Issue yang Ditemukan | Rekomendasi |
|------|----------------------|-------------|
| **Dynamic Island** | Content berpotensi tertutup Dynamic Island | Audit safe area insets untuk semua screen |
| **Color Contrast** | Badge "Baru" dan "beta" warna merah | Pastikan rasio kontras ≥ 4.5:1 (WCAG AA) |
| **Touch Target** | Icon-icon kecil di area filter | Minimum touch target 44×44pt sesuai Apple HIG |

---

## 7. Kesimpulan

Aplikasi AYO iOS memiliki **ekosistem fitur yang sangat kompleks** — 25+ cabang olahraga, 15+ modul utama, multi-layer filter, financial system, referral system, chat, dan gamification.

### Fokus Testing Utama

| # | Area | Alasan |
|---|------|--------|
| 1 | **Profil & Skill Level** | Bug critical blank screen ditemukan di sini; foundation untuk matching algorithm |
| 2 | **Booking & Payment** | Core revenue path |
| 3 | **State Synchronization** | Badge, saldo, notifikasi harus konsisten |
| 4 | **Filter & Search** | Kombinasi filter yang kompleks rentan error |
| 5 | **Beta Feature (MatchCam Hub)** | Perlu stability monitoring berkelanjutan |

### Mekanisme Pengujian yang Direkomendasikan

Pendekatan terbaik adalah **kombinasi** dari:

- 🔍 **Manual Exploratory Testing** — untuk UX, edge case, dan business logic
- 🤖 **Automated UI Testing (XCUITest)** — untuk critical path regression
- 🔌 **API Contract Testing** — untuk backend integration
- ⚡ **Performance & Memory Testing** — untuk scroll-heavy screens (200+ venue, carousel)
- 📱 **Interrupt & Background Testing** — untuk chat dan notifikasi

---
