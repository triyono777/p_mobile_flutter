# Bab 8: Pengenalan & Autentikasi Supabase

## 1. Tujuan Praktikum
- Mengenal konfigurasi layanan Backend-as-a-Service (BaaS) menggunakan Supabase.
- Membuat proyek dasar di portal web Supabase dan mengambil API keys.
- Mengintegrasikan library `supabase_flutter` ke dalam aplikasi.
- Membangun fitur Registrasi (Sign Up) dan Masuk (Sign In) menggunakan email & sandi.

## 2. Dasar Teori
Supabase adalah alternatif *open-source* untuk Firebase yang mengandalkan fungsionalitas dan fitur PostgreSQL yang kokoh sebagai basis datanya. Layanan utamanya termasuk Authentication (Login system), Database (Database PostgreSQL), Realtime, dan Storage (penyimpanan Media Image/Video).

Sistem autentikasi (Authentication) digunakan oleh hampir seluruh aplikasi modern karena memungkinkan program Anda menjaga keamanan akses dan personalisasi tiap user yang mandiri.

## 3. Langkah Praktikum

### 3.1 Setup Akun & Proyek di Supabase Dashboard
1. Buka browser dan arahkan ke alamat browser [supabase.com](https://supabase.com).
2. Klik tombol **Start your project** dan registrasi/login dengan akun GitHub atau Email.
3. Setelah masuk Dashboard, klik **New Project**, tentukan organisasi, nama proyek (e.g. `flutter-pjbl`), serta buat kata sandi *database*(Simpan catatan kata sandi ini baik-baik). Pilih juga server Region (misalnya *Singapore*).
4. Klik **Create new project** (Tunggu proses beberapa menit untuk pendirian database server).
5. Pada menu _Authentication -> Providers_, pastikan pendaftaran Provider **Email** diaktifkan (enabled). Matikan/nonaktifkan bagian **Confirm email** untuk mempermudah masa *development* / uji coba tanpa validasi kotak masuk alamat email yang asli terlebih dahulu.
6. Arahkan kursor pada manu *Project Setting (Ikon roda gigi di pojok kiri bawah)* -> klik submenu **API**. Salin String/Teks nilai dari `Project URL` dan `Project API Keys (Aanon / public)`.

### 3.2 Menambahkan Library SDK & Konfigurasi di Flutter
1. Buka terminal proyek Flutter, tambahkan dependensinya:
   ```bash
   flutter pub add supabase_flutter
   ```
2. Buka `main.dart` dan lakukan inisiasi API key tersebut (agar aplikasi mengarah ke alamat project Supabase yang kamu buat di atas).
   ```dart
   // File: lib/main.dart
   import 'package:flutter/material.dart';
   import 'package:supabase_flutter/supabase_flutter.dart';

   Future<void> main() async {
     WidgetsFlutterBinding.ensureInitialized();

     // Inisialisasi Supabase
     // Ganti URl dan Anon Key sesuai dengan Dashboard Proyek kamu
     await Supabase.initialize(
       url: 'https://xxx-domain-mu-xxx.supabase.co',
       anonKey: 'ey..........',
     );

     runApp(const MyApp());
   }

   final supabase = Supabase.instance.client;

   class MyApp extends StatelessWidget {
     const MyApp({super.key});

     @override
     Widget build(BuildContext context) {
       return const MaterialApp(
         title: 'Auth Supabase',
         home: LoginScreen(),
       );
     }
   }
   ```
   *Catatan: Anda juga perlu mengimpor `login_screen.dart` (yang akan kita buat di langkah selanjutnya) di file `main.dart` dengan menambahkan baris `import 'login_screen.dart';` jika file tersebut diletakkan dalam folder yang sama/memiliki akses rute.*

### 3.3 Menyusun Tampilan Layar Autentikasi Form (LoginScreen)
1. Buat kode di class `LoginScreen` (StatefulWidget karena menangani aksi TextField).
   ```dart
   // File: lib/login_screen.dart (buat file baru di dalam lib/)
   import 'package:flutter/material.dart';
   import 'package:supabase_flutter/supabase_flutter.dart';
   import 'main.dart'; // import supabase instance dari main.dart

   class LoginScreen extends StatefulWidget {
     const LoginScreen({super.key});

     @override
     State<LoginScreen> createState() => _LoginScreenState();
   }

   class _LoginScreenState extends State<LoginScreen> {
     final TextEditingController _emailController = TextEditingController();
     final TextEditingController _passwordController = TextEditingController();
     bool _isLoading = false;

     // Fungsi eksekusi pendaftaran (Register) melalui Supabase Authentication Object
     Future<void> _signUp() async {
       setState(() { _isLoading = true; });
       try {
         await supabase.auth.signUp(
           email: _emailController.text,
           password: _passwordController.text,
         );
         if(mounted) ScaffoldMessenger.of(context).showSnackBar(const SnackBar(content: Text('Registrasi Berhasil!')));
       } on AuthException catch (e) {
         if(mounted) ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text(e.message)));
       } catch (e) {
         if(mounted) ScaffoldMessenger.of(context).showSnackBar(const SnackBar(content: Text('Terjadi error')));
       }
       setState(() { _isLoading = false; });
     }

     // Fungsi Sign-in (Login)
     Future<void> _signIn() async {
       setState(() { _isLoading = true; });
       try {
         await supabase.auth.signInWithPassword(
           email: _emailController.text,
           password: _passwordController.text,
         );
         if(mounted) ScaffoldMessenger.of(context).showSnackBar(const SnackBar(content: Text('Login Berhasil!')));
         // TODO: Lakukan Navigator.pushReplacement ke layar utama (HomeScreen)
       } on AuthException catch (e) {
         if(mounted) ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text(e.message)));
       } catch (e) {
         if(mounted) ScaffoldMessenger.of(context).showSnackBar(const SnackBar(content: Text('Terjadi error tidak terduga')));
       }
       setState(() { _isLoading = false; });
     }

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(title: const Text('Login Sistem')),
         body: Padding(
           padding: const EdgeInsets.all(16.0),
           child: Column(
             mainAxisAlignment: MainAxisAlignment.center,
             children: [
               TextField(
                 controller: _emailController,
                 decoration: const InputDecoration(labelText: 'Email'),
               ),
               const SizedBox(height: 16),
               TextField(
                 controller: _passwordController,
                 decoration: const InputDecoration(labelText: 'Password'),
                 obscureText: true,
               ),
               const SizedBox(height: 16),
               _isLoading ? const CircularProgressIndicator() : Column(
                 children: [
                   ElevatedButton(
                     onPressed: _signIn,
                     child: const Text('Sign In / Masuk'),
                   ),
                   TextButton(
                     onPressed: _signUp,
                     child: const Text('Sign Up / Mendaftar Cepat'),
                   ),
                 ],
               ),
             ],
           ),
         ),
       );
     }
   }
   ```
2. Lakukan *Build test run / flutter run*.
3. Tes masukan email yang acak (Contoh `tes123@coba.com`) dan *password* `rahasia123` lalu tekan **Sign Up**. Periksa notifikasi.
4. Buka Browser Supabase di komputer Anda kembali, navigasikan menunya ke _Authentication -> Users_. Buktikan bahwa data pengguna akun `tes123@coba.com` tersebut telah berhasil tersimpan dan tercipta pada cloud server *backend* aplikasi.

## 4. Tugas Latihan
1. Ciptakan atau hubungkan aplikasi dengan Halaman Utama (Misalnya: `HomeScreen` atau Dashboard aplikasi Anda).
2. Masukkan perintah logika pengecekan saat tombol `Sign In` ditekan. Jika Auth kredensial benar dan Sign In berhasil dilewati di blok try/catch (di komentar `// TODO:`), lakukan proses navigasi melempar *user* beralih dari yang semula Login Screen berpindah ke `HomeScreen` (gunakan `Navigator.pushReplacement()`).
