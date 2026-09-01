---
date: 2026-09-01
project: [[Project — SOLUTION]]
tags: [session, seeder, dummy-data, payroll, hrms]
---

# 2026-09-01 — Seeder Data Dummy Lengkap (Payroll)

## Task
Generate data dummy lengkap untuk semua fitur Payroll di hosting payroll.3putraperkasa.com. User eksplisit: "anda hapus all juga tidak masalah" + "banyak saja, ALL fitur".

## Aksi
1. **Pre-work CIEL:** baca [[Project — SOLUTION]] (schema: 12 model utama, 13 migration).
2. **Inspect hosting** via SSH + MySQL: data existing 5 employees, 3 golongans, 4 jabatans, 5 lokasis, 5 attendance_logs, 1 loan, 3 work_settings, 2 seasonal_schedules, 1 user. DB = MariaDB 10.11.18 di alurelab_adms_payroll.
3. **Schema review** semua 12 model: Employee, AttendanceLog, EmployeeSchedule, Golongan, Jabatan, Loan, LoanPayment, Lokasi, Payroll, Permit, SeasonalSchedule, User, WorkSetting. Catat: kolom `code` sudah di-drop dari golongans/jabatans/lokasis (migration `2026_08_27_000001`).
4. **Branch** `feat/dummy-data-seeder` dari main.
5. **Delegasi OpenCode** (background, ~30 menit) generate 11 Factory + DummyDataSeeder + update DatabaseSeeder.
6. **Push & deploy** ke main: `git fetch origin main && git reset --hard origin/main`.
7. **Run seeder** di hosting — 3 attempt karena 3 bug yang ditemukan saat runtime:
   - **Bug 1**: `PRAGMA foreign_keys = OFF` (SQLite-only) → fix: driver-aware (`SET FOREIGN_KEY_CHECKS = 0` untuk MySQL).
   - **Bug 2**: insert `code` di golongans/jabatans/lokasis padahal kolom sudah di-drop → fix: `hasColumn()` check.
   - **Bug 3**: global helper `fake()` tidak auto-loaded di seeder context → fix: ganti `Str::random()` dan `mt_rand()`.
8. **Patch ketiga** di-commit, push, deploy ulang. Seeder run ke-3 ✅ sukses.
9. **Verifikasi independen** via MySQL query: 6/8/6/2/12/12/4/826/33/7/22/24/1 — semua match.
10. **Verifikasi HTTP**: `/login` 200, `/up` 200, `/iclock` 200, `/` 302 (auth) ✅.

## File Diubah
- 11 Factory baru di `database/factories/`:
  - AttendanceLogFactory, EmployeeFactory, EmployeeScheduleFactory
  - GolonganFactory, JabatanFactory, LoanFactory, LoanPaymentFactory, LokasiFactory
  - PayrollFactory, PermitFactory, SeasonalScheduleFactory, WorkSettingFactory
- `database/seeders/DummyDataSeeder.php` (442 baris) — TRUNCATE + reseed, idempotent, driver-aware
- `database/seeders/DatabaseSeeder.php` — real seeders di-comment, DummyDataSeeder tidak dipanggil default

## Commits
- `6a95e05` feat: data dummy lengkap (12 karyawan, 2 bulan)
- `0a49951` fix(seeder): driver-aware FK + skip dropped code column
- `dbd5c33` fix(seeder): replace fake() global helper with explicit generators

## Hasil Hosting (post-seeder)
| Tabel | Count |
|---|---|
| golongans | 6 |
| jabatans | 8 |
| lokasis | 6 |
| work_settings | 2 |
| employees | 12 (10 active, 1 inactive, 1 resigned) |
| employee_schedules | 12 |
| seasonal_schedules | 4 |
| attendance_logs | 826 (Aug 404 + Sep 422, SENIN-JUMAT) |
| permits | 33 |
| loans | 7 |
| loan_payments | 22 |
| payrolls | 24 (Aug paid Rp 48,5jt + Sep draft Rp 48jt total net) |
| users | 1 (admin@adms.test / password) |

## Akun Login
- Email: `admin@adms.test`
- Password: `password`
- Role: super_admin

## Sample Data
- 12 karyawan Indonesia: Budi Santoso (Manager IT, 7jt), Siti Rahayu (Staff HR, 4.5jt), Ahmad Fauzi, Dewi Lestari, Rizky Pratama, Anisa Putri, Hendra Wijaya, Maya Anggraeni, Dimas Kurniawan (Senior Staff IT), Rina Sari (inactive), Fajar Nugroho (resigned), Lia Marlina
- Lokasi: Jakarta HQ, Bandung, Surabaya, Semarang, Medan, Makassar
- Seasonal: Puasa Ramadan 2026, Lebaran 2026, Natal 2025, Tahun Baru 2026
- Loans mix: 7 pinjaman 500rb-5jt (3 paid, 4 active)
- Permits: 33 izin mix approved/pending/rejected

## Link
- Project: [[Project — SOLUTION]]
- Knowledge: [[Hosting — cPanel emerald]]
- Next: monitor Logs Laravel 1×24 jam, feedback user via WA. Untuk reseed manual: `php artisan db:seed --class=DummyDataSeeder`