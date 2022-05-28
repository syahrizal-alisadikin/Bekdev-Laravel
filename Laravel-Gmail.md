Halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar bagaimana cara mengirim email di Laravel,

Laravel merupakan framework dengan fitur terlengkap. Salah satunya adalah adanya fitur Laravel Mail yang dapat memudahkan Anda mengirim email langsung dari website. Tapi, bagaimana cara kirim email dengan Laravel ya?

Pada kesempatan pertama ini kita semua akan belajar bagaimana cara installasi Framework Laravelnya terlebih dahulu, langsung saja kita mulai.

<h3>Langkah 1 - Install Framework Laravel</h3>
Silahkan masuk ke dalam folder dimana teman-teman akan menyimpan projectnya dan jalankan perintah berikut ini di dalam terminal/CMD :

```
composer create-project --prefer-dist laravel/laravel:^9.1 laravel-mail
```

> **CATATAN** : `laravel-mail` adalah nama dari projeck kita.

Perintah di atas akan membuat project Laravel baru dengan versi 9.x Silahkan tunggu proses installasi sampai selesai dan pastikan teman-teman harus terhubung dengan internet, karena semua paket akan diunduh secara online.

<h3>Langkah 2 - Menjalankan Project Laravel</h3>

Setelah proses installasi selesai, sekarang kita akan belajar untuk menjalankan project Laravel-nya. Silahkan jalankan perintah berikut ini di dalam terminal/CMD :

```
cd laravel-mail
```

Perintah cd di atas digunakan untuk melakukan navigasi ke dalam sebuah folder, dalam contoh di atas kita akan masuk ke dalam folder laravel-mail. Setelah berhasil masuk ke dalam folder project Laravel, sekarang silahkan jalankan perintah berikut ini di dalam terminal/CMD :

```
php artisan serve
```

Jika perintah di atas berhasil dijalankan, maka project Laravel akan dijalankan pada port 8000 di dalam localhost. Kita bisa membukanya di dalam browser dengan http://localhost:8000 dan jika berhasil, maka hasilnya seperti berikut ini.

![](https://i.imgur.com/8H6v3wP.png)

<h3>Langkah 3 - Konfigurasi .env</h3
Tahap konfigurasi adalah tahap yang paling penting dalam tutorial cara kirim email dengan Laravel. Kemungkinan besar error yang akan kita alami berasal dari tahap ini. Jadi, simak dengan teliti, ya :smile:

Silahkan buka project Laravel-nya menggunakan Text Editor dan cari file  `.env` dan kode berikut ini.

```
MAIL_MAILER=smtp
MAIL_HOST=mailhog
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"
```

Kemudian, silahkan ubah menjadi seperti berikut ini.



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

Dari perubahan kode di atas, kita melakukan konfigurasi akun gmail yang akan kita gunakan nanti, yaitu menggunakan `smtp.gmail.com` port `587`.

> Catatan : `port` bisa menggunakan `587` atau `465`

Pastikan untuk mengaktifkan pengaturan **Google** **Keamanan** pada akun Gmail Kita. Caranya, masuk ke halaman **Kelola Akun Google Anda**, lalu klik **Keamanan**. Scroll ke bawah sampai Anda menemukan bagian **Akses aplikasi yang kurang aman**, set menjadi **Nonaktif** seperti gambar di bawah ini.

![](https://i.imgur.com/Wavx3sK.png)





<h3>Langkah 4 - Membuat Mail Class</h3>

Sekarang kita akan belajar membuat Mail Class untuk pengiriman email

Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam project Laravel-nya.



```
php artisan make:mail SendEmail
```

setelah berhasil menjalankan perintah tersebut kita akan mendapatkan sebuah file baru pada project laravel kita, seperti gambar dibawah :

![](https://i.imgur.com/XmN4M6s.png)





Silahkan buka file SendEmail.php dan ubah kode-nya menjadi seperti berikut ini.



```
<?php

namespace App\Mail;

use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;

class SendEmail extends Mailable
{
    use Queueable, SerializesModels;
    public $data;
    /**
     * Create a new message instance.
     *
     * @return void
     */
    public function __construct($data)
    {
        $this->data = $data;
    }

    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
        return $this->subject('Testing Kirim Email')
                    ->view('emails.sendemail');
    }
}

```

Ada 2 method penting dalam class ini, yaitu method **__construct** dan **build**: 

- Method function **__construct()** digunakan untuk menginisialisasi objek yang digunakan pada template email. 
- Method function **build()** digunakan untuk mengatur struktur email yang lebih spesifik seperti melakukan konfigurasi pengirim email, menampilkan template email dan menambahkan attachment.

<h3>Langkah 5 - Membuat Blade View </h3>

Setelah berhasil membuat Mail Class, sekarang kita lanjutkan Anda harus membuat tampilan email yang akan dikirimkan atau disebut dengan blade view. Silahkan buat *folder* baru dengan nama `emails` di dalam *folder* `resources/views`, kemudian di dalam *folder* `emails` silahkan buat *file* baru dengan nama `sendemail.blade.php` dan masukkan kode berikut ini :



```
<!DOCTYPE html>
<html>
<head>
    <title>santrikoding.com</title>
</head>
<body>
    <h3>{{ $data['name'] }}</h3>
    <h4>{{ $data['body'] }}</h4>
   
    <p>Terimakasih</p>
</body>
</html>
```



<h3> Langkah 6 - Membuat Route </h3>

sekarang kita lanjutkan untuk membuat route-nya. Silahkan buka file routes/web.php, kemudian ubah semua kode-nya menjadi seperti berikut ini.



```
<?php
use Illuminate\Support\Facades\Mail;
use App\Mail\SendEmail;
use Illuminate\Support\Facades\Route;

/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/

Route::get('/', function () {
    return view('welcome');
});

Route::get('/send-email',function(){
    $data = [
        'name' => 'Syahrizal As',
        'body' => 'Testing Kirim Email di Santri Koding'
    ];
   
    Mail::to('alisadikinsyahrizal@gmail.com')->send(new SendEmail($data));
   
    dd("Email Berhasil dikirim.");
});


```

Dari penambahan kode di atas, pertama-tama kita import Facades Mail dan memanggil  Class Mailable SendMail.



```
use Illuminate\Support\Facades\Mail;
use App\Mail\SendEmail;
```

diatas kita akan mengirim data ke dalam Mailable SendMail.



```
$data = [
        'name' => 'Syahrizal As',
        'body' => 'Testing Kirim Email di Santri Koding'
    ];
```

Email di atas akan di kirim ke `alisadikinsyahrizal@gmail.com` sebagai contoh

<h3> Langka 7 - Mengirim Email </h3>

Sekarang silahkan akses halaman http://localhost:8000/send-email. Dan jika berhasil maka akan menampilkan hasil seperti berikut ini.



![](https://i.imgur.com/3Ym67vh.png)



Kita juga dapat langsung melakukan pengecekan pada alamat email penerima.



![](https://i.imgur.com/ndc5MNU.png)



<h3>Bagaimana Mengatasi Error Jika Menggunakan SMTP Gmail?</h3>

Error yang paling sering terjadi disebabkan oleh ketidak telitian pada tahap konfigurasi file **.env**, terutama jika Anda menggunakan SMTP gmail.



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



* **Kelima,** buka terminal, dan jalankan perintah `php artisan config:cache`. Seharusnya, email sudah dapat terkirim ke alamat penerima dengan baik.

Sampai disini pembahasan bagaimana cara mengirim email di laravel,  semoga bermanfaat.



Terima Kasih
