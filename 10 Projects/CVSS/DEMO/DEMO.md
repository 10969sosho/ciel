# DEMO — CoffeeShop (E-commerce Kopi & Brewing Gear)

## Overview
Demo e-commerce toko kopi & brewing gear (CoffeeShop) — showcase lengkap untuk CVSS. Mencakup katalog produk, keranjang server-side, checkout dengan alamat wilayah 4-level (Kemendagri), pembayaran Midtrans Snap (sandbox), tracking nomor resi (AWB), dan admin panel. Tema buyer coffee (espresso/caramel/cream).

## Location
Folder lokal dihapus; source di mirror HP `/root/projects/CVSS/DEMO/` (Debian HP):
- **Source**: `/root/projects/CVSS/DEMO/`
- **Backend**: `/root/projects/CVSS/DEMO/backend/`
- **Frontend**: `/root/projects/CVSS/DEMO/frontend/`
- **Dokumentasi fitur lengkap**: `/root/projects/CVSS/DEMO/docs/FEATURES.md`

## Tech Stack
| Layer | Teknologi |
|-------|-----------|
| Frontend | Next.js 16, React 19, Tailwind v4, Zustand, framer-motion, axios |
| Backend | Laravel 11 + Sanctum |
| Payment | Midtrans Snap (sandbox) |
| Shipping | Biteship API (+ fallback ongkir hardcoded) |
| Wilayah | wilayah.id (proxy `/api/wilayah/*`) |

## Status
Active — development/iterasi tema buyer coffee (espresso/caramel/cream) baru saja diselesaikan.

## Fitur Utama
- Auth & role buyer/admin (Sanctum Bearer)
- Katalog produk + kategori + pencarian + paginasi
- Keranjang server-side per user
- **Alamat 1x**: disimpan di profil, otomatis terisi di checkout (default_address + kode wilayah)
- Checkout + pilihan ongkir (Biteship / fallback)
- Order + pembayaran Midtrans Snap + callback webhook
- Tracking AWB (nomor resi) dengan link Lacak via Google
- Admin panel: dashboard, produk, kategori, order (SlideOver + Toast)

## Related
- [[CVSS]]
- [[Laravel]]
- [[Next.js]]
- [[Payment Gateway]]
