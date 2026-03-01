# Bab 11: Supabase Storage

## 1. Tujuan Praktikum
- Mengelola *Storage* (Media Penyimpanan Awan) dalam Supabase ("Buckets").
- Mengimplementasikan pustaka `image_picker` untuk mengambil foto/gambar lokal dari memori atau kamera perangkat.
- Mengunggah (Upload) file media ke Supabase dan menampilkan *URL* gambarnya di layar aplikasi.

## 2. Dasar Teori
Aplikasi berskala besar membedakan penyimpanan antata Data Struktural Teks panjang (seperti profil, alamat, judul disimpan di **Database Server / PostgreSQL**) dengan penyimpanan Binary seperti file Audio, PDF dan Gambar yang berukuran besar. File besar ini dijinakkan dalam infrastruktur **Storage / Bucket**.

Supabase memberikan sebuah layanan bucket yang bisa diakses dengan sintaks SDK dengan mulus. Untuk mengambil gambar dari penyimpanan galeri handphone, kita memanfaatkan library pub.dev bernama `image_picker`.

## 3. Langkah Praktikum

### 3.1 Setup Bucket Storage & Konfigurasi Plugin
1. Di Dashboard web Supabase, klik menu **Storage** (Ikon keranjang file/kotak).
2. Buat bucket baru, beri nama `avatars` dan centang opsi **Public Bucket** agar gambar dapat dibaca siapapun yang memiliki URL-nya.
3. Di Proyek Flutter Anda, tambahkan package tambahan bernama *image_picker*:
   ```bash
   flutter pub add image_picker
   ```
4. *Konfigurasi Platform iOS (Mac)*: jika Anda mem-build iOS, buka `ios/Runner/Info.plist` dan tambahkan baris key untuk meminta izin galeri pengguna ke Apple OS.
   ```xml
   <key>NSPhotoLibraryUsageDescription</key>
   <string>Aplikasi membutuhkan izin untuk mengganti foto profil anda.</string>
   <key>NSCameraUsageDescription</key>
   <string>Aplikasi membutuhkan kamera.</string>
   ```

### 3.2 Kode Implementasi Upload
1. Di `main.dart`, pastikan Supabase telah diinisialisasi dan ubah rute awal ke `ProfileUploadScreen`.
2. Buat layar bernama `ProfileUploadScreen` di file terpisah `lib/profile_upload_screen.dart`
   ```dart
   // File: lib/profile_upload_screen.dart
   import 'dart:io';
   import 'package:flutter/material.dart';
   import 'package:image_picker/image_picker.dart';
   import 'package:supabase_flutter/supabase_flutter.dart';

   class ProfileUploadScreen extends StatefulWidget {
     const ProfileUploadScreen({super.key});

     @override
     State<ProfileUploadScreen> createState() => _ProfileUploadScreenState();
   }

   class _ProfileUploadScreenState extends State<ProfileUploadScreen> {
     bool _isLoading = false;
     String? _imageUrl;

     // Fungsi eksekusi memilih gambar dan langsung mendaratkan file ke Bucket
     Future<void> _uploadImage() async {
       // Memakai plugin image_picker
       final picker = ImagePicker();
       final imageFile = await picker.pickImage(source: ImageSource.gallery);

       if (imageFile == null) return; // User membatalkan pemilihan via galeri

       setState(() { _isLoading = true; });

       try {
         final bytes = await imageFile.readAsBytes();
         final fileExt = imageFile.name.split('.').last; // dapatkan ekstensi: jpg / png
         // Buat nama unik dari satuan waktu (timestamp)
         final fileName = '${DateTime.now().millisecondsSinceEpoch}.$fileExt';

         // PROSES UNGGAH KE SUPABASE
         await Supabase.instance.client.storage
             .from('avatars')
             .uploadBinary(fileName, bytes);

         // Minta Public url dan mengunci statenga
         final imageUrlResponse = Supabase.instance.client
             .storage
             .from('avatars')
             .getPublicUrl(fileName);

         setState(() {
           _imageUrl = imageUrlResponse;
         });
         if(mounted) ScaffoldMessenger.of(context).showSnackBar(const SnackBar(content: Text('Gambar berhasil diunggah!')));

       } catch (error) {
         if(mounted) ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text('Gagal upload: $error')));
       } finally {
         setState(() { _isLoading = false; });
       }
     }

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(title: const Text('Update Profile')),
         body: Center(
           child: Column(
             mainAxisAlignment: MainAxisAlignment.center,
             children: [
               _imageUrl == null
                   ? const Icon(Icons.person, size: 100, color: Colors.grey)
                   : Image.network(_imageUrl!, height: 200, fit: BoxFit.cover),
               const SizedBox(height: 20),
               _isLoading
                   ? const CircularProgressIndicator()
                   : ElevatedButton.icon(
                       onPressed: _uploadImage,
                       icon: const Icon(Icons.upload),
                       label: const Text('Ganti Foto Profil'),
                     ),
             ],
           ),
         ),
       );
     }
   }
   ```
2. Coba Build dan tekan "Ganti Foto Profil" lalu saksikan keajaibannya memuat `Image.network`!

## 4. Tugas Latihan
1. URL gambar hasil _upload_ yang sekarang masih hilang / lenyap jika kita menutup dan membuka aplikasi karena data URL dismpan di *local variable `_imageUrl`*!
2. Integrasikan operasi ini dengan materi **Database (Bab 9)**! Simpan String URL yang didapat (variabel `imageUrlResponse`) dan lalukan _Update User Profile Table Database_ agar alamat avatar yang dipasang permanen untuk setiap profil pengguna yang telah melakukan _login_!
