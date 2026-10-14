# Student Management System


| | |
|---|---|
| Nama | Al Jad Kaukabudduri Hardianto |
| NIM | 5025241248 |
| Kelas | PBKK C |


---

## Dokumentasi Tampilan Aplikasi

### 1. Tampilan Awal Aplikasi (READ)

<img width="1481" height="926" alt="image" src="https://github.com/user-attachments/assets/59561143-2f40-4873-b339-4bfac78deb63" />  

Tampilan saat aplikasi pertama kali dijalankan. Data sampel dari tabel `Students` dimuat dari database dan ditampilkan pada DataGrid. Panel **Informasi** di sisi kanan langsung menampilkan statistik awal: total mahasiswa, jumlah per jurusan, dan jumlah per gender.

### 2. Form Input Data Mahasiswa

<img width="972" height="405" alt="image" src="https://github.com/user-attachments/assets/96592582-2c04-4f21-9f90-4a3b7c3e092b" />  

Form diisi dengan data mahasiswa baru: NIM, Nama, Email (TextBox) serta Jurusan dan Gender (ComboBox). Setiap field terhubung ke objek `SelectedStudent` melalui Data Binding, sehingga isi form langsung tersimpan.

### 3. Berhasil Menambahkan Data (CREATE)

<img width="1437" height="167" alt="image" src="https://github.com/user-attachments/assets/47eb6384-ff69-4e35-84b4-53791b25b6b6" />

Setelah tombol **Simpan** ditekan, data dikirim ke database melalui `StudentRepository.Insert()`. Tabel dimuat ulang sehingga data baru muncul di baris paling atas, angka pada panel statistik bertambah, dan form otomatis dikosongkan.

### 4. Memilih Data untuk Diubah (EDIT)

<img width="1477" height="925" alt="image" src="https://github.com/user-attachments/assets/d52fcff3-b8a3-4b4a-a78b-8ccf24e90ae2" />

Saat sebuah baris pada tabel diklik, data baris tersebut otomatis terisi ke dalam form. Hal ini terjadi karena `SelectedItem` DataGrid terhubung dua arah dengan `SelectedStudent` di ViewModel.

### 5. Berhasil Mengubah Data (UPDATE)

<img width="1433" height="153" alt="image" src="https://github.com/user-attachments/assets/1dd54240-3470-40d1-86d6-bf6e47f8b951" />

Contoh Email Rina diubah menjadi rina01@gmail.com, lalu tombol **Simpan** ditekan. Karena `Id` data tidak bernilai 0, ViewModel memanggil `StudentRepository.Update()`. Tabel menampilkan data yang sudah diperbarui.

### 6. Berhasil Menghapus Data (DELETE)

![Data berhasil dihapus](docs/screenshots/06-hapus-data.png)

Data dipilih pada tabel lalu tombol **Hapus** ditekan. Data dihapus dari database melalui `StudentRepository.Delete()`, baris hilang dari tabel, dan angka pada panel statistik ikut berkurang.

### 7. Pencarian Data (SEARCH)

![Pencarian](docs/screenshots/07-pencarian.png)

Kata kunci dimasukkan pada kolom pencarian, lalu tombol **Cari** ditekan. Aplikasi menjalankan query `LIKE` pada kolom NIM, Nama, dan Jurusan, sehingga tabel hanya menampilkan data yang sesuai. Jika kolom pencarian dikosongkan, seluruh data ditampilkan kembali.

### 8. Statistik Mahasiswa

![Statistik](docs/screenshots/08-statistik.png)

Panel **Informasi** menampilkan Total Mahasiswa, jumlah Informatika, Sistem Informasi, Laki-laki, dan Perempuan. Angka ini dihitung dari koleksi data dan diperbarui otomatis setiap kali ada perubahan (tambah, ubah, hapus, atau cari) melalui `INotifyPropertyChanged`.

### 9. Reset Form

![Reset](docs/screenshots/09-reset.png)

Tombol **Reset** mengosongkan seluruh isi form sehingga siap digunakan untuk memasukkan data baru. Reset dilakukan dengan membuat objek `Student` baru pada `SelectedStudent`.

### 10. Validasi Input

![Validasi](docs/screenshots/10-validasi.png)

Jika tombol **Simpan** ditekan saat field wajib (NIM, Nama, Jurusan, Gender) masih kosong, aplikasi menampilkan pesan peringatan dan data tidak disimpan ke database.

### 11. Data Tersimpan di Database

![Data di SQL Server](docs/screenshots/11-database.png)

Hasil query `SELECT * FROM Students` pada SQL Server Object Explorer, membuktikan bahwa data yang ditambah, diubah, dan dihapus dari aplikasi benar-benar tersimpan di tabel `Students` pada database `StudentDB`.

---

## Struktur Proyek

```
StudentManager/
├── Data/
│   └── StudentRepository.cs
├── Models/
│   └── Student.cs
├── ViewModels/
│   ├── RelayCommand.cs
│   └── StudentViewModel.cs
├── App.xaml
├── App.xaml.cs
├── MainWindow.xaml
└── MainWindow.xaml.cs
```

## Database

Database `StudentDB` memiliki satu tabel `Students`.

| Kolom | Tipe | Keterangan |
|---|---|---|
| Id | INT IDENTITY | Primary key |
| NIM | VARCHAR(20) | Wajib diisi |
| Nama | VARCHAR(100) | Wajib diisi |
| Jurusan | VARCHAR(100) | Wajib diisi |
| Gender | VARCHAR(20) | Wajib diisi |
| Email | VARCHAR(100) | Opsional |

## Penjelasan File

1. **`MainWindow.xaml`**
   Membuat tampilan antarmuka (UI) dengan XAML: header, form input, tombol aksi, panel statistik, kolom pencarian, dan DataGrid. Seluruh elemen dihubungkan ke ViewModel menggunakan Binding.

2. **`MainWindow.xaml.cs`**
   File pendamping `MainWindow.xaml`. Pada file ini `DataContext` diatur ke `StudentViewModel` agar View dapat mengakses properti dan command milik ViewModel.

3. **`App.xaml`**
   Konfigurasi dasar aplikasi WPF, termasuk penentuan jendela awal melalui properti `StartupUri`.

4. **`App.xaml.cs`**
   File pendamping `App.xaml` yang mengatur siklus hidup aplikasi, misalnya saat aplikasi dimulai atau ditutup.

5. **`Models/Student.cs`**
   Mendefinisikan class `Student` yang merepresentasikan data mahasiswa (Id, NIM, Nama, Jurusan, Gender, Email). Objek ini membawa data antara ViewModel dan Repository.

6. **`ViewModels/StudentViewModel.cs`**
   Penghubung antara View dan data. Menyimpan state (`Students`, `SelectedStudent`, `SearchText`), menghitung statistik, mendefinisikan command (Simpan, Hapus, Reset, Cari), dan memberi tahu View saat data berubah lewat `INotifyPropertyChanged`.

7. **`ViewModels/RelayCommand.cs`**
   Implementasi `ICommand` sehingga method di ViewModel dapat dipanggil langsung dari tombol di View melalui Command Binding.

8. **`Data/StudentRepository.cs`**
   Mengelola komunikasi dengan SQL Server LocalDB: `GetAll`, `Search`, `Insert`, `Update`, dan `Delete`. Seluruh query memakai parameter untuk mencegah SQL Injection.

## Arsitektur MVVM

```
View (MainWindow.xaml)
   ↓ Data Binding & Command
ViewModel (StudentViewModel)
   ↓
Model (Student)
   ↓
Repository (StudentRepository, ADO.NET)
   ↓
SQL Server LocalDB (StudentDB)
```

- **Model:** mendefinisikan struktur data mahasiswa melalui `Student.cs`.
- **View:** menampilkan antarmuka pengguna melalui `MainWindow.xaml`.
- **ViewModel:** mengatur properti, perintah, statistik, dan interaksi antara View dengan Repository melalui `StudentViewModel.cs`.
- **Repository:** mengelola akses dan operasi data pada database melalui `StudentRepository.cs`.
- **RelayCommand:** menghubungkan aksi pada ViewModel dengan tombol di View.

Dengan arsitektur MVVM, tanggung jawab setiap bagian terpisah sehingga kode lebih terstruktur, mudah dipahami, serta mudah dikembangkan dan dipelihara.

## Kesimpulan

Aplikasi Student Management System berhasil menerapkan fitur input, tampil, ubah, hapus, pencarian, statistik, reset, dan validasi data mahasiswa dengan WPF dan MVVM. Dokumentasi tampilan di atas menunjukkan setiap fitur berjalan dan datanya tersimpan di SQL Server LocalDB.
