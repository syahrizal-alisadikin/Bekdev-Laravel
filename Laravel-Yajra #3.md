Halo teman-teman semuanya, pada kesemp[atan kali ini kita semua akan belajar bagaimana cara menginstall package Laravel Yajra/laravel-datatables-oracle di project kita.

Pada tutorial kali ini kita semua akan belajar menginstall & konfigurasi tentang Laravel Yajra/laravel-datatables-oracle. Langsung saja kita mulai.

<h3>Langkah 1 - Install Laravel Yajra/laravel-datatables-oracle</h3>
Silahkan teman-teman install Laravel Yajra/laravel-datatables-oracle dengan perintah dibawah ini :


```
composer require yajra/laravel-datatables-oracle:"~9.0"
```
![](https://i.imgur.com/KGmBYlU.png)

Silahkan teman-teman proses installasi Laravel Yajra/laravel-datatables-oracle sampai selesai, proses ini teman-teman harus terhubung ke internet.

<h3>Langkah 2 - Membuat Controller Home</h3>
Sekarang kita akan belajar membuat controller untuk menampilkan data di Yajra DataTables. Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam project Laravel-nya.

```
php artisan make:controller HomeController
```

jika perintah di atas berhasil dijalankan, maka kita akan mendapatkan 1 file baru dengan nama HomeController.php yang berada di dalam folder app/Http/Controllers.

Silahkan buka file tersebut dan ubah semua kode-nya menjadi seperti berikut ini.

```
<?php

namespace App\Http\Controllers;

use App\Models\User;
use Illuminate\Http\Request;
use Yajra\DataTables\Facades\DataTables;

class HomeController extends Controller
{
    public function index()
    {
        if (request()->ajax()) {
            $users = User::all();
            return DataTables::of($users)

                ->make();
        }
        return view('home');
    }
}

```

Dari penambahan kode di atas, pertama-tama kita import Model User.
```
use App\Models\User;
```

Dari penambahan kode di atas, pertama kita import Facades DataTables. Karena kita akan menggunakan DataTables Yajra disini.
```
use Yajra\DataTables\Facades\DataTables;
```
Kemudian di dalam class HomeController kita menambahkan 1 method baru, yaitu :

1. function index

Method ini akan kita gunakan untuk menampilkan list data users dari database. Disini kita menggunakan Yajra DataTables untuk mendapatkan data dari database melalui Model User

Di dalam function index pertama kita tambahkan sebuah kondisi, sama seperti kita menggunakan if statment.

Jadi di dalam if kita memeriksa apakah ada sebuah request dari ajax, jika ada maka kita akan melakukan ekseskusi kode di dalam closoure function, yang isinya adalah menampilkan data yang dikirmkan ajax.


```
if (request()->ajax()) {
    $users = User::all();
    return DataTables::of($users)->make();
    }     
```
Kemudian kita urutkan data yang ditampilkan berdasarkan yang paling terbaru menggunakan method latest()

Dan kita akan return ke dalam view / component yang berada di dalam home

