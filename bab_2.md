# Bab 2: Konsep Widget (Stateless vs Stateful)

## 1. Tujuan Praktikum
- Memahami konsep dasar Widget di Flutter.
- Mengetahui perbedaan mendasar antara `StatelessWidget` dan `StatefulWidget`.
- Mampu mengimplementasikan kedua jenis widget tersebut dalam pembuatan UI.

## 2. Dasar Teori
Di dalam Flutter, hampir semua elemen antarmuka pengguna adalah **Widget**. Widget mendeskripsikan bagaimana tampilan aplikasi seharusnya berdasarkan konfigurasi dan *state* (status) saat ini.
- **StatelessWidget**: Widget yang *state*-nya tidak dapat berubah (immutable) setelah dibangun. Digunakan untuk UI yang statis, seperti ikon atau label teks sederhana.
- **StatefulWidget**: Widget yang *state*-nya dapat berubah secara dinamis (mutable) selama masa hidup *widget* tersebut. Digunakan untuk elemen UI yang interaktif atau dapat diperbarui, seperti checkbox, radio button, atau tampilan bentuk form yang menerima input.

## 3. Langkah Praktikum

### 3.1 Membuat Aplikasi Sederhana dengan StatelessWidget
1. Buka file `lib/main.dart` dari proyek yang sudah dibuat dan buat file baru `lib/stateless_widget_demo.dart`.
2. Hapus seluruh isi `main.dart` dan ganti dengan kode berikut untuk membuat struktur dasar aplikasi:
   ```dart
   // File: lib/main.dart
   import 'package:flutter/material.dart';
   import 'stateless_widget_demo.dart';

   void main() {
     runApp(const MyApp());
   }

   class MyApp extends StatelessWidget {
     const MyApp({super.key});

     @override
     Widget build(BuildContext context) {
       return MaterialApp(
         title: 'Praktikum Widget',
         home: const StatelessWidgetDemo(),
       );
     }
   }
   ```
3. Selanjutnya di dalam file `lib/stateless_widget_demo.dart`, buat komponen Stateless-nya:
   ```dart
   // File: lib/stateless_widget_demo.dart
   import 'package:flutter/material.dart';

   class StatelessWidgetDemo extends StatelessWidget {
     const StatelessWidgetDemo({super.key});

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(
           title: const Text('Stateless Widget'),
         ),
         body: const Center(
           child: Text('Ini adalah StatelessWidget'),
         ),
       );
     }
   }
   ```
4. Jalankan aplikasi (Hot Reload) dan perhatikan hasilnya. Teks di tengah layar tidak akan berubah.

### 3.2 Memodifikasi Aplikasi Sederhana dengan StatefulWidget
1. Kita akan mengubah bagian isi (body) menjadi sebuah `StatefulWidget` agar interaktif.
2. Buat file baru terpisah dengan nama `lib/stateful_widget_demo.dart` yang memuat class `CounterWidget`.
3. Ubah impor (import) pada kode `lib/main.dart` agar memanggil layar dinamis kita:
   ```dart
   // File: lib/main.dart
   import 'package:flutter/material.dart';
   import 'stateful_widget_demo.dart';

   void main() {
     runApp(const MyApp());
   }

   class MyApp extends StatelessWidget {
     const MyApp({super.key});

     @override
     Widget build(BuildContext context) {
       return MaterialApp(
         title: 'Praktikum Widget',
         home: const CounterWidget(),
       );
     }
   }
   ```
4. Buat kode untuk layar interaktif ini di `lib/stateful_widget_demo.dart`:
   ```dart
   // File: lib/stateful_widget_demo.dart
   import 'package:flutter/material.dart';

   class CounterWidget extends StatefulWidget {
     const CounterWidget({super.key});

     @override
     State<CounterWidget> createState() => _CounterWidgetState();
   }

   class _CounterWidgetState extends State<CounterWidget> {
     int _counter = 0;

     void _incrementCounter() {
       setState(() {
         _counter++;
       });
     }

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(
           title: const Text('Stateful Widget'),
         ),
         body: Center(
           child: Column(
             mainAxisAlignment: MainAxisAlignment.center,
             children: <Widget>[
               const Text('Anda telah menekan tombol sebanyak:'),
               Text(
                 '$_counter',
                 style: Theme.of(context).textTheme.headlineMedium,
               ),
               const SizedBox(height: 20),
               ElevatedButton(
                 onPressed: _incrementCounter,
                 child: const Text('Tambah Angka'),
               ),
             ],
           ),
         ),
       );
     }
   }
   ```
5. Jalankan aplikasi.
6. Klik tombol "Tambah Angka" dan perhatikan bahwa teks angka akan terus bertambah. Modifikasi *state* dilakukan menggunakan metode `setState()`.

## 4. Tugas Latihan
1. Modifikasi aplikasi counter di atas dengan menambahkan satu tombol lagi untuk **mengurangi** angka (`_decrementCounter`).
2. Pastikan angka tidak bisa kurang dari 0 (berikan kondisi/validasi pada logika fungsi).
