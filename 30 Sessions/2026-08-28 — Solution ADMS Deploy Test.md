# 2026-08-28 — Solution/ADMS Deploy Test (Payroll.3putraperkasa.com)

## Summary
Testing workflow deploy git untuk project SOLUTION (ADMS payroll) ke domain `payroll.3putraperkasa.com`. User mengira perlu merge `fix → main`, tapi setelah dicek branch sudah seragam. Yang diperlukan hanya `git pull` di server + jalankan migration.

## Context
- Project: [[Project — SOLUTION]]
- Repo: `10969sosho/solution` (bukan `Gen-ei-Ryodan/solution` — di-check dulu)
- Local clone: `/root/workspace/projects/solution` (belum ada, dibuat)
- Server: `~/repositories/solution` (sudah ada)
- Docroot: `~/payroll.3putraperkasa.com` (centralized deploy pattern — index.php load dari repos)
- Branch: `main` (HEAD: `b67d592`), `fix/bugs`, `feature/hrms-revisi`

## Steps
1. **Pre-work (CIEL check):** Tidak ada note untuk SOLUTION di vault → perlu dibuat
2. **User request:** "merge fix ke main, deploy ke payroll.3putraperkasa.com via repositories/solution"
3. **Local clone:** `git clone https://github.com/10969sosho/solution.git` ke `/root/workspace/projects/solution` ✅
4. **Cek state branch:**
   - `origin/main`: `b67d592`
   - `origin/fix/bugs`: `b67d592` (sama!)
   - `git log origin/main..origin/fix/bugs --oneline` → kosong
   - **TIDAK ada commit baru untuk di-merge** ⚠️
5. **Cek server tertinggal:**
   - `~/repositories/solution` HEAD: `9d3faaa` (1 commit di belakang)
   - `origin/main` HEAD: `b67d592`
   - **Server perlu di-pull, bukan di-merge**
6. **Server: `git pull origin main`** di `~/repositories/solution` → HEAD updated ke `b67d592` ✅
7. **Run new migration:** `php artisan migrate --force` → `2026_08_27_000001_drop_code_from_master_tables` DONE ✅
8. **Clear cache:** `php artisan optimize:clear` ✅
9. **HTTP verify:** `curl -I https://payroll.3putraperkasa.com` → 302 redirect ke `/login` (normal Laravel behavior) ✅
10. **Skip Vite rebuild:** commit `b67d592` murni Blade + migration, tidak ada perubahan JS/Vite
11. **Post-work CIEL:** buat `10 Projects/SOLUTION/Project — SOLUTION.md` + session log ini

## Files Touched
- `/root/workspace/projects/solution/` (created via clone)
- `~/repositories/solution/` (server, updated via git pull)
- `10 Projects/SOLUTION/Project — SOLUTION.md` (created)
- `30 Sessions/2026-08-28 — Solution ADMS Deploy Test.md` (this file)

## Pelajaran
- **SELALU cek state branch dulu sebelum merge.** Cek `git log origin/main..origin/fix --oneline` → kalau kosong, branch sudah seragam, skip merge.
- **Server bisa tertinggal dari GitHub.** `git fetch` + `git log HEAD..origin/main --oneline` untuk lihat commit yang belum di-pull.
- **Centralized deploy pattern (repo + docroot shell)** — bukan cp folder Laravel. Lihat `index.php` di docroot: hardcode `$repoDir` lalu load Laravel dari sana. Update cuma butuh `git pull` di server.
- **Cek apakah commit menyentuh JS/Vite** sebelum rebuild. `git show --stat <commit>` → kalau tidak ada perubahan di `package.json` atau `resources/js/`, skip `npm run build`.
- **Database migration wajib di-run setelah pull** kalau ada file baru di `database/migrations/`.

## Related
- [[Project — SOLUTION]]
- [[Hosting — cPanel emerald]]
- [[Sosho — Working Agreement (Hermes)]]
- [[2026-08-28 — Jomotocenter Card Grid Max 3 + Workflow Correction]] (workflow reference)
