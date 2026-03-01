# Bab 3: Layouting & Input Form

## 1. Tujuan Praktikum
- Menguasai teknik dasar mendesain antarmuka (UI) menggunakan widget layout.
- Memahami implementasi `Row`, `Column`, dan `Container`.
- Mampu membuat form input untuk menerima data dari pengguna menggunakan `TextField`.

## 2. Dasar Teori
### Layout Widget
- **Container**: Kotak kosong persegi panjang yang dapat dikustomisasi dengan margin, padding, background color, dan border.
- **Column**: Mengatur susunan children widget secara vertikal (kolom dari atas ke bawah).
- **Row**: Mengatur susunan children widget secara horizontal (baris dari kiri ke kanan).

### Input Widget
- **TextField**: Widget utama yang digunakan untuk mendapatkan input teks dari pengguna. Biasanya dikombinasikan dengan `TextEditingController` untuk mengambil teks yang telah diinput.

## 3. Langkah Praktikum

### 3.1 Berlatih Menggunakan Layout (Row, Column, Container)
1. Buat file `lib/main.dart` Anda. Buat sebuah kerangka yang memanggil layar profile buatan sendiri.
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
       return const MaterialApp(
         home: LayoutDasar(),
       );
     }
   }
   ```
3. Buat file baru terpisah `lib/layout_dasar.dart` dan pindahkan struktur Scaffold di sana:
   ```dart
   // File: lib/layout_dasar.dart
   import 'package:flutter/material.dart';

   class LayoutDasar extends StatelessWidget {
     const LayoutDasar({super.key});

     @override
     Widget build(BuildContext context) {
       return Scaffold(
           appBar: AppBar(title: const Text('Layout Dasar')),
           body: Padding(
             padding: const EdgeInsets.all(16.0),
             child: Column(
               crossAxisAlignment: CrossAxisAlignment.start,
               children: [
                 Container(
                   padding: const EdgeInsets.all(16.0),
                   color: Colors.blueAccent,
                   child: Row(
                     children: const [
                       Icon(Icons.person, size: 50, color: Colors.white),
                       SizedBox(width: 16),
                       Text(
                         'Profil Pengguna',
                         style: TextStyle(fontSize: 24, color: Colors.white),
                       ),
                     ],
                   ),
                 ),
                 const SizedBox(height: 20),
                 const Text('Memanfaatkan Column dan Row untuk menyusun tampilan yang lebih kompleks secara hierarkis.', style: TextStyle(fontSize: 16)),
               ],
             ),
           ),
         ),
       );
     }
   }
   ```
3. Lakukan Hot Reload dan perhatikan letak icon dan teks yang menggunakan `Row` dan `Column`.

### 3.2 Membuat Form Input Sederhana
1. Tambahkan fitur form untuk menerima nama dan email. Fitur form membutuhkan *state* sehingga struktur kelas harus menggunakan `StatefulWidget`. Karena ukurannya besar, kita buat di file baru.
2. Buat/letakkan di `lib/input_form.dart`:
   ```dart
   // File: lib/input_form.dart
   import 'package:flutter/material.dart';

   class InputFormWidget extends StatefulWidget {
     const InputFormWidget({super.key});

     @override
     State<InputFormWidget> createState() => _InputFormWidgetState();
   }

   class _InputFormWidgetState extends State<InputFormWidget> {
     final TextEditingController _nameController = TextEditingController();
     String _hasilInput = '';

     void _submitForm() {
       setState(() {
         _hasilInput = _nameController.text;
       });
     }

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(title: const Text('Input Form')),
         body: Padding(
           padding: const EdgeInsets.all(16.0),
           child: Column(
             crossAxisAlignment: CrossAxisAlignment.start,
             children: [
               TextField(
                 controller: _nameController,
                 decoration: const InputDecoration(
                   labelText: 'Masukkan Nama Anda',
                   border: OutlineInputBorder(),
                 ),
               ),
               const SizedBox(height: 16),
               ElevatedButton(
                 onPressed: _submitForm,
                 child: const Text('Submit'),
               ),
               const SizedBox(height: 20),
               Text(
                 'Teks yang diinput: $_hasilInput',
                 style: const TextStyle(fontSize: 18),
               ),
             ],
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
4. Jalankan aplikasi. Ketikkan nama pada `TextField` dan tekan tombol "Submit".

## 4. Tugas Latihan
1. Kembangkan `InputFormWidget` di atas. Tambahkan `TextField` kedua untuk menginputkan **NIM** atau **Email** (gunakan properti `keyboardType` jika ingin membatasi input hanya angka).
2. Tampilkan Nama dan NIM/Email yang diinput pada dua baris teks yang berbeda setelah tombol Submit ditekan.
