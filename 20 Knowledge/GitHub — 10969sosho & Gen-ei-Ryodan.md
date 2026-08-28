---
type: knowledge
tags: [github, repos, backup]
source: /root/knowledge/github_repos.json
last_updated: 2026-08-28
---

# GitHub — 10969sosho & Gen-ei-Ryodan

> Index 27 repo. Di-backup ke `sosho-knowledge` (private) tiap 3 jam via cron.

## Akun & Auth
- **User Sosho:** `10969sosho`
- **Org mirror:** `Gen-ei-Ryodan` (repo mirror)
- **PAT:** `/root/.git-credentials`
- **SSH key:** `/root/.ssh/id_ed25519_hermes`
- **Backup repo:** `10969sosho/sosho-knowledge` (private)
- **Backup cron:** `94882043c04b` — tiap 3 jam (`0 */3 * * *`)

## Repo Index (27 total)

### Project Aktif
| Repo | Bahasa | Update Terakhir |
|---|---|---|
| `10969sosho/sosho-knowledge` | — | 2026-08-22 (knowledge backup) |
| `10969sosho/deppa` | PHP | 2026-08-21 ([[Project — Deppa]]) |
| `10969sosho/ROBOBUBI` | Python | 2026-08-20 |
| `10969sosho/bennylovescoffee` | Blade | 2026-08-18 |
| `10969sosho/solution` | ? | cek |
| `Gen-ei-Ryodan/hakusaedu` | Blade | 2026-08-21 |
| `Gen-ei-Ryodan/indirakp` | Blade | 2026-08-21 |
| `Gen-ei-Ryodan/bu-vania-formula` | Blade | 2026-08-19 |
| `Gen-ei-Ryodan/pak-joni-ecommerce` | PHP | 2026-08-21 |
| `Gen-ei-Ryodan/demo-repository` | TypeScript | 2026-08-20 |

*Full index ada di `/root/knowledge/github_repos.json` (27 entries).*

## Auth Pitfall
- Org repos invisible via public API (membership hidden) → query `GET /orgs/<org>/repos` pakai org token
- Org kadang forbid fine-grained PAT > 366 hari → 401/403 "Bad credentials" → pakai classic PAT

---
*Lihat juga: [[Hosting — cPanel emerald]] · [[Project — Deppa]]*
