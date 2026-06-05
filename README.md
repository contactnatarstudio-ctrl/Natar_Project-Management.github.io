# NataR Studio — Portofolio & Sistem Manajemen Proyek

Aplikasi web satu-file (HTML + Tailwind CDN, tanpa build) untuk studio arsitektur
**NataR (PT Nata Ruang Nusantara)**. Menggabungkan portofolio karya, halaman
detail tiap proyek, dan workspace manajemen proyek (WBS, Timeline Gantt, Checklist
Izin, Agenda Meeting, Risk Register, Struktur Folder) — semuanya bisa diedit
langsung tanpa menyentuh kode.

## Isi folder
```
natar-management/
├── index.html      # Aplikasi utama: Works (portofolio) · About · Tools · Kelola
├── Tools.html      # Kalkulator ROI Properti + Kalkulator Kapasitas Parkir
├── assets/
│   └── img/        # tempat logo / gambar kecil teroptimasi (.webp) — opsional
├── _source/        # file lama (referensi, TIDAK dipublish)
├── README.md
└── .gitignore
```

## Cara menjalankan secara lokal
Buka lewat **server lokal** (bukan klik-ganda file), agar semua fitur normal:
```powershell
# dari dalam folder ini
python -m http.server 8123
# lalu buka: http://localhost:8123/index.html
```
Atau pakai ekstensi **Live Server** di VS Code.

## Cara publish ke GitHub Pages (dapat URL publik)
1. `git init` → `git add .` → `git commit -m "init"` → `git branch -M main`
2. Buat repo kosong di GitHub (Public), lalu:
   `git remote add origin https://github.com/USERNAME/natar-management.git`
   `git push -u origin main`
3. Repo → **Settings → Pages** → Source: *Deploy from a branch* → **main / (root)**.
4. URL publik: `https://USERNAME.github.io/natar-management/`
   (halaman Tools: `…/natar-management/Tools.html`)

## Catatan penting
- **Data tersimpan di browser (localStorage).** Tambah/edit/hapus proyek lewat
  tab **Kelola**. Untuk backup atau pindah perangkat, gunakan **Export JSON**, lalu
  **Import JSON** di tempat lain. Reset mengembalikan ke 6 proyek contoh.
- **Cover gambar** saat ini disimpan sebagai data di browser. Untuk produksi
  dengan banyak/berat gambar, host gambar di CDN (mis. Cloudinary) dan isi cover
  dengan URL-nya agar situs tetap ringan.
- **Halaman Tools → Kalkulator Parkir** memerlukan *compute engine* FastAPI yang
  berjalan. Di situs statis (GitHub Pages) bagian parkir akan menampilkan
  "engine offline" sampai backend dideploy terpisah (Render/Railway/Fly) dan:
  - field **URL Compute Engine** diarahkan ke URL backend (HTTPS), dan
  - **CORS** backend mengizinkan origin halaman (mis. `https://USERNAME.github.io`).
  Kalkulator **ROI** berjalan penuh tanpa backend.

## Teknologi
HTML5 · Tailwind CSS (CDN) · Google Fonts (Jost + Inter) · Vanilla JS · localStorage.
Tanpa langkah build — cukup file statis.

---
© PT Nata Ruang Nusantara — NataR Studio. Prototipe sistem internal.
