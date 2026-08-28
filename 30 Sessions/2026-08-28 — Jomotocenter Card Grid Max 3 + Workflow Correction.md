# 2026-08-28 — Jomotocenter Card Grid Max 3 + Workflow Correction

## Summary
Revisi card grid di jomotocenter.com (PAK JONI ECOMMERCE): max 3 card horizontal per baris di page motor/merk (choose.blade) dan kategori (category-brand.blade). User mengoreksi workflow deploy: harus lewat git (local → push → server pull), bukan edit langsung di hosting docroot.

## Context
- Project: [[Project — PAK JONI ECOMMERCE]] (jomotocenter.com)
- Repo: `Gen-ei-Ryodan/pak-joni-ecommerce` branch `main`
- Local clone: `/root/workspace/projects/pak-joni-ecommerce`
- Hosting: `alurelab@160.187.143.18:31988` → `~/repositories/pak-joni-ecommerce` → `~/jomotocenter.com` (docroot)

## Steps
1. **Pre-work (CIEL check)** — cari note PAK EFFENDI/PAK JONI di vault. Ketemu `10 Projects/PAK JONI ECOMMERCE/PAK JONI ECOMMERCE.md`
2. **Awal: SALAH** — langsung edit file di `~/jomotocenter.com` di hosting (3 file: `card.css`, `choose.blade.php`, `buyer.blade.php`). ❌ Bukan workflow yang benar
3. **User koreksi:** "workflow anda salah — seharusnya git main → opencode local → push → pull di repositories → deploy ke domain"
4. **Revert/edit ulang di local** `/root/workspace/projects/pak-joni-ecommerce`:
   - `public/assets/css/card.css` → tambah `overflow: hidden` + `max-width: 100%`
   - `resources/views/buyer/product/choose.blade.php` → grid `auto-fill` → `repeat(3, 1fr)` fixed 3 kolom + responsive
   - `resources/views/layouts/buyer.blade.php` → cache buster `?v=2` → `?v=3`
5. **Commit + push:** `59c3de8a fix: card grid max 3 horizontal (motor/merk/kategori browse)`
6. **Deploy via SSH:**
   - `cd ~/repositories/pak-joni-ecommerce` → `git fetch origin main` + `git reset --hard origin/main`
   - Copy `app/`, `bootstrap/`, `config/`, `database/`, `public/`, `resources/`, `routes/`, `artisan`, `composer.json`, `composer.lock` ke `~/jomotocenter.com/`
   - `php artisan optimize:clear` + `view:clear` di docroot
7. **Verify:** diff `~/repositories/...` vs `~/jomotocenter.com/...` MATCH untuk 3 file yang diubah. Cache buster `?v=3` confirmed live
8. **Post-work:** update `PAK JONI ECOMMERCE.md` (Current Update section) + write session log ini

## Files Touched
- `public/assets/css/card.css`
- `resources/views/buyer/product/choose.blade.php`
- `resources/views/layouts/buyer.blade.php`
- `10 Projects/PAK JONI ECOMMERCE/PAK JONI ECOMMERCE.md` (Current Update section)
- `30 Sessions/2026-08-28 — Jomotocenter Card Grid Max 3 + Workflow Correction.md` (this file)

## Pelajaran
- **JANGAN edit langsung di hosting docroot.** Selalu lewat `git push` lalu pull/copy.
- Pakai `diff` post-deploy untuk verify docroot = repositori.
- Pakai cache buster `?v=N` di `<link rel="stylesheet">` untuk force refresh browser.
- Untuk project Laravel yang sudah ada, `deploy.sh` di local (yang copy dari `repositories/` ke docroot) adalah pattern deploy yang benar.

## Related
- [[Project — PAK JONI ECOMMERCE]]
- [[Hosting — cPanel emerald]]
- [[Sosho — Working Agreement (Hermes)]]
- [[Architecture — CIEL System]]
