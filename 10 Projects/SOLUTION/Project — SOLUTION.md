# SOLUTION / ADMS Payroll

## Overview
ADMS (Attendance & Device Management System) + HRMS (HR Management System) untuk **PT 3 Putera Perkasa**. Domain: **payroll.3putraperkasa.com**. Mesin fingerprint (iClock protocol) integration. Built dengan Laravel 12 + Vite + Tailwind v4.

## Location
- Source: `https://github.com/10969sosho/solution.git` (branch `main`, juga `fix/bugs`, `feature/hrms-revisi`)
- Local clone: `/root/workspace/projects/solution`
- Production: `alurelab@160.187.143.18:31988` → `~/repositories/solution` (source) → `~/payroll.3putraperkasa.com` (docroot)

## Deploy Architecture
**Centralized pattern** (sama dengan financeai/backend):
- Docroot `~/payroll.3putraperkasa.com/` hanya shell tipis:
  - `index.php` — hardcode `$repoDir = "/home/alurelab/repositories/solution"` lalu load Laravel dari repo
  - `.htaccess` — URL rewrite
  - `build/` — Vite build output (assets + manifest.json)
- **Tidak ada `app/`, `resources/`, `routes/`, `vendor/` di docroot** — semua di `~/repositories/solution/`
- Update flow: edit di local → push main → server `git pull` di repo → `php artisan migrate` + `optimize:clear` di server → (jika ada perubahan JS/Vite) rebuild + copy `build/` ke docroot

## Database
- DB: `alurelab_adms_payroll` (MariaDB 10.11.18)
- Credentials ada di `~/repositories/solution/.env` (server)

## Tech Stack
| Layer | Teknologi |
|-------|-----------|
| Framework | Laravel 12 |
| PHP | 8.4.23 (server) |
| Frontend | Blade, Vite 7, Tailwind v4, Axios |
| Web Server | LiteSpeed (Apache compatible) |
| Database | MariaDB 10.11.18 |
| iClock | Fingerprint machine integration via `/iclock` endpoint |

## Related
- [[Hosting — cPanel emerald]]
- [[GitHub — 10969sosho & Gen-ei-Ryodan]]
- [[Sosho — Working Agreement (Hermes)]]

## Current Update — 2026-08-28
Deploy commit `b67d592`: "fix: hapus field kode di master golongan/jabatan/lokasi, hapus department/email di form karyawan, perbaiki template form izin". Migration `2026_08_27_000001_drop_code_from_master_tables` berhasil dijalankan di server. 20 file changed (controllers, models, views, migration). Tidak ada perubahan JS/Vite → tidak perlu rebuild.
