**Silabus Detil: Penyesuaian RPS Berbasis Proyek (Flutter + Supabase PjBL)**

---

**Pra-UTS: Proyek Pribadi (Membangun Aplikasi Flutter Mandiri yang Fungsional)**

**Target**: Mahasiswa dapat membuat aplikasi mandiri yang fungsional dengan menggunakan Flutter, yang mencakup pemahaman konsep dasar UI/UX, navigasi antar halaman, dan penyimpanan data secara lokal.

---

### **Pertemuan 1 & 2: Pengenalan Flutter & Perencanaan Proyek**

**Tujuan**: Memahami Flutter sebagai kerangka kerja multi-platform dan menyiapkan proyek pertama.

**Materi**:

* Pengenalan Flutter dan fungsionalitasnya sebagai framework lintas platform.
* Instalasi Flutter SDK dan pengenalan terhadap IDE (Android Studio/VS Code).
* Pengenalan bahasa pemrograman Dart.
* Penjelasan perencanaan proyek pribadi mahasiswa, termasuk memilih ide aplikasi yang akan dibangun.

**Praktikum**:

* Instalasi dan konfigurasi Flutter SDK.
* Menyiapkan proyek Flutter baru.
* Diskusi ide dan perencanaan aplikasi.

---

### **Pertemuan 3: Konsep Widget (Stateless vs Stateful)**

**Tujuan**: Mahasiswa memahami perbedaan antara StatelessWidget dan StatefulWidget serta aplikasinya dalam pembuatan UI.

**Materi**:

* Apa itu StatelessWidget dan StatefulWidget.
* Perbedaan antara kedua jenis widget tersebut dan penggunaannya dalam pembuatan UI dinamis dan statis.

**Praktikum**:

* Membangun aplikasi sederhana menggunakan StatelessWidget dan StatefulWidget.
* Membuat komponen UI seperti teks dan tombol, serta memodifikasi state pada StatefulWidget.

---

### **Pertemuan 4: Layouting & Input Form**

**Tujuan**: Menguasai teknik dasar dalam mendesain UI dengan layout dan input form.

**Materi**:

* Penggunaan komponen layout dasar: Row, Column, dan Container.
* Membuat form input menggunakan widget TextField untuk menerima input dari pengguna.

**Praktikum**:

* Mendesain tampilan aplikasi dengan menggunakan layout Row, Column, dan Container.
* Membangun form input pengguna menggunakan widget TextField.

---

### **Pertemuan 5: Navigasi (Routing) Antar Halaman**

**Tujuan**: Memahami bagaimana cara beralih antar halaman dalam aplikasi menggunakan Flutter.

**Materi**:

* Konsep navigasi antar halaman dalam Flutter.
* Penggunaan Navigator.push untuk membuka halaman baru dan Navigator.pop untuk kembali ke halaman sebelumnya.

**Praktikum**:

* Membangun aplikasi dengan lebih dari satu halaman menggunakan navigasi antar halaman.

---

### **Pertemuan 6: Menampilkan Data List**

**Tujuan**: Mengelola data yang ditampilkan dalam format daftar menggunakan ListView.

**Materi**:

* Penggunaan ListView untuk menampilkan data secara vertikal atau horizontal.
* Menggunakan teknik mapping data ke dalam komponen UI yang ada.

**Praktikum**:

* Membuat daftar yang menampilkan data dalam aplikasi menggunakan ListView.

---

### **Pertemuan 7: Penyimpanan Data Lokal (Shared Preferences)**

**Tujuan**: Menyimpan data pengguna secara lokal menggunakan shared_preferences.

**Materi**:

* Penggunaan modul shared_preferences untuk menyimpan data sederhana secara lokal.
* Mengelola data sederhana seperti pengaturan pengguna dan preferensi.

**Praktikum**:

* Menerapkan penyimpanan data menggunakan shared_preferences untuk menyimpan data seperti nama pengguna atau pengaturan aplikasi.

---

### **Pertemuan 8 (UTS): Presentasi Proyek Pribadi**

**Tujuan**: Mahasiswa mempresentasikan aplikasi mandiri yang telah dibangun dan menunjukkan fungsionalitas yang telah dicapai.

**Aktivitas**:

* Presentasi aplikasi yang telah dibangun.
* Demonstrasi fungsionalitas aplikasi, termasuk UI/UX, navigasi, dan penyimpanan data lokal.

---

**Pasca-UTS: Proyek Tim (Aplikasi Skala Produksi dengan Supabase)**

**Target**: Mahasiswa bekerja dalam tim untuk membangun aplikasi yang lebih besar, terintegrasi dengan backend Supabase untuk berbagai fungsionalitas seperti autentikasi, database, penyimpanan, dan pembaruan real-time.

---

### **Pertemuan 9 & 10: State Management Tim**

**Tujuan**: Memahami dan mengimplementasikan manajemen state untuk kolaborasi tim yang efektif.

**Materi**:

* Pengantar tentang arsitektur manajemen state dalam Flutter.
* Penggunaan Provider, BLoC, atau GetX untuk komunikasi antar bagian aplikasi dan efisiensi memori.

**Praktikum**:

* Implementasi manajemen state dalam tim untuk kolaborasi yang lebih baik dalam aplikasi.

---

### **Pertemuan 11: Pengenalan & Autentikasi Supabase**

**Tujuan**: Mengintegrasikan Supabase untuk autentikasi pengguna.

**Materi**:

* Pengantar Supabase sebagai Backend-as-a-Service (BaaS) berbasis PostgreSQL.
* Menghubungkan aplikasi Flutter dengan Supabase menggunakan API Key.
* Pembuatan dan implementasi fitur Sign Up dan Sign In dengan email dan password.

**Praktikum**:

* Implementasi autentikasi pengguna menggunakan Supabase di aplikasi Flutter.

---

### **Pertemuan 12: Integrasi Supabase Database (CRUD)**

**Tujuan**: Mengintegrasikan aplikasi Flutter dengan database Supabase untuk melakukan operasi CRUD (Create, Read, Update, Delete).

**Materi**:

* Membuat tabel di dashboard Supabase.
* Menghubungkan aplikasi Flutter dengan Supabase untuk melakukan operasi CRUD pada data.

**Praktikum**:

* Melakukan operasi CRUD pada database Supabase dari aplikasi Flutter.

---

### **Pertemuan 13: Supabase Realtime & Streams**

**Tujuan**: Mengimplementasikan fitur real-time untuk pembaruan data secara langsung.

**Materi**:

* Pengenalan fitur Supabase Realtime dan penggunaan stream untuk mendeteksi perubahan data di database.
* Menyusun fitur yang memungkinkan aplikasi untuk merespon pembaruan data secara otomatis.

**Praktikum**:

* Implementasi pembaruan data secara real-time dengan Supabase.

---

### **Pertemuan 14: Supabase Storage**

**Tujuan**: Menggunakan Supabase Storage untuk mengunggah gambar dan file media lainnya.

**Materi**:

* Penggunaan Supabase Storage untuk mengelola penyimpanan file.
* Implementasi Image Picker untuk memilih gambar dari galeri dan mengunggahnya ke Supabase Storage.

**Praktikum**:

* Mengintegrasikan Image Picker untuk memilih gambar dan mengunggahnya ke Supabase.

---

### **Pertemuan 15: HTTP Request & Eksternal API**

**Tujuan**: Mempelajari cara mengakses API eksternal untuk mengambil data menggunakan HTTP requests.

**Materi**:

* Pengantar REST API dan metode HTTP (GET, POST, PUT, DELETE).
* Penanganan proses asinkron dan error menggunakan FutureBuilder untuk berinteraksi dengan API eksternal.

**Praktikum**:

* Mengambil data dari API eksternal menggunakan HTTP requests dan menampilkannya di aplikasi Flutter.

---

### **Pertemuan 16 (UAS): Presentasi & Rilis Aplikasi**

**Tujuan**: Mahasiswa mempresentasikan aplikasi akhir yang dibangun dalam tim dan memahami proses rilis aplikasi.

**Aktivitas**:

* Presentasi aplikasi akhir tim, menunjukkan semua fitur yang telah dibangun.
* Demonstrasi proses rilis aplikasi (deployment) ke platform seperti Google Play atau App Store.

---

**Catatan**: Silabus ini berfokus pada pengembangan aplikasi Flutter yang terintegrasi dengan Supabase. Penekanan pada PjBL (Project-Based Learning) membantu mahasiswa untuk belajar secara langsung dengan membangun aplikasi nyata yang menggunakan teknologi terkini.
