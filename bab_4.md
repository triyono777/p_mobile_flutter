# Bab 4: Navigasi (Routing) Antar Halaman

## 1. Tujuan Praktikum
- Memahami konsep dasar navigasi dan *routing* di Flutter.
- Mampu memindahkan layar menggunakan `Navigator.push` dan `Navigator.pop`.
- Mampu mengirim data antar layar/halaman.

## 2. Dasar Teori
Aplikasi yang kompleks pasti memiliki lebih dari satu layar (halaman). Dalam Flutter, *halaman* dan *rute* memiliki arti yang sama, dan semuanya adalah widget.
Pengelolaan rute menggunakan konsep stack (tumpukan).
- **`Navigator.push`**: Menambahkan halaman baru ke atas tumpukan (stack) sehingga layar ini yang akan ditampilkan ke pengguna.
- **`Navigator.pop`**: Menghapus halaman saat ini dari tumpukan dan kembali ke halaman sebelumnya.

## 3. Langkah Praktikum

### 3.1 Membuat Navigasi Sederhana
1. Siapkan dua buah halaman. Halaman Pertama akan memiliki tombol untuk beralih ke Halaman Kedua dan akan berada di file terpisah.
2. Ketikkan kode berikut pada file `lib/main.dart`:
   ```dart
   // File: lib/main.dart
   import 'package:flutter/material.dart';
   import 'halaman_pertama.dart';

   void main() {
     runApp(const MaterialApp(
       title: 'Navigasi Dasar',
       home: HalamanPertama(),
     ));
   }
   ```
3. Buat file baru untuk Halaman Pertama `lib/halaman_pertama.dart`:
   ```dart
   // File: lib/halaman_pertama.dart
   import 'package:flutter/material.dart';
   import 'halaman_kedua.dart';

   class HalamanPertama extends StatelessWidget {
     const HalamanPertama({super.key});

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(
           title: const Text('Halaman Pertama'),
         ),
         body: Center(
           child: ElevatedButton(
             child: const Text('Pindah ke Halaman Kedua'),
             onPressed: () {
               // Navigasi ke halaman kedua
               Navigator.push(
                 context,
                 MaterialPageRoute(builder: (context) => const HalamanKedua()),
               );
             },
           ),
         ),
       );
     }
   }
   ```
4. Buat file baru untuk Halaman Kedua `lib/halaman_kedua.dart`:
   ```dart
   // File: lib/halaman_kedua.dart
   import 'package:flutter/material.dart';

   class HalamanKedua extends StatelessWidget {
     const HalamanKedua({super.key});

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(
           title: const Text('Halaman Kedua'),
         ),
         body: Center(
           child: ElevatedButton(
             onPressed: () {
               // Kembali ke halaman pertama
               Navigator.pop(context);
             },
             child: const Text('Kembali'),
           ),
         ),
       );
     }
   }
   ```
5. Lakukan Hot Reload atau `flutter run`. Cobalah untuk berpindah bolak-balik antar halaman. Perhatikan fungsi default dari AppBar yang otomatis menyediakan tombol panah mundur (back) ketika layar tersebut ditumpuk (di-*push*).

### 3.2 Mengirim Data antar Halaman (Pass Data via Constructor)
Seringkali ketika berpindah halaman kita ingin membawa informasi suatu data tertentu.
1. Modifikasi kode di `lib/halaman_kedua.dart`. Pada `HalamanKedua`, tambahkan sebuah properti dan definisikan di konstruktor.
   ```dart
   // File: lib/halaman_kedua.dart
   import 'package:flutter/material.dart';

   class HalamanKedua extends StatelessWidget {
     final String pesanData;

     // Modifikasi Constructor agar meminta data (required parameter)
     const HalamanKedua({super.key, required this.pesanData});

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(
           title: const Text('Halaman Kedua (Menerima Data)'),
         ),
         body: Center(
           child: Column(
             mainAxisAlignment: MainAxisAlignment.center,
             children: [
               Text('Data yang diterima: $pesanData', style: const TextStyle(fontSize: 20)),
               const SizedBox(height: 20),
               ElevatedButton(
                 onPressed: () {
                   Navigator.pop(context);
                 },
                 child: const Text('Kembali'),
               ),
             ],
           ),
         ),
       );
     }
   }
   ```
2. Kemudian pada bagian tombol di `lib/halaman_pertama.dart`, perbaiki cara pemanggilan rutenya agar mengirim argumen:
   ```dart
   // Di dalam file lib/halaman_pertama.dart
   Navigator.push(
     context,
     MaterialPageRoute(
       builder: (context) => const HalamanKedua(pesanData: 'Halo dari Halaman Pertama!'),
     ),
   );
   ```
3. Uji coba aplikasi tersebut.

## 4. Tugas Latihan
1. Gabungkan pelajaran dari Bab 3 dan Bab 4.
2. Buat Halaman 1 yang memiliki Form Input (NIM, Nama).
3. Buat tombol "Simpan & Beralih". Ketika tombol ditekan, aplikasi akan beralih ke Halaman 2 dan mengirim (pass) data input teks tersebut lalu dengan menampilkan teks "Selamat Datang [Nama], NIM Anda [NIM]" di halaman kedua.
