---
type: cron
tags: [cron, jobs, automated]
last_updated: 2026-08-28
---

# Hermes Cron Jobs (Aktif)

> Daftar cron job yang jalan otomatis. List via `hermes cron list`.

## Job Aktif

### 🔄 `94882043c04b` — Backup knowledge legacy ke GitHub
- **Schedule:** `0 */3 * * *` (tiap 3 jam)
- **Type:** `no_agent` (script-only)
- **Script:** `~/.hermes/scripts/backup_knowledge.py`
- **Deliver:** `local` → simpan di `~/.hermes/cron/output/94882043c04b/`
- **Last run:** 2026-08-28 03:31 UTC
- **Status:** ✅ ok
- **Tujuan:** push `/root/knowledge/` (legacy JSON: cpanel_hosting, github_repos, percakapan) → `10969sosho/sosho-knowledge` (private)

### 📋 `c164d17a5c84` — TODO list Task Manager
- **Schedule:** `0 * * * *` (tiap jam)
- **Type:** agent
- **Source:** `GET https://qwe.solusisurabaya.com/api/public/tasks`
- **Deliver:** `origin` (balas ke chat WA)
- **Last run:** 2026-08-28 04:00 UTC
- **Status:** ✅ ok
- **Output format:** list task + ⚠️ untuk overdue (sederhana, no penjelasan)

### 💾 `16a0cdfe13f3` — CIEL Vault Auto-Backup
- **Schedule:** `*/5 * * * *` (tiap 5 menit)
- **Type:** `no_agent` (script-only)
- **Script:** `~/.hermes/scripts/ciel_autobackup.sh`
- **Deliver:** `local`
- **Next run:** 2026-08-28 04:35 UTC
- **Status:** ✅ baru dibuat 2026-08-28 04:30
- **Tujuan:** push `/root/projects/ciel/` (vault CIEL) → `10969sosho/ciel` (private)
- **Karakteristik:**
  - Skip kalau tidak ada perubahan (zero overhead)
  - Lock file untuk avoid race
  - Log: `~/.hermes/cron/output/ciel_autobackup.log`
  - Auto-add + commit (timestamped) + push
  - Self-healing: kalau push gagal, retry di tick berikutnya

---
*Lihat juga: [[Architecture — CIEL System]] · [[Hosting — cPanel emerald]] · [[Sosho — Working Agreement (Hermes)]]*
