---
type: knowledge
tags: [project, laravel, pwa, construct, live, game, deppa]
domain: games.alureflow.com
github: 10969sosho/deppa
last_updated: 2026-08-28
---

# Project — Deppa

> Construct 3 PWA game + Laravel 12 API. 🟢 LIVE di `games.alureflow.com`

## Snapshot
- **Domain:** `games.alureflow.com`
- **Hosting:** [[Hosting — cPanel emerald]]
- **Stack:**
  - Frontend: Construct 3 → di-build jadi PWA
  - Backend: Laravel 12 API (Sanctum auth, no Google OAuth)
- **Repo:** [10969sosho/deppa](https://github.com/10969sosho/deppa)
- **Status:** 🟢 live

## Identitas Pemain
- `players.nama` = identifier utama (tanpa Google OAuth)
- Registrasi → return Sanctum token + ID player sebagai metadata

## Workflow PWA Construct (WAJIB)
1. Edit Construct 3
2. Export → dapat `c3main.js`
3. **Sync `c3main.js` ke DUA lokasi:** root project DAN `scripts/`
4. **Bump `offline.json` version** tiap update PWA (WAJIB, biar service worker re-cache)
5. Push ke GitHub → deploy ke cPanel
6. **Update note ini + tambah session log** di `[[CIEL Workflow]]`

## Related Notes (dari vault user)
- [[DEPPA/DEPPA]] — overview original
- [[DEPPA/DEPPA Features]]
- [[DEPPA/DEPPA Recent Updates]]

## Reference
- `[[Sosho — Working Agreement (Hermes)]]` — PWA Construct Workflow section

---
*Lihat juga: [[Hosting — cPanel emerald]] · [[GitHub — 10969sosho & Gen-ei-Ryodan]] · [[Sosho — Working Agreement (Hermes)]] · [[CIEL Workflow]]*
