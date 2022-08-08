Halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar bagaimana cara membuat proses OAuth Login mengggunakan Sociate di Laravel. 



Laravel merupakan framework dengan fitur terlengkap. Salah satunya adalah adanya fitur Laravel Sociate yang dapat memudahkan Kita login dengan akun sosial media seperti Facebook, Twitter, Github, Google. Tapi, bagaimana cara login dengan akun social media tersebut di Laravel ya?



Pada kesempatan pertama ini kita semua akan belajar bagaimana cara installasi Framework Laravelnya terlebih dahulu, langsung saja kita mulai.

<h3>Langkah 1 - Install Framework Laravel</h3>

Silahkan masuk ke dalam folder dimana teman-teman akan menyimpan projectnya dan jalankan perintah berikut ini di dalam terminal/CMD :



``` 
composer create-project --prefer-dist laravel/laravel:^9.3 laravel-sociate
```

> **CATATAN** : `Laravel-sociate` adalah nama dari projeck kita.

Perintah di atas akan membuat project Laravel baru dengan versi 9.x Silahkan tunggu proses installasi sampai selesai dan pastikan teman-teman harus terhubung dengan internet, karena semua paket akan diunduh secara online.



![](https://i.imgur.com/LhOZV4X.png)



### Langkah 2 - Menjalankan Project Laravel

Setelah proses installasi selesai, sekarang kita akan belajar untuk menjalankan project Laravel-nya. Silahkan jalankan perintah berikut ini di dalam terminal/CMD :



```bash
cd Laravel-sociate
```

Perintah cd di atas digunakan untuk melakukan navigasi ke dalam sebuah folder, dalam contoh di atas kita akan masuk ke dalam folder Laravel-Bot. Setelah berhasil masuk ke dalam folder project Laravel, sekarang silahkan jalankan perintah berikut ini di dalam terminal/CMD :



```php
php artisan serve
```

Jika perintah di atas berhasil dijalankan, maka project Laravel akan dijalankan pada port 8000 di dalam localhost. Kita bisa membukanya di dalam browser dengan [http://localhost:8000](http://localhost:8000/) dan jika berhasil, maka hasilnya seperti berikut ini.



![](https://imgur.com/8H6v3wP.png)



### Langkah 3 - Koneksi Database

Pertama, kita akan lakukan konfigurasi untuk koneksi database-nya terlebih dahulu. Silahkan buka project Laravel-nya menggunakan Text Editor dan cari file `.env` dan kode berikut ini.



``` 
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

Kemudian, silahkan ubah menjadi seperti berikut ini.



``` 
DB_DATABASE=laravel_sociate
DB_USERNAME=root
DB_PASSWORD=
```

Dari perubahan kode di atas, kita melakukan konfigurasi nama dabatase yang akan kita gunakan nanti, yaitu `DB_DATABASE` kita berikan value `laravel_sociate`.

Dan untuk `DB_PASSWORD`, silahkan disesuaikan dengan konfigurasi dari MySQL masing-masing, jika menggunakan **XAMPP**, maka secara default kita tidak perlu mengisinya atau dibiarkan kosong.



