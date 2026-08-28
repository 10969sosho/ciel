---
type: knowledge
tags: [hermes, workflow, working-agreement]
last_updated: 2026-08-28
---

# Hermes Working Agreement

> Aturan main antara Sosho & Hermes. Konsisten lintas sesi.

## ATURAN WAJIB Hosting
- `~/repositories/` di cPanel **HANYA** kode sumber — no symlink storage, no upload/migrate/log/DB ke repo
- Operate langsung ke **docroot domain tujuan** (mis. `~/membership.solusisurabaya.com`)
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
