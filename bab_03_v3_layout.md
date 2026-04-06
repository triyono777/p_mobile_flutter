# Bab 3 v3: Layout Dasar Flutter untuk Pemula

## 1. Tujuan Praktikum
- Memahami fungsi dasar widget layout di Flutter.
- Mengenal penggunaan `Scaffold`, `AppBar`, `Container`, `Row`, `Column`, `Text`, dan `SizedBox`.
- Mampu menyusun tampilan sederhana yang rapi dan mudah dibaca.

## 2. Dasar Teori
Dalam Flutter, tampilan aplikasi dibangun dari susunan widget. Agar tampilan rapi, kita perlu memahami widget layout dasar berikut:

- **`Scaffold`**: kerangka utama halaman Flutter.
- **`AppBar`**: bagian atas halaman yang biasanya berisi judul.
- **`Container`**: kotak yang bisa diberi warna, padding, margin, dan dekorasi.
- **`Column`**: menyusun widget dari atas ke bawah.
- **`Row`**: menyusun widget dari kiri ke kanan.
- **`Text`**: menampilkan tulisan.
- **`SizedBox`**: memberi jarak antar widget.

Jika `Row` diibaratkan baris, maka `Column` diibaratkan kolom. Kedua widget ini adalah dasar hampir semua layout di Flutter.

## 3. Langkah Praktikum

### 3.1 Menyiapkan `main.dart`
1. Buka file `lib/main.dart`.
2. Ganti isi file tersebut dengan kode berikut:

```dart
import 'package:flutter/material.dart';
import 'layout_v3_sederhana.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: LayoutV3Sederhana(),
    );
  }
}
```

### 3.2 Membuat File Layout Sederhana
1. Buat file baru dengan nama `lib/layout_v3_sederhana.dart`.
2. Isi file tersebut dengan kode berikut:

```dart
import 'package:flutter/material.dart';

class LayoutV3Sederhana extends StatelessWidget {
  const LayoutV3Sederhana({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Layout Dasar'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Container(
              padding: const EdgeInsets.all(16),
              color: Colors.blue.shade100,
              child: const Row(
                children: [
                  Icon(Icons.person, size: 40),
                  SizedBox(width: 12),
                  Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Budi Santoso',
                        style: TextStyle(
                          fontSize: 20,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                      Text('Mahasiswa Informatika'),
                    ],
                  ),
                ],
              ),
            ),
            const SizedBox(height: 20),
            const Text(
              'Informasi Singkat',
              style: TextStyle(
                fontSize: 18,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 10),
            Container(
              padding: const EdgeInsets.all(12),
              color: Colors.grey.shade200,
              child: const Text(
                'Contoh ini menunjukkan cara menggunakan Row, Column, Container, Text, dan SizedBox dalam satu halaman sederhana.',
              ),
            ),
            const SizedBox(height: 20),
            const Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text('Kelas: TI A'),
                Text('Semester: 4'),
              ],
            ),
          ],
        ),
      ),
    );
  }
}
```

### 3.3 Penjelasan Struktur
Perhatikan susunan widget berikut:

1. `Scaffold` menjadi kerangka utama halaman.
2. `AppBar` menampilkan judul halaman.
3. `Padding` memberi ruang kosong di pinggir isi halaman.
4. `Column` menyusun semua isi dari atas ke bawah.
5. `Container` pertama dipakai sebagai kartu profil sederhana.
6. Di dalam `Container`, ada `Row` yang berisi `Icon`, `SizedBox`, dan `Column`.
7. `Column` kecil di dalam `Row` berisi dua `Text`: nama dan keterangan.
8. `SizedBox` memberi jarak antar elemen agar tidak terlalu rapat.

## 4. Hasil yang Diharapkan
Setelah dijalankan, aplikasi akan menampilkan:
- judul halaman `Layout Dasar`
- satu kartu profil sederhana
- teks penjelasan singkat
- satu baris informasi kelas dan semester

## 5. Kesimpulan
Pada contoh ini, Anda belajar bahwa:
- `Column` cocok untuk susunan vertikal
- `Row` cocok untuk susunan horizontal
- `Container` berguna untuk membungkus area tertentu
- `SizedBox` membantu memberi jarak
- layout Flutter dibangun dari susunan widget kecil yang digabungkan

## 6. Tugas Latihan
1. Ganti nama `Budi Santoso` dengan nama Anda sendiri.
2. Ganti teks `Mahasiswa Informatika` dengan program studi Anda.
3. Tambahkan satu `Text` baru di bawah bagian informasi singkat.
4. Ubah warna `Container` pertama menjadi warna lain.
