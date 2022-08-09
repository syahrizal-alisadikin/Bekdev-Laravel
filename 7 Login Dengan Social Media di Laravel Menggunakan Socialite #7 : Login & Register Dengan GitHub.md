Halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar bagaimana cara mendapatkan `CLIENT ID` & `SECRET ID` dari GitHub.



`CLIENT ID` & `SECRET ID` ini akan kita gunakan sebagai OAuth di project Laravel kita yang menggunakan Socialite. Langsung saja kita mulai.



### Langkah 1 - Mendapatkan CLIENT ID & SECRET ID GitHub

Silahkan teman-teman Daftar / Login di [GitHub](https://github.com/), setelah berhasil login, silahkan teman-teman klik menu setting seperti gambar di bawah ini :



![](https://i.imgur.com/aaqLeiP.png)



Selanjutnya teman-teman klik menu **Developer Settings > OAuth Apps > Create OAuth App**, kurang lebih seperti berikut ini :



![](https://i.imgur.com/peZ3Q0e.png)



![](https://i.imgur.com/a8BspOH.png)



![](https://i.imgur.com/oxl9hBT.png)



Kemudian silahkan teman-teman isi semua form yang dibutuhkan, setelah itu silahkan teman-teman simpan, maka teman-teman akan mendpatkan `CLIENT ID` & `SECRET ID`.



### Langkah 2 - Konfigurasi Credential GitHub

Silahkan copy `CLIENT ID` dan `SECRET ID` teman-teman dan pastekan di variabel di dalam file `.env`, di bagian konfigurasi GitHub



```php
GITHUB_CLIENT_ID=paste_client_id
GITHUB_CLIENT_SECRET=paste_secret_id
GITHUB_CLIENT_REDIRECT=http://localhost:8000/auth/github/callback
```

### Langkah 3 - Uji Coba Login Dengan GitHub

Sekarang Silahkan teman-teman jalankan projectnya dengan perintah dibawah ini :



```
php artisan serve
```

Sekarang teman-teman bisa mencoba aplikasiya dengan Login atau Register dengan akun GitHub.



Sampai disini pembahasan tentang mendapatkan `CLIENT ID` dan `SECRET ID` dari GitHub dan cara menggunakan OAuth Login dengan GitHub di Laravel.



Di artikel selanjutnya kita semua akan belajar denganOAuth Login menggunakan Google.

Terima Kasih