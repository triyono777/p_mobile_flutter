# Bab 1: Pengenalan Flutter & Perencanaan Proyek

## 1. Tujuan Praktikum
- Memahami Flutter sebagai kerangka kerja multi-platform.
- Mampu melakukan instalasi dan mengkonfigurasi Flutter SDK.
- Mengenal bahasa pemrograman Dart dan IDE (Android Studio/VS Code).
- Mengenal struktur dasar antarmuka Flutter menggunakan widget `Scaffold`, `AppBar`, `Center`, dan `Text`.
- Menyiapkan proyek pertama dan merencanakan ide aplikasi.

## 2. Dasar Teori
### 2.1 Pengenalan Flutter
Flutter adalah framework UI open-source dari Google untuk membangun aplikasi multi-platform yang dikompilasi secara native dari basis kode tunggal (Android, iOS, Web, Desktop).

### 2.2 Arsitektur dan Dart
Flutter memanfaatkan bahasa pemrograman Dart yang mendukung kompilasi *Ahead-of-Time* (AOT) untuk performa native dan *Just-in-Time* (JIT) untuk fitur *Hot Reload* saat pengembangan.

### 2.3 Pengenalan Widget Dasar
Dalam Flutter, hampir semua tampilan dibangun dari widget. Widget adalah blok penyusun antarmuka, mulai dari teks sederhana sampai halaman aplikasi yang lengkap.

Berikut beberapa widget dasar yang sangat sering digunakan:
- **`Text`**: menampilkan tulisan pada layar.
- **`Center`**: menempatkan widget tepat di tengah area yang tersedia.
- **`AppBar`**: menampilkan baris header di bagian atas aplikasi, biasanya berisi judul dan aksi.
- **`Scaffold`**: kerangka dasar tampilan aplikasi Material Design. Widget ini biasa digunakan untuk menampung `AppBar`, `body`, `FloatingActionButton`, `Drawer`, dan komponen layar lain.

Contoh hubungan widget dasar:
- `MaterialApp` membungkus aplikasi utama.
- `Scaffold` menjadi kerangka satu halaman.
- `AppBar` diletakkan pada bagian atas `Scaffold`.
- `Center` diletakkan pada `body` untuk memusatkan konten.
- `Text` ditampilkan di dalam `Center`.

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

### 3.3 Mengenal Widget `Scaffold`, `AppBar`, `Center`, dan `Text`
1. Setelah proyek Flutter berhasil dibuat, buka file `lib/main.dart`.
2. Ganti isi file tersebut dengan contoh sederhana berikut:
   ```dart
   import 'package:flutter/material.dart';

   void main() {
     runApp(const MyApp());
   }

   class MyApp extends StatelessWidget {
     const MyApp({super.key});

     @override
     Widget build(BuildContext context) {
       return MaterialApp(
         debugShowCheckedModeBanner: false,
         home: Scaffold(
           appBar: AppBar(
             title: const Text('Aplikasi Flutter Pertama'),
           ),
           body: const Center(
             child: Text(
               'Halo Flutter',
               style: TextStyle(fontSize: 24),
             ),
           ),
         ),
       );
     }
   }
   ```
3. Perhatikan fungsi setiap widget pada contoh di atas:
   - `Scaffold` menyediakan struktur dasar satu halaman.
   - `AppBar` menampilkan judul halaman di bagian atas.
   - `Center` memosisikan isi agar berada di tengah layar.
   - `Text` menampilkan tulisan `Halo Flutter`.
4. Jalankan aplikasi dengan `flutter run`, lalu perhatikan hasil tampilannya.
5. Ubah isi `Text` menjadi nama Anda atau nama proyek Anda sendiri, lalu lakukan *Hot Reload* untuk melihat perubahan secara langsung.

### 3.4 Diskusi Ide dan Perencanaan Aplikasi
1. Pikirkan ide aplikasi mandiri yang fungsional (misal: Aplikasi Catatan, Aplikasi Kasir Sederhana, atau To-Do List).
2. Buat rancangan fitur-fitur minimal (MVP) yang harus ada di dalam aplikasi tersebut.
3. Buat sketsa (wireframe) kasar dari antarmuka pengguna (UI) aplikasi yang akan dibangun.

## 4. Tugas Latihan
1. Lakukan instalasi Flutter di laptop masing-masing dan pastikan hasil dari `flutter doctor` tidak ada error (semua indikator berwarna hijau).
2. Modifikasi contoh `main.dart` dengan ketentuan berikut:
   - Ubah judul `AppBar`
   - Ubah isi widget `Text`
   - Tambahkan satu widget `Text` lagi di bawah teks utama
3. Buat satu dokumen singkat yang berisi:
   - Nama Proyek Aplikasi
   - Deskripsi Singkat Aplikasi
   - Fitur Utama
   - Sketsa UI kasar (1-2 halaman)
