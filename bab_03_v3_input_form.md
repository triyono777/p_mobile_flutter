# Bab 3 v3: Input Form Dasar Flutter untuk Pemula

## 1. Tujuan Praktikum
- Memahami cara menerima input dari pengguna.
- Mengenal widget `TextField`, `ElevatedButton`, `Column`, dan `Text`.
- Mampu menampilkan hasil input pengguna ke layar.
- Memahami penggunaan sederhana `StatefulWidget` dan `setState()`.

## 2. Dasar Teori
Form input digunakan ketika aplikasi perlu menerima data dari pengguna, misalnya nama, NIM, email, atau alamat.

Widget penting yang dipakai:
- **`TextField`**: tempat pengguna mengetik data.
- **`TextEditingController`**: membaca isi dari `TextField`.
- **`ElevatedButton`**: tombol untuk menjalankan aksi.
- **`StatefulWidget`**: digunakan ketika tampilan dapat berubah.
- **`setState()`**: memberi tahu Flutter bahwa data berubah dan tampilan harus diperbarui.

## 3. Langkah Praktikum

### 3.1 Menyiapkan `main.dart`
1. Buka file `lib/main.dart`.
2. Ganti isi file tersebut dengan kode berikut:

```dart
import 'package:flutter/material.dart';
import 'input_form_v3_sederhana.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: InputFormV3Sederhana(),
    );
  }
}
```

### 3.2 Membuat File Form Sederhana
1. Buat file baru dengan nama `lib/input_form_v3_sederhana.dart`.
2. Isi file tersebut dengan kode berikut:

```dart
import 'package:flutter/material.dart';

class InputFormV3Sederhana extends StatefulWidget {
  const InputFormV3Sederhana({super.key});

  @override
  State<InputFormV3Sederhana> createState() => _InputFormV3SederhanaState();
}

class _InputFormV3SederhanaState extends State<InputFormV3Sederhana> {
  final TextEditingController _namaController = TextEditingController();
  final TextEditingController _nimController = TextEditingController();

  String _hasilNama = '';
  String _hasilNim = '';

  void _tampilkanData() {
    setState(() {
      _hasilNama = _namaController.text;
      _hasilNim = _nimController.text;
    });
  }

  @override
  void dispose() {
    _namaController.dispose();
    _nimController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Input Form Dasar'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            TextField(
              controller: _namaController,
              decoration: const InputDecoration(
                labelText: 'Nama',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            TextField(
              controller: _nimController,
              decoration: const InputDecoration(
                labelText: 'NIM',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: _tampilkanData,
              child: const Text('Tampilkan Data'),
            ),
            const SizedBox(height: 24),
            const Text(
              'Hasil Input:',
              style: TextStyle(
                fontSize: 18,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 8),
            Text('Nama: $_hasilNama'),
            Text('NIM: $_hasilNim'),
          ],
        ),
      ),
    );
  }
}
```

### 3.3 Penjelasan Alur Program
Urutan kerja program ini:

1. Pengguna mengetik nama pada `TextField` pertama.
2. Pengguna mengetik NIM pada `TextField` kedua.
3. Saat tombol `Tampilkan Data` ditekan, fungsi `_tampilkanData()` dijalankan.
4. Fungsi tersebut mengambil nilai dari `_namaController` dan `_nimController`.
5. Nilai disimpan ke variabel `_hasilNama` dan `_hasilNim`.
6. Karena prosesnya dibungkus dengan `setState()`, tampilan akan diperbarui secara otomatis.

## 4. Hasil yang Diharapkan
Setelah aplikasi dijalankan:
- pengguna dapat mengetik nama
- pengguna dapat mengetik NIM
- saat tombol ditekan, hasil input akan muncul di bawah tombol

## 5. Kesimpulan
Pada contoh ini, Anda belajar bahwa:
- `TextField` digunakan untuk menerima input
- `TextEditingController` membaca input pengguna
- `StatefulWidget` dipakai saat data pada layar dapat berubah
- `setState()` dipakai untuk memperbarui tampilan

## 6. Tugas Latihan
1. Tambahkan satu `TextField` lagi untuk email.
2. Tampilkan email di bawah hasil nama dan NIM.
3. Tambahkan satu tombol `Reset` untuk mengosongkan input.
4. Ganti teks tombol menjadi `Simpan Data`.
