halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar bagaimana cara mendapatkan `CLIENT ID` & `SECRET ID` dari Google



`CLIENT ID` & `SECRET ID` ini akan kita gunakan sebagai credential OAuth Laravel Socialite. Setelah sebelumnya kita sudah berhasil membuat dengan GitHub. Langsung saja kita mulai.



### Langkah 1 - Mendapatkan CLIENT ID & SECRET ID Google.



Silahkan teman-teman bisa mengunjungi [Google Developer Console](http://console.developers.google.com/), dan silahkan teman-teman klik `NEW PROJECT`, kurang lebih seperti gambar di bawah ini :



![](https://i.imgur.com/K2ueT1i.png)



Kemudian silahkan teman-teman pilih `` OAuth consent screen`` ,pilih  type `` External `` setelah itu klik `` CREATE``



![](https://i.imgur.com/274F8Rb.png)



Kemudian silahkan teman-teman isikan Nama Aplikasinya, disini contohnya saya membuat dengan nama “**Eccomerce**”.



**App Domain** : [http://localhost:8000/](http://localhost:8000/), 



![](https://i.imgur.com/sc8cP5k.png)



setelah semua di isi sesuai sample scrool ke bawah klik ``SAVE AND CONTINUE``



Kemudian silahkan teman-teman create ``Credentials`` Seperti berikut :



![](https://i.imgur.com/mUUoW8V.png)



Silahkan  teman-teman pilih Application type `` Web application `` dan name nya sesuai kebutuhan, disini contohnya saya membuat dengan name “**Web Eccomerce**”.



**Authorized JavaScript origins** :  [http://localhost:8000/](http://localhost:8000/),

**Authorized redirect URIs** :  [http://localhost:8000/auth/google/callback](http://localhost:8000/auth/google/callback)

![](https://i.imgur.com/zu1cvPA.png)



> Note : Url `` http://localhost:8000/auth/google/callback`` otomatis dijalankan setelah berhasil login ke akun google

Setelah semua data di isi lalu klik ``save`` ya



Maka disini teman-teman bisa mendapatkan `CLIENT ID` & `SECRET ID`.



![](https://i.imgur.com/DL3ABeI.png)



Silahkan teman-teman copy `CLIENT ID` & `SECRET ID` tersebut dan paste di dalam file `.env` di bagian credential Google :



```php
GOOGLE_CLIENT_ID=paste_client_id
GOOGLE_CLIENT_SECRET=paste_client_secret
GOOGLE_CLIENT_REDIRECT=http://localhost:8000/auth/google/callback
```



### Langkah 2 - Uji Coba Login Dengan Google

Silahkan teman-teman restart server projectnya, kemudian jalankan lagi dengan mengetikan perintah :



```
php artisan serve
```

Sekarang teman-teman bisa mencoba aplikasinya dengan Login atau Register dengan akun Google.



Sampai disini pembahasan tentang membuat Login dengan Google di Laravel menggunakan Laravel Socialite.



Jika teman-teman ada pertanyaan, silahkan teman-teman bisa bertanya melalui kolom kementar dibawah, dengan menyertakan `error`, `secreenshot`, `source code`, dll.

Jika ada saran, atau ingin menawarkan kersajasama, silahkan teman-teman bisa mengirimkannya ke 

email : [alisadikinsyahrizal@gmail.com](mailto:alisadikinsyahrizal@gmail.com)

wa :  [089649532860](https://wa.me/089649532860)

telegram : syahrizal_as

Ig : syahrizal_alisadikin



Link source code : [https://github.com/syahrizal-alisadikin/Laravel-9.3-Sociate](https://github.com/syahrizal-alisadikin/Laravel-9.3-Sociate)



Terima Kasih