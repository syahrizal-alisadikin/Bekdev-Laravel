Halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar menampilkan logs error di laravel.



Laravel secara default menyimpan log aplikasi di file `storage/logs/laravel.log`. Setiap `error` atau `exception` yang terjadi saat aplikasi berjalan akan disimpan di file tersebut.

Kita sering mengalami kesulitan untuk melihat error log aplikasi Laravel ketika aplikasi sudah di *deploy* ke *production server*, baik di *VPS* ataupun di *Shared Hosting*, karena yang tampil biasanya error 500 seperti ini,



![](https://i.imgur.com/qygvVDm.png)





Langsung aja kita mulai , bagaimana menampilkan logs error di laravel.



###  Langkah 1 - Install Framework Laravel ###

Silahkan masuk ke dalam folder dimana teman-teman akan menyimpan projectnya dan jalankan perintah berikut ini di dalam terminal/CMD :



````
 composer create-project --prefer-dist laravel/laravel:^9.1 Laravel-logs
````

> **CATATAN** : `Laravel-logs` adalah nama dari projeck kita.



Perintah di atas akan membuat project Laravel baru dengan versi 9.x Silahkan tunggu proses installasi sampai selesai dan pastikan teman-teman harus terhubung dengan internet, karena semua paket akan diunduh secara online.



### Langkah 2 - Menjalankan Project Laravel ###

Setelah proses installasi selesai, sekarang kita akan belajar untuk menjalankan project Laravel-nya. Silahkan jalankan perintah berikut ini di dalam terminal/CMD :



```
cd Laravel-logs
```

Perintah cd di atas digunakan untuk melakukan navigasi ke dalam sebuah folder, dalam contoh di atas kita akan masuk ke dalam folder Laravel-Bot. Setelah berhasil masuk ke dalam folder project Laravel, sekarang silahkan jalankan perintah berikut ini di dalam terminal/CMD :



```
php artisan serve
```

Jika perintah di atas berhasil dijalankan, maka project Laravel akan dijalankan pada port 8000 di dalam localhost. Kita bisa membukanya di dalam browser dengan [http://localhost:8000](http://localhost:8000) dan jika berhasil, maka hasilnya seperti berikut ini.



![](https://i.imgur.com/8H6v3wP.png)



### Langkah 3 - Install  laravel-log-viewer ###

Silahkan  masuk ke dalam folder project Laravel, sekarang silahkan jalankan perintah berikut ini di dalam terminal/CMD:



```
composer require rap2hpoutre/laravel-log-viewer:2.2.0
```

Silahkan tunggu proses installasi sampai selesai dan pastikan teman-teman harus terhubung dengan internet, karena semua paket akan diunduh secara online.



Jalankan kembali perintah berikut :



``` 
php artisan vendor:publish \
  --provider="Rap2hpoutre\LaravelLogViewer\LaravelLogViewerServiceProvider" \
  --tag=views
```

Setelah berhasil masuk ke ```config/app.php``` cari ``` providers``` masukan kode berikut :



```
Rap2hpoutre\LaravelLogViewer\LaravelLogViewerServiceProvider::class,
```

Atau jika file `config/app.php` di tulis secara lengkap kurang lebih seperti berikut ini :



![](https://i.imgur.com/LYLucbV.png)



sumber [https://github.com/rap2hpoutre/laravel-log-viewer](https://github.com/rap2hpoutre/laravel-log-viewer)



### Langkah 3 - Menambahkan Route ###

Silahkan buka file `routes/web.php` kemudian masukkan kode berikut



```php
//log-viewers
Route::get('log-viewers', [\Rap2hpoutre\LaravelLogViewer\LogViewerController::class, 'index']);
```

Sekarang jika kita coba jalankan projectnya di http://localhost:8000/log-viewers maka kurang lebih hasilnya seperti berikut ini :



![](https://i.imgur.com/t6UO2n7.png)



> **info** : karena project nya masih baru jadi belum ada error hihi

Gimana proses installasi nya ? mudah bukan hehe 



Sampai disini pembahasan bagaimana cara membuat logs viewers di laravel, semoga bermanfaat

Terima Kasih

