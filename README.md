# updates-modules

Sumber update otomatis untuk modul Magisk / KernelSU / APatch. Repo ini hanya berisi
berkas publik (`update.json`, changelog, dan zip rilis). Kode sumber tidak ada di sini.

## Mengunduh modul

Buka halaman **[Releases](../../releases)**, pilih rilis terbaru, lalu unduh berkas di bagian
**Assets** bernama `IfwShield-Zygisk-vX.Y.Z.zip`.

> **Jangan** pilih *Source code (zip)* atau *Source code (tar.gz)*. Itu hanya isi repo ini
> (`update.json` dan changelog), **bukan** modul, sehingga pemasangan di Magisk akan gagal.

Pasang lewat Magisk: **Modul → Pasang dari penyimpanan → pilih zip → reboot**.

## Pengguna Safari

Safari secara bawaan membuka zip otomatis menjadi folder, sehingga zip-nya hilang.
Matikan: **Safari → Settings → General → hilangkan centang "Open 'safe' files after
downloading"**, atau unduh memakai browser lain.

## Update lewat Magisk

Modul IfwShield v1.5.15 ke atas memeriksa update sendiri. Buka tab **Modul** di Magisk,
tarik layar ke bawah, lalu tekan **Perbarui** pada kartu IfwShield.

## Isi

| Jalur | Fungsi |
|---|---|
| `ifwshield/update.json` | Dibaca Magisk untuk mendeteksi versi baru |
| `ifwshield/CHANGELOG.md` | Catatan perubahan yang tampil di dialog update |
