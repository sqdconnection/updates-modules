# IfwShield

Memblokir layanan/aktivitas zygote lewat IntentFirewall dan menyembunyikan aplikasi dari
aplikasi lain lewat hook di `system_server`, berjalan sebagai modul Zygisk.

> Repo ini hanya berisi dokumentasi dan sumber update (`update.json`). Berkas modul ada di
> **[Releases](../../releases)**.

## Persyaratan

### Umum

+ Android 10+ untuk blokir Layanan & Aktivitas, Android 11+ untuk Sembunyikan
+ Zygisk aktif (ZygiskNext atau Zygisk bawaan Magisk)
+ ABI arm64-v8a, armeabi-v7a, x86, atau x86_64

### Magisk

+ Pasang lewat **Modul → Pasang dari penyimpanan**
+ Magisk Alpha diuji bersama ZygiskNext 1.5.3

### KernelSU / APatch

+ Pasang lewat manajer modul masing-masing

## Mengunduh

Ambil `IfwShield-Zygisk-vX.Y.Z.zip` dari bagian **Assets** pada halaman
[Releases](../../releases).

> **Jangan** memilih *Source code (zip)* atau *Source code (tar.gz)*. Itu hanya isi repo ini
> (dokumentasi dan `update.json`), **bukan** modul.
>
> Safari secara bawaan mengekstrak zip menjadi folder. Matikan **Safari → Settings → General →
> "Open 'safe' files after downloading"**, atau gunakan browser lain.

## Update

Mulai v1.5.15, modul memeriksa update sendiri. Buka tab **Modul** di Magisk, tarik layar ke
bawah, lalu tekan **Perbarui** pada kartu IfwShield.

## Dokumentasi

+ [Fitur dan cara pakai](docs/ifwshield.md)
+ [Pemecahan masalah](docs/troubleshooting.md)
+ [Catatan perubahan](ifwshield/CHANGELOG.md)
