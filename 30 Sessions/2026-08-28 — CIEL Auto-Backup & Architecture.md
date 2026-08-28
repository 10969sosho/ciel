---
date: 2026-08-28
project:
  - [[Project — Kabekabe]]
  - [[Project — Deppa]]
tags: [session, setup, backup, architecture, portable]
---

# 2026-08-28 — CIEL Auto-Backup & Architecture (Portable)

## Task
Sosho minta:
1. CIEL **live bertumbuh** + ter-backup ke GitHub
2. **Tidak mengurangi kecepatan** kerja
3. **Architecture doc** biar portable ke device lain

## Benchmark & Diskusi
- Test write speed di `/root/projects/ciel/`: **0.013s** untuk 5 file append (block-device, BUKAN FUSE)
- `/root/workspace` ternyata mount `/sdcard` (FUSE), tapi `/root/projects` block-device → aman dipakai vault
- Remote git sudah ada: `10969sosho/ciel` (private, sudah di-push sesi sebelumnya)
- **Kesimpulan: tidak perlu pindah path.** Setup cron + script + architecture doc saja.

## Aksi
1. **Architecture doc** (`20 Knowledge/Architecture — CIEL System.md`):
   - Design principles, topologi (diagram ASCII)
   - Struktur folder + kenapa PARA + extras
   - Workflow pre-work & post-work
   - Backup strategy 3 lapis
   - **Setup ulang step-by-step** untuk device baru
   - Anti-pattern & integration skill

2. **Auto-backup script** (`/root/.hermes/scripts/ciel_autobackup.sh`):
   - Cek perubahan → skip kalau kosong (zero overhead)
   - Lock file → avoid race
   - Auto-add + commit (timestamped ISO) + push
   - Log ke `~/.hermes/cron/output/ciel_autobackup.log`
   - Self-healing: kalau push gagal, retry tick berikutnya

3. **Cron `16a0cdfe13f3`** — tiap 5 menit, no_agent, script-only
   - Next run: 2026-08-28 04:35 UTC
   - Test manual: exit 0 idempotent (tidak spam commit kosong)

4. **Git config global**: `user.name="Hermes Agent"`, `user.email="hermes@sosho.local"`

5. **Update docs**:
   - `40 Cron/Hermes Cron Jobs.md` — tambah cron CIEL
   - `index.md` — link ke Architecture doc

## File Diciptakan
- `20 Knowledge/Architecture — CIEL System.md` (8.6KB)
- `/root/.hermes/scripts/ciel_autobackup.sh`
- Cron job `16a0cdfe13f3`

## File Diupdate
- `40 Cron/Hermes Cron Jobs.md`
- `index.md`

## Hasil
- ✅ CIEL vault live + auto-backup ke `10969sosho/ciel` tiap 5 menit
- ✅ **Zero overhead** kalau tidak ada perubahan (skip detection)
- ✅ Architecture doc portable — bisa setup ulang di device apapun
- ✅ Idempotent: panggil script 2x tetap exit 0, no spam
- ✅ Local SHA `1111c67` = remote SHA `1111c67` (verified sync)
- ⚠️ Tidak ada perubahan path — `/root/projects/ciel/` (block-device, fast) sudah optimal

## Konvensi (untuk cross-session)
- **Jangan commit manual** kalau cron auto-backup sudah jalan
- Pakai timestamped commit message: `auto: 2026-08-28T04:30:59Z`
- File pattern: `30 Sessions/YYYY-MM-DD — <topik>.md` (dengan em-dash)

## Link
- [[Architecture — CIEL System]]
- [[CIEL Workflow]]
- [[Sosho — Working Agreement (Hermes)]]
- [[Hermes Cron Jobs]]
- [[Project — Kabekabe]]
- [[Project — Deppa]]

## Next
- (optional) Test: edit file di HP, pull di server, push balik — verifikasi 2-way sync
- (optional) Tambahkan plugin Obsidian: Dataview, Templater, Calendar (untuk dashboard)
- (optional) Bikin alias shell: `ciel=cd /root/projects/ciel` (kalau pakai interactive shell)
