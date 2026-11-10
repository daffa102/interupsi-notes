# Handoff Interupsi

Terakhir diperbarui 10 November 2026 (WIB). Catatan ini merangkum keadaan terakhir yang diketahui. Cek ulang status Git, layanan, dan situs saat mulai pekerjaan baru.

## Lokasi kerja

- Workspace Codex ini menyimpan panduan dan handoff. Kode situs ada di `%USERPROFILE%\Herd\intrupsi-news`. Nama foldernya memang `intrupsi-news`.
- Situs publik di [interupsi.co.id](https://interupsi.co.id/). Pengembangan lokal memakai Herd di `http://intrupsi-news.test/`. Kalau koneksi ditolak, cek apakah Herd sedang berjalan.
- Stack menggunakan Laravel 13, Blade, Filament 5, Livewire 4, Tailwind CSS 4, dan Vite 8. Pakai Composer dan npm sesuai lockfile. Tes memakai Pest.
- Di repo Laravel, baca `AGENTS.md` dan `CLAUDE.md`. Referensi operasional ada di `docs/VPS.md`, `docs/MAINTENANCE.md`, `docs/MEDIA.md`, dan `docs/ROADMAP.md`. Beberapa catatan lama soal backup dan halaman statis mendahului pekerjaan di bawah.

## Sudah tayang

| Perubahan | Rilis production |
| --- | --- |
| Tag topik pindah ke bawah isi artikel | `10fc41c` |
| Patch dependency npm. Audit build di VPS mencatat 0 temuan saat deploy | `a5eaf95` |
| `ads.txt` untuk AdSense | `69ed179` |
| Tag GA4 di layout publik khusus production | `497fa24` |
| Breadcrumb di atas judul artikel dihapus | `8cf51d6` |
| Kebijakan Privasi ditautkan dari footer | `5fe7a8d` |

`5fe7a8d` adalah rilis terakhir yang dikonfirmasi dalam chat ini. Halaman privasi menggunakan Instagram resmi sebagai kanal kontak. [Halaman Kebijakan Privasi](https://interupsi.co.id/kebijakan-privasi)

## Google

- **Search Console** — Properti Domain terverifikasi lewat DNS Cloudflare. `sitemap.xml` dan `news-sitemap.xml` sudah dikirim. Inspeksi artikel menunjukkan Google menemukannya dari kedua sitemap. Daffa sudah meminta pengindeksan untuk beranda dan satu artikel. Cek hasil akhirnya di Inspeksi URL.
- **GA4** — Aliran Web Interupsi memakai URL `https://interupsi.co.id` dan Measurement ID `G-WX30VVSKJ2`. Daffa mengonfirmasi data kunjungan masuk.
- **AdSense** — Status terakhir yang dilaporkan `Sedang disiapkan`, dengan `ads.txt` berstatus `Diotorisasi`. Saat status situs menjadi `Siap`, bahas pemasangan satu slot iklan bawah artikel dan cek tampilan HP. [ads.txt publik](https://interupsi.co.id/ads.txt)

## VPS, gambar, dan backup

- Production berjalan di VPS Linux dengan Nginx, PHP 8.4, dan MySQL 8. Cloudflare berada di depan VPS. Gambar aktif disajikan dari `storage/app/public/media`, dengan varian WebP `thumb`, `medium`, dan `master`. Ketiga varian pada sampel satu artikel mendapat cache Cloudflare `HIT` saat diminta ulang.
- Backup harian di VPS menyimpan dump database dan perubahan media ke penyimpanan backup privat. Format inkremental memakai `media/files/` dan `db/` dalam commit yang sama. Snapshot pertama sudah diverifikasi di penyimpanan backup. Arsip lama masih tersedia di riwayat.
- Uji restore format lama pada 10 November berhasil mengimpor 147 artikel dan 2 akun serta menemukan 441 varian gambar utama. Pilot format inkremental berhasil memulihkan dua snapshot dengan isi identik. Snapshot inkremental pertama di GitHub sudah dicek isi dan integritasnya. Salinan aplikasi hasil restore belum dijalankan.

## Saat melanjutkan pekerjaan

1. Cek status Git di repo Laravel. Buat branch `feat/...` dari `develop` sesuai `CLAUDE.md`.
2. Verifikasi secara lokal lewat Herd atau tes Pest dengan SQLite in-memory. Gunakan RTK untuk perintah yang cocok jika tersedia. Di mesin ini RTK terpasang di `%LOCALAPPDATA%\Programs\RTK\1.2.0\rtk.exe`.
3. Untuk rilis yang sudah disetujui, merge ke `develop`, promosikan ke `release/v1.1`, lalu ikuti `docs/VPS.md`. Push ke remote `production` otomatis menjalankan deploy. Verifikasi URL publik yang terdampak sesudahnya.
4. Setelah perubahan penting, perbarui tanggal, hasil verifikasi, dan pekerjaan yang masih terbuka di file ini. Simpan kredensial dan isi `.env` di tempat yang memang disediakan untuk rahasia.

Untuk memulai chat baru, kirim: **“Baca AGENTS.md dan HANDOFF.md di workspace Interupsi, lalu lanjutkan dari status terakhir.”**
