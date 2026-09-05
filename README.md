# Vercel Monitor

Dashboard statis untuk monitoring akun Vercel — usage, limits, deployment status, dan alert otomatis.

## Fitur

- 📊 Ringkasan akun: bandwidth, function invocations, build minutes, dll
- 🚨 Alert visual saat resource mendekati limit (70/85/95%)
- 📦 Tabel semua project dengan status deployment terakhir
- 🔄 Auto-refresh tiap 60 detik
- 🔐 Akses via token URL (`?token=...`)
- 🔑 Token Vercel disimpan di browser (localStorage) — tidak ada token di file publik

## Cara Pakai

```
https://<user>.github.io/vercel-monitor/?token=vm-monitor-2026
```

Token URL default: `vm-monitor-2026`. Ubah di `EXPECTED_TOKEN` di `index.html`.

Saat pertama buka, Anda diminta masukkan Vercel token (disimpan di localStorage browser).

## Setup Awal (sudah dilakukan)

1. ✅ Vercel CLI token disimpan di `~/.local/share/com.vercel.cli/auth.json`
2. ✅ Repo GitHub: `vercel-monitor`
3. ✅ GitHub Pages di branch `main`

## Token Vercel — Rekomendasi

Saat pertama akses dashboard, masukkan **Personal Access Token read-only** (bukan CLI token):

1. Buka https://vercel.com/account/tokens
2. Klik **Create Token**
3. Name: `vercel-monitor-readonly`
4. Scope: **Read Only**
5. Copy token, paste di dashboard saat diminta

CLI token (`vcp_*`) punya akses deploy + ubah env var. PAT read-only lebih aman.

## ⚠️ Keamanan

- Token Vercel TIDAK di-hardcode di file HTML publik — GitHub Secret Scanning tidak akan reject push
- Token disimpan di `localStorage` browser — hanya device Anda yang punya akses
- Token URL `vm-monitor-2026` hanya阻挡 publik biasa. Untuk produksi, ganti di `EXPECTED_TOKEN`
- Untuk HTTPS-only tanpa expose token: butuh server-side proxy (Vercel Function), tapi berarti balik ke Vercel

## ⚠️ Token yang sudah terekspos

Karena `vcp_4304VJ...` (CLI token Vercel) dan `ghp_5zsvvs...` (GitHub PAT) sudah pernah di-paste di chat ini, **segera rotate/revoke keduanya** di dashboard masing-masing.

## Update Dashboard

Edit `index.html`, lalu:

```bash
cd vercel-monitor
git add . && git commit -m "update" && git push
```

GitHub Pages auto-deploy dalam ~1 menit.

## Konfigurasi

- **Auto-refresh**: 60 detik (ubah `setInterval(refresh, 60000)` di `index.html`)
- **Alert threshold**: 70% / 85% / 95% (ubah fungsi `pctClass` di `index.html`)
- **Token URL**: ubah `EXPECTED_TOKEN` di `index.html`

## Catatan

- `usage.usage` items tergantung paket Vercel; Hobby punya lebih sedikit, Pro/Enterprise lebih banyak
- Per-project bandwidth breakdown pakai `/projects/{id}/usage` — terbatas untuk akun Pro+
- Deploy status real-time datang dari `/v6/deployments?projectId=...`