Halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar Queue Laravel,

Sebelum Mengikuti Tutorial ini, disarankan mengikuti Tutorial 

* https://santrikoding.com/tutorial-kirim-email-di-laravel-menggunakan-gmail
* https://santrikoding.com/tips-mengatasi-error-mengirim-email-menggunakan-smtp-gmail-di-laravel 



Oke tanpa lama-lama kita lanjut aja😁

**Apa itu Queue ?** Yap betul, antrian. Namun pada konteks sebuah aplikasi web biasa dikenal dengan *Task Queue.* Laravel Queue biasanya digunakan untuk memecah beban kerja pada suatu page atau API Endpoint agar response time menjadi lebih cepat. Contoh beban kerja  pada website yang biasanya dipecah dan akan dijalankan oleh Queue adalah :

• Mengirim email notifikasi (contoh : pada resgister user atau pemberitahuan waktu expired subscription)

• Export database dari file csv

• menggunakan **service** dari **third-party** (contoh : mengirim real-time notification)

• dll.



Pada dasarnya Laravel Queue adalah sistem yang menjalankan *Task Queue* ( antrian tugas) dengan beban kerja besar tapi dijalankan oleh *worker* dari background server. Job pada Laravel sendiri dapat diartikan sebagai tugas yang akan dimasukan kedalam antrian tertentu untuk dikerjakan oleh worker. Pada Laravel sendiri ada beberapa drivers yang didukung untuk memproses Task Queue yaitu database, [Amazon SQS](https://aws.amazon.com/sqs/), [Redis](https://redis.io/), dan [Beanstalkd](https://beanstalkd.github.io/).



Sumber : https://techblog.revivaltv.id/laravel-queue/amp/

<h3> Langkah 1 - Konfigurasi .env</h3>

Silahkan buka project Laravel-nya menggunakan Text Editor dan cari file  `.env` dan kode berikut ini.



``` 
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=

QUEUE_CONNECTION=sync
```

Kemudian, silahkan ubah menjadi seperti berikut ini.



``` 
DB_DATABASE=db_laravel_queue
DB_USERNAME=root
DB_PASSWORD=

QUEUE_CONNECTION=database
```

Dari perubahan kode di atas, kita melakukan konfigurasi nama dabatase yang akan kita gunakan nanti, yaitu DB_DATABASE kita berikan value ```db_laravel_queue```.

Dan untuk DB_PASSWORD, silahkan disesuaikan dengan konfigurasi dari MySQL masing-masing, jika menggunakan XAMPP, maka secara default kita tidak perlu mengisinya atau dibiarkan kosong,

Kemudian QUEUE_CONNECTION menjadi `database`.



<h3> Langkah 2 - Membuat Migration Queue </h3>

Sekarang kita akan belajar membuat migration di laravel

Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam project Laravel-nya.



```php
php artisan queue:table
```

Setelah itu jalankan perintah berikut :

 

```php+HTML
php artisan migrate
```

<h3>Langkah 3 - Membuat Job Class </h3>	

Sekarang kita akan belajar membuat Job Class untuk pengiriman email

Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam project Laravel-nya.



``` 
php artisan make:job SendMailJob
```

setelah berhasil menjalankan perintah tersebut kita akan mendapatkan sebuah file baru pada project laravel kita, seperti gambar dibawah :



![](https://i.imgur.com/IqRiBha.png)



Silahkan buka file SendEmailJob.php dan ubah kode-nya menjadi seperti berikut ini.



```php
<?php

namespace App\Jobs;

use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldBeUnique;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Queue\SerializesModels;
use Illuminate\Support\Facades\Mail;
use App\Mail\SendEmail;

class SendMailJob implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;
    public $data;
    /**
     * Create a new job instance.
     *
     * @return void
     */
    public function __construct($data)
    {
        $this->data = $data;
    }

    /**
     * Execute the job.
     *
     * @return void
     */
    public function handle()
    {
        $email = new SendEmail($this->data);
        Mail::to($this->data['email'])->send($email);
    }
}
```

Dari penambahan kode di atas, pertama-tama kita import Facades Mail dan memanggil  Class Mailable SendMail.



```
use Illuminate\Support\Facades\Mail;
use App\Mail\SendEmail;
```

Ada 2 method penting dalam class ini, yaitu method **__construct** dan **handle**: 

- Method function **__construct()** digunakan untuk menginisialisasi objek yang digunakan pada template email. 
- Method function **handle()** adalah tempat untuk menjalankan proses Queue/Job.

<h3>Langkah 4 - Membuat Controller SendEmail</h3>

Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam *project* Laravel-nya.



``` php
php artisan make:controller SendEmailController
```

Jika perintah di atas berhasil dijalankan, maka kita akan mendpatkan 1 *file* *controller* baru dengan nama `SendEmailController` yang berada di dalam *folder* `app/Http/Controllers`. Silahkan buka *file* tersebut dan ubah kode-nya menjadi seperti berikut ini :



```php
<?php

namespace App\Http\Controllers;

use App\Jobs\SendMailJob;
use Illuminate\Http\Request;

class SendEmailController extends Controller
{
    public function index()
    {
        return view('kirim-email');
    }

    public function store(Request $request)
    {
        $data = $request->all();

        dispatch(new SendMailJob($data));
        return redirect()->route('kirim-email')->with('status', 'Email berhasil dikirim');
    }
}
```

Dari penambahan kode di atas, pertama-tama kita import Job Class.



```
use App\Jobs\SendMailJob;
```

kemudian membuat 2 function :

* Index : untuk menampilkan form kirim email
* store : untuk menjalankan proses queue/job

<h3> Langkah 5 - Membuat Route </h3>

sekarang kita lanjutkan untuk membuat route-nya. Silahkan buka file routes/web.php, kemudian ubah semua kode-nya menjadi seperti berikut ini.



```php
<?php

use App\Http\Controllers\SendEmailController;
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

Route::get('/send-email', [SendEmailController::class, 'index'])->name('kirim-email');

Route::post('/post-email', [SendEmailController::class, 'store'])->name('post-email');
```

Dari penambahan kode di atas, pertama-tama kita import Controller SendEmailController. 



```php
use App\Http\Controllers\SendEmailController;
```

untuk *route* *send-email* sendiri kita menggunakan *method* `GET` dan untuk *name* dari route ini kita berikan nama `kirim-email`.



```php
Route::get('/send-email', [SendEmailController::class, 'index'])->name('kirim-email');
```

Kemudian untuk *post-email* menggunakan menggunakan method ``post`` dan untuk *name* dari route ini kita berikan nama ```post-email```



```php
Route::post('/post-email', [SendEmailController::class, 'store'])->name('post-email');
```



<h3> Langkah 6 - Membuat View</h3>

Sekarang silahkan buat file baru dengan nama kirim-email.blade.php, kemudian masukkan kode berikut ini di dalamnya.



```php+HTML
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">

    <title>Tutorial Queue Laravel!</title>
  </head>
  <body>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <div class="container">
          <a class="navbar-brand" href="#">santrikoding.com</a>
          <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
          </button>
          {{-- <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
            <div class="navbar-nav">
              <a class="nav-link active" aria-current="page" href="#">Home</a>
              <a class="nav-link" href="#">Features</a>
              <a class="nav-link" href="#">Pricing</a>
              <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
            </div>
          </div> --}}
        </div>
      </nav>
    <div class="row justify-content-center">
            <h3 class=" text-center my-2">Tutorial Queue Laravel di Santri Koding</h3>
            <div class="col-md-4 p-4">
                {{-- send email --}}
                @if (session('status'))
                    <div class="alert alert-primary" role="alert">
                    {{ session('status') }}
                    </div>
                    
                @endif
                <form action="{{ route('post-email') }}" method="post">
                    @csrf
                    <div class="form-group">
                        <label for="name">Nama</label>
                        <input type="text" class="form-control" name="name" id="name" placeholder="Nama">
                    </div>
                    <div class="form-group my-3">
                        <label for="email">Email Tujuan</label>
                        <input type="email" class="form-control" name="email" id="email" placeholder="Email Tujuan">
                    </div>
                    <div class="form-group my-3">
                        <label for="name">Body Deskripsi</label>
                        <textarea name="body" class="form-control" id="" cols="30" rows="10"></textarea>
                    </div>
                    <div class="form-group">
                        <button class="btn btn-primary">Kirim Email</button>
                    </div>
            </div>
    </div>

    <!-- Optional JavaScript; choose one of the two! -->

    <!-- Option 1: Bootstrap Bundle with Popper -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous"></script>

    <!-- Option 2: Separate Popper and Bootstrap JS -->
    <!--
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.2/dist/umd/popper.min.js" integrity="sha384-IQsoLXl5PILFhosVNubq5LC7Qb9DXgDA9i+tQ8Zj3iwWAwPtgFTxbJ8NT4GN1R8p" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.min.js" integrity="sha384-cVKIPhGWiC2Al4u+LWgxfKTRIcfu0JTxR+EQDz/bgldoEyl4H0zUF0QKbrJ0EcQF" crossorigin="anonymous"></script>
    -->
  </body>
</html>
```

Dari penambahan kode di atas, kita menggunakan Bootstrap 5.



```php+HTML
  <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
     <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous"></script>
```

<h3>Langkah 7 - Uji Coba Halaman Kirim Email</h3>

Sekarang silahkan reload halaman http://localhost:8000/send-email. Dan jika berhasil maka akan menampilkan hasil seperti berikut ini.



![](https://i.imgur.com/l3kEL0z.png)



kita bisa isi data terlebih dahulu kemudian klik tombol kirim email, 

jika berhasil tampilan nya seperti berikut :



![](https://i.imgur.com/bHNV7ng.png)



Kita juga dapat langsung melakukan pengecekan pada alamat email tujuan 



![](https://i.imgur.com/Jsck0J3.png)



jika kita lihat proses queue nya seperti berikut :



![](https://i.imgur.com/lgIsm5o.png)



Sampai disini pembahasan belajar Queue Laravel,  semoga bermanfaat.



terimakasih