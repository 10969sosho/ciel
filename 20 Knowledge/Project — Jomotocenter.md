---
type: knowledge
tags: [project, laravel, ecommerce, motor, live]
domain: jomotocenter.com
github: Gen-ei-Ryodan/pak-joni-ecommerce (confirm)
hosting_path: ~/jomotocenter.com/
tech: [Laravel 13, MySQL, Blade]
last_updated: 2026-08-28
---

# Project — Jomotocenter.com

> E-commerce sparepart & motor. Buyer-facing app dengan flow: navbar produk → pilih type (motor/mobil/atv/sparepart) → pilih merk → muncul card produk.

## Flow Navigation
1. `/` (buyer.home) → Navbar dropdown "Produk"
2. Pilih type: `route('buyer.product.choose', ['categoryType' => 'motor'])` → `buyer/product/choose.blade.php` — grid merk
3. Klik merk → `route('buyer.product.categories', ['categoryType', 'brand'])` → `category-brand.blade.php` — card produk (motor/sparepart)

## File Penting

### Blade
| File | Fungsi |
|------|--------|
| `resources/views/buyer/product/choose.blade.php` | Grid pilih merk |
| `resources/views/buyer/category-brand.blade.php` | Grid card produk (merk → kategori) |
| `resources/views/layouts/buyer.blade.php` | Master layout buyer |

### CSS Assets
| File | Fungsi |
|------|--------|
| `public/assets/css/card.css` | `.grid`, `.grid-3` — card product grid |
| `public/assets/css/homepage.css` | Homepage sections |
| `public/assets/css/navbar.css` | Navbar + dropdown |

### Route Check
```bash
ssh -p 31988 alurelab@160.187.143.18 'grep -r "buyer.product" ~/jomotocenter.com/routes/'
```

## ⚠️ Issue (2026-08-28)
Card grid overflow horizontal — sudah di-fix:
- `card.css`: `.grid` + `.grid-3` ditambah `overflow: hidden`
- `choose.blade.php`: `auto-fill` → `repeat(3, 1fr)` agar max 3 per row
- Cache buster: `card.css?v=2` → `?v=3`

## CIEL Session
- [[2026-08-28 — Jomotocenter Card Grid Fix]]
