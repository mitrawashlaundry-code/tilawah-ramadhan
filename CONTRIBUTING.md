# Panduan Kontribusi

Terima kasih sudah tertarik berkontribusi pada **Target Tilawah Ramadhan**! 🌙

---

## 📋 Daftar Isi

- [Code of Conduct](#code-of-conduct)
- [Cara Melaporkan Bug](#cara-melaporkan-bug)
- [Cara Mengusulkan Fitur](#cara-mengusulkan-fitur)
- [Setup Development](#setup-development)
- [Alur Kontribusi Kode](#alur-kontribusi-kode)
- [Konvensi Kode](#konvensi-kode)
- [Konvensi Commit](#konvensi-commit)

---

## Code of Conduct

Proyek ini menjaga lingkungan yang ramah dan inklusif. Semua kontributor diharapkan:
- Bersikap hormat dan membangun dalam diskusi
- Menerima kritik konstruktif dengan lapang dada
- Fokus pada apa yang terbaik untuk komunitas pengguna

---

## Cara Melaporkan Bug

1. Pastikan bug belum dilaporkan di [Issues](https://github.com/mitrawashlaundry-code/tilawah-ramadhan/issues)
2. Buat issue baru dengan template berikut:

```
**Deskripsi Bug**
Penjelasan singkat apa yang salah

**Langkah Reproduksi**
1. Buka ...
2. Klik ...
3. Lihat error di ...

**Perilaku yang Diharapkan**
Apa yang seharusnya terjadi

**Perilaku Aktual**
Apa yang benar-benar terjadi

**Environment**
- Browser: Chrome 120 / Safari 17 / Firefox 121
- OS: Android 14 / iOS 17 / Windows 11
- PWA: Ya / Tidak (dari browser)
- Versi App: v1.5.0 (tertera di footer Pengaturan)
```

---

## Cara Mengusulkan Fitur

1. Buka [Issues](https://github.com/mitrawashlaundry-code/tilawah-ramadhan/issues) → New Issue → "Feature Request"
2. Jelaskan:
   - **Masalah yang diselesaikan**: "Saya selalu kesulitan karena..."
   - **Solusi yang diusulkan**: "Akan lebih baik jika..."
   - **Alternatif yang dipertimbangkan** (jika ada)

### Prioritas Fitur
Fitur akan diprioritaskan berdasarkan:
- Jumlah user yang membutuhkan (upvote di issue)
- Kesesuaian dengan misi aplikasi (sederhana & efektif untuk non-teknis)
- Kompleksitas implementasi tanpa merusak single-file architecture

---

## Setup Development

### Requirements
- Browser modern (Chrome/Firefox/Safari)
- Text editor (VS Code, Sublime, dll.)
- Git

### Langkah Setup

```bash
# 1. Fork dan clone repository
git clone https://github.com/YOUR_USERNAME/tilawah-ramadhan.git
cd tilawah-ramadhan

# 2. Jalankan server lokal (pilih salah satu)
python3 -m http.server 8080
# atau
npx serve .
# atau cukup buka index.html di browser

# 3. Buka di browser
# http://localhost:8080
```

### Tidak Perlu
- ❌ `npm install`
- ❌ Build process
- ❌ Environment variables
- ❌ Database setup

---

## Alur Kontribusi Kode

```bash
# 1. Pastikan fork kamu up-to-date
git fetch upstream
git checkout main
git merge upstream/main

# 2. Buat branch baru
git checkout -b feat/nama-fitur
# atau
git checkout -b fix/nama-bug

# 3. Edit index.html
# ... lakukan perubahan ...

# 4. Test manual (lihat checklist di bawah)

# 5. Commit
git add index.html
git commit -m "feat: tambahkan kalender visual Ramadhan"

# 6. Push
git push origin feat/nama-fitur

# 7. Buat Pull Request di GitHub
```

### Checklist Sebelum Pull Request

- [ ] Aplikasi terbuka tanpa error di console browser
- [ ] Setup wizard bisa diselesaikan (step 1-3)
- [ ] Input log berfungsi dan tersimpan ke localStorage
- [ ] Tidak ada `console.error` baru
- [ ] Tidak ada penggunaan `eval()` atau `new Function(string)` baru
- [ ] Ukuran font tidak ada yang di bawah 11px (cek dengan DevTools)
- [ ] Semua text penting punya atribut `aria-label`
- [ ] Export CSV terbuka dengan benar di Excel (tidak ada kolom yang rusak)
- [ ] Responsif di layar 360px dan 428px
- [ ] File `index.html` tetap satu file tunggal (tidak pecah jadi beberapa file)

---

## Konvensi Kode

### JavaScript

```javascript
// ✅ BAIK — function reference, bukan string
showModal('Judul', 'Body', 'OK', doSomething, 'Batal', closeModal);

// ❌ HINDARI — string sebagai callback (XSS risk)
showModal('Judul', 'Body', 'OK', 'doSomething()', 'Batal', 'closeModal()');

// ✅ BAIK — event delegation
container.querySelectorAll('.item').forEach((el, i) => {
  el.addEventListener('click', () => handler(i));
});

// ❌ HINDARI — inline onclick di innerHTML
container.innerHTML = `<div onclick="handler(${i})">...</div>`;

// ✅ BAIK — escape semua field CSV
rows.push([date, prayer, pages, pos].map(csvEscape).join(','));

// ❌ HINDARI — hanya sebagian yang di-escape
rows.push([date, prayer, `"${pos}"`].join(','));
```

### CSS

```css
/* ✅ BAIK — gunakan CSS variables */
.element { color: var(--gold2); font-size: 0.82rem; }

/* ❌ HINDARI — hardcode warna */
.element { color: #f5d06e; }

/* ✅ BAIK — font minimum 11px (0.69rem @ 16px base) */
.nav-label { font-size: 0.69rem; }

/* ❌ HINDARI — font terlalu kecil */
.tiny-text { font-size: 0.5rem; } /* 8px — tidak terbaca */
```

### HTML Accessibility

```html
<!-- ✅ BAIK -->
<button aria-label="Hapus catatan tilawah" onclick="deleteLog()">🗑</button>
<input aria-label="Jumlah halaman yang dibaca" type="number">

<!-- ❌ KURANG BAIK — tidak ada label yang jelas -->
<button onclick="deleteLog()">🗑</button>
```

---

## Konvensi Commit

Format: `type: deskripsi singkat`

| Type | Digunakan Untuk |
|------|----------------|
| `feat` | Fitur baru |
| `fix` | Perbaikan bug |
| `style` | Perubahan CSS/UI tanpa mengubah logika |
| `refactor` | Refaktor kode tanpa bug fix atau fitur baru |
| `docs` | Perubahan dokumentasi saja |
| `a11y` | Perbaikan aksesibilitas |
| `perf` | Optimasi performa |
| `security` | Perbaikan keamanan |

### Contoh
```
feat: tambahkan kalender visual 5x6 untuk streak
fix: streak counter tidak konsisten di hari terakhir Ramadhan
style: perbesar streak dots dari 16px ke 24px
fix(a11y): tambahkan aria-label ke semua tombol ikon
security: ganti new Function() dengan function reference di showModal
docs: perbarui README dengan instruksi deploy
```

---

## ❓ Pertanyaan?

Buka [Discussion](https://github.com/mitrawashlaundry-code/tilawah-ramadhan/discussions) atau buat Issue dengan label `question`.

Jazakallahu khairan atas kontribusinya! 🌙
