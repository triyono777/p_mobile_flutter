# Bab 5: Menampilkan Data List

## 1. Tujuan Praktikum
- Mengelola data dalam bentuk array/list.
- Memahami implementasi `ListView` untuk menampilkan daftar data.
- Mengubah (mapping) data array menjadi komponen antarmuka pengguna (Widget).

## 2. Dasar Teori
Pada aplikasi nyata, data jarang sekali berjumlah statis (hanya 1 atau 2). Kita sering berhadapan dengan data koleksi seperti daftar kontak, daftar produk, riwayat pesan, dan lainnya.
Dalam Flutter, cara utama menampilkan data koleksi yang bisa digulir (*scrollable*) adalah menggunakan `ListView`.
- **`ListView`**: Digunakan untuk daftar statis atau sedikit data.
- **`ListView.builder`**: Sangat direkomendasikan untuk daftar data dengan jumlah yang banyak atau tidak terbatas (seperti data dari internet), karena `builder` ini hanya merender elemen yang tampak di layar sehingga lebih hemat memori.

## 3. Langkah Praktikum

### 3.1 Menampilkan List Sederhana Secara Statis
1. Modifikasi atau buat file baru `lib/main.dart` untuk memanggil file list.
2. Gunakan `ListView` bawaan untuk menyusun widget secara statis di file terpisah:
   ```dart
   // File: lib/main.dart
   import 'package:flutter/material.dart';
   import 'list_statis.dart';

   void main() {
     runApp(const MaterialApp(
       home: Scaffold(
         body: ListStatis(),
       ),
     ));
   }
   ```
3. Buat file baru bernama `lib/list_statis.dart` dan pindahkan kode widget ke dalamnya:
   ```dart
   // File: lib/list_statis.dart
   import 'package:flutter/material.dart';

   class ListStatis extends StatelessWidget {
     const ListStatis({super.key});

     @override
     Widget build(BuildContext context) {
       return ListView(
         padding: const EdgeInsets.all(8),
         children: <Widget>[
           Container(
             height: 50,
             color: Colors.amber[600],
             child: const Center(child: Text('Data A')),
           ),
           Container(
             height: 50,
             color: Colors.amber[500],
             child: const Center(child: Text('Data B')),
           ),
           Container(
             height: 50,
             color: Colors.amber[100],
             child: const Center(child: Text('Data C')),
           ),
         ],
       );
     }
   }
   ```
3. Lakukan Hot Reload dan amati perubahan layarnya.

### 3.2 Menampilkan List Data Dinamis (ListView.builder)
1. Praktik ini menggunakan `ListView.builder` dengan menyiapkan list data berbasis list of object / array terlebih dahulu. Kita akan buat file terpisah.
2. Modifikasi kode `lib/main.dart` agar mengimpor file khusus list:
   ```dart
   // File: lib/main.dart
   import 'package:flutter/material.dart';
   import 'list_dinamis.dart';

   void main() {
     runApp(const MyApp());
   }

   class MyApp extends StatelessWidget {
     const MyApp({super.key});

     @override
     Widget build(BuildContext context) {
       return MaterialApp(
         title: 'Praktikum ListView',
         home: Scaffold(
           appBar: AppBar(title: const Text('Daftar Mahasiswa')),
           body: const ListDinamis(),
         ),
       );
     }
   }
   ```
3. Buat file baru `lib/list_dinamis.dart` dan tulis logika list view di dalamnya:
   ```dart
   // File: lib/list_dinamis.dart
   import 'package:flutter/material.dart';

   class ListDinamis extends StatelessWidget {
     const ListDinamis({super.key});

     // Daftar nama mahasiswa (bisa berupa data API di dunia nyata)
     final List<String> entries = const ['Budi', 'Siti', 'Joko', 'Ani', 'Dewi', 'Agus'];

     @override
     Widget build(BuildContext context) {
       return ListView.builder(
         padding: const EdgeInsets.all(8),
         itemCount: entries.length,
         itemBuilder: (BuildContext context, int index) {
           return Card(
             child: ListTile(
               leading: CircleAvatar(
                 child: Text(entries[index][0]), // Huruf inisial
               ),
               title: Text(entries[index]),
               subtitle: Text('Mahasiswa ke-${index + 1}'),
               trailing: const Icon(Icons.arrow_forward_ios),
               onTap: () {
                 // Aksi ketika item ListView ditekan
                 ScaffoldMessenger.of(context).showSnackBar(
                   SnackBar(content: Text('Anda memilih ${entries[index]}')),
                 );
               },
             ),
           );
         },
       );
     }
   }
   ```
3. Lakukan pengujian. Tampilan sekarang menggunakan desain `Card` dan komponen bawaan Material UI bernama `ListTile`. `ListTile` biasa digunakan untuk list lengkap dengan icon, judul, dan subjudul.

## 4. Tugas Latihan
1. Buatlah list data array yang berisi objek yang lebih kompleks. Contohnya daftar Produk:
   - Nama Obat/Produk
   - Harga (Int)
   - Deskripsi Singkat
2. Tampilkan semua data objek tersebut dengan `ListView.builder` dalam format card yang informatif.
