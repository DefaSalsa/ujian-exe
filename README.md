<!-- Panduan versi Windows -->
# Ujian MAN untuk Windows

Versi Windows dari aplikasi ujian. Dibuat dengan Tauri (Rust + WebView2).

## Cara membuat .exe
Tidak perlu install apa pun di komputer. Build dijalankan di GitHub.

1. Buat repository baru di GitHub (sebaiknya Private).
2. Upload **seluruh isi** folder ini, termasuk folder `.github` (aktifkan "tampilkan file tersembunyi" jika tidak terlihat).
3. Buka tab **Actions**, pilih **Build Windows**, tunggu tanda centang hijau (sekitar 10 sampai 15 menit pertama kali).
4. Klik run tersebut, lalu unduh artifact **UjianMAN**. Hasilnya berupa `UjianMAN.zip`.
5. Di dalam zip ada `UjianMAN.exe` (langsung dijalankan) dan installer `Ujian MAN_1.0.0_x64-setup.exe`. Tidak ada file lain yang perlu disimpan di samping aplikasi.

## Konfigurasi (build.env)
Semua pengaturan ada di file `build.env`. Ubah nilainya, lalu upload ulang dan jalankan build lagi.

| Kunci | Fungsi | Contoh |
|---|---|---|
| `APP_NAME` | Nama aplikasi (judul jendela, Start menu, nama installer) | `Ujian MAN` |
| `APP_ID` | ID unik aplikasi | `id.dstech.ujianmankeerom` |
| `PASSWORD` | Password untuk masuk dan keluar | `MAN123` |
| `HOME_URL` | Alamat halaman ujian | `https://mankeerom.yzz.me/ujian` |
| `LOGIN_TITLE` | Judul di halaman login | `Ujian Online MAN Keerom` |
| `LOGO` | File logo (PNG, sebaiknya persegi) untuk ikon dan halaman login | `logo.png` |
| `ARTIFACT_NAME` | Nama zip hasil download | `UjianMAN` |

Catatan:
- Spasi di nama file hasil build otomatis diganti `_`, misalnya `Ujian MAN` menjadi `Ujian_MAN_1.0.0_x64-setup.exe`.
- Mengubah `APP_ID` membuat data lama (sesi login) tidak ikut terbawa.
- Format setiap baris: `KUNCI=NILAI`, tanpa tanda kutip.

## Suara alarm
Suara alarm sudah ditanam di dalam `UjianMAN.exe`, jadi tidak ada file alarm terpisah di folder hasil build.

Untuk mengganti suara:
1. Siapkan file suara sendiri dengan nama `alarm.wav` (format WAV).
2. Ganti file `alarm.wav` di folder utama repository (sejajar dengan folder `src` dan `src-tauri`).
3. Upload ulang ke GitHub dan jalankan **Build Windows** lagi.

## Cara kerja
- Halaman login tampil sebagai jendela Windows biasa di tengah layar, bisa dipindah dan diubah ukurannya. Setelah password benar, aplikasi berubah jadi layar penuh dan selalu di atas, lalu halaman ujian dibuka.
- Navbar: Mundur, Maju, Refresh, Home, dan ✕ (✕ minta password).
- Warna navbar dan halaman login mengikuti tema Windows. Tema terang memakai putih, tema gelap memakai hitam pekat.
- Saat ujian berjalan: X dan Alt+F4 tidak menutup aplikasi. Hanya lewat ✕ atau Ctrl+Shift+Alt+K dengan password benar.
- Saat keluar dengan password benar, alarm berbunyi 1 kali penuh, lalu aplikasi tertutup.
- Tombol Windows diblokir selama aplikasi berjalan. Alt+Tab, Alt+Esc, dan Ctrl+Esc juga diblokir.
- Saat koneksi putus di halaman ujian, muncul "Jaringan lemah" dan aplikasi menyambung lagi saat koneksi kembali.

## Batasan (dibanding versi Android)
- **Tidak ada pin layar.** Windows tidak punya padanan langsung. Pengawas perlu menutup aplikasi lain sendiri.
- **Tidak ada pengaturan Jangan Ganggu.** Itu fitur Android.
- **Volume alarm dinaikkan lewat tombol volume Windows.** Aplikasi menekan tombol volume naik sampai maksimum saat alarm berbunyi. Kalau volume dikunci oleh pengaturan admin, cara ini tidak berhasil.
- **Jika halaman ujian gagal dimuat sama sekali** (misalnya internet mati saat dibuka), layar browser bawaan bisa muncul. Pesan "Jaringan lemah" hanya untuk koneksi yang putus setelah halaman sudah terbuka.
- **Kode belum pernah diuji di Windows.** Jika build gagal, kirim pesan error dari tab Actions.
