# Bab 4: Navigasi (Routing) Antar Halaman

## 1. Tujuan Praktikum
- Memahami konsep dasar navigasi dan *routing* di Flutter.
- Mampu memindahkan layar menggunakan `Navigator.push` dan `Navigator.pop`.
- Mampu mengirim data antar layar/halaman.
- Mampu menerima kembali data dari halaman kedua.
- Mengenal widget yang sering dipakai pada alur navigasi seperti `Card`, `ListTile`, `FloatingActionButton`, `SnackBar`, `ElevatedButton`, dan `OutlinedButton`.

## 2. Dasar Teori
Aplikasi yang kompleks pasti memiliki lebih dari satu layar (halaman). Dalam Flutter, *halaman* dan *route* sama-sama direpresentasikan sebagai widget.

Pengelolaan route menggunakan konsep *stack* (tumpukan).
- **`Navigator.push`**: Menambahkan halaman baru ke atas *stack*, lalu menampilkan halaman tersebut.
- **`Navigator.pop`**: Menghapus halaman aktif dan kembali ke halaman sebelumnya.
- **`Navigator.push`** mengembalikan objek `Future`, sehingga halaman pertama dapat menunggu hasil dari halaman kedua.
- Data antar halaman dapat dikirim melalui konstruktor, sedangkan data balik dapat dikembalikan saat `Navigator.pop(context, data)`.

## 3. Langkah Praktikum

### 3.1 Membuat Navigasi Sederhana yang Lebih Realistis
1. Siapkan dua halaman. Halaman pertama akan menampilkan beberapa menu yang bisa dibuka ke halaman detail.
2. Ketikkan kode berikut pada file `lib/main.dart`:
   ```dart
   // File: lib/main.dart
   import 'package:flutter/material.dart';
   import 'halaman_pertama.dart';

   void main() {
     runApp(const MyApp());
   }

   class MyApp extends StatelessWidget {
     const MyApp({super.key});

     @override
     Widget build(BuildContext context) {
       return MaterialApp(
         debugShowCheckedModeBanner: false,
         title: 'Navigasi Dasar',
         theme: ThemeData(
           colorSchemeSeed: Colors.teal,
           useMaterial3: true,
         ),
         home: const HalamanPertama(),
       );
     }
   }
   ```
3. Buat file `lib/halaman_pertama.dart`:
   ```dart
   // File: lib/halaman_pertama.dart
   import 'package:flutter/material.dart';
   import 'halaman_kedua.dart';

   class HalamanPertama extends StatelessWidget {
     const HalamanPertama({super.key});

     Future<void> _bukaHalaman(
       BuildContext context, {
       required String judul,
       required String deskripsi,
     }) async {
       final hasil = await Navigator.push<String>(
         context,
         MaterialPageRoute(
           builder: (context) => HalamanKedua(
             judulHalaman: judul,
             deskripsi: deskripsi,
             pesanData: 'Halo dari Halaman Pertama',
           ),
         ),
       );

       if (!context.mounted) return;

       if (hasil != null && hasil.isNotEmpty) {
         ScaffoldMessenger.of(context).showSnackBar(
           SnackBar(content: Text(hasil)),
         );
       }
     }

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(
           title: const Text('Halaman Pertama'),
         ),
         body: ListView(
           padding: const EdgeInsets.all(16),
           children: [
             const Text(
               'Pilih menu di bawah untuk membuka halaman detail.',
               style: TextStyle(fontSize: 16),
             ),
             const SizedBox(height: 16),
             Card(
               child: ListTile(
                 leading: const CircleAvatar(
                   child: Icon(Icons.person),
                 ),
                 title: const Text('Profil Mahasiswa'),
                 subtitle: const Text(
                   'Contoh navigasi dengan data via constructor',
                 ),
                 trailing: const Icon(Icons.arrow_forward_ios),
                 onTap: () => _bukaHalaman(
                   context,
                   judul: 'Profil Mahasiswa',
                   deskripsi: 'Halaman ini menampilkan data profil sederhana.',
                 ),
               ),
             ),
             Card(
               child: ListTile(
                 leading: const CircleAvatar(
                   child: Icon(Icons.book),
                 ),
                 title: const Text('Mata Kuliah'),
                 subtitle: const Text(
                   'Contoh halaman detail kedua dengan route yang sama',
                 ),
                 trailing: const Icon(Icons.arrow_forward_ios),
                 onTap: () => _bukaHalaman(
                   context,
                   judul: 'Daftar Mata Kuliah',
                   deskripsi:
                       'Halaman ini dapat diisi daftar mata kuliah semester berjalan.',
                 ),
               ),
             ),
             const SizedBox(height: 16),
             ElevatedButton.icon(
               onPressed: () => _bukaHalaman(
                 context,
                 judul: 'Promo Praktikum',
                 deskripsi:
                     'Tombol biasa juga bisa dipakai untuk melakukan routing.',
               ),
               icon: const Icon(Icons.open_in_new),
               label: const Text('Buka Halaman Kedua'),
             ),
           ],
         ),
         floatingActionButton: FloatingActionButton.extended(
           onPressed: () => _bukaHalaman(
             context,
             judul: 'Bantuan',
             deskripsi:
                 'FloatingActionButton juga bisa digunakan untuk navigasi cepat.',
           ),
           icon: const Icon(Icons.navigation),
           label: const Text('Go'),
         ),
       );
     }
   }
   ```
4. Buat file `lib/halaman_kedua.dart`:
   ```dart
   // File: lib/halaman_kedua.dart
   import 'package:flutter/material.dart';

   class HalamanKedua extends StatelessWidget {
     final String judulHalaman;
     final String deskripsi;
     final String pesanData;

     const HalamanKedua({
       super.key,
       required this.judulHalaman,
       required this.deskripsi,
       required this.pesanData,
     });

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(
           title: Text(judulHalaman),
         ),
         body: Padding(
           padding: const EdgeInsets.all(16),
           child: Column(
             crossAxisAlignment: CrossAxisAlignment.stretch,
             children: [
               Card(
                 elevation: 2,
                 child: Padding(
                   padding: const EdgeInsets.all(16),
                   child: Column(
                     children: [
                       const Icon(Icons.send, size: 48, color: Colors.teal),
                       const SizedBox(height: 12),
                       Text(
                         pesanData,
                         textAlign: TextAlign.center,
                         style: Theme.of(context).textTheme.titleMedium,
                       ),
                     ],
                   ),
                 ),
               ),
               const SizedBox(height: 16),
               Text(
                 'Deskripsi Halaman',
                 style: Theme.of(context).textTheme.titleMedium,
               ),
               const SizedBox(height: 8),
               Text(deskripsi),
               const Spacer(),
               ElevatedButton.icon(
                 onPressed: () {
                   Navigator.pop(
                     context,
                     'Data dari $judulHalaman berhasil dikirim balik',
                   );
                 },
                 icon: const Icon(Icons.check_circle),
                 label: const Text('Kirim Hasil & Kembali'),
               ),
               const SizedBox(height: 12),
               OutlinedButton(
                 onPressed: () {
                   Navigator.pop(context);
                 },
                 child: const Text('Kembali Tanpa Data'),
               ),
             ],
           ),
         ),
       );
     }
   }
   ```
5. Jalankan aplikasi, lalu cobalah berpindah antar halaman lewat `ListTile`, `ElevatedButton`, dan `FloatingActionButton`.

### 3.2 Mengirim Data antar Halaman
Pada contoh di atas, data dikirim dari halaman pertama ke halaman kedua melalui konstruktor:
```dart
MaterialPageRoute(
  builder: (context) => HalamanKedua(
    judulHalaman: judul,
    deskripsi: deskripsi,
    pesanData: 'Halo dari Halaman Pertama',
  ),
),
```
Dengan cara ini, `HalamanKedua` dapat menerima data yang berbeda-beda meskipun menggunakan widget yang sama.

### 3.3 Mengembalikan Data dari Halaman Kedua
Halaman kedua juga dapat mengirim hasil kembali ke halaman pertama:
```dart
Navigator.pop(
  context,
  'Data dari $judulHalaman berhasil dikirim balik',
);
```
Karena `Navigator.push` mengembalikan `Future`, maka halaman pertama bisa menunggu hasil tersebut:
```dart
final hasil = await Navigator.push<String>(...);

if (hasil != null && hasil.isNotEmpty) {
  ScaffoldMessenger.of(context).showSnackBar(
    SnackBar(content: Text(hasil)),
  );
}
```
Pada pola ini, widget tambahan yang dipakai bukan hanya tombol navigasi, tetapi juga `SnackBar`, `Card`, `ListTile`, dan `CircleAvatar`.

## 4. Tugas Latihan
1. Gabungkan pelajaran dari Bab 3 dan Bab 4.
2. Buat Halaman 1 yang memiliki form input `NIM` dan `Nama`.
3. Buat tombol `Simpan & Beralih`. Ketika tombol ditekan, aplikasi harus berpindah ke Halaman 2 dan menampilkan teks `Selamat Datang [Nama], NIM Anda [NIM]`.
4. Tambahkan satu tombol pada Halaman 2 untuk mengirim pesan balik `Data berhasil disimpan` ke Halaman 1, lalu tampilkan hasilnya menggunakan `SnackBar`.
