---
type: knowledge
tags: [project, laravel, membership, live, kabekabe]
domain: membership.solusisurabaya.com
last_updated: 2026-08-28
---

# Project — Kabekabe

> Membership site Laravel. 🟢 LIVE di `membership.solusisurabaya.com`

## Snapshot
- **Domain:** `membership.solusisurabaya.com`
- **Hosting:** [[Hosting — cPanel emerald]] → docroot: `~/membership.solusisurabaya.com`
- **Stack:** Laravel + Filament + MySQL
- **Status:** 🟢 live & ter-maintain

## Workflow Deploy (WAJIB)
1. Edit lokal
2. Build
3. **Push main DULU** ke GitHub
4. Deploy ke cPanel (`ssh -p 31988`)
5. Copy assets ke docroot
6. **Update note ini + tambah session log** di `[[CIEL Workflow]]`

## Storage (PENTING)
Disk `public` root di-redirect ke **docroot storage** — lihat commit `6ccf3bb`. JANGAN symlink storage dari `~/repositories/` (itu source-only, bukan operasional).

## Tools
- `faster-whisper` terinstall → transkrip voice ID langsung

## Reference
- `[[Sosho — Working Agreement (Hermes)]]` — Kabekabe Workflow section
- `[[Hosting — cPanel emerald]]` — server detail

---
*Lihat juga: [[Hosting — cPanel emerald]] · [[Sosho — Working Agreement (Hermes)]] · [[Profile — Yang Mulia]] · [[CIEL Workflow]]*
