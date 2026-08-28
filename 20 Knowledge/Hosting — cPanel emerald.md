---
type: knowledge
tags: [hosting, cpanel, infra]
source: /root/knowledge/cpanel_hosting.json
last_updated: 2026-08-28
---

# Hosting — cPanel emerald (160.187.143.18)

> Single source of truth buat semua domain & project. Diupdate tiap deploy/change.

## Akses SSH

| Item | Value |
|---|---|
| Host | `160.187.143.18` |
| Port | `31988` (non-standard!) |
| User | `alurelab` |
| Key | `/root/.ssh/id_ed25519_cpanel` |
| Command | `ssh -p 31988 alurelab@160.187.143.18` |
| Server | emerald.hidden-server.net |
| Package | developer3 |
| cPanel | 136.0 (build 35) |
| Stack | Apache 2.4.68 · MariaDB 10.11.18 · PHP multi · AlmaLinux 9 |
| Disk | 1.2T/1.8T (72%) |

⚠️ **Port 31988** sering bikin orang mengira SSH mati — selalu specify.

## Domain & Project

### solusisurabaya.com
- **Type:** static/landing
- **Subdomain:**
  - `digital` — Next.js
  - `fashion` — HTML statis
  - `furniture`
  - `qwe` — Task Manager (Sosho's TODO)
  - `membership` — [[Project — Kabekabe]] 🟢 LIVE

### alureflow.com
- **Type:** static (Next export)
- **Subdomain:**
  - `app` — Next.js
  - `games` — [[Project — Deppa]] 🟢 LIVE (Laravel 12 + Construct PWA)
  - `gym` — PHP
  - `photobox`

### 3putraperkasa.com
- **Type:** park/utama
- **Subdomain:**
  - `absplt` — Laravel 12 (absensi)
  - `formula` — Laravel 12 (BU Vania Formula)
  - `payroll` — PHP kecil

### indirakp.com
- `dashboard` — Laravel 13

### Domain lain
- `hakusaedu.com` — Laravel 13
- `jayapetir.com` — Laravel 12
- `jomotocenter.com` — Laravel 13 + docs API/INTEGRATION
- `ymiits.com` — Laravel (di repositories)
- `ptabadimp.com` — ada folder, cek detail

## Deploy scripts (di server)
- `deploy-photobox.sh`
- `deploy-solution.sh`

## Git mirrors
Server `~/repositories` berisi 18 repo (mirror GitHub: alureflow, hakusaedu, indirakp, pak-joni-ecommerce, dll).

⛔ **ATURAN WAJIB:** `~/repositories/` HANYA kode sumber. Operate langsung ke docroot domain tujuan (mis. `~/membership.solusisurabaya.com`). NO symlink storage, NO upload/migrate/log/DB ke repo.

## Backups
- `backup-sparepart-before-delete-20260819-080505.sql.gz`
- `backups/solusisurabaya.com-20260810-*`

---
*Lihat juga: [[GitHub — 10969sosho & Gen-ei-Ryodan]] · [[Project — Kabekabe]] · [[Project — Deppa]]*
