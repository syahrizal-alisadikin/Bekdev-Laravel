Halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar bagaimana cara membuat Model & Migration table untuk dijadikan sebagai OAuth Sosial Media dari aplikasi kita.



Jadi nanti kita akan melakukan sedikit konfigurasi dari file migration User bawaan Laravel dan kita juga akan membuat baru untuk menyimpan data dari Sosial Media menggunakan One to Many.



Jadi nanti 1 User bisa memiliki lebih dari 1 Sosial Media, dan tentunya kita akan membuat sebuah model dan sekaligus migration baru. Langsung saja kita mulai.



### Langkah 1 - Edit File Migration User

Secara default laravel sudah menyediakan file Model & Migration untuk User, nah ini akan sangat membantu kita sekali dalam pembuatan sebuah aplikasi.



Silahkan teman-teman buka file `database/migrations/2014_10_12_000000_create_users_table.php` silahkan ubah pada `function up` menjadi seperti berikut ini :



``` php
public function up()
{
  Schema::create('users', function (Blueprint $table) {
      $table->id();
      $table->string('name');
      $table->string('email')->unique()->nullable();
      $table->timestamp('email_verified_at')->nullable();
      $table->string('password')->nullable();
      $table->rememberToken();
      $table->timestamps();
   });
}
```

Bisa kita lihat, pada perubahan di atas, kita hanya menambahkan function `nullable()` pada coloumn `email` dan `password`.



Karena pada dasarnya tidak semua Sosial Media mengizinkan aplikasi kita mengakses kredensial emailnya.



### Langkah 2 - Membuat Model & Migration SocialAccount

Pada tahap kali ini, kita semua akan membuat sebuah Model berserta Migration untuk menyimpan data provider dari Sisial Media yang akan User gunakan untuk melakukan OAuth.

Silahkan teman-teman jalankan perintah dibawah ini :



```bash
php artisan make:model SocialAccount -m
```

Dari perintah di atas, kita akan mendapatkan `2 file` baru, yaitu:

• App/Models/SocialAccount.php

• database/migrations/2022_08_03_020214_create_social_accounts_table.php



Silahkan teman-teman buka file migrationnya `2022_08_03_020214_create_social_accounts_table.php`

dan ubahlah pada `function up` menjadi seperti berikut ini :



```php
public function up()
{
  Schema::create('social_accounts', function (Blueprint $table) {
    $table->id();
    $table->bigInteger('user_id');
    $table->string('provider_id')->unique();
    $table->string('provider_name');
    $table->timestamps();
  });
}
```



### Langkah 3 - Menjalankan Migration

Setelah kita berhasil mengubah beberapa file migration User dan kita juga sudah berhasil membuat migration baru untuk menyimpan data Sosial Medinya, langkah selanjutnya kita akan menjalankan proses migration.



Silahkan teman-teman jalankan perintah dibawah ini :



```bash
php artisan migrate
```

![](https://i.imgur.com/OnvTMxx.png)



Jika berhasil, teman-teman bisa melihat kita sudah dibuatkan tabelnya di dalam database, kurang lebih seperti berikut ini :



![](https://i.imgur.com/jLSgTp0.png)



Sampai disini pembahasan tentang mengubah dan membuat Migration baru, di artikel selanjutnya kita semua akan belajar bagaimana cara mengkonfigurasi Relationships One to Many dari tabel / Model Users ke SocialAccount.

Terima Kasih