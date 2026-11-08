# Ringan Launcher

Launcher Android TV yang sangat ringan, dibuat untuk TV berspesifikasi rendah seperti **Xiaomi Mi TV 4A** (RAM sekitar 1 GB) yang sudah mulai lambat. Tampilannya bersih dan modern, tanpa layanan latar belakang, tanpa iklan, tanpa internet.

> **Status:** kode ini ditulis dan diperiksa strukturnya, tetapi belum diuji langsung di TV. Jika ada yang error saat build atau tidak berjalan di TV, catat pesan errornya lalu minta perbaikan.

## Fitur

- Grid aplikasi yang nyaman dinavigasi dengan remote (D-pad), kartu membesar sedikit dan berbingkai warna aksen saat disorot.
- Jam besar dan tanggal di kiri atas.
- Info RAM bebas dan tombol **Bersihkan RAM** (menghentikan proses latar belakang aplikasi lain).
- Pilih aplikasi yang ditampilkan: **Tampil**, **Di atas** (favorit), atau **Disembunyikan**.
- Tampilan bisa diatur:
  - Warna aksen (7 pilihan)
  - Latar belakang: Gelap, Hitam pekat, Aurora tenang (bawaan), Laut tenang, Hutan berkabut, Lavender senja, Terang, atau **foto sendiri**
  - Gaya kartu (Solid, Kaca transparan, Tanpa kartu) dan sudut kartu
  - Ukuran ikon dan jumlah kolom
  - Tampilkan atau sembunyikan nama aplikasi, jam, tanggal, info RAM
  - Format jam 24 atau 12 jam
  - Animasi saat disorot (matikan jika TV terasa berat)
- Pengaturan tersimpan otomatis. Ada tombol untuk mengembalikan tampilan ke awal.

## Cara membangun APK (tanpa PC, cukup dari HP)

Build dilakukan di server GitHub (GitHub Actions), jadi tidak perlu Android Studio.

1. Buat akun di [github.com](https://github.com) lalu buat repo baru (boleh privat).
2. Di repo, pilih **Add file > Create new file**. Pada kolom nama ketik `.github/workflows/build.yml`.
3. Tempel seluruh isi file `build.yml` yang diberikan, lalu tekan **Commit changes**.
4. Buka tab **Actions** dan tunggu proses selesai (sekitar 5 menit, tanda centang hijau).
5. Buka tab **Releases** di repo dan unduh `app-release.apk`. Jika gagal dipasang, pakai `app-debug.apk`.

`build.yml` sudah berisi seluruh source code. Setiap kali isinya diganti dan di-commit, APK baru otomatis dibuat dengan nomor build berikutnya.

## Memasang di TV

Pilih salah satu:

- **Flashdisk:** salin APK ke flashdisk, colok ke TV, buka lewat aplikasi pengelola file di TV, lalu pasang.
- **Send Files to TV:** pasang aplikasinya di HP dan TV (satu jaringan Wi-Fi), kirim APK dari HP, lalu pasang dari TV.
- **ADB lewat Wi-Fi:** `adb connect IP_TV:5555` lalu `adb install app-release.apk`. Bisa dari HP memakai aplikasi ADB seperti Bugjaeger.

Jika TV menolak pemasangan, aktifkan **Install dari sumber tidak dikenal** untuk aplikasi pengelola file yang dipakai.

## Menjadikan launcher utama

1. Setelah terpasang, tekan tombol **Home** di remote. Jika muncul pilihan launcher, pilih **Ringan Launcher** lalu **Selalu**.
2. Jika TV tetap membuka PatchWall dan tidak menawarkan pilihan, PatchWall perlu dinonaktifkan lewat ADB:
   1. Aktifkan ADB di TV: **Pengaturan > Preferensi Perangkat > Tentang**, tekan **Build** tujuh kali, lalu masuk ke **Opsi Pengembang** dan aktifkan **USB debugging** (atau **ADB over network**).
   2. Cari nama paket launcher bawaan: `adb shell pm list packages | grep -i -E "tvhome|patchwall"`. Nama paket bisa berbeda antar versi TV.
   3. Nonaktifkan: `adb shell pm disable-user --user 0 NAMA_PAKET`
   4. Untuk mengembalikan: `adb shell pm enable NAMA_PAKET`

**Penting:** nonaktifkan PatchWall hanya setelah Ringan Launcher terpasang. Kalau tidak ada launcher sama sekali, layar Home TV akan kosong.

## Cara memakai

| Tombol | Fungsi |
| --- | --- |
| Panah | Pindah sorotan |
| OK | Buka aplikasi |
| Menu atau tahan OK | Info aplikasi (hapus, nonaktifkan, paksa berhenti) |
| Home | Kembali ke atas daftar |
| Back | Tidak menutup launcher |

Tombol di kanan atas:

- **Bersihkan RAM**: menghentikan proses latar belakang aplikasi lain.
- **Atur Launcher**: pengaturan tampilan dan pilihan aplikasi.
- **Pengaturan TV**: pengaturan sistem bawaan TV.

### Mengatur aplikasi yang tampil

**Atur Launcher > Aplikasi yang ditampilkan.** Tekan OK pada sebuah aplikasi untuk memutar status Tampil, Di atas, Disembunyikan. Ada pintasan **Tampilkan semua** dan **Sembunyikan semua** (berguna jika ingin mulai dari kosong lalu memilih beberapa saja).

### Memakai foto sendiri sebagai latar

1. Kirim foto ke penyimpanan TV (misalnya folder **Download**) dengan Send Files to TV atau cara lain. Foto yang hanya ada di flashdisk mungkin tidak terbaca.
2. **Atur Launcher > Pilih foto latar**, izinkan akses foto, lalu pilih fotonya.
3. Foto dipotong 16:9 dan diperkecil ke 1280x720 agar ringan.
4. Atur **Redupkan foto latar** supaya teks tetap terbaca.

## Tips agar TV lebih cepat

Launcher ringan membantu, tetapi RAM 1 GB tetap terbatas. Kombinasikan dengan:

- Nonaktifkan PatchWall dan aplikasi bawaan yang tidak dipakai lewat ADB (`pm disable-user`), tanpa root.
- Sembunyikan aplikasi yang tidak dipakai dari launcher, lalu hapus atau nonaktifkan lewat info aplikasi.
- Matikan animasi sistem: **Opsi Pengembang** > skala animasi jendela, transisi, dan durasi menjadi 0,5x atau mati.
- Matikan **Animasi saat disorot** di **Atur Launcher** jika masih terasa berat.
- Tekan **Bersihkan RAM** sebelum membuka aplikasi yang berat.

## Pemecahan masalah

- **Build gagal di tab Actions:** buka proses yang gagal, salin pesan error pada langkah yang merah, lalu minta perbaikan.
- **APK tidak bisa dipasang:** coba `app-debug.apk`, atau aktifkan pemasangan dari sumber tidak dikenal.
- **Home tetap membuka PatchWall:** ikuti langkah menonaktifkan PatchWall di atas.
- **Aplikasi tertentu tidak muncul:** buka **Atur Launcher > Aplikasi yang ditampilkan** dan pastikan statusnya bukan Disembunyikan.
- **Foto latar tidak muncul di daftar:** pastikan fotonya ada di penyimpanan internal TV, bukan hanya di flashdisk, lalu buka layar pilih foto lagi.

## Struktur project

File `build.yml` menulis semua file berikut saat build:

```
settings.gradle.kts, build.gradle.kts, gradle.properties
app/build.gradle.kts
app/src/main/AndroidManifest.xml
app/src/main/java/id/ringan/launcher/
    MainActivity.kt        layar utama dan grid aplikasi
    SettingsActivity.kt    layar Atur Tampilan
    ManageAppsActivity.kt  pilih aplikasi yang tampil
    WallpaperActivity.kt   pilih foto latar
    ListPage.kt            kerangka layar pengaturan
    Prefs.kt               penyimpanan pengaturan
    Look.kt                warna dan bentuk dari pengaturan
    SoftBackground.kt      latar tenang (gradasi + cahaya lembut)
    Apps.kt                daftar aplikasi dan cache ikon
app/src/main/res/          layout, ikon, dan banner
```

Teknologi: Kotlin, Android Gradle Plugin 8.5, minSdk 21 (Android 5.0), targetSdk 34, satu dependensi (`androidx.recyclerview`).
