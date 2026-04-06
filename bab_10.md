# Bab 10: Supabase Realtime & Streams

## 1. Tujuan Praktikum
- Memahami konsep data Real-time pada Supabase.
- Menggunakan Flutter `StreamBuilder` alih-alih me-*refresh* UI secara manual (seperti Bab 9).
- Membuat aplikasi bisa merespon perubahan data *database* secara langsung dan otomatis.

## 2. Dasar Teori
Pada sistem konvensional aplikasi mobile menggunakan HTTP untuk meminta data (pull) dari server. Apabila data pada server berubah (contohnya ada pesan chat masuk), maka layar di HP tidak sadar kecuali pengguna melakukan *Refresh (Swipe to Refresh)* atau pindah halaman.
Teknologi **WebSocket** atau fitur **Realtime Streams** Supabase memungkinkan aplikasi terus mendengarkan perubahan dari database yang dipublikasikan kembali. Jika sebuah baris data baru ditambahkan ke tabel `notes` dari dashboard web misalnya, aplikasi Flutter yang posisinya terbuka akan otomatis merender/menampilkannya dalam hitungan mili-detik.

## 3. Langkah Praktikum

### 3.1 Mengonfigurasi Tabel agar Mendukung Fitur Realtime
1. Buka kembali dashboard Supabase.
2. Ke menubar, pilih menu **Database *->* Replication** di bawah sub-kategori Publication.
3. Centang bagian tabel `notes` untuk masuk ke daftar source realtime. (Atau secara mudah, atur properti realtime "ON" pada tabel di Table Editor).

### 3.2 Implementasi Stream pada Aplikasi Flutter
Kita akan merevisi UI dan Logic dari praktikum Bab 9 yang sebelumnya masih perlu memanggil paksa `_fetchNotes()` setiap kali data diubah.

1. Pastikan Anda telah mengonversi `lib/main.dart` (seperti pada Bab 9) agar memanggil `NoteStreamScreen` untuk UI ini.
   ```dart
   // File: lib/main.dart
   // ... Konfigurasi Supabase
   // runApp(const MaterialApp(home: NoteStreamScreen()));
   ```
2. Buat/ubah file `lib/note_stream_screen.dart` dan ubah/tambahkan properti tipe datanya menjadi model Stream:
   ```dart
   // File: lib/note_stream_screen.dart (atau ubah file lib/note_list_screen.dart dari Bab 9)
   import 'package:flutter/material.dart';
   import 'package:supabase_flutter/supabase_flutter.dart';

   class NoteStreamScreen extends StatefulWidget {
     const NoteStreamScreen({super.key});

     @override
     State<NoteStreamScreen> createState() => _NoteStreamScreenState();
   }

   class _NoteStreamScreenState extends State<NoteStreamScreen> {
     // Mendefinisikan Stream dari Table
     // Supabase stream mengharuskan Anda mendefinisikan kunci unik dari tabelnya (biasanya adalah kolom primary_keys).
     final _notesStream = Supabase.instance.client.from('notes').stream(primaryKey: ['id']).order('id', ascending: false);

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(title: const Text('Realtime Notes')),
         // Dibandingkan memakai Future (atau list manual), kita menggunakan StreamBuilder bawaan dari framework Flutter
         body: StreamBuilder<List<Map<String, dynamic>>>(
           stream: _notesStream,
           builder: (context, snapshot) {
             // Penanganan State status loading ketika sedang menunggu data jaringan pertama kali
             if (!snapshot.hasData) {
               return const Center(child: CircularProgressIndicator());
             }

             final notes = snapshot.data!;

             // Jika daftar kosong dari tabel
             if (notes.isEmpty) {
               return const Center(child: Text('Tidak ada cacatan sama sekali.'));
             }

             // Menggambar / Memvisualisasikan list yang selalu ter-update
             return ListView.builder(
               itemCount: notes.length,
               itemBuilder: (context, index) {
                 final note = notes[index];
                 return Card(
                   child: ListTile(
                     title: Text(note['title'] ?? ''),
                     subtitle: Text(note['content'] ?? ''),
                     // Logika Delete seperti Bab 9 tetap bisa dicopy dan  dipasang di sini
                   ),
                 );
               },
             );
           },
         ),
       );
     }
   }
   ```
3. Lakukan pengujian aplikasi. Cobalah biarkan layar terbuka dalam *emulator*.
4. Kemudian buka Dashboard web *Supabase Table Editor*. Insert row baru secara manual lewat web browser dan lihat perbedaannya! Flutter akan secara ajaib mem-build item baru seketika pada emulator.

## 4. Tugas Latihan
1. Pikirkan dan buatlah antarmuka sederhana baru bernama **Grup Chat Sederhana**.
2. Anda cukup membuat tabel baru bernama `chats` di Database Dashboard yang isinya *Kolom Sender(Pengirim)* dan *Kolom Message(Pesan)*.
3. Jadikan ListView realtime di atas sebagai komponen untuk me-render daftar pesan chat yang saling berbalasan dengan UI/UX menarik bersama teman satu kelompok Anda!
