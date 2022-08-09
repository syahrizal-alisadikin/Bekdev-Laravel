Halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar bagaimana cara menginstall package Laravel Socialite di project kita.



Pada tutorial kali ini kita semua akan belajar menginstall & konfigurasi tentang Laravel Socialite. Langsung saja kita mulai.



### Langkah 1 - Install Laravel Socialite

Silahkan teman-teman install Laravel Socialite dengan perintah dibawah ini :



```
composer require laravel/socialite
```

Silahkan tunggu  proses installasi Laravel Socialite sampai selesai, proses ini teman-teman harus terhubung ke internet.



![](https://i.imgur.com/Sl5X9lA.png)

### Langkah 2 - Konfigurasi

Setelah proses installasi selesai, kita lanjutkan untuk mengatur konfigurasinya. Silahkan teman-teman buka file `config/service.php` dan tambahkan kode berikut ini :



```php
'github' => [
        'client_id' => env('GITHUB_CLIENT_ID'),
        'client_secret' => env('GITHUB_CLIENT_SECRET'),
        'redirect' => env('GITHUB_CLIENT_REDIRECT'),
    ],

'google' => [
        'client_id' => env('GOOGLE_CLIENT_ID'),
        'client_secret' => env('GOOGLE_CLIENT_SECRET'),
        'redirect' => env('GOOGLE_CLIENT_REDIRECT'),
    ],
```

Pada penambahan kode diatas, kita akan menggunakan `2 provider`, yaitu `google` dan `GitHub`. Setelah itu, silahkan teman-teman juga buka file `.env` dan tambahkan kode dibawah ini :



```php
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
GOOGLE_CLIENT_REDIRECT=http://localhost:8000/auth/google/callback

GITHUB_CLIENT_ID=
GITHUB_CLIENT_SECRET=
GITHUB_CLIENT_REDIRECT=http://localhost:8000/auth/github/callback
```

Dari penambahan kode di atas, kita menambahkan masing-masing 3 variabel dari provider, yaitu ada `CLIENT ID`, `SECRET ID `dan `CLIENT_REDIRECT`.

Untuk `CLIENT ID` dan `SECRET ID` kita akan bahas di tutorial yang selanjutnya. karena kita harus mendaftar di masing-masing provider untuk mendapatkan credential tersebut.

Sampai disini pembahasan tentang installasi Laravel Socialite dan Konfigurasinya. di Artikel selanjutnya kita semua akan belajar bagaimana cara membuat OAuth menggunakan Laravel Socialite.

Terima Kasih