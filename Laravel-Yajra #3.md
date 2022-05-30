Halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar bagaimana cara menginstall package Laravel Yajra/laravel-datatables-oracle di project kita.

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

Dan kita akan return ke dalam view yang berada folder di dalam resources/views

<h3>Langkah 3 -Menambahkan Route Home</h3>

Setelah berhasil membuat controller Home, sekarang kita lanjutkan untuk membuat route-nya. Silahkan buka file routes/web.php, kemudian ubah semua kode-nya menjadi seperti berikut ini.

```
<?php

use App\Http\Controllers\HomeController;
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

Route::get('/', [HomeController::class,'index'])->name('home');

```

Dari penambahan kode di atas, pertama kita import Controller Home . Karena kita akan menggunakan Controller Home disini

```
use App\Http\Controllers\HomeController;
```

Di atas, pertama kita set agar url "/" merupakan URL yang secara default di akses

kita mengubah kode

```
Route::get('/', function () {
    return view('welcome');
});
```

ubah menjadi seperti berikut ini :

```
Route::get('/', [HomeController::class,'index'])->name('home');
```

<h3>Langkah 4 -Menambahkan View Home</h3>

Sekarang silahkan buat folder baru dengan nama layouts di dalam folder resources/views dan di dalam folder layouts tersebut, silahkan buat file baru dengan nama apps.blade.php, kemudian masukkan kode berikut ini di dalamnya.

```
<!doctype html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- CSRF Token -->
    <meta name="csrf-token" content="{{ csrf_token() }}">

    <title>{{ config('app.name', 'Laravel') }}</title>

    <!-- Scripts -->
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.js"></script>
    <script src="https://cdn.datatables.net/1.10.16/js/jquery.dataTables.min.js"></script>
    <!-- Fonts -->
    <link rel="dns-prefetch" href="//fonts.gstatic.com">
    <link href="https://fonts.googleapis.com/css?family=Nunito" rel="stylesheet">
    <!-- Styles -->
    <link  href="https://cdn.datatables.net/1.10.16/css/jquery.dataTables.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
</head>
<body>
    <div id="app">
        <nav class="navbar navbar-expand-md navbar-light bg-white shadow-sm">
            <div class="container">
                <a class="navbar-brand" href="{{ url('/') }}">
                    {{ config('app.name', 'Laravel') }}
                </a>
                <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="{{ __('Toggle navigation') }}">
                    <span class="navbar-toggler-icon"></span>
                </button>

                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <!-- Left Side Of Navbar -->
                    <ul class="navbar-nav mr-auto">

                    </ul>

                    <!-- Right Side Of Navbar -->

                </div>
            </div>
        </nav>

        <main class="py-4">
            @yield('content')
        </main>
    </div>
    @stack('scripts')
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous"></script>
</body>
</html>
```

Dari penambahan kode di atas, pertama kita import Jquery DataTables menggunakan cdn.

```
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.js"></script>
<script src="https://cdn.datatables.net/1.10.16/js/jquery.dataTables.min.js"></script>
<link  href="https://cdn.datatables.net/1.10.16/css/jquery.dataTables.min.css" rel="stylesheet">
```

setelah itu kita buat file home.blade.php di dalam resources/view, kemudian masukkan kode berikut ini di dalamnya.

```
@extends('layouts.apps')

@section('content')
<div class="container">
    <div class="row justify-content-center">
        <div class="col-md-12">
            <div class="card">
                <div class="card-header">Daftar User</div>

                <div class="card-body">
                    <a href="#" class="btn btn-sm btn-success mb-2">Tambah Data</a>
                    <table id="tbl_list" class="table table-striped table-bordered" cellspacing="0" width="100%">
                    <thead>
                        <tr>
                            <th>No</th>
                            <th>Name</th>
                            <th>Email</th>

                        </tr>
                    </thead>
                    <tbody>

                    </tbody>
                </table>
                </div>
            </div>
        </div>
    </div>
</div>
@endsection
@push('scripts')
<script type="text/javascript">
$(document).ready(function () {
   $('#tbl_list').DataTable({
        processing: true,
        serverSide: true,
        ajax: '{{ url()->current() }}',
        columns: [
            { data: 'id', name: 'id' },
            { data: 'name', name: 'nama_lengkap' },
            { data: 'email', name: 'email' },

        ]
    });
 });
</script>
@endpush
```

<h3>Langkah 5 -Uji Coba Halaman Home</h3>

Sekarang silahkan reload halaman http://localhost:8000/. Dan jika berhasil maka akan menampilkan hasil seperti berikut ini.

![](https://i.imgur.com/f4Znwtl.png)

Di atas, data user masih kosong, temen-temen bisa menambahkan data dummy di tabel users untuk lihat hasil nya, terimakasih.
