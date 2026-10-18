# RK Apps — jadikan web https://rkapps.netlify.app/ sebagai aplikasi Android

## Cara kerja
Aplikasi ini adalah "cangkang" (WebView) yang membuka `https://rkapps.netlify.app/` secara langsung.
Karena isinya diambil dari web, **setiap kali kamu deploy ke Netlify, aplikasi di HP semua orang ikut berubah** tanpa APK baru.
APK baru hanya perlu dibuat kalau kamu mengubah: ikon, nama aplikasi, alamat web, atau kode di folder `app/`.
Aplikasi butuh internet. Unduhan APK dari katalog disimpan di folder **Download** HP.

## Langkah 1 — Buat repositori GitHub
1. Masuk ke github.com → tombol **+** → **New repository**.
2. Nama: `rk-apps-android`, pilih **Public**, klik **Create repository**.
   (Public aman: kunci rahasia disimpan di *Secrets*, bukan di kode.)

## Langkah 2 — Unggah proyek
1. Ekstrak ZIP ini di laptop.
2. Di repositori: **Add file → Upload files**, seret SEMUA isi folder (`app`, `build.gradle`, `settings.gradle`, `gradle.properties`, `PANDUAN.md`, dst.). Klik **Commit changes**.
3. Folder tersembunyi `.github` sering tidak ikut terunggah. Cek: kalau tidak ada folder `.github/workflows/build.yml` di repositori, klik **Add file → Create new file**, ketik nama persis `.github/workflows/build.yml`, tempel isi file `SALIN-KE-.github-workflows-build.yml`, lalu **Commit**.

## Langkah 3 — Isi kunci tanda tangan (sekali saja)
Buka file `RAHASIA-KUNCI-APK.txt` (terpisah dari ZIP ini, JANGAN diunggah ke GitHub).
Di repositori: **Settings → Secrets and variables → Actions → New repository secret**. Buat 4 secret dengan nama & nilai persis seperti di file itu:
`KEYSTORE_BASE64`, `KEYSTORE_PASSWORD`, `KEY_ALIAS`, `KEY_PASSWORD`.
Kunci ini membuat semua APK yang kamu rilis punya tanda tangan sama, sehingga APK baru bisa **menimpa/memperbarui** APK lama. Simpan file itu baik-baik.
(Kalau dilewati, build tetap jalan, tapi APK memakai kunci sementara dan harus di-uninstall dulu sebelum memasang versi baru.)

## Langkah 4 — Build APK
1. Tab **Actions** → **Build RK Apps APK** → **Run workflow** → **Run workflow**. (Setiap commit ke `main` juga otomatis membangun ulang.)
2. Tunggu ±3–6 menit sampai ada tanda centang hijau.
3. Buka tab **Code** → bagian kanan **Releases** → **RK Apps (terbaru)** → unduh **RK-Apps.apk**.
   Itu tautan yang bisa kamu bagikan. (Alternatif: Actions → klik run → *Artifacts* → RK-Apps-APK.)

## Langkah 5 — Pasang di HP
Buka RK-Apps.apk dari Download → izinkan **Pasang dari sumber ini / sumber tidak dikenal** jika diminta → Pasang.

## Memperbarui
- **Konten/katalog/fitur web:** deploy ke Netlify seperti biasa. Aplikasi otomatis ikut. Tidak perlu apa-apa lagi.
- **APK (ikon, nama, alamat web):** ubah file → commit → naikkan `versionCode` di `app/build.gradle` (1 → 2 → 3, dst.) → tunggu build → unduh RK-Apps.apk terbaru dan pasang menimpa yang lama.

## Mengganti alamat web
Ubah `HOME` dan `HOST` di `app/src/main/java/com/rkdesign/rkapps/MainActivity.java` dan alamat di `app/src/main/assets/offline.html`.

## Kalau build gagal
Actions → klik run yang merah → klik langkah **Build APK** → salin pesan error paling bawah dan kirim ke saya.
