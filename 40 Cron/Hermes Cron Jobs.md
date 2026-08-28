---
type: cron
tags: [cron, jobs, automated]
last_updated: 2026-08-28
---

# Hermes Cron Jobs (Aktif)

> Daftar cron job yang jalan otomatis. List via `hermes cron list`.

## Job Aktif

### 🔄 `94882043c04b` — Backup knowledge ke GitHub
- **Schedule:** `0 */3 * * *` (tiap 3 jam)
- **Type:** `no_agent` (script-only)
- **Script:** `~/.hermes/scripts/backup_knowledge.py`
- **Deliver:** `local` → simpan di `~/.hermes/cron/output/94882043c04b/`
- **Last run:** 2026-08-28 03:31 UTC
- **Status:** ✅ ok
- **Tujuan:** push `/root/knowledge/` → `10969sosho/sosho-knowledge` (private)

### 📋 `c164d17a5c84` — TODO list Task Manager
- **Schedule:** `0 * * * *` (tiap jam)
- **Type:** agent
- **Source:** `GET https://qwe.solusisurabaya.com/api/public/tasks`
- **Deliver:** `origin` (balas ke chat WA)
- **Last run:** 2026-08-28 04:00 UTC
- **Status:** ✅ ok
- **Output format:** list task + ⚠️ untuk overdue (sederhana, no penjelasan)

---
*Lihat juga: [[Hosting — cPanel emerald]] · [[Sosho — Working Agreement (Hermes)]]*
