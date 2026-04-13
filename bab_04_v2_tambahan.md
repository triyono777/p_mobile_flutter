# Bab 4 Tambahan: Navigasi Lanjutan di Flutter

## 1. Tujuan Praktikum
- Mengenal beberapa metode navigasi Flutter selain `push()` dan `pop()`.
- Memahami kapan memakai `pushReplacement()`.
- Memahami fungsi `pushAndRemoveUntil()` untuk membersihkan tumpukan halaman.
- Memahami fungsi `popUntil()` untuk kembali ke halaman tertentu.
- Mengenal varian navigasi berbasis nama route seperti `pushNamed()` dan `pushReplacementNamed()`.
- Mengenal `Navigator.replace()` sebagai materi tambahan lanjutan.

## 2. Dasar Teori
Pada bab sebelumnya, navigasi dasar memakai:

- `Navigator.push()` untuk membuka halaman baru.
- `Navigator.pop()` untuk kembali ke halaman sebelumnya.

Namun pada aplikasi nyata, sering ada kebutuhan lain, misalnya:

- halaman login tidak boleh bisa dibuka lagi setelah pengguna berhasil masuk
- seluruh halaman lama perlu dihapus setelah login atau logout
- pengguna harus langsung kembali ke halaman awal tanpa menekan tombol back berkali-kali
- halaman dibuka berdasarkan **nama route** agar pengelolaan navigasi lebih rapi

Karena itu, Flutter menyediakan beberapa metode navigasi tambahan.

## 3. Ringkasan Metode Navigasi Lanjutan

### 3.1 `Navigator.pushReplacement()`
Dipakai untuk **mengganti halaman saat ini** dengan halaman baru.

Artinya:

- halaman baru dibuka
- halaman lama dihapus dari stack
- tombol back tidak kembali ke halaman lama

Contoh:

```dart
Navigator.pushReplacement(
  context,
  MaterialPageRoute(
    builder: (context) => const HalamanDashboard(),
  ),
);
```

### 3.2 `Navigator.pushAndRemoveUntil()`
Dipakai untuk membuka halaman baru lalu **menghapus beberapa halaman lama** sampai syarat tertentu terpenuhi.

Contoh paling umum:

- setelah login berhasil
- setelah logout
- setelah splash screen selesai

Contoh:

```dart
Navigator.pushAndRemoveUntil(
  context,
  MaterialPageRoute(
    builder: (context) => const HalamanBeranda(),
  ),
  (route) => false,
);
```

Jika memakai `(route) => false`, semua route lama akan dihapus.

### 3.3 `Navigator.popUntil()`
Dipakai untuk kembali ke halaman tertentu yang sudah ada di dalam stack.

Contoh:

```dart
Navigator.popUntil(context, ModalRoute.withName('/'));
```

Artinya aplikasi akan terus `pop()` sampai menemukan route dengan nama `'/'`.

### 3.4 `Navigator.pushNamed()`
Dipakai untuk membuka halaman berdasarkan **nama route**.

Contoh:

```dart
Navigator.pushNamed(context, '/profil');
```

### 3.5 `Navigator.pushReplacementNamed()`
Dipakai untuk mengganti halaman saat ini dengan halaman baru berdasarkan nama route.

Contoh:

```dart
Navigator.pushReplacementNamed(context, '/dashboard');
```

### 3.6 `Navigator.pushNamedAndRemoveUntil()`
Dipakai untuk membuka halaman baru berdasarkan nama route lalu menghapus route lama sampai syarat tertentu.

Contoh:

```dart
Navigator.pushNamedAndRemoveUntil(
  context,
  '/login',
  (route) => false,
);
```

### 3.7 `Navigator.canPop()` dan `Navigator.maybePop()`
Keduanya dipakai untuk membantu proses kembali.

- `Navigator.canPop(context)` mengecek apakah halaman saat ini masih bisa kembali.
- `Navigator.maybePop(context)` mencoba kembali jika memang memungkinkan.

Contoh:

```dart
if (Navigator.canPop(context)) {
  Navigator.pop(context);
}
```

## 4. Kapan Menggunakan Metode Tertentu
- Gunakan `push()` jika pengguna masih boleh kembali ke halaman sebelumnya.
- Gunakan `pushReplacement()` jika halaman lama tidak perlu dibuka lagi.
- Gunakan `pushAndRemoveUntil()` jika ingin memulai alur baru dan menghapus riwayat halaman lama.
- Gunakan `popUntil()` jika ingin kembali cepat ke halaman tertentu.
- Gunakan `pushNamed()` dan keluarga `Named` jika aplikasi mulai memiliki banyak halaman.

## 5. Contoh 1: `pushReplacement()` untuk Login ke Dashboard
Contoh ini cocok untuk kasus:

- pengguna berada di halaman login
- setelah login berhasil, halaman login diganti oleh dashboard
- tombol back tidak kembali ke login

### 5.1 Struktur File

```text
lib/
├── main.dart
├── halaman_login.dart
└── halaman_dashboard.dart
```

### 5.2 Kode `lib/main.dart`

```dart
import 'package:flutter/material.dart';
import 'halaman_login.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Push Replacement',
      theme: ThemeData(
        colorSchemeSeed: Colors.indigo,
        useMaterial3: true,
      ),
      home: const HalamanLogin(),
    );
  }
}
```

### 5.3 Kode `lib/halaman_login.dart`

```dart
import 'package:flutter/material.dart';
import 'halaman_dashboard.dart';

class HalamanLogin extends StatelessWidget {
  const HalamanLogin({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Halaman Login'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pushReplacement(
              context,
              MaterialPageRoute(
                builder: (context) => const HalamanDashboard(),
              ),
            );
          },
          child: const Text('Login'),
        ),
      ),
    );
  }
}
```

### 5.4 Kode `lib/halaman_dashboard.dart`

```dart
import 'package:flutter/material.dart';

class HalamanDashboard extends StatelessWidget {
  const HalamanDashboard({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        automaticallyImplyLeading: false,
        title: const Text('Dashboard'),
      ),
      body: const Center(
        child: Text(
          'Selamat datang di Dashboard',
          style: TextStyle(
            fontSize: 22,
            fontWeight: FontWeight.bold,
          ),
        ),
      ),
    );
  }
}
```

### 5.5 Penjelasan
- `Navigator.pushReplacement()` membuka `HalamanDashboard`.
- `HalamanLogin` langsung diganti, bukan hanya ditumpuk.
- Saat pengguna sudah ada di dashboard, tombol back tidak kembali ke login.

## 6. Contoh 2: `pushAndRemoveUntil()` untuk Menghapus Semua Halaman Lama
Metode ini dipakai jika satu halaman baru harus menjadi awal dari alur baru.

Contoh yang umum:

- dari splash ke beranda
- dari login ke dashboard
- dari dashboard ke login saat logout

### 6.1 Struktur File

```text
lib/
├── main.dart
├── halaman_awal.dart
├── halaman_login.dart
└── halaman_beranda.dart
```

### 6.2 Kode `lib/main.dart`

```dart
import 'package:flutter/material.dart';
import 'halaman_awal.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Push And Remove Until',
      theme: ThemeData(
        colorSchemeSeed: Colors.teal,
        useMaterial3: true,
      ),
      home: const HalamanAwal(),
    );
  }
}
```

### 6.3 Kode `lib/halaman_awal.dart`

```dart
import 'package:flutter/material.dart';
import 'halaman_login.dart';

class HalamanAwal extends StatelessWidget {
  const HalamanAwal({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Halaman Awal'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) => const HalamanLogin(),
              ),
            );
          },
          child: const Text('Masuk ke Login'),
        ),
      ),
    );
  }
}
```

### 6.4 Kode `lib/halaman_login.dart`

```dart
import 'package:flutter/material.dart';
import 'halaman_beranda.dart';

class HalamanLogin extends StatelessWidget {
  const HalamanLogin({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Halaman Login'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pushAndRemoveUntil(
              context,
              MaterialPageRoute(
                builder: (context) => const HalamanBeranda(),
              ),
              (route) => false,
            );
          },
          child: const Text('Login Berhasil'),
        ),
      ),
    );
  }
}
```

### 6.5 Kode `lib/halaman_beranda.dart`

```dart
import 'package:flutter/material.dart';

class HalamanBeranda extends StatelessWidget {
  const HalamanBeranda({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        automaticallyImplyLeading: false,
        title: const Text('Halaman Beranda'),
      ),
      body: const Center(
        child: Text(
          'Sekarang Anda berada di Beranda',
          style: TextStyle(
            fontSize: 22,
            fontWeight: FontWeight.bold,
          ),
          textAlign: TextAlign.center,
        ),
      ),
    );
  }
}
```

### 6.6 Penjelasan
- Dari `HalamanAwal`, pengguna masuk ke `HalamanLogin` dengan `push()`.
- Dari `HalamanLogin`, pengguna pindah ke `HalamanBeranda` dengan `pushAndRemoveUntil()`.
- Karena syaratnya `(route) => false`, maka semua halaman lama dihapus.
- Setelah sampai di beranda, pengguna tidak bisa kembali ke halaman awal maupun login dengan tombol back.

## 7. Contoh 3: `popUntil()` untuk Kembali Langsung ke Halaman Awal
Contoh ini menunjukkan bagaimana pengguna bisa kembali langsung ke halaman tertentu tanpa menekan back berkali-kali.

### 7.1 Struktur File

```text
lib/
├── main.dart
├── halaman_beranda.dart
├── halaman_produk.dart
└── halaman_detail_produk.dart
```

### 7.2 Kode `lib/main.dart`

```dart
import 'package:flutter/material.dart';
import 'halaman_beranda.dart';
import 'halaman_produk.dart';
import 'halaman_detail_produk.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Pop Until',
      theme: ThemeData(
        colorSchemeSeed: Colors.deepOrange,
        useMaterial3: true,
      ),
      initialRoute: '/',
      routes: {
        '/': (context) => const HalamanBeranda(),
        '/produk': (context) => const HalamanProduk(),
        '/detail': (context) => const HalamanDetailProduk(),
      },
    );
  }
}
```

### 7.3 Kode `lib/halaman_beranda.dart`

```dart
import 'package:flutter/material.dart';

class HalamanBeranda extends StatelessWidget {
  const HalamanBeranda({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Beranda'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pushNamed(context, '/produk');
          },
          child: const Text('Buka Halaman Produk'),
        ),
      ),
    );
  }
}
```

### 7.4 Kode `lib/halaman_produk.dart`

```dart
import 'package:flutter/material.dart';

class HalamanProduk extends StatelessWidget {
  const HalamanProduk({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Halaman Produk'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pushNamed(context, '/detail');
          },
          child: const Text('Buka Detail Produk'),
        ),
      ),
    );
  }
}
```

### 7.5 Kode `lib/halaman_detail_produk.dart`

```dart
import 'package:flutter/material.dart';

class HalamanDetailProduk extends StatelessWidget {
  const HalamanDetailProduk({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Detail Produk'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.popUntil(
              context,
              ModalRoute.withName('/'),
            );
          },
          child: const Text('Kembali ke Beranda'),
        ),
      ),
    );
  }
}
```

### 7.6 Penjelasan
- Aplikasi mulai dari route `'/'`.
- Dari beranda pengguna pindah ke `'/produk'`.
- Dari produk pengguna pindah ke `'/detail'`.
- Pada halaman detail, `Navigator.popUntil(context, ModalRoute.withName('/'))` akan menghapus halaman demi halaman sampai kembali ke beranda.

Jika ingin kembali ke halaman pertama tanpa nama route, bisa juga memakai:

```dart
Navigator.popUntil(context, (route) => route.isFirst);
```

## 8. Contoh 4: Named Route untuk `pushNamed()`, `pushReplacementNamed()`, dan `pushNamedAndRemoveUntil()`
Contoh ini menunjukkan varian metode navigasi dengan **nama route**.

### 8.1 Struktur File

```text
lib/
├── main.dart
├── halaman_login.dart
├── halaman_dashboard.dart
└── halaman_profil.dart
```

### 8.2 Kode `lib/main.dart`

```dart
import 'package:flutter/material.dart';
import 'halaman_login.dart';
import 'halaman_dashboard.dart';
import 'halaman_profil.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Named Route',
      theme: ThemeData(
        colorSchemeSeed: Colors.purple,
        useMaterial3: true,
      ),
      initialRoute: '/login',
      routes: {
        '/login': (context) => const HalamanLogin(),
        '/dashboard': (context) => const HalamanDashboard(),
        '/profil': (context) => const HalamanProfil(),
      },
    );
  }
}
```

### 8.3 Kode `lib/halaman_login.dart`

```dart
import 'package:flutter/material.dart';

class HalamanLogin extends StatelessWidget {
  const HalamanLogin({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Login'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pushReplacementNamed(
              context,
              '/dashboard',
            );
          },
          child: const Text('Login ke Dashboard'),
        ),
      ),
    );
  }
}
```

### 8.4 Kode `lib/halaman_dashboard.dart`

```dart
import 'package:flutter/material.dart';

class HalamanDashboard extends StatelessWidget {
  const HalamanDashboard({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Dashboard'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/profil');
              },
              child: const Text('Buka Profil'),
            ),
            const SizedBox(height: 12),
            OutlinedButton(
              onPressed: () {
                Navigator.pushNamedAndRemoveUntil(
                  context,
                  '/login',
                  (route) => false,
                );
              },
              child: const Text('Logout'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### 8.5 Kode `lib/halaman_profil.dart`

```dart
import 'package:flutter/material.dart';

class HalamanProfil extends StatelessWidget {
  const HalamanProfil({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Profil'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: const Text('Kembali ke Dashboard'),
        ),
      ),
    );
  }
}
```

### 8.6 Penjelasan
- `pushReplacementNamed()` dipakai saat login agar halaman login diganti dashboard.
- `pushNamed()` dipakai untuk membuka halaman profil dari dashboard.
- `pushNamedAndRemoveUntil()` dipakai saat logout agar route lama dihapus dan pengguna kembali ke login.

## 9. Catatan Tambahan: `Navigator.replace()`
Flutter juga memiliki metode `Navigator.replace()`.
Metode ini dipakai untuk **mengganti route tertentu** dengan route lain jika kita sudah memiliki referensi route lama.

Namun untuk pemula, metode ini **jarang dipakai langsung** karena lebih rumit dibanding `pushReplacement()`.

Contoh bentuk penggunaannya:

```dart
final currentRoute = ModalRoute.of(context);

if (currentRoute != null) {
  Navigator.of(context).replace(
    oldRoute: currentRoute,
    newRoute: MaterialPageRoute(
      builder: (context) => const HalamanBaru(),
    ),
  );
}
```

Catatan:

- `replace()` lebih sering muncul pada pengelolaan route yang lebih kompleks.
- Untuk materi dasar dan menengah, biasanya `pushReplacement()` sudah cukup.

## 10. Perbandingan Singkat
- `push()` menambah halaman baru di atas stack.
- `pushReplacement()` mengganti halaman saat ini.
- `pushAndRemoveUntil()` membuka halaman baru dan menghapus beberapa atau semua halaman lama.
- `popUntil()` kembali ke halaman tertentu yang sudah ada.
- `pushNamed()` membuka halaman dengan nama route.
- `pushReplacementNamed()` mengganti halaman saat ini dengan nama route.
- `pushNamedAndRemoveUntil()` membuka route baru lalu membersihkan route lama.

## 11. Kesalahan yang Sering Terjadi
- Menggunakan `pushReplacement()` padahal pengguna masih perlu kembali ke halaman sebelumnya.
- Menggunakan `pushAndRemoveUntil()` dengan `(route) => false` tanpa sadar bahwa semua halaman lama akan hilang.
- Salah menulis nama route seperti `'/profil'` dan `'/profile'`.
- Lupa mendaftarkan route pada `MaterialApp`.
- Memakai `popUntil()` ke nama route yang tidak ada di stack.

## 12. Tugas Latihan
1. Buat aplikasi dengan halaman `Login`, `Dashboard`, dan `Profil`.
2. Dari `Login`, gunakan `pushReplacement()` atau `pushReplacementNamed()` ke `Dashboard`.
3. Dari `Dashboard`, buka `Profil` dengan `push()` atau `pushNamed()`.
4. Dari `Profil`, tambahkan tombol untuk kembali ke `Dashboard`.
5. Pada `Dashboard`, tambahkan tombol logout dengan `pushAndRemoveUntil()` atau `pushNamedAndRemoveUntil()`.
6. Buat simulasi alur `Beranda -> Produk -> Detail`.
7. Dari `Detail`, gunakan `popUntil()` untuk kembali langsung ke `Beranda`.

## 13. Kesimpulan
Metode navigasi Flutter tidak hanya `push()` dan `pop()`.
Dalam aplikasi nyata, kita sering membutuhkan penggantian halaman, pembersihan stack, dan navigasi berdasarkan nama route.

Jika materi dasar sudah dipahami, maka metode yang paling penting untuk dikuasai berikutnya adalah:

- `pushReplacement()`
- `pushAndRemoveUntil()`
- `popUntil()`
- `pushNamed()` dan turunannya

Dengan memahami metode-metode ini, mahasiswa akan lebih siap membuat alur aplikasi Flutter yang lebih realistis dan rapi.
