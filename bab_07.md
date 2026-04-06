# Bab 7: State Management Tim (Provider/GetX)

## 1. Tujuan Praktikum
- Memahami alasan dan kebutuhan arsitektur manajemen state dalam proyek berskala yang lebih besar.
- Mengubah aplikasi yang awalnya diregulasi secara lokal (`setState`) menjadi menggunakan state khusus yang terpisah dari UI komponen.
- Menyiapkan format kolaborasi dengan anggota tim untuk pemisahan *logic* kode.

## 2. Dasar Teori
Seiring semakin berkembangnya kompleksitas aplikasi, menggunakan `setState` dapat mengakibatkan kode sulit dibaca, memori yang boros, dan "Callback Hell" karena *state* di-pasok dari komponen atas secara manual hingga berantai-rantai ke dalam komponen bawah (prop drilling).
State Management berfungsi untuk sentralisasi state sehingga terpisah jelas mana *logic* (business rules) mana *view* (kode UI desain).
Sistem ini mempermudah sekelompok programmer (sebagai tim Frontend/Tim UI vs Tim Logic/Tim Fitur) untuk mengerjakan project sama.

Dalam praktikum ini, kita merekomendasikan **Provider** (solusi standar rekomendasi awal) atau library populer alternatif lainnya (jika kelas memutuskan menggunakan GetX).

## 3. Langkah Praktikum (Menggunakan Provider)

### 3.1 Instalasi Provider
1. Buka terminal atau command prompt pada direktori proyek.
2. Tambahkan dependensi `provider`:
   ```bash
   flutter pub add provider
   ```

### 3.2 Menggunakan Provider pada Logic Keranjang Belanja Sederhana
1. Buat folder baru di dalam folder `lib/` bernama `models/` dan `screens/` untuk mulai menyusun proyek terstruktur.
2. Buat file `lib/models/cart_model.dart`
   ```dart
   // File: lib/models/cart_model.dart
   import 'package:flutter/material.dart';

   // Model kita mewarisi sifat dari ChangeNotifier
   class CartModel extends ChangeNotifier {
     final List<String> _items = [];

     // Getter untuk mengambil data item (dibuat unmodifiable demi keamanan manipulasi)
     List<String> get items => _items;

     // Metode bisnis: menambahkan item belanja
     void addItem(String item) {
       _items.add(item);
       // Memberitahu para widget penyimak (Listener) bahwa state berubah sehingga ia butuh direbuild.
       notifyListeners();
     }

     // Metode bisnis: menghapus item
     void removeAll() {
       _items.clear();
       notifyListeners();
     }
   }
   ```
3. Ubah `lib/main.dart` untuk membungkus program dengan mekanisme Provider.
   ```dart
   // File: lib/main.dart
   import 'package:flutter/material.dart';
   import 'package:provider/provider.dart';
   import 'models/cart_model.dart';
   import 'screens/catalog_screen.dart';

   void main() {
     runApp(
       ChangeNotifierProvider(
         create: (context) => CartModel(),
         child: const MyApp(),
       ),
     );
   }

   class MyApp extends StatelessWidget {
     const MyApp({super.key});

     @override
     Widget build(BuildContext context) {
       return MaterialApp(
         title: 'Provider Demo',
         home: const CatalogScreen(),
       );
     }
   }
   ```
4. Buat file `lib/screens/catalog_screen.dart` untuk menunjukkan antarmuka widget yang memanggil logic.
   ```dart
   // File: lib/screens/catalog_screen.dart
   import 'package:flutter/material.dart';
   import 'package:provider/provider.dart';
   import '../models/cart_model.dart';

   class CatalogScreen extends StatelessWidget {
     const CatalogScreen({super.key});

     @override
     Widget build(BuildContext context) {
       // Kita hanya mendengarkan ke sebuah Model untuk menggambar ulang sebagian layar.
       return Scaffold(
         appBar: AppBar(
           title: const Text('Katalog Toko'),
           actions: [
             IconButton(
               icon: const Icon(Icons.shopping_cart),
               onPressed: () {},
             ),
             // Komponen yang merender state
             Consumer<CartModel>(
               builder: (context, cart, child) {
                 return Center(
                   child: Text(
                     '${cart.items.length}',
                     style: const TextStyle(fontWeight: FontWeight.bold, fontSize: 18),
                   ),
                 );
               },
             ),
             const SizedBox(width: 20),
           ],
         ),
         body: Center(
           child: Column(
             mainAxisAlignment: MainAxisAlignment.center,
             children: [
               ElevatedButton(
                 onPressed: () {
                   // Akses provider untuk menulis/merubah data tanpa rebuilding keseluruhan (listen: false)
                   Provider.of<CartModel>(context, listen: false).addItem('Laptop Baru');
                 },
                 child: const Text('Tambah "Laptop Baru" ke Keranjang'),
               ),
               const SizedBox(height: 20),
               ElevatedButton(
                 onPressed: () {
                   Provider.of<CartModel>(context, listen: false).removeAll();
                 },
                 child: const Text('Kosongkan Keranjang'),
               ),
             ],
           ),
         ),
       );
     }
   }
   ```

5. Lakukan Hot Restart (Restart keseluruhan) dan coba berinteraksi dengan tombol untuk melihat update yang terjadi di ikon AppBar tanpa memicu re-render file utama program.

## 4. Tugas Latihan (Proyek Tim Pembagian Moduler)
*Kerjakan secara Berkelompok*
1. Setup kerangka utama ini (satu project bersama di Github/Gitlab).
2. Tambahkan halaman `CartDetailScreen` yang di-navigasikan dari AppBar Icon. Layar ini harus menampilkan apa saja list product (melalui list array provider `items` dari `CartModel`) yang telah ada dalam keranjang menggunakan Widget ListView.
