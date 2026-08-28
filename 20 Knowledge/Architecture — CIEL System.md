---
type: knowledge
tags: [architecture, ciel, portable, system-design, infra]
last_updated: 2026-08-28
---

# Architecture — CIEL System (Portable)

> Dokumentasi arsitektur lengkap CIEL vault + workflow + backup. Tujuannya: kalau di-replikasi ke device lain, cukup baca note ini → setup ulang bisa jalan tanpa mikir.

## 📌 Design Principles

1. **Single source of truth** — semua catatan kerja, keputusan, knowledge ada di satu vault.
2. **Live growth** — vault bertumbuh tiap ada kerjaan; tidak statis.
3. **Auto-backup** — perubahan di-push ke GitHub private tanpa intervensi.
4. **Zero-friction** — workflow tidak boleh bikin lambat. Kalau ada conflict, tanya user, jangan diam-diam overwrite.
5. **Portable** — device apapun (HP, laptop, server, container) bisa adopsi arsitektur sama.
6. **Reversible** — semua perubahan punya audit trail (git history + session log).

## 🏗️ Topologi

```
┌─────────────────────────────────────────────────────────────┐
│                      CIEL Vault                              │
│  /root/projects/ciel/  (Hermes mirror, block-device, fast)  │
└─────────────────┬───────────────────────────────────────────┘
                  │ git push (cron auto-backup)
                  ▼
       ┌────────────────────┐         ┌──────────────────────┐
       │ 10969sosho/ciel    │ ◀────── │ /sdcard/Obsidian/    │
       │ (GitHub private)   │  sync   │ Ciel/  (HP, manual)  │
       │ AUTHORITATIVE      │         │ user edits, no remote│
       └────────────────────┘         └──────────────────────┘
                  ▲
                  │ git pull (manual saat butuh sync HP→server)
                  │
       ┌────────────────────┐
       │ Other devices      │
       │ (laptop, future)   │
       │ clone 10969sosho/  │
       │ ciel → open di     │
       │ Obsidian → edit    │
       └────────────────────┘
```

## 📁 Struktur Folder (PARA + extras)

```
ciel/
├── 00 Inbox/         ← quick capture, belum diproses
├── 10 Projects/      ← per-project note (user-managed, folder per project)
├── 20 Knowledge/     ← knowledge base (hosting, github, profile, architecture)
├── 30 Sessions/      ← YYYY-MM-DD-<topik>.md (auto-generated per kerjaan)
├── 40 Cron/          ← cron jobs docs (audit trail)
├── 50 Workflow/      ← workflow & convention docs
├── 99 Archive/       ← note lama / selesai
├── record/           ← YouTube transcripts & record (user-managed, jangan disentuh Hermes)
├── .git/             ← git repo (private remote: 10969sosho/ciel)
├── .gitignore        ← exclude .obsidian/workspace.json, *.log, *.tmp
├── .obsidian/        ← Obsidian config (per-device, di-ignore)
└── index.md          ← Map of Content (halaman depan)
```

### Kenapa folder "20 Knowledge" bukan langsung di "10 Projects"?
- `10 Projects/` = project spesifik yang lagi dikerjakan / ada (DEPPA, CVSS, ADMS, dll)
- `20 Knowledge/` = info kontekstual yang dipakai lintas project (Hosting, GitHub, Profile, Architecture)
- `30 Sessions/` = jejak kerja (audit log)
- `40 Cron/` = daftar automation (audit trail)
- `50 Workflow/` = konvensi (cara kerja)

## 🔄 Workflow (lengkap)

### Pre-Work (WAJIB)
1. `search_files pattern="<keyword>" path="<vault>" file_glob="*.md"`
2. Baca `10 Projects/<Project>/` atau `20 Knowledge/Project — <Nama>.md`
3. Scroll `30 Sessions/` terakhir
4. Pakai catatan lama → jangan duplikat
5. Konfirmasi kalau ada conflict

### Post-Work (WAJIB)
1. Update project note (frontmatter `last_updated`, `status`, Session Log section)
2. Tambah `30 Sessions/YYYY-MM-DD-<topik>.md`:
   - Frontmatter: `date`, `project: [[...]]`, `tags: [session, ...]`
   - Body: Task / Aksi / File Diubah / Hasil / Link / Next
3. Kalau ada keputusan baru → tambah/update `20 Knowledge/<topik>.md`
4. **Jangan commit manual** — biarkan cron yang push tiap 5 menit (lihat [[Hermes Cron Jobs]])

### Wikilink Convention
- Project: `[[Project — Kabekabe]]` (samain dengan nama file)
- Knowledge: `[[Hosting — cPanel emerald]]`
- Session: `[[2026-08-28 — Setup CIEL Workflow]]`
- Pakai `[[ ]]`, BUKAN `[ ](path)` hardlink

## 🔐 Backup Strategy (3 lapis)

### Lapis 1: Git auto-push (HERMES)
- **Cron:** tiap 5 menit cek perubahan → `git add -A && git commit && git push`
- **Script:** `~/.hermes/scripts/ciel_autobackup.sh` (lihat file)
- **Remote:** `git@github.com:10969sosho/ciel.git` (private)
- **Coverage:** semua file MD + struktur folder
- **Exclude:** `.obsidian/workspace.json` (per-device), `*.log`, `*.tmp`, `.bench_test`

### Lapis 2: Obsidian Sync (USER, manual/optional)
- Sync plugin Obsidian antar device (HP ↔ laptop)
- Atau via Syncthing / Google Drive
- Conflict resolution: pakai versi paling baru + cek git log

### Lapis 3: Local snapshot (jika ada)
- `/root/workspace/archive/` untuk snapshot manual kalau perlu rollback besar

## 🚀 Setup Ulang di Device Baru

Ikuti langkah ini di device manapun (HP, laptop, server, container):

### Prasyarat
- `git` terinstall
- `bash` (atau compatible shell)
- `ssh` key yang sudah di-add ke GitHub (`10969sosho`)
- Obsidian (optional, untuk UI)

### Langkah
1. **Clone vault:**
   ```bash
   git clone git@github.com:10969sosho/ciel.git /path/to/ciel
   cd /path/to/ciel
   ```
2. **Buat cron auto-backup** (kalau ini server/Hermes, bukan HP):
   ```bash
   # 1. Buat script
   cat > ~/.hermes/scripts/ciel_autobackup.sh <<'EOF'
   #!/bin/bash
   VAULT="/path/to/ciel"
   cd "$VAULT" || exit 1
   [ -z "$(git status --porcelain)" ] && exit 0
   git add -A
   git commit -m "auto: $(date -u +%Y-%m-%dT%H:%M:%SZ)" || exit 0
   git push origin main
   EOF
   chmod +x ~/.hermes/scripts/ciel_autobackup.sh
   
   # 2. Cron tiap 5 menit
   (crontab -l 2>/dev/null; echo "*/5 * * * * ~/.hermes/scripts/ciel_autobackup.sh") | crontab -
   ```
3. **Load skill `ciel-workflow`** di Hermes (sudah built-in di skill bundle).
4. **Buka di Obsidian** (optional):
   - Open vault as folder → `/path/to/ciel`
   - Install plugin: Templater, Dataview, Calendar (opsional)
5. **Verifikasi:**
   - Edit file baru di `30 Sessions/`
   - Tunggu ≤ 5 menit
   - Cek `git log` di remote: `gh repo view 10969sosho/ciel --web`

## 🛡️ Aturan (Anti-Pattern)

- ❌ Jangan tulis catatan kerja di luar CIEL (kecuali JSON data di `/root/knowledge/`)
- ❌ Jangan hardlink path di wikilink — pakai `[[Nama]]`
- ❌ Jangan overwrite note user di `00 Inbox/` atau `record/` tanpa konfirmasi
- ❌ Jangan commit manual kalau cron auto-backup sudah jalan (kecuali untuk commit message bermakna)
- ❌ Jangan sync `/sdcard/Obsidian/Ciel` ke `/root/projects/ciel` via copy/symlink — biarkan git yang jadi single sync mechanism (untuk hindari FUSE slowness & conflict)

## 🧩 Integration dengan Skill lain

| Skill | Gunanya |
|---|---|
| `ciel-workflow` | enforce pre-work check + post-work update |
| `obsidian` | read/write/patch file di vault |
| `sosho-infra-ops` | deploy cPanel, GitHub, Termux |
| `github-pr-workflow` | kalau mau pakai PR untuk perubahan besar |

## 📊 State Saat Ini (2026-08-28)

- **Vault path:** `/root/projects/ciel/` (15MB, block-device, fast)
- **Remote:** `10969sosho/ciel` (private, sudah di-push)
- **Cron auto-backup:** `ciel_autobackup.sh` (akan dibuat di sesi ini)
- **Skill:** `ciel-workflow` (loaded)
- **Memory:** sudah di-update dengan path & protocol
- **Original HP location:** `/sdcard/Obsidian/Ciel/` (no remote, user-managed)

## 🔄 Migrasi Data Existing

Data existing user di HP (`/sdcard/Obsidian/Ciel/`) **tidak dipindah** — biarkan sync via git. Alasan:
- HP mungkin edit lokal tanpa network
- Symlink FUSE bikin lambat
- Git conflict resolution lebih reliable daripada rsync/cp

Saat user pull dari device baru, history CIEL sudah ada di GitHub → tinggal `git pull`.

---
*Lihat juga: [[CIEL Workflow]] · [[Sosho — Working Agreement (Hermes)]] · [[Hermes Cron Jobs]] · [[Hosting — cPanel emerald]] · [[index]]*
