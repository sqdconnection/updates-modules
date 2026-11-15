# Changelog

## v1.5.15

### Changed
- **Sumber update dipindah** ke repo publik khusus `sqdconnection/updates-modules`
  (hanya `update.json`, changelog, dan zip rilis; kode sumber tetap private).
  `updateJson` di `module.prop` kini menunjuk ke sana. Modul v1.5.14 yang masih
  menunjuk repo lama tetap menemukan update ini lewat jembatan di repo lama.

## v1.5.14

Fokus: memperbaiki **ZygiskNext mati sendiri setelah IfwShield dipasang** (Android
11, Infinix Hot Smart 6), dan merapikan WebUI. Teruji live di Infinix X657B
(Android 11, 32-bit, Magisk Alpha + ZygiskNext 1.5.3).

### Fixed
- **system_server crash-loop, ZygiskNext "Stop zygisk due to crash".** Thread
  refresh `AppHide` memanggil `getSystemService("role")` sebelum service `role`
  terbit. Di Android 11 kegagalan fetch pertama meracuni cache
  `SystemServiceRegistry` pada Context sistem, sehingga
  `VoiceInteractionManagerService` mendapat `RoleManager` null -> NPE ->
  system_server mati berulang -> ZygiskNext berhenti dan Hide tak jalan.
  Kini `homeRoleHolder` memeriksa `ServiceManager.checkService("role")` dulu
  (tanpa efek samping ke cache) dan memakai `resolveActivity` bila belum terbit.
- **Daftar Sembunyikan tak tersimpan.** `hide.list` hanya ditulis saat tombol
  Terapkan ditekan, jadi melepas centang lalu Restart Launcher tak mengubah apa pun.

### Added
- **Update lewat Magisk.** `module.prop` kini punya `updateJson` yang menunjuk ke
  repo publik (`ifw-hide-assets`); kartu modul menampilkan tombol Update bila ada
  versi lebih baru. `scripts/publish-update.sh` menerbitkan `update.json` + zip.
  Rilis v1.5.14 adalah yang pertama membawa fitur ini, jadi update pertama yang
  akan muncul otomatis adalah v1.5.15.

### Changed
- **Sembunyikan tersimpan otomatis** (debounce 500 ms: `hide-set`,
  `connected-map-set`, `launcher-visible-set`, `hide-inject`); hook memuat ulang
  live. Restart Launcher menuntaskan simpan yang tertunda lebih dulu. Mode Buta,
  Terhubung, dan Opsi Pengembang sudah otomatis sebelumnya. Layanan & Aktivitas
  tetap lewat tombol Terapkan.
- Tombol "Restart Pengaturan" diganti nama **Terapkan** (EN: Apply).
- **Dialog Tentang** ditulis ulang: header berlogo + badge versi, fitur berikon
  beserta cara simpannya, footer kredit.
- **Bottom nav lampu sorot**: satu berkas cahaya meluncur ke tab aktif dengan
  pegas, jeda, dan kedipan saat tiba; hormati prefers-reduced-motion.

### Removed
- **Fallback Hide tanpa Zygisk** (`/data/adb/service.d/ifwshield-fallback.sh`)
  dibuang total. Memasang modul membersihkan salinan lamanya. Konsekuensi: saat
  Zygisk mati, Hide tidak lagi disuntik lewat ptrace dan aturan Layanan/Aktivitas
  tidak dipulihkan saat boot.

## v1.5.13

Fokus: Hide tetap jalan di perangkat tempat Zygisk (ZygiskNext) mati atau bootnya
lambat, dan modul native jauh lebih kecil. Teruji live di TECNO CN7c (Android 16,
Magisk Alpha + ZygiskNext).

### Fixed
- **Hook Hide tak pernah terpasang ulang.** `installHooks()` dulu dicoba sekali dan
  baru diulang kalau `config.json` berubah. Kini ada supervisor dengan backoff
  (2..30 dtk) sampai berhasil, kunci dilepas di semua jalur gagal, dan hook tidak
  menumpuk ganda.
- **Penanda "hook hidup" palsu.** PID `system_server` sama di tiap boot, jadi
  penanda boot lama dikira masih berlaku. Penanda kini `<pid> <starttime>`;
  `ifwctl hook-live` baru memeriksa boot ini. Juga memperbaiki jaga suntik-ganda
  pada `hide-inject`.
- **Lag di jalur panas hook.** Refleksi `Field`/`Method` di-cache per kelas (dulu
  `NoSuchMethodException` dilempar tiap panggilan `shouldFilterApplication` di
  Android 11); cache uid->paket tak lagi dikosongkan tiap 3 dtk.
- **Migrasi nama paket helper shortcut** `org.sqd.ifwshield.shortcut` ->
  `io.github.ifwshield.shortcut` di daftar tersimpan dan `config.json`.

### Added
- **Fallback Hide tanpa Zygisk** (`/data/adb/service.d/ifwshield-fallback.sh`).
  Magisk mengabaikan modul ber-folder `zygisk/` saat Zygisk mati, jadi skrip ini
  mandiri: begitu layanan paket siap (sebelum launcher menyala) ia menyuntik hook
  lewat `ifwctl hide-inject`; me-restart launcher hanya bila launcher terlanjur
  menyala sebelum suntikan; dan menjalankan `ifwctl sync` agar aturan
  Layanan/Aktivitas tetap dipulihkan. Diam total saat Zygisk normal.
- **Pemutus sirkuit companion.** Halaman memori bersama (dibuat di zygote) membuat
  companion yang macet hanya menahan satu peluncuran app per 30 dtk, bukan semua.
- `ifwctl hook-live`.

### Changed
- **Modul native 63% lebih kecil**: `libifwshield.so` arm64 328 KB -> 123 KB,
  armeabi-v7a 200 KB -> 75 KB. Penyebab: `std::ifstream`/`ostringstream` di
  `apphide.cpp` menyeret seluruh mesin locale libc++ ke `.so` yang dimuat di
  setiap proses aplikasi; kini POSIX `read()`.
- Timeout companion 5 -> 2 dtk.
- **Ikon app WebUI memakai `ksu://icon/<pkg>`** (dirender manager); jalur helper
  lama (broadcast, polling, cache, noicon) dibuang. Ikon shortcut dan ikon tab
  Mode Buta diganti.

### Catatan
- armeabi-v7a/x86 hanya dikompilasi, belum dijalankan (perangkat uji arm64).
- Detail v1.5.9 - v1.5.12 tidak dicatat di sini.

## v1.5.8

### Added
- **Mode Buta (whitelist ala HMA-OSS).** Saklar baru di WebUI (tab Sembunyikan):
  app yang ditandai **Mode Buta** hanya bisa melihat **dirinya sendiri + app
  sistem**; selebihnya buta total. Kebalikan arah dari Hide (Hide menyembunyikan
  satu paket dari semua; Mode Buta membutakan satu caller terhadap semua
  non-sistem). Enforcement: `AppHide` + `ConfigStore` (field `whitelist` /
  `whitelistAllow`), caller selalu melihat dirinya & paket sistem.
- **Wiring native whitelist.** `ifwctl whitelist-set` (regen `config.json` live,
  terap ~3 dtk tanpa reboot), storage `whitelist.list`, `hide-status` melaporkan
  `whitelist`. App ber-Mode-Buta diurutkan ke paling atas daftar.

### Changed
- **Redesain WebUI.** Tampilan manager diperbarui (style.css) tanpa mengubah
  logika; semua id/class/i18n tetap sinkron.
- **Refresh staged dex saat boot.** `post-fs-data.sh` memanggil `ifwctl stage`
  sebelum `system_server`, supaya boot pertama setelah update modul memuat
  `hook.dex` terbaru (bukan dex lama) tanpa perlu hide-inject manual.
- Aplikasi manager terpisah dihapus; pengaturan sepenuhnya lewat WebUI.

## v1.5.7

### Added
- **Hide: full PrivacySpace-parity coverage + Android ≤10 path.** The Hide
  feature (ported from PrivacySpace 1.3.11) now hooks every package-query method
  PrivacySpace covers, closing the gaps where a hidden app could still be
  observed:
  - `getNameForUid` (uid→name lookups) — nulls the returned name when the
    queried uid's package is hidden from the caller (shared-uid names skipped).
  - `applyPostResolutionFilter` (PMS + ComputerEngine) — filters resolved
    results after resolution.
  - `getRunningAppProcesses` (ActivityManager) — drops a process record only
    when **all** of its `pkgList` packages are hidden from the caller.
  - `queryUsageStats` / `getUsageList` (UsageStatsService) — filters usage-stats
    entries for hidden packages.
  - `Settings.filterAppAccessLPr` — the **Android ≤10** access path, so Hide now
    installs on older (often 32-bit) devices that predate `ComputerEngine`
    (Android 12L/13+) and `AppsFilter` (Android 11+). Idempotent no-op on newer
    Android.
- All new hooks reuse the same `hiddenFromCaller` guard and `hookAll` registfrom
  registration (tolerant of a class/method being absent), so they are safe across Android
  versions and ABIs. Hide already ships ARM32 (`armeabi-v7a`) trampolines via
  v7878 vmtools — the only remaining 32-bit limiter was Android version, now
  addressed by the `filterAppAccessLPr` path.

### Changed
- Bumped version to **1.5.7** (versionCode 21).

### Notes
- The hook layer (`hook.dex`) loads at boot, so the new coverage takes effect
  after one **reboot**.

## v1.5.6

### Added
- **Diagnostics: Debug Mode + Collect Logs.** A new **Settings → Diagnostics**
  section to troubleshoot devices where Hide fails, IfwShield stalls, or the
  Zygisk implementation (e.g. ZygiskNext) turns itself off on install — seen on
  some MediaTek Android 11/12 phones, where a 32-bit (`armeabi-v7a`) zygote
  injection is the prime suspect.
  - **Debug Mode** toggle writes a `hide_debug` flag. From the next boot,
    `post-fs-data.sh` starts a persistent `logcat` capture **before zygote**,
    recording zygote/zygote64, native crash (`DEBUG`/`libc`) tombstones, the
    module tags and Zygisk/Magisk logs to `/data/adb/ifwshield/logs/` (rotating,
    survives reboot) — so the exact crash that disables Zygisk is captured.
  - **Collect Logs** button bundles everything (capture logs, current logcat,
    native tombstones, `config.json`, hook `module.log`, module list with
    disable/remove markers, device ABI and shipped `.so` list, `ifwctl` status)
    into a single `.tar.gz` in the **Download** folder, ready to share.
- **DEBUG build variant.** A separate `*-DEBUG.zip` that auto-enables Debug Mode
  on install (a `.debug` marker handled by `customize.sh`), so a tester can
  capture the first-boot crash without touching a shell. The normal release zip
  is unchanged and records nothing unless Debug Mode is turned on.
- **ABI diagnostics during install.** `customize.sh` now prints the device
  primary ABI, the full ABI list, and the module `.so` files it ships, to spot
  32-bit/64-bit mismatches immediately.

### Changed
- Bumped version to **1.5.6** (versionCode 20).

### Notes
- The boot-time log capture only runs when Debug Mode is on (or on a DEBUG
  build); the normal build has zero logging overhead otherwise.
- Capturing the zygote crash requires one **reboot** after enabling Debug Mode,
  because the capture starts at `post-fs-data`.

## v1.5.5

### Added
- **Hide Developer Options / USB debugging (per-app).** A self-contained feature
  with its own app list, independent of the zygote/Services (watch) list. Choose
  which apps see **Developer options** and **USB debugging** as OFF from a
  checklist under **Settings → Privacy** (collapsible; selected apps float to the
  top). It spoofs `adb_enabled`, `development_settings_enabled`, and
  `adb_wifi_enabled` only for the apps you tick — the real toggles in system
  Settings stay honest. Implemented as a per-caller hook on the Settings provider
  inside `system_server`; no APK is modified and no extra native injection is used.
- **Continuous Integration (GitHub Actions).** Builds the Zygisk module on every
  push / pull request and publishes a GitHub Release with the module zip on `v*`
  tags.
- **GPLv3 `LICENSE`** file.

### Changed
- **New module banner** (`ifwbanner.png`): the gold "IFW SHIELD" wordmark on a
  premium charcoal gradient. The previous banner is kept in the assets repo as an
  alternate option.
- **Modernized `README`**: `for-the-badge` badges (Build / Download / Android /
  License) and the new banner.
- Bumped version to **1.5.5** (versionCode 19).

### Notes
- The developer-options hook is loaded into `system_server` at boot, so a **reboot**
  is required once after installing/updating for its behavior to take effect.
  Changes to the app selection afterwards apply without a reboot (target apps may
  need a cold start to refresh their settings cache).
