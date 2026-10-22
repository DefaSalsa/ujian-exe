# Ujian MAN untuk Windows

Versi Windows dari aplikasi ujian. Dibuat dengan Tauri (Rust + WebView2).

## Cara membuat .exe
Tidak perlu install apa pun di komputer. Build dijalankan di GitHub.

1. Buat repository baru di GitHub (sebaiknya Private).
2. Upload **seluruh isi** folder ini, termasuk folder `.github` (aktifkan "tampilkan file tersembunyi" jika tidak terlihat).
3. Buka tab **Actions**, pilih **Build Windows**, tunggu tanda centang hijau (sekitar 10 sampai 15 menit pertama kali).
4. Klik run tersebut, lalu unduh artifact **UjianMAN**. Hasilnya berupa `UjianMAN.zip`.
5. Di dalam zip ada `UjianMAN.exe` (langsung dijalankan) dan installer `Ujian MAN_1.0.0_x64-setup.exe`. Tidak ada file lain yang perlu disimpan di samping aplikasi.

## Mengganti nama hasil build
Ada dua nama yang bisa diganti: nama zip hasil download, dan nama file `.exe`.

**Nama zip (yang didownload dari tab Actions):**
1. Buka file `.github/workflows/build.yml`.
2. Cari baris `name:` di bagian `upload-artifact` (ada di bagian paling bawah).
3. Ganti nilainya, misalnya `name: UjianMAN`. Nama zip akan mengikuti nilai ini, jadi `UjianMAN.zip`.

**Nama file `.exe` dan installer:**
1. Di `src-tauri/Cargo.toml`, ubah `name` di bagian `[package]`.
2. Di `src-tauri/tauri.conf.json`, ubah `productName`.
3. Di `.github/workflows/build.yml`, ubah nama file di baris `Copy-Item src-tauri/target/release/...exe`.
Ketiganya harus sama. Kalau tidak, build bisa gagal atau file tidak ditemukan.

## Mengganti nama yang tampil di Windows
Ada dua nama yang dilihat di Windows, dan keduanya diatur terpisah:

**Nama aplikasi (Start menu, daftar aplikasi di Pengaturan, dan installer):**
- Ubah `productName` di `src-tauri/tauri.conf.json`, misalnya menjadi `"Ujian MAN"`.
- Ini juga mengubah nama file installer, jadi cek lagi nama `.exe` sesuai bagian di atas.

**Judul jendela (bar judul di bagian atas jendela):**
- Buka `src-tauri/src/main.rs`, cari `.title("Ujian MAN")` di fungsi `main()`, lalu ganti teks di dalam tanda kutip.

Setelah mengubah, upload ulang dan jalankan **Build Windows** lagi.

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

## Cara mengganti identifier (ID aplikasi)
Identifier (`id.dstech.ujianmankeerom`) adalah ID unik aplikasi di Windows. Nilainya ada di `src-tauri/tauri.conf.json`, baris `"identifier"`.

1. Buka `src-tauri/tauri.conf.json`, lalu ganti nilai `"identifier"`, misalnya menjadi `"id.dstech.ujianbaru"`.
2. Upload ulang ke GitHub dan jalankan **Build Windows** lagi.

Catatan:
- Identifier juga menentukan folder data aplikasi di `%LOCALAPPDATA%`. Kalau diganti, data lama (cookies dan sesi login) tidak ikut terbawa.
- Nama yang tampil di Windows tidak diatur oleh identifier. Itu diatur di `productName` dan judul jendela.

## Batasan (dibanding versi Android)
- **Tidak ada pin layar.** Windows tidak punya padanan langsung. Pengawas perlu menutup aplikasi lain sendiri.
- **Tidak ada pengaturan Jangan Ganggu.** Itu fitur Android.
- **Volume alarm dinaikkan lewat tombol volume Windows.** Aplikasi menekan tombol volume naik sampai maksimum saat alarm berbunyi. Kalau volume dikunci oleh pengaturan admin, cara ini tidak berhasil.
- **Jika halaman ujian gagal dimuat sama sekali** (misalnya internet mati saat dibuka), layar browser bawaan bisa muncul. Pesan "Jaringan lemah" hanya untuk koneksi yang putus setelah halaman sudah terbuka.
- **Kode belum pernah diuji di Windows.** Jika build gagal, kirim pesan error dari tab Actions.
