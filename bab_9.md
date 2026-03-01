# Bab 9: Integrasi Supabase Database (CRUD)

## 1. Tujuan Praktikum
- Menggunakan Supabase Database (PostgreSQL) sebagai penyimpan data utama aplikasi.
- Membuat skema tabel dari Dashboard antarmuka web Supabase.
- Mampu melakukan operasi dasar Database: Create, Read, Update, Delete (CRUD) dari aplikasi Flutter.

## 2. Dasar Teori
Aplikasi membutuhkan database untuk menyimpan dan mengolah rentetan data, seperti data produk, pegawai, atau daftar aktivitas.
Supabase menyediakan database SQL (Postgres) yang sangat tangguh, dan dengan `supabase_flutter`, kita bisa melakukan manipulasi operasi database (CRUD) dari sisi *client* (aplikasi mobile) hanya dengan bahasa Dart tanpa harus mengatur bahasa *backend* lain yang rumit seperti PHP/Node.js secara langsung.
- **Create**: Memasukkan/insert rekaman (`.insert()`).
- **Read**: Membaca atau menyeleksi dari database (`.select()`).
- **Update**: Memperbarui baris data tertentu (`.update()`).
- **Delete**: Menghilangkan rekaman dari tabel (`.delete()`).

## 3. Langkah Praktikum

### 3.1 Menyiapkan Tabel Database di Dashboard Supabase
1. Buka kembali dashboard Supabase ([supabase.com](https://supabase.com)).
2. Masuk ke proyek buatanmu yang digunakan di praktikum sebelumnya (Bab 8).
3. Di bilah navigasi kiri, pilih **Table Editor**.
4. Klik **Create a new table**.
5. Isi konfigurasi tabel baru:
   - Name: `notes`
   - Beri centang pada *Enable Row Level Security (RLS)* (Abaikan RLS warning pada pengaturan sementara untuk uji coba / buat *policy* public read/write).
   - Tambahkan *Columns*:
     - `id`: tipe *int8* (dicentang primary & auto increment).
     - `title`: tipe *text*.
     - `content`: tipe *text*.
6. Klik **Save**.
7. *PENTING:* Masuk ke menu **Authentication -> Policies** dan matikan RLS pada tabel `notes` sementara (atau buat Policy yang memperbolehkan ALL aksi untuk public). Di dunia nyata ini tidak aman, tapi untuk praktikum awal amat sangat mempermudah eksplorasi fungsional Flutter Anda.

### 3.2 Menulis Operasi CRUD dalam Flutter
1. Di proyek Flutter, modifikasi `lib/main.dart` agar memanggil halaman dari file terpisah:
   ```dart
   // File: lib/main.dart
   import 'package:flutter/material.dart';
   import 'package:supabase_flutter/supabase_flutter.dart';
   import 'note_list_screen.dart';

   Future<void> main() async {
     WidgetsFlutterBinding.ensureInitialized();
     await Supabase.initialize(
       url: 'https://xxx-domain-mu-xxx.supabase.co',
       anonKey: 'ey..........',
     );
     runApp(const MaterialApp(home: NoteListScreen()));
   }
   ```
2. Buat file baru `lib/note_list_screen.dart` untuk *Read* (Menampilkan List) catatan:
   ```dart
   // File: lib/note_list_screen.dart (buat file baru)
   import 'package:flutter/material.dart';
   import 'package:supabase_flutter/supabase_flutter.dart';

   class NoteListScreen extends StatefulWidget {
     const NoteListScreen({super.key});

     @override
     State<NoteListScreen> createState() => _NoteListScreenState();
   }

   class _NoteListScreenState extends State<NoteListScreen> {
     List<dynamic> _notes = [];
     bool _isLoading = true;

     @override
     void initState() {
       super.initState();
       _fetchNotes();
     }

     // READ: Menarik daftar catatan dari tabel "notes"
     Future<void> _fetchNotes() async {
       try {
         final data = await Supabase.instance.client.from('notes').select().order('id', ascending: false);
         setState(() {
           _notes = data;
           _isLoading = false;
         });
       } catch (e) {
         print('Error fetching data: $e');
         setState(() { _isLoading = false; });
       }
     }

     // DELETE: Menghapus catatan
     Future<void> _deleteNote(int id) async {
       await Supabase.instance.client.from('notes').delete().eq('id', id);
       _fetchNotes(); // Refresh layar setelah menghapus
     }

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(title: const Text('My Notes (Supabase)')),
         body: _isLoading
             ? const Center(child: CircularProgressIndicator())
             : ListView.builder(
                 itemCount: _notes.length,
                 itemBuilder: (context, index) {
                   final note = _notes[index];
                   return ListTile(
                     title: Text(note['title'] ?? ''),
                     subtitle: Text(note['content'] ?? ''),
                     trailing: IconButton(
                       icon: const Icon(Icons.delete, color: Colors.red),
                       onPressed: () => _deleteNote(note['id']),
                     ),
                   );
                 },
               ),
         floatingActionButton: FloatingActionButton(
           onPressed: () {
             // Buka halaman form / Dialog untuk Input menambah catatan
             _showAddNoteDialog();
           },
           child: const Icon(Icons.add),
         ),
       );
     }

     // Fungsi Tampilan PopUp Form untuk CREATE Data
     void _showAddNoteDialog() {
       final titleController = TextEditingController();
       final contentController = TextEditingController();

       showDialog(
         context: context,
         builder: (context) => AlertDialog(
           title: const Text('New Note'),
           content: Column(
             mainAxisSize: MainAxisSize.min,
             children: [
               TextField(controller: titleController, decoration: const InputDecoration(labelText: 'Title')),
               TextField(controller: contentController, decoration: const InputDecoration(labelText: 'Content')),
             ],
           ),
           actions: [
             TextButton(
               onPressed: () => Navigator.pop(context),
               child: const Text('Cancel'),
             ),
             ElevatedButton(
               onPressed: () async {
                 // CREATE: Memasukkan rekaman baru
                 await Supabase.instance.client.from('notes').insert({
                   'title': titleController.text,
                   'content': contentController.text,
                 });
                 if(mounted) Navigator.pop(context);
                 _fetchNotes(); // Refresh daftar setelah menambah data berhasil disematkan
               },
               child: const Text('Save'),
             ),
           ],
         ),
       );
     }
   }
   ```
3. Uji Coba: Jalankan kode tersebut, klik Floating Action Button (`+`), isi form dan *Save*. Data akan di-insert ke Supabase dan akan ditampilkan dalam layar *ListView*.

## 4. Tugas Latihan
Kita belum menyelesaikan fitur **UPDATE** di kode di atas.
Tugas Anda: Tambahkan fitur Edit!
1. Tambahkan sebuah elemen tombol ikon `edit` (Icons.edit) di samping kanan text List.
2. Ketika ditekan, munculkan Pop-up dialog form yang datanya sudah terisi (judul & konten lama).
3. Setelah tombol *Update* ditekan, panggil fungsi fitur fungsi Supabase Update:
   `await Supabase.instance.client.from('notes').update({'title': 'baru'}).eq('id', idTujuan);`
4. Lakukan pemanggilan metode `_fetchNotes()` kembali untuk merefresh halaman UI.
