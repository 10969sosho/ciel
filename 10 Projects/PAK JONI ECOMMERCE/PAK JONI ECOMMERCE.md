# PAK JONI / ECOMMERCE — JOMOTO CENTER

## Overview
Platform e-commerce dealer motor & sparepart (ATV, motor, mobil, part) untuk client **Pak Joni**. Nama domain: **jomotocenter.com**. Dibangun dengan Laravel + Filament admin panel + storefront Blade.

## Location
- Source: `https://github.com/Gen-ei-Ryodan/pak-joni-ecommerce`; HP clone: `/root/projects/pak-joni-ecommerce/`; storage backup: `/root/manual-backups/pak-joni-storage/`
- Git remote: `https://github.com/Gen-ei-Ryodan/pak-joni-ecommerce.git` (branch `main`)
- Production: `alurelab@emerald.hidden-server.net` port `31988`

## Tech Stack
| Layer | Teknologi |
|-------|-----------|
| Framework | Laravel 13 (Laravel Framework `^13.0`) |
| PHP | local 8.5.5, server 8.4.23 (requirement `^8.3`) |
| Admin Panel | Filament v4 (`filament/filament ^4.0`) |
| Frontend | Blade, Livewire, Tailwind, Vite |
| Testing | PHPUnit, Dusk browser tests |
| Payment | Midtrans |
| Template Engine | Blade |

## Related
- [[CVSS]]
- [[STOCK_MANAGEMENT]]
- [[DEPLOYMENT_JOMOTO]]
- [[BUGS_JOMOTO]]
- [[Laravel]]
- [[Filament]]

## Current Update — 2026-08-28: Card Grid Max 3 Horizontal

### Request
User ingin revisi flow browse:
1. Klik navbar **Motor** → muncul list **merk**
2. Klik salah satu **merk** → muncul list **kategori produk** (di page `category-brand`)
3. Card di kedua page tersebut → **max 3 horizontal**, sisanya turun ke bawah

### Files Changed
- `public/assets/css/card.css` — `.grid` + `.grid-3`: tambah `overflow: hidden` dan `max-width: 100%` agar card tidak overflow horizontal
- `resources/views/buyer/product/choose.blade.php` — `.product-brand-grid`: ganti `repeat(auto-fill, minmax(220px, 1fr))` → `repeat(3, 1fr)` (fixed 3 kolom). Tambah responsive breakpoint: 2 kolom di ≤980px, 1 kolom di ≤640px
- `resources/views/layouts/buyer.blade.php` — bump cache buster `card.css` `?v=2` → `?v=3` agar browser force-refresh

### Deploy
- Commit: `59c3de8a` di branch `main`
- Remote: `https://github.com/Gen-ei-Ryodan/pak-joni-ecommerce.git`
- Hosting: `alurelab@160.187.143.18:31988` → `~/repositories/pak-joni-ecommerce` → `~/jomotocenter.com` (docroot)
- Laravel cache: `php artisan optimize:clear` sudah dijalankan

### Workflow yang dipakai (BENAR)
1. Edit di **local** `/root/workspace/projects/pak-joni-ecommerce`
2. `git add` → `git commit` → `git push origin main`
3. SSH ke hosting → `cd ~/repositories/pak-joni-ecommerce` → `git fetch origin main` + `git reset --hard origin/main`
4. Copy folder/file dari `~/repositories/pak-joni-ecommerce` ke `~/jomotocenter.com`
5. `php artisan optimize:clear` + `view:clear` di docroot

⚠️ **PELAJARAN:** Jangan edit langsung di `~/jomotocenter.com` di hosting. Selalu lewat local → git → server repos → deploy script.

