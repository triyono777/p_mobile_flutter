# Bab Tambahan: Dasar-dasar Pemrograman Dart

## 1. Tujuan Praktikum
- Mengenal dan memahami sintaks dasar bahasa pemrograman Dart.
- Memahami konsep tipe data, variabel, percabangan, dan perulangan di Dart.
- Mengenal fungsi dasar (Function) dan konsep Pemrograman Berorientasi Objek (OOP) secara sederhana.

## 2. Dasar Teori
Sebelum menggunakan framework Flutter, penguasaan terhadap bahasa pemrogramannya, yaitu Dart, sangatlah penting. Dart adalah bahasa yang berorientasi objek (Object-Oriented), memiliki sintaks bergaya C (mirip Java, C#, atau JavaScript), dan mendukung *Strong Typing* atau tipe data yang ketat.

Materi Dart ini tidak memerlukan instalasi SDK Flutter jika Anda hanya ingin menjalankannya secara online menggunakan platform **DartPad**.

## 3. Langkah Praktikum

### 3.1 Bekerja dengan DartPad
1. Buka browser komputer Anda dan kunjungi URL [dartpad.dev](https://dartpad.dev).
2. Tuliskan struktur program utama (Main Function) Dart. Semua program Dart bermula dari fungsi `main()`.
   ```dart
   // File: di dalam editor DartPad
   void main() {
     print('Halo dari Dart!');
   }
   ```
3. Tekan tombol **Run** dan perhatikan hasilnya di terminal (Console) sebelah kanan.

### 3.2 Variabel dan Tipe Data
Dart memiliki 2 gaya pendefinisian variabel: secara eksplisit (misal: `String`, `int`) atau menggunakan tipe inferensial dinamis (`var`).

1. Hapus isi fungsi `main()` sebelumnya.
2. Tuliskan contoh deklarasi dan pencetakan (print) variabel berikut:
   ```dart
   void main() {
     // A. Tipe Data Eksplisit Secara Statis
     String nama = 'Andi';
     int umur = 20;
     double tinggiBadan = 175.5;
     bool isMahasiswa = true;

     // B. Membiarkan Dart menebak tipe data secara cerdas
     var kampus = 'Universitas XYZ'; // Dart otomatis menganggap kampus adalah String
     var nilai = [80, 90, 85];       // Dart menganggap ini List<int> (Array)

     // Mencetak dengan teknik interpolasi (Kombinasi String + Variabel dengan tanda $)
     print('Nama: $nama, Umur: $umur tahun');
     print('Tinggi: $tinggiBadan cm, Mahasiswa: $isMahasiswa');
     print('Kuliah di: $kampus');
     print('Nilai semester 1: ${nilai[0]}');
   }
   ```
3. Tekan **Run**. Pahami bahwa `var` pada Dart akan bersifat kuat (*Strong Type*) setelah ditebak. Anda tidak bisa merubah properti `kampus` menjadi angka (`kampus = 12`) setelah deklarasi pertama menggunakan string.

### 3.3 Percabangan (If - Else) & Perulangan (For - Loop)
1. Tuliskan kode perhitungan if-else dan for loop untuk mencetak angka ganjil/genap.
   ```dart
   void main() {
     // Percabangan
     int kkm = 75;
     int nilaiku = 80;

     if (nilaiku >= kkm) {
       print('Selamat, Anda Lulus!');
     } else {
       print('Maaf, Anda perlu remidi.');
     }

     print('---');

     // Perulangan untuk mencetak angka 1 sampai 5
     for (int i = 1; i <= 5; i++) {
       if (i % 2 == 0) {
         print('Angka $i adalah Genap');
       } else {
         print('Angka $i adalah Ganjil');
       }
     }
   }
   ```
2. Eksperimen ubah rentang perulangan `for` hingga angka 10 dan jalankan.

### 3.4 Fungsi (Function)
Selain fungsi utama `main()`, kita dapat membuat fungsi terpisah agar kode bisa digunakan berulang (Re-usable).

1. Tuliskan contoh fungsi `hitungLuas` dan `sapaUser` berikut ini:
   ```dart
   // Fungsi tanpa nilai kembalian (void)
   void sapaUser(String nama) {
     print('Halo $nama, selamat belajar Dart!');
   }

   // Fungsi yang mengembalikan nilai tipe int
   int hitungLuasPersegiPanjang(int panjang, int lebar) {
     int luas = panjang * lebar;
     return luas;
   }

   void main() {
     sapaUser('Budi');

     int p = 10;
     int l = 5;
     int hasilLuas = hitungLuasPersegiPanjang(p, l);
     print('Luas persegi panjang dari $p x $l = $hasilLuas');
   }
   ```

### 3.5 Pemrograman Berorientasi Objek Dasar (Class & Object)
Dart berorientasi objek sepenuhnya. Fitur ini krusial karena kita akan berurusan dengan _Class_ di Flutter nanti (contohnya pembuatan Form Login yang sebenarnya adalah instansiasi layar class `LoginScreen`).

1. Praktikkan pembuatan *blueprint* (Class) dan penciptaan objek.
   ```dart
   // Ini adalah Blueprint blueprint (Cetakan Objek)
   class Kucing {
     // Atribut / Properties
     String nama;
     int umur;

     // Constructor (Fungsi yang dieksekusi pertama kali saat objek dibuat)
     Kucing(this.nama, this.umur);

     // Method / Tingkah laku
     void bersuara() {
       print('Meong! Nama saya ${this.nama}');
     }
   }

   void main() {
     // Membuat objek pertama dari cetakan Class Kucing
     Kucing kucingMika = Kucing('Mika', 2);

     // Membuat objek kedua
     Kucing kucingGarfield = Kucing('Garfield', 5);

     // Memanggil method
     kucingMika.bersuara();
     print('Kucing ${kucingGarfield.nama} berumur ${kucingGarfield.umur} tahun.');
   }
   ```
2. Tekan **Run** dan lihat perbandingan penciptaan objek. Dart tidak menggunakan kata kunci `new` seperti Java. Instansiasi objek dipanggil langsung dengan nama metode konstruktornya.

## 4. Tugas Latihan
Gunakan platform DartPad (atau File Dart command line biasa jika Anda sudah install SDK lokal).
1. Buat class baru dengan nama `Mahasiswa`.
2. Berikan atribut `nama`, `nim`, dan variabel Map/List `nilaiPelajaran`.
3. Buat konstruktor untuk instansiasinya.
4. Buat method `hitungRataRata()` yang bertugas menjumlahkan dan membagi rata seluruh isi data `nilaiPelajaran`. Cetak hasil rata-ratanya tersebut ke console!
