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

## Session Log
- 2026-09-01 (seeder): [[2026-09-01 — Seeder Data Dummy Lengkap]] — 11 factory + DummyDataSeeder komprehensif, run di hosting: 12 karyawan, 826 attendance, 24 payrolls (2 bln). Akun: admin@adms.test / password.
- 2026-09-01 (deploy): [[2026-09-01 — Deploy Payroll Laporan Pinjaman]] — merge `fix/bugs` → `main` (`52fcc56`): fitur Laporan Pinjaman + sidebar grouping. Deploy sukses, semua endpoint hijau.
- 2026-08-28: Deploy `b67d592` (drop kode master + fix izin template) — migration sukses.

## Current Update — 2026-09-01
Seeder DummyDataSeeder sukses di hosting. Server running `dbd5c33` (main). Database: 12 employees (Indonesia, salary 3.1-7jt), 6 golongan, 8 jabatan, 6 lokasi, 826 attendance logs (Aug+Sep 2026, SENIN-JUMAT), 33 permits, 7 loans + 22 payments, 24 payrolls. Akun login: `admin@adms.test` / `password` (super_admin). Untuk reseed manual: `php artisan db:seed --class=DummyDataSeeder`.
