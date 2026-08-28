---
date: 2026-08-28
project:
  - [[Project — Kabekabe]]
  - [[Project — Deppa]]
tags: [session, setup, obsidian, ciel]
---

# 2026-08-28 — Setup CIEL Workflow (Single Source of Truth)

## Task
Sosho minta semua catatan kerja (workflow, hosting, docs, cron) dicatat di **Obsidian vault CIEL** biar:
- Saya bisa search sebelum kerjain sesuatu
- Update setelah kerja biar semua nyambung di graph
- Single source of truth lintas sesi

## Pre-Work Check
- Search vault → ketemu banyak note existing: `10 Projects/DEPPA/`, `10 Projects/CVSS/`, `20 Knowledge/SSH HP Android sebagai Storage VPS.md`, `record/` (YouTube transcripts), dll
- Pattern PARA-style (00/10/20/30/40/50/99)
- Vault sudah punya `.git` di `/root/projects/ciel/` (mirror Termux) dan `/sdcard/Obsidian/Ciel/` (asli HP)

## Aksi
1. **Resolved vault path:** `/root/projects/ciel/` (mirror Termux, FUSE tapi reliable buat Hermes)
2. **Created folder:** `30 Sessions/`, `40 Cron/`, `50 Workflow/`
3. **Created knowledge notes** (4 + 3 project):
   - `20 Knowledge/Hosting — cPanel emerald.md` (dari `cpanel_hosting.json`)
   - `20 Knowledge/GitHub — 10969sosho & Gen-ei-Ryodan.md` (dari `github_repos.json`)
   - `20 Knowledge/Profile — Yang Mulia.md` (dari `user_profile.md`)
   - `20 Knowledge/Sosho — Working Agreement (Hermes).md`
   - `20 Knowledge/Project — Kabekabe.md` (hosting info + workflow)
   - `20 Knowledge/Project — Deppa.md` (PWA Construct workflow + link ke `[[DEPPA/DEPPA]]`)
   - `20 Knowledge/CIEL Workflow.md` (salin ke `50 Workflow/CIEL Workflow.md`)
4. **Created `index.md`** (map of content)
5. **Created skill `ciel-workflow`** di `/root/.hermes/skills/productivity/ciel-workflow/SKILL.md` — enforce pre-work check + post-work update
6. **Avoided duplicate:** Hapus `Project — Deppa.md` & `Project — Kabekabe.md` di `10 Projects/` karena bentrok dengan folder existing `DEPPA/`. Knowledge note di `20 Knowledge/` jadi canonical reference.

## File Diciptakan
- `20 Knowledge/Hosting — cPanel emerald.md`
- `20 Knowledge/GitHub — 10969sosho & Gen-ei-Ryodan.md`
- `20 Knowledge/Profile — Yang Mulia.md`
- `20 Knowledge/Sosho — Working Agreement (Hermes).md`
- `20 Knowledge/Project — Kabekabe.md`
- `20 Knowledge/Project — Deppa.md`
- `40 Cron/Hermes Cron Jobs.md`
- `50 Workflow/CIEL Workflow.md`
- `index.md`
- `/root/.hermes/skills/productivity/ciel-workflow/SKILL.md`

## File Dihapus
- `10 Projects/Project — Deppa.md` (duplicate dari `DEPPA/DEPPA.md`)
- `10 Projects/Project — Kabekabe.md` (kabekabe belum ada note, dipindah ke `20 Knowledge/`)

## Hasil
- ✅ Vault CIEL punya struktur lengkap (knowledge + project + cron + workflow)
- ✅ Skill `ciel-workflow` aktif — pre-work search & post-work update jadi habit
- ✅ Index.md jadi peta besar
- ⚠️ Git sync `/root/projects/ciel` ↔ `/sdcard/Obsidian/Ciel` belum di-setup (opsional, FUSE jadi pertimbangan)
- ⚠️ Data existing Obsidian user (CVSS, ADMS, dll) tetap utuh — saya hanya tambah di `20 Knowledge/` & `40 Cron/`, tidak sentuh `10 Projects/` user

## Link
- [[CIEL Workflow]] — protocol
- [[Sosho — Working Agreement (Hermes)]] — aturan hosting & deploy
- [[Project — Kabekabe]]
- [[Project — Deppa]]
- [[Hosting — cPanel emerald]]
- [[GitHub — 10969sosho & Gen-ei-Ryodan]]
- [[Hermes Cron Jobs]]
- [[Profile — Yang Mulia]]

## Next
- [ ] (Optional) Setup git sync `/root/projects/ciel` ↔ `/sdcard/Obsidian/Ciel` — biar edit di HP langsung ke mirror Termux
- [ ] Tambah project notes lain (ADMS, PAK JONI, dll) kalau perlu
- [ ] Cron `94882043c04b` backup — tambah juga `ciel/` ke backup? (opsional, vault sudah di-git)
