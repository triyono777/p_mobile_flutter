# Bab 3: Layouting & Input Form

## 1. Tujuan Praktikum
- Menguasai teknik dasar mendesain antarmuka (UI) menggunakan widget layout.
- Memahami implementasi `Row`, `Column`, `Container`, `Expanded`, dan `Wrap`.
- Mampu membuat form input untuk menerima data dari pengguna menggunakan `TextField` maupun `TextFormField`.
- Mengenal widget pelengkap form seperti `DropdownButtonFormField`, `SwitchListTile`, `Card`, dan `ListTile`.

## 2. Dasar Teori
### Layout Widget
- **`Container`**: Kotak persegi panjang yang dapat dikustomisasi dengan padding, margin, warna, border, dan radius.
- **`Column`**: Menyusun children widget secara vertikal.
- **`Row`**: Menyusun children widget secara horizontal.
- **`Expanded`**: Membuat widget di dalam `Row` atau `Column` mengambil ruang yang tersedia secara proporsional.
- **`Wrap`**: Menyusun widget ke baris berikutnya secara otomatis ketika ruang horizontal tidak cukup.

### Input Widget
- **`TextField`**: Widget dasar untuk menerima input teks.
- **`TextFormField`**: Versi yang lebih cocok untuk form karena mendukung validasi.
- **`TextEditingController`**: Membaca dan mengelola nilai input.
- **`DropdownButtonFormField`**: Digunakan untuk memilih satu nilai dari beberapa opsi.
- **`SwitchListTile`**: Menggabungkan switch dan teks dalam satu baris agar lebih rapi.

## 3. Langkah Praktikum

### 3.1 Berlatih Menggunakan Layout (Row, Column, Container, Expanded, Wrap)
1. Buka file `lib/main.dart` dan buat kerangka aplikasi yang memanggil layout profil sederhana.
2. Ketikkan kode berikut pada file `lib/main.dart`:
   ```dart
   // File: lib/main.dart
   import 'package:flutter/material.dart';
   import 'layout_dasar.dart';

   void main() => runApp(const MyApp());

   class MyApp extends StatelessWidget {
     const MyApp({super.key});

     @override
     Widget build(BuildContext context) {
       return MaterialApp(
         debugShowCheckedModeBanner: false,
         theme: ThemeData(
           colorSchemeSeed: Colors.blue,
           useMaterial3: true,
         ),
         home: const LayoutDasar(),
       );
     }
   }
   ```
3. Buat file baru `lib/layout_dasar.dart` dan isi dengan contoh layout berikut:
   ```dart
   // File: lib/layout_dasar.dart
   import 'package:flutter/material.dart';

   class LayoutDasar extends StatelessWidget {
     const LayoutDasar({super.key});

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(title: const Text('Layout Dasar')),
         body: SingleChildScrollView(
           padding: const EdgeInsets.all(16),
           child: Column(
             crossAxisAlignment: CrossAxisAlignment.start,
             children: [
               Container(
                 padding: const EdgeInsets.all(16),
                 decoration: BoxDecoration(
                   color: Colors.blueAccent,
                   borderRadius: BorderRadius.circular(16),
                 ),
                 child: Row(
                   children: [
                     const CircleAvatar(
                       radius: 28,
                       backgroundColor: Colors.white,
                       child: Icon(
                         Icons.person,
                         size: 32,
                         color: Colors.blueAccent,
                       ),
                     ),
                     const SizedBox(width: 16),
                     Expanded(
                       child: Column(
                         crossAxisAlignment: CrossAxisAlignment.start,
                         children: const [
                           Text(
                             'Profil Pengguna',
                             style: TextStyle(
                               fontSize: 22,
                               fontWeight: FontWeight.bold,
                               color: Colors.white,
                             ),
                           ),
                           SizedBox(height: 4),
                           Text(
                             'Contoh penggunaan Container, Row, Column, dan Expanded',
                             style: TextStyle(color: Colors.white70),
                           ),
                         ],
                       ),
                     ),
                   ],
                 ),
               ),
               const SizedBox(height: 20),
               Text(
                 'Menu Cepat',
                 style: Theme.of(context).textTheme.titleMedium,
               ),
               const SizedBox(height: 12),
               const Wrap(
                 spacing: 12,
                 runSpacing: 12,
                 children: [
                   _MenuCard(icon: Icons.assignment, label: 'Tugas'),
                   _MenuCard(icon: Icons.schedule, label: 'Jadwal'),
                   _MenuCard(icon: Icons.grade, label: 'Nilai'),
                   _MenuCard(icon: Icons.mail, label: 'Pesan'),
                 ],
               ),
               const SizedBox(height: 20),
               const Row(
                 children: [
                   Expanded(
                     child: _InfoBox(title: '12', subtitle: 'Pertemuan'),
                   ),
                   SizedBox(width: 12),
                   Expanded(
                     child: _InfoBox(title: '4', subtitle: 'Tugas Aktif'),
                   ),
                 ],
               ),
               const SizedBox(height: 20),
               const Card(
                 child: ListTile(
                   leading: Icon(Icons.lightbulb),
                   title: Text('Catatan Layout'),
                   subtitle: Text(
                     'Gunakan Expanded agar widget di dalam Row membagi ruang secara proporsional.',
                   ),
                 ),
               ),
             ],
           ),
         ),
       );
     }
   }

   class _MenuCard extends StatelessWidget {
     final IconData icon;
     final String label;

     const _MenuCard({
       required this.icon,
       required this.label,
     });

     @override
     Widget build(BuildContext context) {
       return SizedBox(
         width: 150,
         child: Card(
           child: Padding(
             padding: const EdgeInsets.all(16),
             child: Column(
               mainAxisSize: MainAxisSize.min,
               children: [
                 Icon(icon, size: 32, color: Colors.blueAccent),
                 const SizedBox(height: 8),
                 Text(label),
               ],
             ),
           ),
         ),
       );
     }
   }

   class _InfoBox extends StatelessWidget {
     final String title;
     final String subtitle;

     const _InfoBox({
       required this.title,
       required this.subtitle,
     });

     @override
     Widget build(BuildContext context) {
       return Container(
         padding: const EdgeInsets.all(16),
         decoration: BoxDecoration(
           color: Colors.blue.shade50,
           borderRadius: BorderRadius.circular(16),
         ),
         child: Column(
           children: [
             Text(
               title,
               style: const TextStyle(
                 fontSize: 24,
                 fontWeight: FontWeight.bold,
               ),
             ),
             const SizedBox(height: 4),
             Text(subtitle),
           ],
         ),
       );
     }
   }
   ```
4. Lakukan Hot Reload lalu perhatikan penggunaan `Row`, `Column`, `Expanded`, `Wrap`, `Card`, `ListTile`, dan `Container` pada satu halaman yang sama.

### 3.2 Membuat Form Input yang Lebih Lengkap
1. Tambahkan fitur form untuk menerima nama, NIM, email, program studi, dan status aktif mahasiswa.
2. Buat file baru `lib/input_form.dart`:
   ```dart
   // File: lib/input_form.dart
   import 'package:flutter/material.dart';

   class InputFormWidget extends StatefulWidget {
     const InputFormWidget({super.key});

     @override
     State<InputFormWidget> createState() => _InputFormWidgetState();
   }

   class _InputFormWidgetState extends State<InputFormWidget> {
     final _formKey = GlobalKey<FormState>();
     final TextEditingController _nameController = TextEditingController();
     final TextEditingController _nimController = TextEditingController();
     final TextEditingController _emailController = TextEditingController();

     String _selectedProdi = 'Informatika';
     bool _isActive = true;
     String _hasilInput = '';

     @override
     void dispose() {
       _nameController.dispose();
       _nimController.dispose();
       _emailController.dispose();
       super.dispose();
     }

     void _submitForm() {
       if (_formKey.currentState!.validate()) {
         setState(() {
           _hasilInput =
               'Nama: ${_nameController.text}\n'
               'NIM: ${_nimController.text}\n'
               'Email: ${_emailController.text}\n'
               'Prodi: $_selectedProdi\n'
               'Status: ${_isActive ? "Aktif" : "Tidak Aktif"}';
         });
       }
     }

     void _resetForm() {
       _formKey.currentState?.reset();

       setState(() {
         _nameController.clear();
         _nimController.clear();
         _emailController.clear();
         _selectedProdi = 'Informatika';
         _isActive = true;
         _hasilInput = '';
       });
     }

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(title: const Text('Input Form')),
         body: Padding(
           padding: const EdgeInsets.all(16),
           child: Form(
             key: _formKey,
             child: ListView(
               children: [
                 TextFormField(
                   controller: _nameController,
                   decoration: const InputDecoration(
                     labelText: 'Nama Lengkap',
                     border: OutlineInputBorder(),
                     prefixIcon: Icon(Icons.person),
                   ),
                   validator: (value) {
                     if (value == null || value.isEmpty) {
                       return 'Nama wajib diisi';
                     }
                     return null;
                   },
                 ),
                 const SizedBox(height: 16),
                 TextFormField(
                   controller: _nimController,
                   keyboardType: TextInputType.number,
                   decoration: const InputDecoration(
                     labelText: 'NIM',
                     border: OutlineInputBorder(),
                     prefixIcon: Icon(Icons.badge),
                   ),
                   validator: (value) {
                     if (value == null || value.isEmpty) {
                       return 'NIM wajib diisi';
                     }
                     return null;
                   },
                 ),
                 const SizedBox(height: 16),
                 TextFormField(
                   controller: _emailController,
                   keyboardType: TextInputType.emailAddress,
                   decoration: const InputDecoration(
                     labelText: 'Email',
                     border: OutlineInputBorder(),
                     prefixIcon: Icon(Icons.email),
                   ),
                 ),
                 const SizedBox(height: 16),
                 DropdownButtonFormField<String>(
                   value: _selectedProdi,
                   decoration: const InputDecoration(
                     labelText: 'Program Studi',
                     border: OutlineInputBorder(),
                   ),
                   items: const [
                     DropdownMenuItem(
                       value: 'Informatika',
                       child: Text('Informatika'),
                     ),
                     DropdownMenuItem(
                       value: 'Sistem Informasi',
                       child: Text('Sistem Informasi'),
                     ),
                     DropdownMenuItem(
                       value: 'Teknik Komputer',
                       child: Text('Teknik Komputer'),
                     ),
                   ],
                   onChanged: (value) {
                     setState(() {
                       _selectedProdi = value!;
                     });
                   },
                 ),
                 const SizedBox(height: 8),
                 SwitchListTile(
                   contentPadding: EdgeInsets.zero,
                   title: const Text('Mahasiswa Aktif'),
                   subtitle: const Text('Status aktif/nonaktif'),
                   value: _isActive,
                   onChanged: (value) {
                     setState(() {
                       _isActive = value;
                     });
                   },
                 ),
                 const SizedBox(height: 16),
                 Row(
                   children: [
                     Expanded(
                       child: ElevatedButton.icon(
                         onPressed: _submitForm,
                         icon: const Icon(Icons.send),
                         label: const Text('Submit'),
                       ),
                     ),
                     const SizedBox(width: 12),
                     Expanded(
                       child: OutlinedButton.icon(
                         onPressed: _resetForm,
                         icon: const Icon(Icons.refresh),
                         label: const Text('Reset'),
                       ),
                     ),
                   ],
                 ),
                 const SizedBox(height: 20),
                 if (_hasilInput.isNotEmpty)
                   Card(
                     child: Padding(
                       padding: const EdgeInsets.all(16),
                       child: Text(
                         _hasilInput,
                         style: const TextStyle(fontSize: 16),
                       ),
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
3. Panggil `InputFormWidget` pada aplikasi dengan mengubah properti `home` di `lib/main.dart`:
   ```dart
   // Di dalam lib/main.dart
   import 'input_form.dart';
   // ...
   // home: const InputFormWidget(),
   ```
4. Jalankan aplikasi. Isi form, tekan tombol `Submit`, dan amati hasil data yang ditampilkan di dalam `Card`.

### 3.3 Widget yang Dicoba pada Bab Ini
- Layout: `Container`, `Row`, `Column`, `Expanded`, `Wrap`, `SizedBox`.
- Presentasi data: `Card`, `ListTile`, `CircleAvatar`, `Icon`.
- Form: `TextFormField`, `DropdownButtonFormField`, `SwitchListTile`, `ElevatedButton`, `OutlinedButton`.

## 4. Tugas Latihan
1. Tambahkan `RadioListTile` untuk memilih jenis kelamin atau kelas.
2. Tambahkan field `Alamat` dengan input multi-baris menggunakan properti `maxLines`.
3. Tampilkan hasil input dalam bentuk kartu profil yang lebih menarik, bukan hanya teks biasa.
