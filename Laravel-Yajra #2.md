Halo teman-teman semuanya, setelah sebelumnya kita mengenal apa itu Yajra/laravel-datatables-oracle, maka sekarang kita akan mencoba belajar untuk implementasi di dalam project Laravel. Sebelum kita memulai membuat project baru, pastikan teman-teman sudah melakukan installasi Composer di dalam komputer.

Composer merupakan package manager untuk bahasa pemrogramman PHP, dimana kita bisa melakukan installasi dan update sebuah package / library dengan lebih mudah. Untuk Installasi Composer teman-teman bisa mengunjungi di situs resminya di : https://getcomposer.org/ 
* Installasi untuk Linux/Unix/MacOS : https://getcomposer.org/doc/00-intro.md#installation-linux-unix-macos
* Installasi untuk Windows : https://getcomposer.org/doc/00-intro.md#installation-windows

Untuk mengetahui apakah Composer sudah berhasil terinstall, kita bisa menjalankan perintah di bawah ini di dalam terminal atau CMD :
```
composer
```
![](https://i.imgur.com/53nJtIx.png)

<h3>Langkah 1 - Membuat Project Laravel</h3>
Karena Composer sudah berhasil terinstall, maka langkah selanjutnya kita akan belajar bagaimana cara membuat project Laravel. Silahkan masuk ke dalam folder dimana teman-teman akan menyimpan projectnya dan jalankan perintah berikut ini di dalam terminal/CMD :

```
composer create-project --prefer-dist laravel/llaravel:^9.1 laravel9-yajra
```
![](https://i.imgur.com/z0kXYoV.png)

Perintah di atas akan membuat project Laravel baru dengan versi 9.x dan untuk nama project-nya adalah laravel-yajra. Silahkan tunggu proses installasi sampai selesai dan pastikan teman-teman harus terhubung dengan internet, karena semua paket akan diunduh secara online.

<h3>Langkah 2 - Menjalankan Project Laravel</h3>

Setelah proses installasi selesai, sekarang kita akan belajar untuk menjalankan project Laravel-nya. Silahkan jalankan perintah berikut ini di dalam terminal/CMD :

```
cd laravel-yajra
```

Perintah cd di atas digunakan untuk melakukan navigasi ke dalam sebuah folder, dalam contoh di atas kita akan masuk ke dalam folder laravel-yajra. Setelah berhasil masuk ke dalam folder project Laravel, sekarang silahkan jalankan perintah berikut ini di dalam terminal/CMD :

```
php artisan serve
```

Jika perintah di atas berhasil dijalankan, maka project Laravel akan dijalankan pada port 8000 di dalam localhost. Kita bisa membukanya di dalam browser dengan http://localhost:8000 dan jika berhasil, maka hasilnya seperti berikut ini.

![](https://i.imgur.com/8H6v3wP.png)

<h3>Langkah 3 - Koneksi Database</h3>

Pertama, kita akan lakukan konfigurasi untuk koneksi database-nya terlebih dahulu. Silahkan buka project Laravel-nya menggunakan Text Editor dan cari file .env dan kode berikut ini.


```
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

Kemudian, silahkan ubah menjadi seperti berikut ini.

```
DB_DATABASE=db_laravel_yajra
DB_USERNAME=root
DB_PASSWORD=
```

Dari perubahan kode di atas, kita melakukan konfigurasi nama dabatase yang akan kita gunakan nanti, yaitu DB_DATABASE kita berikan value db_laravel_yajra.

Dan untuk DB_PASSWORD, silahkan disesuaikan dengan konfigurasi dari MySQL masing-masing, jika menggunakan XAMPP, maka secara default kita tidak perlu mengisinya atau dibiarkan kosong.

<h3>Langkah 4 - Membuat Database</h3>

Setelah berhasil melakukan konfigurasi database di dalam Laravel, sekarang kita akan lanjutkan untuk membuat database-nya di dalam MySQL.

Silahkan buka http://localhost/phpmyadmin, kemudian buat database baru dengan nama db_laravel_yajra. Kurang lebih seperti berikut ini.  

![](https://i.imgur.com/mAgqRph.png)  

<h3>Langkah 5 - Menjalankan Migration</h3>

Secara default Laravel sudah memberikan kita sebuah migration yang digunakan untuk melakukan generate table users, disini kita tidak perlu membuat sebuah migration lagi dan kita tinggal menjalankan perintah migrate, maka kita akan mendapatkan beberapa table.

Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam project Laravel.

```
php artisan migrate
```
![](https://i.imgur.com/iVketEp.png) 
Jika perintah di atas berhasil dijalankan, maka kita akan mendapatkan beberapa table di dalam database. Kurang lebih seperti berikut ini :

![](https://i.imgur.com/6NckxPd.png)  
![](https://i.imgur.com/6NckxPd.png)  

Sampai disini pembahasan bagaimana cara membuat project baru di Laravel. Di artikel selanjutnya kita semua akan belajar bagaimana cara installasi dan konfigurasi Yajra/laravel-datatables-oracle di dalam Laravel.

Terima Kasih
