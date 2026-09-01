---
date: 2026-09-01
project: [[Project — SOLUTION]]
tags: [session, deploy, payroll, hrms, fix]
---

# 2026-09-01 — Deploy Payroll: Laporan Pinjaman

## Task
Deploy ulang payroll.3putraperkasa.com setelah revisi Sabtu — merge branch `fix/bugs` ke `main` lalu push & deploy.

## Aksi
1. **Pre-work CIEL:** baca [[Project — SOLUTION]] (memori deploy pattern centralized)
2. **Cek repo lokal** `/root/workspace/projects/solution`:
   - `main` lokal di `bf9d4c4`, fetch: `fix/bugs` punya 1 commit baru `4894fe7`
3. **Review diff** `main..origin/fix/bugs`:
   - 5 file, +224/-1
   - `LoanController::laporan()` — method baru: laporan bulanan (sisa, bon, bayar, status lunas/belum)
   - `resources/views/loans/laporan.blade.php` — view baru (128 baris, filter karyawan/bulan/tahun)
   - `resources/views/loans/show.blade.php` — tambah info lokasi & jabatan di header
   - `resources/views/layouts/app.blade.php` — pindahkan link "Laporan Pinjaman" ke grup Laporan (di sidebar)
   - `routes/web.php` — tambah route `GET /loans/laporan`
4. **Merge** `origin/fix/bugs` → `main` (no-ff) → merge commit `52fcc56`
5. **Push** `origin main` (`747d6a8..52fcc56`)
6. **Server** alurelab@160.187.143.18:
   - `git fetch origin main` + `git reset --hard origin/main` di `~/repositories/solution`
   - `php artisan migrate --force` → nothing to migrate (aman, tidak ada migration)
   - `php artisan optimize:clear` → sukses
   - Tidak rebuild Vite karena tidak ada perubahan JS/CSS
7. **Verifikasi HTTP:**
   - `/` → 302 (auth redirect) ✅
   - `/loans/laporan` → 302 ke login (route aktif) ✅
   - `/up` (health) → 200 ✅
   - `/iclock` (fingerprint) → 200 ✅
   - `php artisan route:list --name=loans` → `loans.laporan` terdaftar ✅

## File Diubah
- `app/Http/Controllers/LoanController.php` (+81 baris)
- `resources/views/layouts/app.blade.php` (+5/-1)
- `resources/views/loans/laporan.blade.php` (file baru, 128 baris)
- `resources/views/loans/show.blade.php` (+10)
- `routes/web.php` (+1)

## Hasil
✅ Deploy sukses. Server running `52fcc56`. Menu sidebar sekarang punya "Laporan Pinjaman" di bawah grup Laporan. Halaman `/loans/laporan` siap dipakai.

## Link
- Project: [[Project — SOLUTION]]
- Knowledge: [[Hosting — cPanel emerald]]
- Next: monitor Logs Laravel 1×24 jam; feedback user TIAN/CECIL via WA kalau ada bug