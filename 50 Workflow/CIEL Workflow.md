---
type: knowledge
tags: [workflow, obsidian, ciel, single-source-of-truth]
last_updated: 2026-08-28
---

# CIEL Workflow

> Single source of truth semua catatan kerja. Berlaku SETIAP pengerjaan project, deploy, fix, atau perubahan infra.

## Struktur CIEL

```
ciel/
├── 00 Inbox/         ← quick capture
├── 10 Projects/      ← per-project note (PARA-style)
├── 20 Knowledge/     ← knowledge base (hosting, github, project, agreement)
├── 30 Sessions/      ← YYYY-MM-DD-<topik>.md (per kerjaan)
├── 40 Cron/          ← cron jobs docs
├── 50 Workflow/      ← workflow & convention
├── 99 Archive/       ← archived notes
└── record/           ← transcript & record (user-managed)
```

## Pre-Work Check (WAJIB)
Sebelum eksekusi task apapun:

1. **Search CIEL** untuk keyword:
   ```
   search_files pattern="<keyword>" path="/root/projects/ciel" file_glob="*.md"
   ```
2. **Buka project note** di `10 Projects/<Project>/` atau `20 Knowledge/Project — <Nama>.md` kalau ada
3. **Scroll session log** terakhir di `30 Sessions/` (cari YYYY-MM-DD terbaru)
4. **Pakai catatan lama** — jangan duplicate atau overwrite
5. **Konfirmasi** ke user kalau ada conflict/catatan obsolete

Skip rule: trivial task (typo fix, "halo", dll) → diskresi sendiri.

## Post-Work Update (WAJIB)
Tiap deploy/fix selesai:

1. **Update project note:**
   - Bump `last_updated` di frontmatter
   - Update `status` kalau berubah
   - Tambah baris Session Log dengan `[[YYYY-MM-DD — topik]]`

2. **Buat session log** di `30 Sessions/YYYY-MM-DD-<topik>.md`:
   ```markdown
   ---
   date: 2026-08-28
   project: [[Project — Kabekabe]]
   tags: [session, deploy, fix]
   ---
   
   # 2026-08-28 — <Topik Singkat>
   
   ## Task
   <apa yang diminta>
   
   ## Aksi
   - <langkah 1>
   - <langkah 2>
   
   ## File Diubah
   - `path/to/file.php` — <apa yang berubah>
   
   ## Hasil
   <verifikasi>
   
   ## Link
   - [[Project — Kabekabe]]
   - [[Hosting — cPanel emerald]]
   - Next: <TODO>
   ```

3. **Update knowledge note** kalau ada keputusan baru (di `20 Knowledge/<topik>.md`)

## Wikilink Convention
- Project: `[[Project — Kabekabe]]`
- Knowledge: `[[Hosting — cPanel emerald]]`
- Session: `[[2026-08-28 — Setup CIEL Workflow]]`

## Vault Paths
- **Mirror Hermes:** `/root/projects/ciel/`
- **Asli HP (user):** `/sdcard/Obsidian/Ciel/`
- Sync via git (optional, nanti setup)

---
*Lihat juga: [[Sosho — Working Agreement (Hermes)]] · [[index]]*
