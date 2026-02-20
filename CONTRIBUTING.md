# 🤝 Panduan Kontribusi

Terima kasih telah tertarik untuk berkontribusi pada **Target Tilawah Ramadhan**!

🌐 **Live:** https://mitrawashlaundry-code.github.io/tilawah-ramadhan/

---

## Cara Berkontribusi

### 1. Fork & Clone
```bash
git clone https://github.com/USERNAME_KAMU/tilawah-ramadhan.git
cd tilawah-ramadhan
```

### 2. Buat Branch & Kembangkan
```bash
git checkout -b feat/export-csv
python3 -m http.server 8000   # test lokal di http://localhost:8000
```

### 3. Commit & Pull Request
```bash
git commit -m "feat: tambah export data ke CSV"
git push origin feat/export-csv
# Buka GitHub → buat Pull Request
```

---

## Panduan Kode

### ✅ DO — Yang Harus Dilakukan

```javascript
// 1. Null-safe DOM access
function el(id) { return document.getElementById(id); }
function setText(id, val) { const e = el(id); if (e) e.textContent = val; }

// 2. Gunakan _activeInputDate, bukan DOM logDate.value
const dateStr = getActiveInputDate(); // ✅

// 3. updateAllUI() setelah setiap perubahan data
save(); updateAllUI(); // ✅

// 4. calcBreakdown() dengan cap Maghrib
function calcBreakdown(daily) {
  // ... hitung bobot ...
  if (bd['Maghrib'] > MAGHRIB_MAX) {
    bd['Tarawih'] += bd['Maghrib'] - MAGHRIB_MAX;
    bd['Maghrib'] = MAGHRIB_MAX;
  }
  return bd;
}

// 5. renderPrayerPreview WAJIB pakai calcBreakdown
function renderPrayerPreview(containerId, daily) {
  const bd = calcBreakdown(daily); // ✅
}

// 6. Fitur delete selalu perlu konfirmasi modal
function confirmDeleteReminder(id) {
  showModal('Hapus?', '...', 'Hapus', `deleteReminder(${id})`, 'Batal', 'closeModal()');
}
function deleteReminder(id) {
  S.reminders = S.reminders.filter(r => r.id !== id);
  save(); closeModal(); renderReminders(); showToast('Dihapus');
}
```

### ❌ DON'T — Yang Harus Dihindari

```javascript
document.getElementById('x').textContent = val;  // ❌ bisa null
const date = document.getElementById('logDate').value; // ❌ state dari DOM
el.value += n;  // ❌ akumulatif, pakai el.value = n
setInterval(() => updateAllUI(), 1000); // ❌ pakai event-driven

// Jangan hitung breakdown manual — bypass cap Maghrib
const pg = Math.round((PRAYER_WEIGHTS[p.name] / totalW) * daily); // ❌ di renderPrayerPreview
```

### Prinsip Desain

1. **Single File** — semua dalam 1 `index.html`
2. **Offline First** — tidak ada network request saat runtime
3. **Null Safe** — setiap akses DOM via helper
4. **No Framework** — vanilla JS murni
5. **Bahasa Indonesia** — semua UI dalam Bahasa Indonesia
6. **Mobile First** — desain untuk layar kecil
7. **Kontrol Penuh** — fitur hapus wajib ada di semua daftar yang bisa bertambah

---

## Konvensi Commit

```
feat: tambah export data ke CSV
fix: cap halaman Maghrib maksimal 6, redistribusi ke Tarawih
fix: tambah tombol hapus pengingat dengan konfirmasi
style: perbesar huruf kini berlaku menyeluruh
docs: update README dengan ikon homescreen
chore: update index.html v1.3.0
```

---

## Checklist PR

- [ ] Tidak ada error di browser console
- [ ] `calcBreakdown()` masih menerapkan cap Maghrib + redistribusi Tarawih
- [ ] `renderPrayerPreview()` masih pakai `calcBreakdown()`
- [ ] Fitur hapus (reminder/log) masih menggunakan konfirmasi modal
- [ ] Font size toggle masih mencakup semua kelas teks
- [ ] `index.html` masih single file

---

Jazakallahu khairan atas kontribusinya! 🌙
