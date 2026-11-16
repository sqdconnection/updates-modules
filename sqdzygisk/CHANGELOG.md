# Changelog

## pre-release-147

Titik mula fitur cek-update mandiri modul (`updateJson` di `module.prop`,
mengarah ke `update.json` ini). Versi SEBELUM ini tidak akan pernah menemukan
update apa pun lewat Magisk/KernelSU/APatch -- `module.prop`-nya tidak punya
`updateJson`.

### Added
- `version=`/`versionCode=` `module.prop` kini dibakukan saat build dari satu
  sumber versi yang sama dengan APK manager (tidak lagi nilai statis), supaya
  cek-update selalu melaporkan versi yang benar-benar dibundel di zip.
- Repo sumber tetap privat (`sqdconnection/SQD-Zygisk`); hanya `update.json`,
  `CHANGELOG.md`, dan zip rilis yang dipublikasikan di sini -- sama seperti pola
  `ifwshield/`.
