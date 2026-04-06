# Bab 6: Penyimpanan Data Lokal (Shared Preferences)

## 1. Tujuan Praktikum
- Memahami konsep dasar penyimpanan data lokal berupa struktur *key-value*.
- Menginstalasi dan menggunakan package/library eksternal `shared_preferences`.
- Menyimpan preferensi pengguna dan mempertahankannya meski aplikasi telah ditutup.

## 2. Dasar Teori
Aplikasi sering perlu untuk menyimpan sejumlah kecil data lokal. Data ini meliputi hal-hal seperti pengaturan tema, sesi login ("Apakah pengguna sudah login?"), atau preferensi kecil lainnya agar aplikasi tidak perlu memulai dari konfigurasi awal setiap saat.
Library resmi di ekosistem Flutter yang mudah digunakan untuk ini adalah `shared_preferences`.
- Memanfaatkan UserDefaults pada iOS dan SharedPreferences pada Android.
- Data disimpan dalam bentuk pasangan `String` (Key) dan tipe dasar (Value: `int`, `double`, `bool`, `String`, atau `List<String>`).

## 3. Langkah Praktikum

### 3.1 Menginstal Dependensi
1. Buka terminal atau command prompt pada direktori proyek.
2. Tambahkan package *shared_preferences* ke dalam proyek dengan menjalankan:
   ```bash
   flutter pub add shared_preferences
   ```
3. Atau buka file `pubspec.yaml` dan tambahkan di bawah label `dependencies:`
   ```yaml
   dependencies:
     flutter:
       sdk: flutter
     shared_preferences: ^2.2.2  # versi bisa menyesuaikan
   ```
   Lalu klik ikon "Get dependencies" atau jalankan perintah `flutter pub get`.

### 3.2 Menggunakan Shared Preferences
1. Buat atau perbarui `lib/main.dart` agar memanggil layar pengaturan tema:
   ```dart
   // File: lib/main.dart
   import 'package:flutter/material.dart';
   import 'pengaturan_tema.dart';

   void main() {
     runApp(const MyApp());
   }

   class MyApp extends StatelessWidget {
     const MyApp({super.key});

     @override
     Widget build(BuildContext context) {
       return const MaterialApp(
         title: 'Shared Preferences',
         home: PengaturanTema(),
       );
     }
   }
   ```
2. Buat file baru `lib/pengaturan_tema.dart` untuk memisahkan UI dan Logic penyimpanan data:
   ```dart
   // File: lib/pengaturan_tema.dart
   import 'package:flutter/material.dart';
   import 'package:shared_preferences/shared_preferences.dart';

   class PengaturanTema extends StatefulWidget {
     const PengaturanTema({super.key});

     @override
     State<PengaturanTema> createState() => _PengaturanTemaState();
   }

   class _PengaturanTemaState extends State<PengaturanTema> {
     bool _isDarkMode = false; // default awal
     late SharedPreferences _prefs;

     @override
     void initState() {
       super.initState();
       _loadThemePreference();
     }

     // Method untuk mengambil data (membaca key "tema_gelap")
     Future<void> _loadThemePreference() async {
       _prefs = await SharedPreferences.getInstance();
       setState(() {
         // Jika nilai tidak ditemukan (null), set otomatis as default (false)
         _isDarkMode = _prefs.getBool('tema_gelap') ?? false;
       });
     }

     // Method untuk menyimpan data (menyimpan dengan key "tema_gelap")
     Future<void> _saveThemePreference(bool value) async {
       await _prefs.setBool('tema_gelap', value);
       setState(() {
         _isDarkMode = value;
       });
     }

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(
           title: const Text('Pengaturan Preferensi'),
           backgroundColor: _isDarkMode ? Colors.black87 : Colors.blue,
           foregroundColor: Colors.white,
         ),
         body: Container(
           color: _isDarkMode ? Colors.grey[900] : Colors.white,
           child: Center(
             child: Row(
               mainAxisAlignment: MainAxisAlignment.center,
               children: [
                 Text(
                   'Mode Gelap: ',
                   style: TextStyle(
                     fontSize: 20,
                     color: _isDarkMode ? Colors.white : Colors.black,
                   ),
                 ),
                 Switch(
                   value: _isDarkMode,
                   onChanged: (bool newValue) {
                     _saveThemePreference(newValue);
                   },
                 ),
               ],
             ),
           ),
         ),
       );
     }
   }
   ```
2. Jalankan aplikasi menggunakan `flutter run`.
3. Coba mainkan *switch* atau sakelar yang ada di layar.
4. Penting: Matikan seluruh aplikasi hingga log terputus, lalu jalankan kembali aplikasinya. State terakhir sakelar akan terus disimpan karena diselamatkan ke disk perangkat!

## 4. Tugas Latihan
1. Integrasikan materi pembuatan input form pada Bab 3 dengan materi ini.
2. Buat program untuk menyimpan **Nama Pengguna** ke dalam shared preferences. Ketika login lagi (buka program), text 'Halo [Nama_yang_Disimpan]' sudah tercetak secara otomatis.
