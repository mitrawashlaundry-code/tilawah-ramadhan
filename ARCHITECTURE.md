# Dokumentasi Arsitektur Teknis

**Target Tilawah Ramadhan v1.5.0**  
Single-file Progressive Web Application

---

## Gambaran Umum

Aplikasi ini adalah **single-file PWA** — seluruh logika (HTML, CSS, JavaScript) berada dalam satu file `index.html` sekitar 3.900 baris. Pendekatan ini dipilih untuk:

1. **Kemudahan deployment** — upload 1 file ke GitHub Pages, selesai
2. **Kemudahan sharing** — kirim 1 file via WhatsApp/Drive, langsung jalan
3. **Tidak ada dependency** — tidak perlu npm, build tools, atau server
4. **Privasi** — tidak ada data yang dikirim ke server manapun

---

## Struktur File

```
index.html
├── <head>
│   ├── Meta tags (PWA manifest inline, apple-touch-icon SVG inline)
│   ├── Google Fonts (DM Sans + Amiri)
│   └── <style> — ~1.400 baris CSS dengan CSS Variables
│
├── <body>
│   ├── #welcomeScreen     — Landing page awal
│   ├── #setupWizard       — 3-step onboarding wizard
│   ├── #mainApp           — Aplikasi utama
│   │   ├── .app-header    — Header sticky dengan progress mini-bar
│   │   ├── #section-home  — Tab Beranda
│   │   ├── #section-input — Tab Catat
│   │   ├── #section-history — Tab Riwayat
│   │   ├── #section-share   — Tab Bagikan
│   │   └── #section-settings — Tab Pengaturan
│   ├── .bottom-nav        — Navigasi bawah
│   ├── .fab               — Floating Action Button
│   ├── #inputSheet        — Bottom sheet input via FAB
│   ├── #shareSheet        — Bottom sheet share options
│   ├── #modalBackdrop     — Modal universal
│   └── #toast             — Toast notification
│
└── <script>
    ├── CONSTANTS          — APP_VERSION, PRAYERS, PRAYER_WEIGHTS, BADGES, dll.
    ├── STATE (S)          — Objek state tunggal
    ├── FONT SIZE          — Level 0/1/2
    ├── DATE HELPERS       — localDate(), getHijriYear(), dll.
    ├── STORAGE            — save(), load() ke localStorage
    ├── HELPERS            — el(), setText(), csvEscape(), dll.
    ├── ACTIVE INPUT DATE  — getActiveInputDate(), setActiveInputDate()
    ├── ONBOARDING         — startOnboarding(), wizNext(), finalizeSetup()
    ├── LAUNCH             — launchMainApp(), unlockNav()
    ├── NAVIGATION         — showSection(), toggleHomeDetails()
    ├── MASTER UI UPDATE   — updateAllUI()
    ├── HEADER             — updateHeader()
    ├── HOME SECTION       — updateHomeSection(), renderStreakBar(), dll.
    ├── INPUT SECTION      — updateInputSection(), addLog(), quickInput()
    ├── ADD LOG            — doAddLog(), deleteLog()
    ├── HISTORY            — renderHistory(), renderChart(), renderTrendChart()
    ├── BADGES             — checkBadges(), renderBadges(), getStreakCount()
    ├── CELEBRATION        — showCelebration(), closeCelebration()
    ├── INPUT SHEET        — openInputSheet(), addLogFromSheet()
    ├── SHARE CANVAS       — renderShareCanvas(), shareProgressCard()
    ├── BACKUP & RESTORE   — backupData(), restoreData()
    ├── BACKFILL BANNER    — checkBackfillBanner(), dismissBackfill()
    ├── REMINDERS          — initReminders(), renderReminders(), toggleReminder()
    ├── EXPORT CSV         — exportCSV('detail'|'summary')
    ├── BREAKDOWN EDIT     — showEditBreakdown(), applyCustomBreakdown()
    ├── SETTINGS           — renderSettings(), calcCatchUp(), showEditSetup()
    ├── MODAL              — showModal(), closeModal()
    ├── TOAST              — showToast()
    ├── AUTOCOMPLETE       — onPosInput(), onPosKey(), selectSurah()
    └── INIT               — init()
```

---

## State Management

### Global State Object `S`

```typescript
interface AppState {
  // Setup
  userName: string;
  startDate: string;          // "YYYY-MM-DD"
  khatamTarget: number;       // 1–10
  totalTarget: number;        // khatamTarget × 604
  dailyTarget: number;        // ⌈totalTarget ÷ 30⌉
  modeSet: boolean;

  // Progress
  totalRead: number;
  currentKhatam: number;

  // Logs
  dailyLogs: {
    [date: string]: LogEntry[];
  };

  // Prayer Breakdown
  breakdown: { [prayer: string]: number };
  customBreakdown: { [prayer: string]: number } | null;

  // Features
  reminders: Reminder[];
  earnedBadges: string[];
  yearHistory: YearRecord[];
}

interface LogEntry {
  id: number;           // timestamp
  prayer: string;       // 'Subuh' | 'Dhuha' | 'Dzuhur' | 'Ashar' | 'Maghrib' | 'Tarawih'
  pages: number;
  pos: string;          // posisi baca (nama surah, opsional)
  time: string;         // "HH:MM" waktu catat
}
```

### Persistensi

State disimpan ke `localStorage` dengan key `tilawahV3` setiap kali ada perubahan:

```javascript
function save() { localStorage.setItem('tilawahV3', JSON.stringify(S)); }
function load() {
  const d = localStorage.getItem('tilawahV3');
  if (d) S = { ...S, ...JSON.parse(d) }; // spread merge — toleran versi berbeda
}
```

**Mengapa `tilawahV3`?** Key versi memungkinkan migrasi data dari versi lama tanpa konflik.

---

## Algoritma Kritis

### 1. Distribusi Target Per Sholat

```javascript
function calcBreakdown(daily) {
  const MAGHRIB_MAX = 6; // Waktu Maghrib terbatas (berbuka puasa)
  const totalW = Object.values(PRAYER_WEIGHTS).reduce((a,b) => a+b, 0); // = 9.0
  const bd = {};
  let assigned = 0;

  PRAYERS.forEach((p, i) => {
    let pg;
    if (i === PRAYERS.length - 1) {
      pg = Math.max(0, daily - assigned); // Tarawih = sisa (fix rounding drift)
    } else {
      pg = Math.max(0, Math.round((PRAYER_WEIGHTS[p.name] / totalW) * daily));
    }
    bd[p.name] = pg;
    assigned += pg;
  });

  // Cap Maghrib & redistribusi excess ke Tarawih
  if (bd['Maghrib'] > MAGHRIB_MAX) {
    const excess = bd['Maghrib'] - MAGHRIB_MAX;
    bd['Maghrib'] = MAGHRIB_MAX;
    bd['Tarawih'] = Math.max(0, (bd['Tarawih'] || 0) + excess);
  }

  return bd;
}
```

**Contoh untuk target 20 hal/hari:**
| Sholat | Bobot | Kalkulasi | Hasil |
|--------|-------|-----------|-------|
| Subuh | 2.5 | 2.5/9 × 20 = 5.6 → 6 | 6 hal |
| Dhuha | 1.0 | 1/9 × 20 = 2.2 → 2 | 2 hal |
| Dzuhur | 1.0 | 1/9 × 20 = 2.2 → 2 | 2 hal |
| Ashar | 1.0 | 1/9 × 20 = 2.2 → 2 | 2 hal |
| Maghrib | 1.5 | 1.5/9 × 20 = 3.3 → 3 | 3 hal (< max 6) |
| Tarawih | 2.0 | sisa = 20-15 = 5 | 5 hal |

### 2. Unified Streak Counter (BUG-003 Fix)

```javascript
// mode: 'above' = hanya hari di atas target | 'any' = semua hari ada bacaan
function getStreakCount(mode) {
  const todayStr = localDate();
  let streak = 0, cur = 0;
  for (let i = 0; i < DAYS_RAMADHAN; i++) {
    const dt = new Date(S.startDate + 'T00:00:00');
    dt.setDate(dt.getDate() + i);
    const ds = dt.toLocaleDateString('en-CA');
    if (ds > todayStr) break;
    const pages = getTodayPages(ds);
    const qualifies = mode === 'above' ? pages >= S.dailyTarget : pages > 0;
    if (qualifies) { cur++; streak = Math.max(streak, cur); }
    else { cur = 0; }
  }
  return streak;
}
```

Fungsi ini digunakan oleh `checkBadges()` dan `renderStreakBar()` — memastikan angka streak selalu konsisten.

### 3. CSV Escaping (BUG-004 Fix)

```javascript
function csvEscape(val) {
  const str = String(val == null ? '' : val);
  // Wrap in quotes jika ada koma, kutip, atau newline
  if (str.includes(',') || str.includes('"') || str.includes('\n')) {
    return '"' + str.replace(/"/g, '""') + '"'; // double-quote escape per RFC 4180
  }
  return str;
}
```

### 4. Smart Prayer Suggestion

```javascript
function getSuggestedPrayer() {
  const h = new Date().getHours();
  if (h >= 3 && h < 7) return 'Subuh';
  if (h >= 7 && h < 11) return 'Dhuha';
  if (h >= 11 && h < 15) return 'Dzuhur';
  if (h >= 15 && h < 18) return 'Ashar';
  if (h >= 18 && h < 20) return 'Maghrib';
  return 'Tarawih'; // default malam hari
}
```

---

## Keamanan

### Masalah yang Diperbaiki di v1.5.0

**BUG-001: `new Function(string)` → Function Reference**

Sebelum (v1.4.0):
```javascript
// BERBAHAYA: eksekusi string sembarang
b.onclick = new Function(fn1); // fn1 = 'doResetProgress()'
```

Sesudah (v1.5.0):
```javascript
// AMAN: function reference langsung
showModal('Judul', 'Body', 'OK', doSomething, 'Batal', closeModal);
// showModal menerima Function, bukan string
if (typeof fn1 === 'function') b.onclick = fn1;
```

**Inline onclick di innerHTML → Event Delegation**

Sebelum:
```javascript
drop.innerHTML = results.map((s, i) =>
  `<div onmousedown="selectSurah(${i})">...</div>`
).join('');
```

Sesudah:
```javascript
drop.innerHTML = results.map((s, i) =>
  `<div class="ac-item" data-idx="${i}">...</div>`
).join('');
drop.querySelectorAll('.ac-item').forEach((el, i) => {
  el.addEventListener('mousedown', () => selectSurah(inputId, results[i].n, dropId));
});
```

---

## Pengujian

Tidak ada test framework yang digunakan. Testing dilakukan manual dengan checklist:

### Matrix Browser Support

| Browser | Desktop | Mobile | PWA Install |
|---------|---------|--------|-------------|
| Chrome 100+ | ✅ | ✅ | ✅ |
| Firefox 100+ | ✅ | ✅ | ⚠️ (PWA terbatas) |
| Safari 15+ | ✅ | ✅ | ✅ (via homescreen) |
| Samsung Internet | ✅ | ✅ | ✅ |
| Edge 100+ | ✅ | ✅ | ✅ |

### Known Limitations

1. **iOS Notifications**: Hanya berfungsi jika app dipasang di homescreen (iOS 16.4+)
2. **localStorage Limit**: ~5MB per origin — cukup untuk ~10 tahun data tilawah harian
3. **Google Fonts**: Memerlukan koneksi internet saat pertama kali load; setelah itu ter-cache browser
4. **No true offline**: Service Worker tidak diimplementasikan; app perlu dimuat sekali sebelum bisa offline

---

## Kontribusi ke Arsitektur

### Menambah Tab Baru

```javascript
// 1. Tambah tombol nav di HTML
<button class="nav-item" id="nav-newtab" onclick="showSection('newtab')">
  <span class="nav-icon">🆕</span>
  <span class="nav-label">Tab Baru</span>
</button>

// 2. Tambah section di HTML
<div id="section-newtab" class="section">
  <!-- konten tab -->
</div>

// 3. Tambah handler di showSection()
if (id === 'newtab') { renderNewTab(); }

// 4. Tambah ke unlockNav() jika perlu dikunci
['input','history','share','newtab'].forEach(id => { ... });
```

### Menambah Badge Baru

```javascript
// 1. Tambah ke array BADGES
const BADGES = [
  // ...existing...
  {
    id: 'badge_id',
    emoji: '🆕',
    name: 'Nama Badge',
    desc: 'Deskripsi singkat',
    criterion: 'Syarat yang harus dipenuhi (untuk modal info)'
  }
];

// 2. Tambah logika di checkBadges()
function checkBadges() {
  // ...existing checks...
  if (kondisiTerpenuhi && !earned.includes('badge_id')) earned.push('badge_id');
}
```

---

## Migrasi Storage di Masa Depan

Jika perlu migrasi dari `tilawahV3` ke `tilawahV4`:

```javascript
function load() {
  try {
    // Coba load versi terbaru
    const d = localStorage.getItem('tilawahV4');
    if (d) { S = { ...S, ...JSON.parse(d) }; return; }

    // Fallback: migrate dari V3
    const old = localStorage.getItem('tilawahV3');
    if (old) {
      const oldData = JSON.parse(old);
      S = { ...S, ...oldData, newField: defaultValue };
      save(); // simpan dalam format baru
      localStorage.removeItem('tilawahV3'); // hapus versi lama
    }
  } catch(e) { /* silent */ }
}
```
