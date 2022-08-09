Halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar bagaimana cara membuat Auth di Laravel.



Auth secara bawaan memang sudah disediakan oleh Laravel, kita bisa mengaktifkan Auth ini dengan beberapa langkah saja.



Untuk mempermudah dalam membuat aplikasi baru dengan laravel, kita dapat menggunakan. starter kit ini secara otomatis menyediakan routes, controllers dan views untuk autentikasi (login & register).

Ada 3 stater kit yang bisa kita pilih



* **Laravel Breeze**, terbuat dari template blade sederhan dengan style dari Tailwind CSS. 

* **Laravel Jeatsream**, Jetstream menambah fungsionalitas tersebut dengan fitur yang lebih kuat dan stak teknologi frontend tambahan

* **Laravel Fortify**, Laravel Fortify adalah backend otentikasi tanpa kepala untuk Laravel yang mengimplementasikan banyak fitur yang ditemukan dalam dokumentasi ini,

  

### Langkah 1 - Make Auth Laravel

langsung saja kita mulai untuk mengaktifkan Auth  menggunkan Laravel Breeze . silahkan teman-teman jalankan perintah dibawah ini :



```cmd
composer require laravel/breeze --dev
```

Silahkan tunggu proses installasi dengan composer sampai selesai, pastikan teman-teman harus terhubung dengan internet.



![](https://i.imgur.com/rXvCC7I.png)



Setelah proses installasi selesai, silahkan teman-teman jalankan perintah di bawah ini :



```
php artisan breeze:install
```

```
npm install
```

```
npm run dev
```

> Note : Laravel 9.2 ke atas udah menggunakan vite bisa menggunakan ``npm run build ``

Silahkan di tunggu sampai proses build atau compile dengan `Laravel Vite` selesai seperti berikut.



![](https://i.imgur.com/8ylpicr.png)



### Langkah 2 - Uji Coba Auth Laravel

Silahkan teman-teman jalankan perintah dibawah ini :



```
php artisan serve
```

Maka teman-teman akan melihat ada `2 link` yaitu `Login` dan `Register`, atau kurang lebih seperti gambar di bawah ini :



![](https://i.imgur.com/xENiaSI.png)



![](https://i.imgur.com/7ZY7MNC.png)



![](https://i.imgur.com/nWRqF42.png)



Sampai disini teman-teman sudah berhasil membuat Auth  dengan Laravel, akan tetapi disini kita belum menerapkan Laravel Socialitenya, di artikel selanjutnya kita semua akan belajar bagaimana cara Install Package Laravel Socialite.

Terima Kasih