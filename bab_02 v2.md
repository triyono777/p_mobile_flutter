## 5. Bab 2 v2: Halaman Profil Sederhana untuk Pemula
Bagian ini adalah versi yang lebih sederhana dari latihan Bab 2. Fokusnya hanya pada penggunaan widget dasar:
- `CircleAvatar`
- `Column`
- `Row`
- `Text`
- `Container`
- `SizedBox`

### 5.1 Tujuan
- Mengenal cara menyusun tampilan profil sederhana.
- Memahami fungsi widget dasar untuk membuat UI Flutter.
- Melatih mahasiswa pemula agar terbiasa membaca struktur widget dari atas ke bawah.

### 5.2 Langkah Praktikum
1. Buat file `lib/main.dart` dan ubah isinya menjadi:
   ```dart
   import 'package:flutter/material.dart';
   import 'profil_sederhana_v2.dart';

   void main() {
     runApp(const MyApp());
   }

   class MyApp extends StatelessWidget {
     const MyApp({super.key});

     @override
     Widget build(BuildContext context) {
       return const MaterialApp(
         debugShowCheckedModeBanner: false,
         home: ProfilSederhanaV2(),
       );
     }
   }
   ```
2. Buat file baru `lib/profil_sederhana_v2.dart`, lalu isi dengan kode berikut:
   ```dart
   import 'package:flutter/material.dart';

   class ProfilSederhanaV2 extends StatelessWidget {
     const ProfilSederhanaV2({super.key});

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(
           title: const Text('Profil Sederhana'),
         ),
         body: Center(
           child: Container(
             padding: const EdgeInsets.all(16),
             margin: const EdgeInsets.all(16),
             color: Colors.blue.shade50,
             child: Column(
               mainAxisSize: MainAxisSize.min,
               children: [
                 const CircleAvatar(
                   radius: 40,
                   child: Icon(Icons.person, size: 40),
                 ),
                 const SizedBox(height: 16),
                 const Text(
                   'Budi Santoso',
                   style: TextStyle(
                     fontSize: 20,
                     fontWeight: FontWeight.bold,
                   ),
                 ),
                 const SizedBox(height: 8),
                 const Text('Mahasiswa Informatika'),
                 const SizedBox(height: 16),
                 const Row(
                   mainAxisSize: MainAxisSize.min,
                   children: [
                     Icon(Icons.email),
                     SizedBox(width: 8),
                     Text('budi@gmail.com'),
                   ],
                 ),
                 const SizedBox(height: 8),
                 const Row(
                   mainAxisSize: MainAxisSize.min,
                   children: [
                     Icon(Icons.phone),
                     SizedBox(width: 8),
                     Text('08123456789'),
                   ],
                 ),
               ],
             ),
           ),
         ),
       );
     }
   }
   ```

### 5.3 Penjelasan Widget
- `Scaffold` dipakai sebagai kerangka utama halaman.
- `AppBar` menampilkan judul halaman.
- `Center` memosisikan isi di tengah layar.
- `Container` menjadi pembungkus area profil.
- `Column` menyusun isi profil dari atas ke bawah.
- `CircleAvatar` menampilkan foto atau ikon profil.
- `SizedBox` memberi jarak antar widget.
- `Row` dipakai untuk menampilkan icon dan teks pada satu baris.
- `Text` menampilkan nama, status, email, dan nomor telepon.

### 5.4 Hasil yang Diharapkan
Mahasiswa akan mendapatkan satu halaman profil sederhana dengan:
- foto profil berbentuk lingkaran
- nama dan keterangan singkat
- baris email
- baris nomor telepon

### 5.5 Tugas Latihan v2
1. Ganti nama `Budi Santoso` dengan nama Anda sendiri.
2. Ganti email dan nomor telepon dengan data contoh lain.
3. Tambahkan satu `Row` lagi untuk menampilkan alamat atau kelas.
