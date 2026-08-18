# SSH HP Android sebagai Storage VPS

## Tujuan

HP POCO digunakan sebagai storage dan environment Linux remote untuk menyimpan atau menjalankan project CVSS. GitHub tetap menjadi source of truth utama; HP adalah working copy/backup remote.

## Konfigurasi Koneksi

Alias SSH berada di `~/.ssh/config` pada laptop:

```sshconfig
Host hppoco
    HostName 100.101.194.87
    Port 8022
    User u0_a279
    RequestTTY force
    RemoteCommand proot-distro login debian
```

Detail penting:

- `100.101.194.87` adalah alamat Tailscale HP.
- Port SSH Termux adalah `8022`.
- User SSH adalah `u0_a279`.
- Setelah login, konfigurasi otomatis masuk ke Debian melalui `proot-distro`.
- Password tidak disimpan di dokumentasi. Gunakan SSH key untuk setup permanen.

## Pengujian Koneksi

Uji jaringan tanpa login:

```bash
nc -vz -w 5 100.101.194.87 8022
```

Login interaktif:

```bash
ssh hppoco
```

Untuk menjalankan command non-interaktif, override `RemoteCommand` dan TTY karena alias memaksa shell Debian interaktif:

```bash
ssh -tt -o RemoteCommand=none -o RequestTTY=force hppoco \
  'proot-distro login debian -- bash -lc "pwd; git --version"'
```

## Workflow Git Project

Untuk project yang sudah ada di GitHub, clone langsung di Debian HP:

```bash
ssh -tt -o RemoteCommand=none -o RequestTTY=force hppoco \
  'proot-distro login debian -- bash -lc "git clone https://github.com/OWNER/REPOSITORY.git /root/projects/REPOSITORY"'
```

Verifikasi working copy:

```bash
ssh -tt -o RemoteCommand=none -o RequestTTY=force hppoco \
  'proot-distro login debian -- bash -lc "cd /root/projects/REPOSITORY && git status --short --branch"'
```

Untuk project lokal yang belum memiliki `.git` atau remote GitHub, `git clone` tidak dapat digunakan. Push project tersebut ke GitHub terlebih dahulu, atau gunakan `rsync` untuk pemindahan awal.

## Transfer Besar via ADB (USB) — Tercepat

Ketika HP terhubung ke Mac via kabel USB, gunakan `adb push` (jauh lebih cepat dari SSH Tailscale, ~63 MB/s).

Rencana umum:

1. Siapkan staging lokal (buang `.git`, `node_modules`, `vendor`, `.next`, dan folder yang sudah di-clone di HP).
2. `tar -cf` staging → satu file.
3. `adb push <file.tar> /sdcard/cvss-transfer.tar`.
4. Dari dalam Debian HP, ekstrak ke tujuan:

```bash
ssh -tt -o RemoteCommand=none -o RequestTTY=force hppoco \
  'proot-distro login debian -- bash -lc "mkdir -p /root/projects/CVSS && tar -C /root/projects/CVSS -xf /data/data/com.termux/files/home/storage/shared/cvss-transfer.tar && rm -f /data/data/com.termux/files/home/storage/shared/cvss-transfer.tar"'
```

Detail penting:

- `/sdcard` di Android = `/data/data/com.termux/files/home/storage/shared/` dari dalam Debian.
- Rootfs proot berada di `/data/data/com.termux/files/usr/var/lib/proot-distro/containers/debian/rootfs`; `/root/projects` = `.../rootfs/root/projects`.
- Tar dari macOS menyisipkan file `._*` (AppleDouble) dan folder `__MACOSX`; bersihkan setelah ekstrak:

```bash
find /root/projects/CVSS -name "._*" -type f -delete
find /root/projects/CVSS -type d -name "__MACOSX" -exec rm -rf {} +
```

- Pastikan perangkat terdeteksi: `adb devices -l`.

## Hasil Validasi 2026-08-17

- Port `100.101.194.87:8022` dapat dijangkau.
- Login password ke Termux berhasil.
- Debian `proot-distro` berhasil diakses.
- Git `2.47.3` tersedia di Debian.
- Koneksi keluar dari HP ke GitHub berhasil.
- Repository publik `octocat/Hello-World` berhasil diuji dengan `git clone`.
- Folder lokal `REFERENCE` berukuran sekitar `48 MB`, tetapi bukan repository Git.
- Repository portfolio `https://github.com/10969sosho/solusisurabaya.git` berhasil di-clone.
- Working copy portfolio berada di `/root/projects/solusisurabaya` pada HP.
- Clone portfolio tervalidasi pada branch `feature/portfolio-websites` dan folder `portofolio` tersedia.

## Keamanan

- Password yang pernah dibagikan di chat harus segera diganti.
- Jangan menaruh password di file konfigurasi, script, command history, atau note Obsidian.
- Buat SSH key khusus laptop ke HP dan matikan autentikasi password setelah key tervalidasi.
- Untuk GitHub private repository di HP, key `/root/.ssh/ymiits_deploy_ed25519` ditambahkan ke akun GitHub `10969sosho`; repository YMIITS menyimpan konfigurasi `core.sshCommand` sendiri.
- Batasi akses SSH melalui jaringan Tailscale dan jangan expose port `8022` ke internet publik.

## Obsidian Vault di HP

Repository vault: `git@github.com:10969sosho/ciel.git`

Clone pada Debian HP:

```text
/root/projects/ciel
```

Clone pada shared storage Android untuk dibuka oleh aplikasi Obsidian:

```text
/data/data/com.termux/files/home/storage/shared/Obsidian/Ciel
```

Di aplikasi Obsidian Android, pilih **Open folder as vault** lalu pilih folder `Obsidian/Ciel` dari shared storage.

Workflow aman:

1. Sebelum mengedit dari HP, jalankan `git pull --ff-only origin main`.
2. Edit note di Obsidian.
3. Commit dan push perubahan dari Termux atau Debian.
4. Sebelum mengedit dari laptop, jalankan `git pull --ff-only origin main`.

Jangan mengedit vault secara bersamaan di laptop dan HP karena dapat menghasilkan merge conflict. File `.obsidian/workspace.json` tidak disimpan agar layout setiap perangkat tetap independen.

## OpenCode di HP (Config + Auth)

Config OpenCode Mac disalin persis ke Debian HP (`HOME=/root`), termasuk auth, sehingga opencode di HP punya provider dan skills yang sama.

| Item | Lokasi di HP |
|---|---|
| Config global | `/root/.config/opencode/opencode.jsonc` (provider `bandelbanget`, `model: deepseek/deepseek-v4-flash`) |
| Global AGENTS | `/root/.config/opencode/AGENTS.md` |
| Skills (18) | `/root/.config/opencode/skills/` |
| Plugin | `/root/.config/opencode/plugins/crg-plugin.ts` (code-review-graph) |
| Auth | `/root/.local/share/opencode/auth.json` (OpenAI, DeepSeek, OpenCode Go) |
| Package | `node_modules` HP (aarch64) tidak di-overwrite; sudah punya `@opencode-ai/plugin` 1.18.18 |

Catatan: permission `external_directory` untuk Obsidian di config HP menunjuk ke `/data/data/com.termux/files/home/storage/shared/Obsidian/Ciel/**` (bukan path Mac).

Cara menjalankan opencode di HP dari Mac:

```bash
ssh -tt -o RemoteCommand=none -o RequestTTY=force hppoco 'proot-distro login debian -- bash -lc "cd /root/projects/ciel && opencode"'
```

Verifikasi: `opencode auth list` (3 credentials), `opencode models` (bandelbanget/deepseek/opencode-go), dan `opencode run --model deepseek/deepseek-v4-flash` berhasil.

## Related

- [[CVSS/CVSS]]
- [[PROJECT INDEX]]
- [[Git]]
- [[SSH]]
