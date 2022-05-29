

<h3>Bagaimana Mengatasi Error Jika Menggunakan SMTP Gmail di Laravel ?</h3>

Halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar bagaimana cara Mengatasi Error  Menggunakan SMTP Gmail di Laravel ,

Error yang paling sering terjadi disebabkan oleh ketidak telitian pada tahap konfigurasi file **.env**, terutama jika Anda menggunakan SMTP gmail.



```
MAIL_DRIVER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=emailkita@gmail.com
MAIL_PASSWORD=passwordkita
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=emailkita@gmail.com
MAIL_FROM_NAME=”${APP_NAME}”
```

kode di atas, kita melakukan konfigurasi akun gmail yang akan kita gunakan nanti, yaitu menggunakan `smtp.gmail.com` port `587`.



![](https://i.imgur.com/kjxl0lV.png)



Jika Kita tidak dapat terhubung dengan akun Gmail dan mendapatkan error seperti gambar diatas, Kita dapat mengatasinya dengan menggunakan fitur **Sandi Aplikasi** pada akun Google.



Sandi Aplikasi akan membuatkan **16 karakter** password yang dapat Anda gunakan pada SMTP. Hal ini tentu akan lebih aman, karena password akan berbeda dengan password login akun google Anda.

Untuk mengaktifkannya, ikuti lima langkah berikut :



* **Pertama**, masuk ke pengaturan akun Google , kemudian klik **Keamanan**. Pilih menu **Aktif Verifikasi 2 Langkah**, maka akan muncul satu kolom lagi dibawahnya, yaitu **Sandi Aplikasi**.



![](https://i.imgur.com/n4ehQfy.png)

* **Kedua**, klik **Sandi Aplikasi** dan Anda akan diarahkan untuk memasukkan password akun gmail Anda. Jika sudah, pilih **Email** pada pilih Aplikasi dan **Lainnya** untuk pilih perangkat.

![](https://i.imgur.com/6IcvKef.png)



* **Ketiga**, tulis nama aplikasi pada kolom yang tersedia sesuai dengan keinginan Anda, kemudian klik **BUAT**.

![](https://i.imgur.com/s8FhZ2v.png)



* **Keempat**, saat muncul tampilan bahwa password sudah di-*buat*, Anda dapat memasukkan password ini ke bagian **MAIL_PASSWORD** di file .env

  

![](https://i.imgur.com/ioVqto1.png)



![Imgur](https://i.imgur.com/o9COtjp.png)

* **Kelima,** buka terminal, dan jalankan perintah `php artisan config:cache`. Seharusnya, email sudah dapat terkirim ke alamat penerima dengan baik.

Sampai disini pembahasan bagaimana cara Mengatasi Error Jika Menggunakan SMTP Gmail di Laravel,  semoga bermanfaat.



Terima Kasih