# NataR Studio — Portofolio & Sistem Manajemen Proyek

Aplikasi web (HTML + Tailwind CDN, tanpa build) untuk studio arsitektur **NataR
(PT Nata Ruang Nusantara)**: portofolio karya, halaman tiap proyek, dan workspace
manajemen (WBS, Gantt, Izin, Agenda, Risk, **Folder/penyimpanan file**) — semua
dapat diedit langsung. Mendukung **kolaborasi real-time** antar perangkat/tim
(opsional, via Firebase), dengan fallback otomatis ke mode lokal.

## Isi folder
```
natar-management/
├── index.html              # Aplikasi utama: Works · About · Tools · Contact · Kelola
├── Tools.html              # Kalkulator ROI Properti + Kalkulator Kapasitas Parkir
├── parking-calculator.html # Kalkulator parkir versi STANDALONE (100% browser, tanpa server)
├── assets/img/             # logo / gambar kecil teroptimasi (.webp) — opsional
├── _source/                # file lama (referensi, tidak dipublish)
├── README.md
└── .gitignore
```

## Menjalankan lokal
Buka lewat **server lokal** (bukan klik-ganda), agar semua fitur normal:
```powershell
python -m http.server 8123      # dari dalam folder ini → http://localhost:8123/index.html
```
Atau ekstensi **Live Server** di VS Code.

## Publish ke GitHub Pages (URL publik)
1. `git init` → `git add .` → `git commit -m "init"` → `git branch -M main`
2. Buat repo kosong (Public) di GitHub, lalu:
   `git remote add origin https://github.com/USERNAME/natar-management.git` → `git push -u origin main`
3. Repo → **Settings → Pages** → Source: *Deploy from a branch* → **main / (root)**.
4. URL: `https://USERNAME.github.io/natar-management/`

---

## ☁️ Mengaktifkan KOLABORASI REAL-TIME tim (Firebase) — penting

Tanpa langkah ini, tiap perangkat menyimpan datanya sendiri (localStorage) — **tidak
sinkron**. Aktifkan Firebase (gratis) agar seluruh tim berbagi & menyunting data
yang sama secara real-time, plus **unggah/unduh file proyek dengan riwayat versi**.

**Langkah (sekali, ±10 menit):**
1. Buka <https://console.firebase.google.com> → **Add project** (nama bebas, mis. `natar-studio`).
2. **Build → Firestore Database → Create database** → mode *production* → pilih lokasi (mis. `asia-southeast2` Jakarta).
3. **Build → Storage → Get started** (untuk unggah file).
4. **Build → Authentication → Get started → Sign-in method → Anonymous → Enable.**
5. **Project settings (⚙) → General → Your apps → Web (</>)** → daftarkan app → salin objek `firebaseConfig`.
6. Buka **`index.html`**, cari blok `window.NATAR_FIREBASE_CONFIG = {…}` (di dalam `<head>`),
   ganti semua nilai `PASTE_...` dengan milik Anda. Simpan, commit, push.
7. Buka situs → chip kanan-atas berubah jadi **hijau "Tim ·"** = tersinkron. Klik chip untuk mengisi nama Anda.

**Aturan keamanan (Rules).** Untuk uji coba tim internal, set rules berikut
(Firestore → Rules, dan Storage → Rules) agar hanya anggota yang login (anonim) bisa baca/tulis:
```
// Firestore Rules
rules_version = '2';
service cloud.firestore {
  match /databases/{db}/documents {
    match /teams/{team}/{document=**} { allow read, write: if request.auth != null; }
  }
}
```
```
// Storage Rules
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} { allow read, write: if request.auth != null; }
  }
}
```
> Untuk keamanan lebih ketat (mis. hanya email tim tertentu), naikkan dari Anonymous
> ke Google/Email Sign-in lalu batasi rules berdasarkan `request.auth.uid`/email.

**Catatan kolaborasi:**
- Sinkron per-proyek (bukan satu blob), jadi edit di proyek berbeda tidak saling menimpa.
- Bila dua orang menyunting **proyek yang sama** bersamaan, berlaku *last-write-wins* (tersimpan terakhir menang). Untuk tim kecil ini memadai.
- Gambar **cover** disimpan sebagai data (base64) di dalam dokumen proyek. Jaga ukurannya kecil (otomatis dikecilkan ~1200px). Untuk render besar/banyak, lebih baik host di CDN (lihat bawah) dan rujuk lewat URL.

## 🗂️ Folder proyek = penyimpanan aktif (bukan sekadar tampilan)
Setelah Firebase aktif, tab **Folder** di tiap proyek menjadi manajer file:
unggah file ke folder standar (00_ADMIN … 09_SERAH_TERIMA, 00_ARCHIVE), unduh,
hapus, dan **riwayat versi otomatis** (unggah ulang nama sama = versi baru, versi
lama tetap tersimpan & bisa diunduh). Semua terlihat seluruh tim secara real-time.

> **Kenapa bukan GitHub untuk file proyek?** GitHub menolak file > 100 MB, repo
> membengkak permanen oleh biner (gambar/video), dan butuh token (tidak aman di
> situs statis). Firebase Storage (atau Cloudinary/Bunny/S3) jauh lebih tepat untuk
> drawing, PDF, dan render. Simpan **master** video/render di Drive/DAM; taruh
> turunan teroptimasi di CDN; rujuk via URL.

---

## Halaman & info kontak
**Contact** (`#/contact`): Jl. Sulaksana No. 12a, Kec. Kiaracondong, Kota Bandung,
Jawa Barat, Indonesia · ✉ contact.natar@gmail.com · ☎ 0881023993341 · plus peta.

## Kalkulator Parkir (standalone)
`parking-calculator.html` adalah engine SRP (Kepdirjen Hubdat 1996/1998 · PP 16/2021)
yang **ditulis ulang 100% di JavaScript** — tidak perlu backend FastAPI lagi,
langsung jalan di GitHub Pages. Tools.html menampilkannya via iframe. Kalkulator ROI
juga sepenuhnya berjalan di browser.

## Strategi aset media (ringkas)
- Gambar: kompres + **WebP/AVIF**, beberapa ukuran, `loading="lazy"`. Yang kecil boleh di `assets/img/`.
- Video animasi: **jangan** di Git → **Vimeo/YouTube** (embed) atau Cloudflare/Bunny Stream.
- CDN gambar/video: **Cloudinary** (transformasi otomatis), Cloudflare R2/Images, Bunny.net.

## Teknologi
HTML5 · Tailwind CSS (CDN) · Jost + Inter · Vanilla JS · localStorage · Firebase (opsional).
Tanpa build — cukup file statis.

---
© PT Nata Ruang Nusantara — NataR Studio.
