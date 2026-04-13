# Bab 4: Navigasi Antar Halaman (Versi 2)

## 1. Tujuan Praktikum
- Memahami konsep navigasi antar halaman pada Flutter.
- Mengenal `Navigator.push()` untuk membuka halaman baru.
- Mengenal `Navigator.pop()` untuk kembali ke halaman sebelumnya.
- Mampu mengirim data dari halaman pertama ke halaman kedua.
- Mampu menerima data balik dari halaman kedua ke halaman pertama.

## 2. Dasar Teori
Pada Flutter, perpindahan dari satu halaman ke halaman lain disebut **navigasi**.
Setiap halaman biasanya dibuat dalam widget terpisah, dan pada praktik yang rapi setiap halaman diletakkan pada **file yang berbeda**.

Pada bab ini, `main.dart` tetap dipakai sebagai titik awal aplikasi, tetapi isi halaman tidak ditaruh semua di satu file.

Flutter menggunakan konsep **stack** atau tumpukan:

- `Navigator.push()` menambahkan halaman baru ke atas tumpukan.
- `Navigator.pop()` menutup halaman yang sedang aktif lalu kembali ke halaman sebelumnya.

Contoh paling dasar:

```dart
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => const HalamanDua(),
  ),
);
```

Untuk kembali:

```dart
Navigator.pop(context);
```

## 3. Pola File yang Dipakai
Supaya lebih rapi, pola file pada bab ini seperti berikut:

```text
lib/
├── main.dart
├── halaman_satu.dart
└── halaman_dua.dart
```

`main.dart` bertugas menjalankan aplikasi.
Halaman-halaman lain dibuat pada file terpisah lalu di-`import` sesuai kebutuhan.

## 4. Contoh 1: Navigasi Dasar dengan File Terpisah
Contoh ini hanya fokus pada dua hal:

- pindah ke halaman kedua
- kembali ke halaman pertama

### 4.1 Struktur File

```text
lib/
├── main.dart
├── halaman_satu.dart
└── halaman_dua.dart
```

### 4.2 Kode `lib/main.dart`

```dart
import 'package:flutter/material.dart';
import 'halaman_satu.dart';

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
        colorSchemeSeed: Colors.blue,
        useMaterial3: true,
      ),
      home: const HalamanSatu(),
    );
  }
}
```

### 4.3 Kode `lib/halaman_satu.dart`

```dart
import 'package:flutter/material.dart';
import 'halaman_dua.dart';

class HalamanSatu extends StatelessWidget {
  const HalamanSatu({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Halaman Satu'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) => const HalamanDua(),
              ),
            );
          },
          child: const Text('Pindah ke Halaman Dua'),
        ),
      ),
    );
  }
}
```

### 4.4 Kode `lib/halaman_dua.dart`

```dart
import 'package:flutter/material.dart';

class HalamanDua extends StatelessWidget {
  const HalamanDua({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Halaman Dua'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: const Text('Kembali ke Halaman Satu'),
        ),
      ),
    );
  }
}
```

### 4.5 Penjelasan
- `main.dart` hanya memanggil `HalamanSatu`.
- File `halaman_satu.dart` berisi halaman pertama.
- File `halaman_dua.dart` berisi halaman kedua.
- Saat tombol ditekan, `Navigator.push()` membuka `HalamanDua`.
- Saat tombol pada halaman kedua ditekan, `Navigator.pop(context)` dipakai untuk kembali.

## 5. Contoh 2: Mengirim Data ke Halaman Kedua
Pada contoh ini, pengguna memasukkan nama di halaman pertama.
Nama tersebut lalu dikirim ke halaman kedua.

### 5.1 Struktur File

```text
lib/
├── main.dart
├── halaman_input_nama.dart
└── halaman_hasil_nama.dart
```

### 5.2 Kode `lib/main.dart`

```dart
import 'package:flutter/material.dart';
import 'halaman_input_nama.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Kirim Data',
      theme: ThemeData(
        colorSchemeSeed: Colors.green,
        useMaterial3: true,
      ),
      home: const HalamanInputNama(),
    );
  }
}
```

### 5.3 Kode `lib/halaman_input_nama.dart`

```dart
import 'package:flutter/material.dart';
import 'halaman_hasil_nama.dart';

class HalamanInputNama extends StatefulWidget {
  const HalamanInputNama({super.key});

  @override
  State<HalamanInputNama> createState() => _HalamanInputNamaState();
}

class _HalamanInputNamaState extends State<HalamanInputNama> {
  final TextEditingController _namaController = TextEditingController();

  @override
  void dispose() {
    _namaController.dispose();
    super.dispose();
  }

  void _kirimData() {
    final nama = _namaController.text.trim();

    if (nama.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Nama tidak boleh kosong'),
        ),
      );
      return;
    }

    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => HalamanHasilNama(nama: nama),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Input Nama'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            const Text(
              'Masukkan nama Anda:',
              style: TextStyle(fontSize: 16),
            ),
            const SizedBox(height: 12),
            TextField(
              controller: _namaController,
              decoration: const InputDecoration(
                labelText: 'Nama',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: _kirimData,
              child: const Text('Kirim ke Halaman Hasil'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### 5.4 Kode `lib/halaman_hasil_nama.dart`

```dart
import 'package:flutter/material.dart';

class HalamanHasilNama extends StatelessWidget {
  final String nama;

  const HalamanHasilNama({
    super.key,
    required this.nama,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Halaman Hasil'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Halo, $nama',
              style: const TextStyle(
                fontSize: 24,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 16),
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context);
              },
              child: const Text('Kembali'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### 5.5 Penjelasan
- Data diambil dari `TextField` pada `halaman_input_nama.dart`.
- Data dikirim lewat konstruktor:

```dart
HalamanHasilNama(nama: nama)
```

- Pada file `halaman_hasil_nama.dart`, data diterima melalui:

```dart
final String nama;
```

Dengan cara ini, data dari halaman pertama bisa ditampilkan pada halaman kedua.

## 6. Contoh 3: Mengirim Data Balik ke Halaman Pertama
Contoh ini menunjukkan cara menerima hasil dari halaman kedua.

### 6.1 Struktur File

```text
lib/
├── main.dart
├── halaman_utama.dart
└── halaman_pilihan.dart
```

### 6.2 Kode `lib/main.dart`

```dart
import 'package:flutter/material.dart';
import 'halaman_utama.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Data Balik',
      theme: ThemeData(
        colorSchemeSeed: Colors.orange,
        useMaterial3: true,
      ),
      home: const HalamanUtama(),
    );
  }
}
```

### 6.3 Kode `lib/halaman_utama.dart`

```dart
import 'package:flutter/material.dart';
import 'halaman_pilihan.dart';

class HalamanUtama extends StatefulWidget {
  const HalamanUtama({super.key});

  @override
  State<HalamanUtama> createState() => _HalamanUtamaState();
}

class _HalamanUtamaState extends State<HalamanUtama> {
  String _hasil = 'Belum ada pilihan';

  Future<void> _bukaHalamanPilihan() async {
    final hasil = await Navigator.push<String>(
      context,
      MaterialPageRoute(
        builder: (context) => const HalamanPilihan(),
      ),
    );

    if (!mounted) return;

    if (hasil != null) {
      setState(() {
        _hasil = hasil;
      });

      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text(hasil),
        ),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Halaman Utama'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            Text(
              _hasil,
              textAlign: TextAlign.center,
              style: const TextStyle(
                fontSize: 20,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: _bukaHalamanPilihan,
              child: const Text('Buka Halaman Pilihan'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### 6.4 Kode `lib/halaman_pilihan.dart`

```dart
import 'package:flutter/material.dart';

class HalamanPilihan extends StatelessWidget {
  const HalamanPilihan({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Halaman Pilihan'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context, 'Anda memilih Profil');
              },
              child: const Text('Pilih Profil'),
            ),
            const SizedBox(height: 12),
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context, 'Anda memilih Jadwal');
              },
              child: const Text('Pilih Jadwal'),
            ),
            const SizedBox(height: 12),
            OutlinedButton(
              onPressed: () {
                Navigator.pop(context);
              },
              child: const Text('Kembali tanpa memilih'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### 6.5 Penjelasan
Pada file `halaman_utama.dart`, baris berikut dipakai untuk membuka halaman kedua sambil menunggu hasil:

```dart
final hasil = await Navigator.push<String>(...);
```

Pada file `halaman_pilihan.dart`, hasil dikirim balik dengan:

```dart
Navigator.pop(context, 'Anda memilih Profil');
```

Artinya:

- `push()` membuka halaman baru
- `await` menunggu halaman itu ditutup
- `pop(context, data)` mengirim data balik ke halaman sebelumnya

## 7. Ringkasan Materi
- `main.dart` sebaiknya dipakai sebagai entry point aplikasi.
- Setiap halaman dapat dibuat pada file yang berbeda agar kode lebih rapi.
- `Navigator.push()` dipakai untuk membuka halaman baru.
- `Navigator.pop()` dipakai untuk kembali ke halaman sebelumnya.
- Data ke halaman lain bisa dikirim lewat konstruktor.
- Data balik bisa diterima dengan `await Navigator.push(...)`.

## 8. Kesalahan yang Sering Terjadi
- Lupa menambahkan `import` pada file yang membutuhkan halaman lain.
- Salah menulis nama file atau nama class halaman tujuan.
- Lupa menuliskan `context` pada `Navigator.push()` atau `Navigator.pop()`.
- Lupa memakai `await` saat ingin menerima data balik.
- Input kosong tetap dipakai untuk berpindah halaman.

## 9. Tugas Latihan
1. Buat file `main.dart`, `halaman_beranda.dart`, dan `halaman_detail.dart`.
2. Pada `halaman_beranda.dart`, tampilkan 3 tombol: `Profil`, `Jadwal`, dan `Nilai`.
3. Saat salah satu tombol ditekan, kirim judul tombol ke `halaman_detail.dart`.
4. Pada `halaman_detail.dart`, tampilkan teks sesuai tombol yang dipilih.
5. Tambahkan tombol kembali.
6. Kembangkan lagi dengan menambahkan input `Nama` pada halaman beranda.
7. Tampilkan `Halo, [Nama]` pada halaman detail.
8. Tambahkan tombol untuk mengirim pesan balik `Data diterima` ke halaman beranda.

## 10. Kesimpulan
Navigasi Flutter akan lebih mudah dipahami jika setiap halaman dibuat terpisah.
Dengan pola ini, mahasiswa tidak hanya belajar `push()` dan `pop()`, tetapi juga belajar menyusun project Flutter dengan lebih rapi sejak awal.
