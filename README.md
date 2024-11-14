# CRUD Mahasiswa
- Hendra Latieful Maajid
- H1D022018
- Shift Baru F
- Shift KRS D

## 1. Create (Tambah Data)
Sistem mengimplementasikan penambahan mahasiswa melalui komponen berikut:
- Proses dimulai dengan menekan tombol "Tambah Mahasiswa" yang terletak di bagian atas aplikasi
- <img src="1.png" width="300px">
- Setelah tombol ditekan, sistem akan membuka form modal yang berisi:
  - Field input untuk Nama Mahasiswa: Digunakan untuk mengisi nama lengkap mahasiswa
  - Field input untuk Jurusan: Digunakan untuk mengisi jurusan mahasiswa
  - Tombol "Tambah Mahasiswa" untuk mengonfirmasi penambahan data
  - Tombol "Batal" untuk membatalkan proses
- Proses implementasi di backend:
  ```typescript
  tambahMahasiswa() {
    if (this.nama != '' && this.jurusan != '') {
      let data = {
        nama: this.nama,
        jurusan: this.jurusan,
      }
      this.api.tambah(data, 'tambah.php')
  ```
- <img src="2.png" width="300px">
- Sistem melakukan validasi data:
  - Memeriksa apakah field nama sudah diisi
  - Memeriksa apakah field jurusan sudah diisi
  - Jika ada field yang kosong, proses tidak akan dilanjutkan
- Setelah validasi berhasil:
  - Data dikirim ke server menggunakan metode POST
  - Backend (tambah.php) memproses penyimpanan data ke database
  - Sistem menampilkan feedback berhasil
  - List mahasiswa otomatis diperbarui
- <img src="3.png" width="300px">

## 2. Read (Tampil Data)
Sistem mengimplementasikan dua jenis pengambilan data:

### 2.1 Menampilkan Semua Mahasiswa
- Proses tampil data dimulai secara otomatis saat halaman dibuka (`ngOnInit()`)
- Implementasi kode untuk mengambil data:
  ```typescript
  getMahasiswa() {
    this.api.tampil('tampil.php').subscribe({
      next: (res: any) => {
        this.dataMahasiswa = res;
      }
    });
  }
  ```
- Alur proses:
  1. Frontend mengirim request GET ke tampil.php
  2. Backend mengambil semua data dari database
  3. Data dikirim kembali dalam format JSON
  4. Frontend menerima data dan menampilkan dalam bentuk kartu
  5. Setiap kartu menampilkan informasi:
     - Nama mahasiswa
     - Jurusan
     - Tombol Edit dan Hapus
- <img src="3.png" width="300px">

### 2.2 Pengambilan Data Mahasiswa Tunggal
- Proses ini terjadi saat pengguna akan mengedit data mahasiswa
- Implementasi kode pengambilan data spesifik:
  ```typescript
  ambilMahasiswa(id: any) {
    this.api.lihat(id, 'lihat.php?id=').subscribe({
      next: (hasil: any) => {
        let mahasiswa = hasil;
        this.id = mahasiswa.id;
        this.nama = mahasiswa.nama;
        this.jurusan = mahasiswa.jurusan;
      }
    })
  }
  ```
- Alur prosesnya:
  1. Pengguna menekan tombol Edit pada kartu mahasiswa
  2. Sistem mengirim request ke lihat.php dengan parameter ID mahasiswa
  3. Backend mencari data mahasiswa berdasarkan ID
  4. Data dikirim kembali ke frontend
  5. Form edit ditampilkan dengan data yang sudah terisi
- <img src="4.png" width="300px">

## 3. Update (Edit Data)
Operasi update memiliki alur sebagai berikut:
- Dimulai dengan menekan tombol Edit pada kartu mahasiswa
- Sistem membuka modal edit yang berisi:
  - Field nama yang sudah terisi dengan data sebelumnya
  - Field jurusan yang sudah terisi dengan data sebelumnya
  - Tombol "Edit Mahasiswa" untuk menyimpan perubahan
  - Tombol "Batal" untuk membatalkan edit
- Implementasi proses edit:
  ```typescript
  editMahasiswa() {
    let data = {
      id: this.id,
      nama: this.nama,
      jurusan: this.jurusan
    }
    this.api.edit(data, 'edit.php')
  ```
- Alur proses lengkap:
  1. Data dari form dikumpulkan dalam objek
  2. Sistem melakukan validasi input
  3. Data dikirim ke server menggunakan metode PUT
  4. Backend memproses update data di database
  5. Sistem menampilkan feedback berhasil
  6. List mahasiswa otomatis diperbarui
- <img src="4.png" width="300px">
- <img src="5.png" width="300px">

## 4. Delete (Hapus Data)
Operasi hapus memiliki alur dan fitur sebagai berikut:
- Proses dimulai dengan menekan tombol Hapus (merah) pada kartu mahasiswa
- Sistem menampilkan dialog konfirmasi untuk mencegah penghapusan tidak sengaja
- Implementasi proses hapus:
  ```typescript
  hapusMahasiswa(id: any) {
    const alert = await this.alertController.create({
      header: 'Konfirmasi Hapus',
      message: 'Apakah anda yakin ingin menghapus data mahasiswa ini?',
      buttons: [
        {
          text: 'Batal',
          role: 'cancel'
        }, 
        {
          text: 'Hapus',
          role: 'confirm',
          handler: () => {
            this.api.hapus(id, 'hapus.php?id=')
          }
        }
      ]
    });
  }
  ```
- <img src="6.png" width="300px">
- Alur proses lengkap:
  1. Pengguna menekan tombol Hapus
  2. Muncul dialog konfirmasi dengan 2 pilihan:
     - "Batal" untuk membatalkan penghapusan
     - "Hapus" untuk melanjutkan penghapusan
  3. Jika dikonfirmasi:
     - Sistem mengirim request DELETE ke hapus.php
     - Backend menghapus data dari database
     - Frontend memperbarui tampilan list
     - Feedback penghapusan ditampilkan
- <img src="7.png" width="300px">

## Endpoint API dan Komunikasi Server

Sistem menggunakan beberapa endpoint PHP untuk operasi CRUD:
1. CREATE: `tambah.php`
   - Menerima request POST
   - Memproses penambahan data ke database
   - Mengembalikan status sukses/gagal

2. READ: 
   - `tampil.php`: 
     - Menerima request GET
     - Mengambil semua data mahasiswa
     - Mengembalikan array JSON
   - `lihat.php?id=`: 
     - Menerima request GET dengan parameter ID
     - Mengambil data mahasiswa spesifik
     - Mengembalikan objek JSON

3. UPDATE: `edit.php`
   - Menerima request PUT
   - Memperbarui data di database
   - Mengembalikan status sukses/gagal

4. DELETE: `hapus.php?id=`
   - Menerima request DELETE dengan parameter ID
   - Menghapus data dari database
   - Mengembalikan status sukses/gagal




