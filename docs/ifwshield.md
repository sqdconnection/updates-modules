# Fitur dan cara pakai

Pengaturan dilakukan lewat **WebUI** modul (ketuk kartu modul di KernelSU / APatch / MMRL;
di Magisk dibutuhkan KsuWebUI atau MMRL). Tombol Aksi tidak membuka WebUI, hanya menyimpan log
diagnostik.

| Fitur | Fungsi | Penyimpanan |
|---|---|---|
| **Layanan** | Blokir service zygote per aplikasi. Opsi *Blokir Keras* (`pm disable`) lebih tahan bobol, tetapi aplikasi bisa tahu komponennya dimatikan. | Tekan **Terapkan** |
| **Aktivitas** | Blokir activity tertentu per aplikasi. | Tekan **Terapkan** |
| **Sembunyikan** | Sembunyikan aplikasi dari semua aplikasi lain. | Otomatis |
| **Terhubung** | Di dalam kartu aplikasi tersembunyi: pilih aplikasi yang tetap boleh melihatnya. | Otomatis |
| **Tampilkan di Launcher** | Aplikasi tersembunyi tetap tampil ikonnya di launcher. | Otomatis |
| **Mode Buta** | Aplikasi terpilih hanya bisa melihat dirinya sendiri, aplikasi sistem, dan yang diizinkan. | Otomatis |
| **Opsi Pengembang** (Pengaturan) | Palsukan status USB debugging / Opsi pengembang menjadi mati untuk aplikasi pilihan. | Otomatis |

Ikon di launcher baru berubah setelah **Restart Launcher**. Tombol **Restart Sistem** me-reboot
perangkat.

## Alat bantu

+ **Mode Debug** dan **Kumpulkan Log**: merekam log sejak boot dan membungkusnya menjadi `.tar.gz`
  untuk diagnosa.
+ **Ekspor / Impor / Reset** konfigurasi.
+ **Pintasan IfwWebUI**: membuka WebUI tanpa root.
