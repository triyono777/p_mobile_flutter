# Bab 12: HTTP Request & Eksternal API

## 1. Tujuan Praktikum
- Memahami konsep API (Application Programming Interface), khususnya RESTful API.
- Melakukan komunikasi lintas Jaringan dari aplikasi handphone terhadap server publik menggunakan protokol HTTP (GET).
- Menguraikan (Parsing) respon data dari JSON yang rumit ke wujud/objek di struktur bahasa pemrograman Dart.

## 2. Dasar Teori
Pada tahap tim bekerja industri, mungkin backend Anda belum disiapkan dengan Supabase melainkan aplikasi Flutter Anda ditugaskan mengambil informasi berita cuaca dari BMKG Pemerintah, informasi mata uang dan harga Kripto dari Bursa Server di Amerika, dsb.

Komunikasi kepada dan dari pihak ketiga ditarik lewat modul khusus menggunakan plugin/package bawaan **`http`**.
- Metodenya yang sering dipanggil: `GET` (Menarik/Mengambil data), `POST` (Melempar form form registrasi rahasia ke Server API), `PUT/PATCH` (Update data API), dan `DELETE`.
- Format antaran komunikasi balasan paling populer didistribusikan dalam teks wujud **JSON** (JavaScript Object Notation). Dalam Dart program, teks JSON harus di-*Decode* dan *Parse* dulu agar bisa divisualisasikan menjadi komponen Widget di layar dengan mudah.

## 3. Langkah Praktikum

### 3.1 Instalasi dan Konfigurasi Modul HTTP
1. Dalam terminal, install package `http`:
   ```bash
   flutter pub add http
   ```
2. Aplikasi harus terkoneksi denga internet perangkat. Untuk sistem operasi Android di emulator terkadang belum otomatis diberi izin. Pastikan pada _project->android/app/src/main/AndroidManifest.xml_ terdaftar properti ini tepat di luar kerangka section main application program:
   `<uses-permission android:name="android.permission.INTERNET" />`

### 3.2 Menarik Data Publik (Contoh JSONPlaceholder Fake API)
Kita tidak akan membuat server API Backend Custom dulu sementara waktu, melainkan menggunakan API pengujian terbuka gratis yang mensimulasikan sekumpulan List Artikel Blog *Dummy* yang disiapkan developer web. URL yang digunakan: `https://jsonplaceholder.typicode.com/posts`

1. Buat pemanggilan awal di `lib/main.dart`:
   ```dart
   // File: lib/main.dart
   import 'package:flutter/material.dart';
   import 'post_list_screen.dart';

   void main() {
     runApp(const MaterialApp(home: PostListScreen()));
   }
   ```
2. Buatlah kelas `PostListScreen` dalam file baru:
   ```dart
   // File: lib/post_list_screen.dart
   import 'package:flutter/material.dart';
   import 'dart:convert'; // Diperlukan untuk proses JSON Decoding Text to Object
   import 'package:http/http.dart' as http; // Alias penamaan untuk membedakan namespace plugin

   class PostListScreen extends StatefulWidget {
     const PostListScreen({super.key});

     @override
     State<PostListScreen> createState() => _PostListScreenState();
   }

   class _PostListScreenState extends State<PostListScreen> {
     // Variable penyimpan list dari JSON Api
     List<dynamic> _posts = [];
     bool _isLoading = true;

     @override
     void initState() {
       super.initState();
       fetchPostsFromApi();
     }

     // Fungsi menembak Jaringan Network Http
     Future<void> fetchPostsFromApi() async {
       try {
         final responServerURL = Uri.parse('https://jsonplaceholder.typicode.com/posts');
         final response = await http.get(responServerURL);

         // Kode 200 OK berarti koneksi web berhasil ditarik dengan baik
         if (response.statusCode == 200) {
           setState(() {
             // Konversi teks string response yang kotor (response.body) ke wujud format Array List dinamis
             _posts = json.decode(response.body);
             _isLoading = false;
           });
         } else {
           // Jika terjadi error dari API Web Provider Server luar (Misal: 404, 500)
           throw Exception('Failed to load API Data dengan kode error: ${response.statusCode}');
         }
       } catch (e) {
         print(e);
         setState(() {
           _isLoading = false;
         });
       }
     }

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(title: const Text('Http Request (API)')),
         body: _isLoading
             ? const Center(child: CircularProgressIndicator())
             : ListView.builder(
                 itemCount: _posts.length,   // Menggunakan limit jumlah daftar sebanyak respon API Server
                 itemBuilder: (context, index) {
                   return Card(
                     margin: const EdgeInsets.symmetric(vertical: 4, horizontal: 8),
                     child: ListTile(
                       leading: CircleAvatar(child: Text(_posts[index]['id'].toString())),
                       title: Text(_posts[index]['title'], maxLines: 1, overflow: TextOverflow.ellipsis,),
                       subtitle: Text(_posts[index]['body'], maxLines: 2, overflow: TextOverflow.ellipsis),
                     ),
                   );
                 },
               ),
       );
     }
   }
   ```
2. Masukkan implementasi rute file di atas tersebut ke main program kemudian simulasikan, daftar 100 artike blog buatan *random text generator* tersebut akan terunduh otomatis dari internet lalu divisualisasikan menjadi `ListView` Flutter Application Anda!

## 4. Tugas Latihan
1. Buka dan pahami kode struktur HTTP Address Response Url ini di browser Anda: `https://jsonplaceholder.typicode.com/users`. URL itu berisi daftar Nama Pengguna Buatan dan _Company_ Email perusahaan dan nomor handphone palsu JSON Format.
2. Buat widget aplikasi baru: `UserContactApiScreen` yang me-request/memanggil data di URL Users tersebut menggunakan metode HTTP Get dan visualisasikan pada desain UI yang menarik sesuai imajinasi layouting tim kalian (Membuat List Kontak Whatsapp misalnya)!
