# Pemecahan masalah

## Pemasangan gagal di Magisk

Pastikan berkas yang dipasang adalah `IfwShield-Zygisk-vX.Y.Z.zip` dari **Assets**, bukan
*Source code*. Zip modul memiliki `module.prop` di akar zip. Kalau Safari mengekstrak zip
menjadi folder, matikan **Open 'safe' files after downloading** di pengaturan Safari.

## Zygisk berhenti sendiri ("Stop zygisk due to crash")

Diperbaiki pada **v1.5.14**: `system_server` crash berulang pada Android 11 karena layanan
`role` diakses sebelum terbit. Perbarui ke versi terbaru.

## Aplikasi tidak tersembunyi

+ Pastikan Zygisk aktif dan perangkat sudah di-reboot setelah pemasangan.
+ Setelah mengubah daftar **Sembunyikan**, tekan **Restart Launcher**.

## Meminta bantuan

Aktifkan **Mode Debug**, ulangi masalahnya, lalu tekan **Kumpulkan Log** untuk mendapatkan berkas
`.tar.gz`.
