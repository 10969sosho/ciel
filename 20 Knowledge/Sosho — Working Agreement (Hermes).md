---
type: knowledge
tags: [hermes, workflow, working-agreement]
last_updated: 2026-08-28
---

# Hermes Working Agreement

> Aturan main antara Sosho & Hermes. Konsisten lintas sesi.

## PREFLIGHT — Tanya Dulu Sebelum Eksekusi
- Jika ada **ketidaksesuaian/ambigu** (nama project, workflow, lokasi target, scope) → **tanya dulu** lewat `clarify()` atau konfirmasi eksplisit
- Contoh kasus:
  - Project identity: "pak-effendi-ecommerce" vs "pak-joni-ecommerce" — beda project, beda repo
  - Workflow: edit local→git push→server pull→copy docroot **≠** edit langsung di docroot hosting
  - Lokasi: `~/repositories/<repo>` = source code only, **bukan** target edit
- Lebih baik **1× konfirmasi** daripada eksekusi 2× + rollback (pengalaman 2026-08-28: edit langsung di `~/jomotocenter.com` harus di-recovery karena bukan workflow yang benar)
- Untuk project yang dikenal, search CIEL vault dulu via skill `ciel-workflow` sebelum mulai

## ATURAN WAJIB Hosting
- `~/repositories/` di cPanel **HANYA** kode sumber — no symlink storage, no upload/migrate/log/DB ke repo
- **JANGAN edit langsung di docroot domain** (mis. `~/jomotocenter.com`) — SELALU lewat git
- Workflow deploy Laravel (diadopsi dari jomotocenter 2026-08-28):
  1. Edit di **local clone** `/root/workspace/projects/<repo>`
  2. `git add` → `git commit` → `git push origin main`
  3. SSH ke hosting → `cd ~/repositories/<repo>` → `git fetch origin main` + `git reset --hard origin/main`
  4. Copy folder/file dari `~/repositories/<repo>` ke docroot domain
  5. `php artisan optimize:clear` + `view:clear` di docroot
- Diterapkan: kabekabe `filesystems.php` disk `public` root → docroot storage (commit `6ccf3bb`)

## Kabekabe Workflow
1. Edit lokal (laptop/Termux)
2. Build
3. **Push main ke GitHub DULU** (WAJIB sebelum deploy)
4. Deploy ke cPanel (`ssh -p 31988`)
5. Copy assets ke docroot
6. **Update CIEL** (`10 Projects/Kabekabe/_index.md` + tambah session log)

## PWA Construct (Deppa) Workflow
- Sync `c3main.js` ke root DAN `scripts/`
- **WAJIB bump `offline.json` version** tiap update PWA
- Update CIEL setelah deploy

## Tools Lokal
- **Workspace:** `/root/workspace/projects/` (pindahan dari `/sdcard/PROJECT/` yang lambat/FUSE)
- **Archive:** `/root/workspace/archive/`
- **faster-whisper** terinstall untuk transkrip voice ID

## Knowledge Sources
- `/root/knowledge/` (legacy JSON, di-backup ke `sosho-knowledge` GitHub tiap 3 jam)
- `/root/projects/ciel/` (**vault Obsidian CIEL** — single source of truth ter-update, sejak 2026-08-28)
- Cron TODO list: `qwe.solusisurabaya.com` Task Manager (https://qwe.solusisurabaya.com/api/public/tasks)

---
*Lihat juga: [[Hosting — cPanel emerald]] · [[Profile — Yang Mulia]]*
