# Bab 1: Pengenalan Flutter & Perencanaan Proyek

## 1. Tujuan Praktikum
- Memahami Flutter sebagai kerangka kerja multi-platform.
- Mampu melakukan instalasi dan mengkonfigurasi Flutter SDK.
- Mengenal bahasa pemrograman Dart dan IDE (Android Studio/VS Code).
- Menyiapkan proyek pertama dan merencanakan ide aplikasi.

## 2. Dasar Teori
### 2.1 Pengenalan Flutter
Flutter adalah framework UI open-source dari Google untuk membangun aplikasi multi-platform yang dikompilasi secara native dari basis kode tunggal (Android, iOS, Web, Desktop).

### 2.2 Arsitektur dan Dart
Flutter memanfaatkan bahasa pemrograman Dart yang mendukung kompilasi *Ahead-of-Time* (AOT) untuk performa native dan *Just-in-Time* (JIT) untuk fitur *Hot Reload* saat pengembangan.

## 3. Langkah Praktikum
### 3.1 Instalasi Flutter SDK dan IDE
1. Unduh Flutter SDK dari situs resmi Flutter (flutter.dev) sesuai dengan sistem operasi (Windows/macOS/Linux).
2. Ekstrak file yang diunduh dan tambahkan path direktori `flutter/bin` ke *Environment Variables* (Windows) atau `.bash_profile` / `.zshrc` (macOS/Linux).
3. Instal IDE: Visual Studio Code atau Android Studio.
4. Instal ekstensi/plugin **Flutter** dan **Dart** pada IDE yang dipilih.
5. Jalankan perintah `flutter doctor` di terminal untuk memastikan semua dependensi (seperti Android SDK dan Android Emulator) sudah terinstal dan terkonfigurasi dengan benar.

### 3.2 Menyiapkan Proyek Flutter Baru
1. Buka terminal atau command prompt.
2. Buat proyek baru dengan menjalankan perintah:
   ```bash
   flutter create proyek_pertama
   ```
3. Masuk ke direktori proyek: `cd proyek_pertama`
4. Buka proyek tersebut menggunakan IDE (misalnya dengan mengetik `code .` jika menggunakan VS Code).
5. Hubungkan perangkat Android secara fisik atau jalankan Android Emulator.
6. Jalankan proyek dengan perintah `flutter run` atau melalui tombol "Run" di IDE.

### 3.3 Diskusi Ide dan Perencanaan Aplikasi
1. Pikirkan ide aplikasi mandiri yang fungsional (misal: Aplikasi Catatan, Aplikasi Kasir Sederhana, atau To-Do List).
2. Buat rancangan fitur-fitur minimal (MVP) yang harus ada di dalam aplikasi tersebut.
3. Buat sketsa (wireframe) kasar dari antarmuka pengguna (UI) aplikasi yang akan dibangun.

## 4. Tugas Latihan
1. Lakukan instalasi Flutter di laptop masing-masing dan pastikan hasil dari `flutter doctor` tidak ada error (semua indikator berwarna hijau).
2. Buat satu dokumen singkat yang berisi:
   - Nama Proyek Aplikasi
   - Deskripsi Singkat Aplikasi
   - Fitur Utama
   - Sketsa UI kasar (1-2 halaman)
