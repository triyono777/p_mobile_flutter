# Bab 2: Konsep Widget (Stateless vs Stateful)

## 1. Tujuan Praktikum
- Memahami konsep dasar widget di Flutter.
- Mengetahui perbedaan mendasar antara `StatelessWidget` dan `StatefulWidget`.
- Mampu mengimplementasikan kedua jenis widget tersebut dalam pembuatan UI.
- Mengenal lebih banyak widget dasar seperti `Text`, `Icon`, `Card`, `Chip`, `ElevatedButton`, `SwitchListTile`, `CheckboxListTile`, dan `Slider`.
- Mampu menyusun halaman profil sederhana menggunakan `CircleAvatar`, `Container`, `Row`, `Column`, `Text`, dan `SizedBox`.

## 2. Dasar Teori
Di dalam Flutter, hampir semua elemen antarmuka pengguna adalah **widget**. Widget mendeskripsikan bagaimana tampilan aplikasi seharusnya dibangun berdasarkan konfigurasi dan *state* (status) saat ini.
- **`StatelessWidget`**: Widget yang *state*-nya tidak berubah setelah dibangun. Cocok untuk tampilan yang bersifat presentasional, misalnya judul, icon, kartu profil, label, atau banner informasi.
- **`StatefulWidget`**: Widget yang *state*-nya bisa berubah selama aplikasi berjalan. Cocok untuk elemen yang interaktif seperti counter, checkbox, switch, slider, dan form.
- Metode `build()` akan dipanggil ketika Flutter perlu menggambar ulang tampilan.
- Pada `StatefulWidget`, setiap perubahan data yang memengaruhi UI harus dibungkus dengan `setState()`.

### 2.1 Contoh Widget yang Sering Dipakai
- `Text`: menampilkan teks.
- `Icon`: menampilkan simbol visual.
- `Image`: menampilkan gambar dari aset lokal atau internet.
- `Card`: membungkus informasi dalam panel.
- `Row` dan `Column`: menyusun widget secara horizontal dan vertikal.
- `ElevatedButton`, `OutlinedButton`, dan `IconButton`: menerima interaksi dari pengguna.

## 3. Langkah Praktikum

### 3.1 Membuat Aplikasi Sederhana dengan StatelessWidget
1. Buka file `lib/main.dart` dari proyek yang sudah dibuat dan buat file baru `lib/stateless_widget_demo.dart`.
2. Hapus seluruh isi `main.dart` dan ganti dengan kode berikut:
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
         debugShowCheckedModeBanner: false,
         title: 'Praktikum Widget',
         theme: ThemeData(
           colorSchemeSeed: Colors.indigo,
           useMaterial3: true,
         ),
         home: const StatelessWidgetDemo(),
       );
     }
   }
   ```
3. Selanjutnya di dalam file `lib/stateless_widget_demo.dart`, buat komponen stateless berikut:
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
         body: SingleChildScrollView(
           padding: const EdgeInsets.all(16),
           child: Column(
             crossAxisAlignment: CrossAxisAlignment.start,
             children: [
               Card(
                 elevation: 3,
                 child: Padding(
                   padding: const EdgeInsets.all(16),
                   child: Row(
                     children: [
                       CircleAvatar(
                         radius: 30,
                         backgroundColor: Colors.indigo.shade100,
                         child: const Icon(
                           Icons.person,
                           size: 32,
                           color: Colors.indigo,
                         ),
                       ),
                       const SizedBox(width: 16),
                       Expanded(
                         child: Column(
                           crossAxisAlignment: CrossAxisAlignment.start,
                           children: [
                             Text(
                               'Alya Putri',
                               style: Theme.of(context).textTheme.titleLarge,
                             ),
                             const SizedBox(height: 4),
                             const Text('Mahasiswa Informatika'),
                             const SizedBox(height: 8),
                             const Wrap(
                               spacing: 8,
                               runSpacing: 8,
                               children: [
                                 Chip(label: Text('Aktif')),
                                 Chip(label: Text('Flutter Dasar')),
                                 Chip(label: Text('Semester 4')),
                               ],
                             ),
                           ],
                         ),
                       ),
                     ],
                   ),
                 ),
               ),
               const SizedBox(height: 20),
               Text(
                 'Contoh widget statis lainnya',
                 style: Theme.of(context).textTheme.titleMedium,
               ),
               const SizedBox(height: 12),
               const Row(
                 mainAxisAlignment: MainAxisAlignment.spaceAround,
                 children: [
                   _InfoIcon(icon: Icons.email, label: 'Email'),
                   _InfoIcon(icon: Icons.phone_android, label: 'Telepon'),
                   _InfoIcon(icon: Icons.school, label: 'Kampus'),
                 ],
               ),
               const SizedBox(height: 20),
               ElevatedButton.icon(
                 onPressed: () {
                   ScaffoldMessenger.of(context).showSnackBar(
                     const SnackBar(
                       content: Text('Tombol ditekan tanpa mengubah state'),
                     ),
                   );
                 },
                 icon: const Icon(Icons.info_outline),
                 label: const Text('Tampilkan Info'),
               ),
             ],
           ),
         ),
       );
     }
   }

   class _InfoIcon extends StatelessWidget {
     final IconData icon;
     final String label;

     const _InfoIcon({
       required this.icon,
       required this.label,
     });

     @override
     Widget build(BuildContext context) {
       return Column(
         children: [
           Icon(icon, size: 32, color: Colors.indigo),
           const SizedBox(height: 8),
           Text(label),
         ],
       );
     }
   }
   ```
4. Jalankan aplikasi dan perhatikan bahwa tampilan statis di atas sudah memanfaatkan beberapa widget sekaligus: `Card`, `CircleAvatar`, `Icon`, `Text`, `Chip`, `Row`, `Column`, dan `ElevatedButton.icon`.

### 3.2 Membuat Halaman Profil Sederhana
1. Sekarang buat contoh yang lebih fokus pada penyusunan tampilan profil sederhana.
2. Buat file baru `lib/profil_sederhana.dart`.
3. Ubah `lib/main.dart` agar sementara memanggil halaman profil:
   ```dart
   // File: lib/main.dart
   import 'package:flutter/material.dart';
   import 'profil_sederhana.dart';

   void main() {
     runApp(const MyApp());
   }

   class MyApp extends StatelessWidget {
     const MyApp({super.key});

     @override
     Widget build(BuildContext context) {
       return MaterialApp(
         debugShowCheckedModeBanner: false,
         title: 'Profil Sederhana',
         theme: ThemeData(
           colorSchemeSeed: Colors.blue,
           useMaterial3: true,
         ),
         home: const ProfilSederhana(),
       );
     }
   }
   ```
4. Isi file `lib/profil_sederhana.dart` dengan kode berikut:
   ```dart
   // File: lib/profil_sederhana.dart
   import 'package:flutter/material.dart';

   class ProfilSederhana extends StatelessWidget {
     const ProfilSederhana({super.key});

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(
           title: const Text('Halaman Profil'),
         ),
         body: Center(
           child: Container(
             width: 320,
             padding: const EdgeInsets.all(20),
             margin: const EdgeInsets.all(16),
             decoration: BoxDecoration(
               color: Colors.white,
               borderRadius: BorderRadius.circular(20),
               boxShadow: [
                 BoxShadow(
                   color: Colors.black.withOpacity(0.08),
                   blurRadius: 12,
                   offset: const Offset(0, 6),
                 ),
               ],
             ),
             child: Column(
               mainAxisSize: MainAxisSize.min,
               children: [
                 const CircleAvatar(
                   radius: 42,
                   backgroundColor: Colors.blueAccent,
                   child: Icon(
                     Icons.person,
                     size: 42,
                     color: Colors.white,
                   ),
                 ),
                 const SizedBox(height: 16),
                 const Text(
                   'Nadia Rahma',
                   style: TextStyle(
                     fontSize: 22,
                     fontWeight: FontWeight.bold,
                   ),
                 ),
                 const SizedBox(height: 6),
                 const Text(
                   'Mahasiswa Flutter Dasar',
                   style: TextStyle(
                     fontSize: 15,
                     color: Colors.black54,
                   ),
                 ),
                 const SizedBox(height: 20),
                 Container(
                   padding: const EdgeInsets.all(12),
                   decoration: BoxDecoration(
                     color: Colors.blue.shade50,
                     borderRadius: BorderRadius.circular(14),
                   ),
                   child: const Row(
                     children: [
                       Icon(Icons.email, color: Colors.blueAccent),
                       SizedBox(width: 12),
                       Expanded(
                         child: Text('nadiarahma@student.ac.id'),
                       ),
                     ],
                   ),
                 ),
                 const SizedBox(height: 12),
                 Container(
                   padding: const EdgeInsets.all(12),
                   decoration: BoxDecoration(
                     color: Colors.blue.shade50,
                     borderRadius: BorderRadius.circular(14),
                   ),
                   child: const Row(
                     children: [
                       Icon(Icons.phone, color: Colors.blueAccent),
                       SizedBox(width: 12),
                       Expanded(
                         child: Text('0812-3456-7890'),
                       ),
                     ],
                   ),
                 ),
                 const SizedBox(height: 12),
                 Container(
                   padding: const EdgeInsets.all(12),
                   decoration: BoxDecoration(
                     color: Colors.blue.shade50,
                     borderRadius: BorderRadius.circular(14),
                   ),
                   child: const Row(
                     children: [
                       Icon(Icons.location_on, color: Colors.blueAccent),
                       SizedBox(width: 12),
                       Expanded(
                         child: Text('Surakarta, Jawa Tengah'),
                       ),
                     ],
                   ),
                 ),
               ],
             ),
           ),
         ),
       );
     }
   }
   ```
5. Jalankan aplikasi dan amati fungsi widget yang dipakai:
   - `CircleAvatar` menampilkan avatar profil.
   - `Column` menyusun elemen profil secara vertikal.
   - `Row` menyusun icon dan informasi kontak secara horizontal.
   - `Text` menampilkan nama, deskripsi, dan data profil.
   - `Container` membungkus kartu profil dan tiap baris informasi.
   - `SizedBox` memberi jarak antar elemen agar tampilan lebih rapi.

### 3.3 Membuat Widget Interaktif dengan StatefulWidget
1. Sekarang kita ubah aplikasi menjadi interaktif dengan memanfaatkan `StatefulWidget`.
2. Buat file baru terpisah bernama `lib/stateful_widget_demo.dart`.
3. Ubah `lib/main.dart` agar memanggil layar stateful:
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
         debugShowCheckedModeBanner: false,
         title: 'Praktikum Widget',
         theme: ThemeData(
           colorSchemeSeed: Colors.indigo,
           useMaterial3: true,
         ),
         home: const StatefulWidgetDemo(),
       );
     }
   }
   ```
4. Buat kode layar interaktif berikut di `lib/stateful_widget_demo.dart`:
   ```dart
   // File: lib/stateful_widget_demo.dart
   import 'package:flutter/material.dart';

   class StatefulWidgetDemo extends StatefulWidget {
     const StatefulWidgetDemo({super.key});

     @override
     State<StatefulWidgetDemo> createState() => _StatefulWidgetDemoState();
   }

   class _StatefulWidgetDemoState extends State<StatefulWidgetDemo> {
     int _counter = 0;
     bool _isFavorite = false;
     bool _isReminderActive = true;
     double _fontSize = 18;

     void _incrementCounter() {
       setState(() {
         _counter++;
       });
     }

     void _decrementCounter() {
       setState(() {
         if (_counter > 0) {
           _counter--;
         }
       });
     }

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(
           title: const Text('Stateful Widget'),
         ),
         body: ListView(
           padding: const EdgeInsets.all(16),
           children: [
             Card(
               child: Padding(
                 padding: const EdgeInsets.all(16),
                 child: Column(
                   crossAxisAlignment: CrossAxisAlignment.start,
                   children: [
                     Row(
                       children: [
                         Icon(
                           _isFavorite
                               ? Icons.favorite
                               : Icons.favorite_border,
                           color: _isFavorite ? Colors.red : Colors.grey,
                           size: 36,
                         ),
                         const SizedBox(width: 12),
                         Expanded(
                           child: Text(
                             'Jumlah klik: $_counter',
                             style: TextStyle(
                               fontSize: _fontSize,
                               fontWeight: FontWeight.bold,
                             ),
                           ),
                         ),
                       ],
                     ),
                     const SizedBox(height: 16),
                     Row(
                       children: [
                         Expanded(
                           child: ElevatedButton.icon(
                             onPressed: _incrementCounter,
                             icon: const Icon(Icons.add),
                             label: const Text('Tambah'),
                           ),
                         ),
                         const SizedBox(width: 12),
                         Expanded(
                           child: OutlinedButton.icon(
                             onPressed: _decrementCounter,
                             icon: const Icon(Icons.remove),
                             label: const Text('Kurang'),
                           ),
                         ),
                       ],
                     ),
                   ],
                 ),
               ),
             ),
             const SizedBox(height: 16),
             SwitchListTile(
               title: const Text('Aktifkan pengingat belajar'),
               subtitle: const Text('Contoh perubahan nilai boolean'),
               value: _isReminderActive,
               onChanged: (value) {
                 setState(() {
                   _isReminderActive = value;
                 });
               },
             ),
             CheckboxListTile(
               title: const Text('Tandai materi sebagai favorit'),
               value: _isFavorite,
               onChanged: (value) {
                 setState(() {
                   _isFavorite = value ?? false;
                 });
               },
             ),
             const SizedBox(height: 8),
             Text(
               'Ukuran teks preview: ${_fontSize.toStringAsFixed(0)}',
             ),
             Slider(
               value: _fontSize,
               min: 14,
               max: 32,
               divisions: 9,
               label: _fontSize.toStringAsFixed(0),
               onChanged: (value) {
                 setState(() {
                   _fontSize = value;
                 });
               },
             ),
             Container(
               padding: const EdgeInsets.all(16),
               decoration: BoxDecoration(
                 color: Colors.indigo.shade50,
                 borderRadius: BorderRadius.circular(16),
               ),
               child: Text(
                 'Favorit: ${_isFavorite ? "Ya" : "Tidak"}\n'
                 'Pengingat: ${_isReminderActive ? "Aktif" : "Nonaktif"}\n'
                 'Counter saat ini: $_counter',
               ),
             ),
           ],
         ),
       );
     }
   }
   ```
5. Jalankan aplikasi, lalu coba tekan tombol, geser `Slider`, ubah `Switch`, dan centang `Checkbox`.
6. Perhatikan bahwa beberapa widget berubah secara langsung karena nilainya diperbarui melalui `setState()`.

### 3.4 Ringkasan Widget yang Dicoba
Pada bab ini, mahasiswa sudah mencoba beberapa widget penting:
- Widget statis: `Text`, `Icon`, `Chip`, `Card`, `CircleAvatar`.
- Widget interaktif: `ElevatedButton`, `OutlinedButton`, `SwitchListTile`, `CheckboxListTile`, `Slider`.
- Widget penyusun tampilan: `Row`, `Column`, `Wrap`, `ListView`, `Container`, `SizedBox`.

## 4. Tugas Latihan
1. Tambahkan satu tombol `Reset` untuk mengembalikan nilai counter ke `0`.
2. Tambahkan widget `RadioListTile` untuk memilih tema warna favorit pengguna.
3. Modifikasi halaman `ProfilSederhana` agar menampilkan foto profil menggunakan `Image.network` atau `Image.asset`.

