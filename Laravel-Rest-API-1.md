<p>Halo teman-teman semuanya, pada seri artikel kali ini kita semua akan belajar bagaimana cara membuat <em>Restful</em> <em>API</em> di <strong>Laravel</strong> <strong>11</strong> yang ditunjukkan untuk para pemula. Dimana pada seri ini kita akan mulai materi-nya secara step by step dari dasar sampai testing <em>Rest</em> <em>API</em> yang telah dibuat. Pada pembuatan <em>Rest</em> <em>API</em> ini, kita juga akan belajar melakukan <em>upload</em> gambar di dalam <strong>Laravel</strong>.</p>
<p>Dan kita juga akan memanfaatkan fitur bawaan dari <strong>Laravel</strong> yang bernama <em>API</em> <em>Resources</em>. Dengan menggunakan <em>API</em> <em>Resources</em>, kita bisa melakukan transformasi data dari <em>Model</em> menjadi format <em>JSON</em> dengan sangat mudah dan cepat.</p>
<h3><a id="content-langkah-1---installasi-composer" href="#content-langkah-1---installasi-composer" class="heading-permalink" aria-hidden="true" title="Permalink"></a> Installasi Composer</h3>
<p>Hal pertama yang mesti teman-teman lakukan adalah melakukan installasi <strong>Composer</strong> di dalam komputer, untuk melakukan installasi <strong>Composer</strong> teman-teman bisa ikuti langkah-langkah yang telah disediakan oleh situs resminya, yaitu di:</p>
<ol>
<li>
<strong>Installation - Windows</strong> : <a href="https://getcomposer.org/doc/00-intro.md#installation-windows">https://getcomposer.org/doc/00-intro.md#installation-windows</a>
</li>
<li>
<strong>Installation - Linux / Unix / macOS</strong> : <a href="https://getcomposer.org/doc/00-intro.md#installation-linux-unix-macos">https://getcomposer.org/doc/00-intro.md#installation-linux-unix-macos</a>
</li>
</ol>
<p>Jika teman-teman sudah melakukan proses installasi, maka kita bisa memastikannya di dalam terminal/CMD dengan mengetikkan perintah berikut ini.</p>

```
composer
```

<p>Jika berhasil, maka akan keluar hasil seperti berikut ini.</p>

![](https://i.imgur.com/cRBIxP2.png)

<h3><a id="content-langkah-2---membuat-project-laravel-11" href="#content-langkah-2---membuat-project-laravel-11" class="heading-permalink" aria-hidden="true" title="Permalink"></a> Membuat Project Laravel 11</h3>
<p>Sekarang silahkan teman-teman masuk ke dalam <em>folder</em> dimana akan menyimpan <em>project</em>-nya, kemudian jalankan perintah berikut ini di dalam terminal/CMD.</p>

```
composer create-project --prefer-dist laravel/laravel:^11.0 laravel11-api
```
<p>Perintah di atas, akan membuat sebuah <em>project</em> <strong>Laravel</strong> baru dengan versi <strong>11</strong> dan nama <em>project</em> yang akan digunakan adalah <code>laravel11-api</code>.</p>
<h3><a id="content-langkah-2---membuat-project-laravel-11" href="#content-langkah-2---membuat-project-laravel-11" class="heading-permalink" aria-hidden="true" title="Permalink"></a> Koneksi Database</h3>
Pertama, kita akan lakukan konfigurasi untuk koneksi database-nya terlebih dahulu. Silahkan buka project Laravel-nya menggunakan Text Editor dan cari file .env dan kode berikut ini.

```
DB_CONNECTION=sqlite
# DB_HOST=127.0.0.1
# DB_PORT=3306
# DB_DATABASE=laravel
# DB_USERNAME=root
# DB_PASSWORD=
```

Kemudian, silahkan ubah menjadi seperti berikut ini.

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_api
DB_USERNAME=root
DB_PASSWORD=
```

<p>Di atas, pertama kita ubah driver koneksinya yang semula menggunakan <code>sqlite</code> menjadi <code>mysql</code>. Kemudian untuk <code>DB_DATABASE</code> kita ubah menjadi <code>db_laravel11_api</code>,</p>
<h3> Menjalankan Proses Migrate</h3>
<p>Silahkan teman-teman jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam <em>project</em> <strong>Laravel</strong>-nya.</p>

```
php artisan migrate
```
<p>Jika muncul pertanyaan seperti berikut ini, maka silahkan ketik <code>yes</code> dan <strong>Enter</strong>.</p>

![Imgur](https://i.imgur.com/9yW3mHK.png)

![Imgur](https://i.imgur.com/jwlKfN5.png)

<h3><a id="content-langkah-3---menjalankan-project-laravel-11" href="#content-langkah-3---menjalankan-project-laravel-11" class="heading-permalink" aria-hidden="true" title="Permalink"></a>Menjalankan Project Laravel 11</h3>
<p>Sekarang kita akan belajar bagaimana cara menjalankan <em>project</em> <strong>Laravel</strong> <strong>11</strong> yang baru saja kita buat. Silahkan teman-teman jalankan perintah berikut ini di dalam terminal/CMD.</p>

```
cd laravel11-api
```

<p>Setelah berhasil masuk ke dalam <em>folder</em> <em>project</em>, maka sekarang silahkan teman-teman jalankan perintah berikut ini di dalam terminal/CMD.</p>

```
php artisan serve
```
<p>Perintah di atas digunakan untuk menjalankan <em>server</em> <strong>Laravel</strong>, jika berhasil maka <em>project</em> kita akan dijalankan di dalam <em>localhost</em> dengan <em>port</em> <code>8000</code>.</p>
<p>Teman-teman bisa membukanya di dalam <em>browser</em> dengan mengetikkan <a href="http://127.0.0.1:8000" target="_blank">http://127.0.0.1:8000</a>, jika berhasil maka akan menampilkan hasil seperti berikut ini.</p>

![](https://i.imgur.com/b34ZUNM.png)

<h3><a id="content-langkah-4---menjalankan-storage-link" href="#content-langkah-4---menjalankan-storage-link" class="heading-permalink" aria-hidden="true" title="Permalink"></a>Menjalankan Storage Link</h3>
<p>Setelah berhasil belajar membuat <em>project</em> <strong>Laravel</strong> dan menjalankannya, maka sekarang kita akan lanjutkan belajar bagaimana cara menjalankan perintah <code>storage:link</code> di dalam <strong>Laravel</strong>.</p>
<h3><a id="content-apa-itu-storage-link" href="#content-apa-itu-storage-link" class="heading-permalink" aria-hidden="true" title="Permalink"></a>Apa itu Storage Link?</h3>
<p><strong><em>Storage</em> <em>Link</em></strong> digunakan untuk melakukan <em>symlink</em> atau melakukan <em>link</em> sebuah folder di dalam project Laravel.</p>
<p>Di <strong>Laravel</strong> kita hanya bisa mengakses <em>file</em>-<em>file</em> yang ada di dalam <em>folder</em> <code>public</code>, sedangkan <em>file</em>-<em>file</em> yang akan di<em>upload</em> akan berada di dalam <em>folder</em> <code>storage</code>. Oleh sebab itu kita harus melakukan <em>link</em> dari <em>folder</em> <code>storage</code> ke dalam <em>folder</em> <code>public</code>. Dengan begitu kita bisa mengakses <em>file</em> yang telah di<em>upload</em> melalui <em>folder</em> <code>public</code>.</p>
<p>Silahkan teman-teman jalankan perintah berikut ini di dalam terminal/CMD dan pastikan sudah berada di dalam <em>project</em> <strong>Laravel</strong>-nya.</p>

```
php artisan storage:link
```

<h3>Rancang Struktur Database</h3>

![](https://i.imgur.com/I4Fteiz.png)

<p><em>Referensi</em>  <a href="https://drawdb.vercel.app" target="_blank">https://drawdb.vercel.app</a>.</p>

<h3>Membuat API Login dan Register Menggunakan Sanctum</h3>
<p>Laravel Sanctum menyediakan sistem otentikasi yang ringan untuk aplikasi satu halaman (SPA), aplikasi mobile, dan API berbasis token yang sederhana. Sanctum memungkinkan setiap pengguna aplikasi Anda untuk membuat beberapa token API untuk akun mereka. Token-token ini dapat diberikan kemampuan atau ruang lingkup yang menentukan tindakan apa yang diizinkan token untuk lakukan.</p>
<h3>Installation Sanctum</h3>
<p>Silahkan jalankan perintah di bawah ini di terminal/CMD :</p>

```
php artisan install:api
```
<p>Jika muncul pertanyaan seperti berikut ini, maka silahkan ketik <code>yes</code> dan <strong>Enter</strong>.</p>

![Imgur](https://i.imgur.com/W0gV6ia.png)

![Imgur](https://i.imgur.com/JWaFKpa.png)

<h3>Update Model User</h3>
<p>Sekarang, kita lanjutkan untuk melakukan beberapa konfigurasi di dalam <em>model</em> <code>User</code> agar bisa menggunakan <strong>Sanctum</strong> untuk <em>generate</em> <em>token</em>.</p>
<p>Silahkan buka <em>file</em> <code>app/Models/User.php</code> kemudian ubah kode-nya menjadi seperti berikut ini :</p>

```
<?php

namespace App\Models;

// use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens,HasFactory, Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var array<int, string>
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for serialization.
     *
     * @var array<int, string>
     */
    protected $hidden = [
        'password',
        'remember_token',
    ];

    /**
     * Get the attributes that should be cast.
     *
     * @return array<string, string>
     */
    protected function casts(): array
    {
        return [
            'email_verified_at' => 'datetime',
            'password' => 'hashed',
        ];
    }
}
```
<p>Di atas, pertama kita <em>import</em> <code> Trait HasApiTokens </code> di dalam <em>model</em> <code>User </code>.</p>

```
use Laravel\Sanctum\HasApiTokens;
```

<p>Setelah itu kita tambahkan <code>HasApiTokens</code> di dalam <em>class</em> <em>User</em>.</p>

```
use HasApiTokens,HasFactory, Notifiable;
```
<p><em>Referensi</em>  <a href="https://laravel.com/docs/11.x/sanctum#installation" target="_blank">https://laravel.com/docs/11.x/sanctum#installation</a>.</p>

<h3>Membuat Model dan Migration</h3>
<p>Sekarang kita lanjutkan belajar membuat <em>Model</em> dan <em>Migration</em> di dalam <strong>Laravel</strong>. Untuk membuat <em>Model</em> dan <em>Migration</em> di dalam <strong>Laravel</strong> kita bisa menggunakan perintah artisan <code>make:model</code>.</p>
<p>Silahkan teman-teman jalankan perintah berikut ini di dalam terminal/CMD dan pastikan sudah berada di dalam <em>project</em> <strong>Laravel</strong>-nya.</p>

```
php artisan make:model Book -m
```

<p>Jika berhasil, maka kita akan mendapatkan 2 <em>file</em> baru yang berada di dalam <em>folder</em> :</p>

<ol>
<li>
<code>app/Models/Book.php</code>
</li>
<li>
<code>database/migrations/2024_05_10_020535_create_books_table.php</code>
</li>
</ol>

<blockquote>
<p><strong>INFORMASI</strong> : untuk nama <em>file</em> <em>migration</em> akan random sesuai tanggal pembuatannya.</p>
</blockquote>

<h3><a id="content-langkah-3---menambahkan-field--kolom" href="#content-langkah-3---menambahkan-field--kolom" class="heading-permalink" aria-hidden="true" title="Permalink"></a>Menambahkan Field / Kolom ke dalam migrations</h3>

<p>Sekarang kita lanjutkan menambahkan <em>field</em> / kolom di dalam <em>file</em> <em>migration</em>. Silahkan teman-teman buka <em>file</em> <code>database/migrations/024_05_10_020535_create_books_table.php</code>, kemudian pada <code>function up</code> ubah kode-nya menjadi seperti berikut ini.</p>

```
 public function up(): void
    {
        Schema::create('books', function (Blueprint $table) {
            $table->id();
            $table->integer('user_id');
            $table->string('name');
            $table->integer('harga');
            $table->integer('stock');
            $table->string('image');
            $table->timestamps();
        });
    }
```

<p>Di atas, kita menambahkan 5 <em>field</em> baru di dalam <em>migration</em>, yaitu <code>user_id</code>, <code>name</code>, <code>harga</code>, <code>stock</code> dan <code>image</code>. Berikut ini detailnya.</p>

<div class="table-responsive"><table>
<thead>
<tr>
<th>FIELD / KOLOM</th>
<th>TIPE DATA</th>
<th>KETERANGAN</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>user_id</code></td>
<td><code>integer</code></td>
<td>untuk menyimpan id <em>user</em> yang melakukan tambah <em>book</em> </td>
</tr>
<tr>
<td><code>name</code></td>
<td><code>string</code></td>
<td>untuk menyimpan name <em>book</em>.</td>
</tr>
<tr>
<td><code>harga</code></td>
<td><code>integer</code></td>
<td>untuk menyimpan harga <em>book</em>.</td>
</tr>
 <tr>
<td><code>stock</code></td>
<td><code>integer</code></td>
<td>untuk menyimpan stock <em>book</em>.</td>
</tr>
  <tr>
<td><code>image</code></td>
<td><code>string</code></td>
<td>untuk menyimpan nama <em>file</em> image yang di <em>upload</em> .</td>
</tr>
</tbody>
</table>
</div>

<h3>Menambahkan Mass Assignment</h3>
<p>Karena <em>field</em> sudah berhasil ditambahkan di dalam <em>file</em> <em>migration</em>, maka langkah berikutnya adalah menambahkan <em><strong>Mass</strong></em> <em><strong>Assignment</strong></em> di dalam <em>Model</em>. Ini bertujuan agar <em>field</em> yang sudah kita tambahkan di atas dapat menyimpan sebuah nilai.</p>
<p>Silahkan teman-teman buka <em>file</em> <code>app/Models/Post.php</code>, kemudian ubah semua kode-nya menjadi seperti berikut ini.</p>

```
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Book extends Model
{
    use HasFactory;
    
    /**
     * fillable
     *
     * @var array
     */
    protected $fillable = [
        'user_id',
        'name',
        'harga',
        'stock',
        'image',
    ];
}
```

<p>Dari perubahan kode di atas, kita menambahkan properti baru dengan nama <code>$fillable</code>. properti tersebut disebut dengan <em><strong>Mass</strong></em> <em><strong>Assignment</strong></em> dan di dalamnya kita berikan <em>field</em>-<em>field</em> yang sudah kita buat sebelumnya di dalam <em>file</em> <em>migration</em>.</p>

<h3>Menjalankan Proses Migrate</h3>
<p>Setelah semua berhasil dibuat, maka langkah berikutnya adalah menjalankan perintah <em>migrate</em>. Dengan menjalankan perintah ini, maka <em>database</em> akan di<em>generate</em> beserta <em>table</em> dan juga <em>field</em>-nya.</p>
<p>Silahkan teman-teman jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam <em>project</em> <strong>Laravel</strong>-nya.</p>

```
php artisan migrate
```

![Imgur](https://i.imgur.com/O7zoUEN.png)

<h3>Menambahkan Accessor di Model</h3>
<p><em>Accessor</em> memungkinkan kita mengubah nilai saat <em>field</em>  diakses. Untuk mendefinisikan <em>Accessor</em>, kita bisa membuat method di dalam <em>Model</em> untuk menentukan <em>field</em> yang akan diakses.</p>
<blockquote>
<p><strong>Aturan Penamaan Accessor</strong></p>
<hr>
<p>Penamaan <em>method</em> yang dibuat harus sama dengan nama <em>field</em> yang akan di<em>format</em> dan menggunakan <code>CamelCase</code>.</p>
</blockquote>

<p>Sekarang kita akan membuat <em>Accessor</em> di dalam <em>Model</em> untuk <em>field</em> <code>image</code>, jadi silahkan teman-teman buka <em>file</em> <code>app/Models/Book.php</code>, kemudian ubah kode-nya menjadi seperti berikut ini.</p>

```
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Casts\Attribute;
use Illuminate\Database\Eloquent\Model;

class Book extends Model
{
    use HasFactory;

    /**
     * fillable
     *
     * @var array
     */
    protected $fillable = [
        'user_id',
        'name',
        'harga',
        'stock',
        'image',
    ];

    protected function image(): Attribute
    {
        return Attribute::make(
            get: fn ($image) => url('/storage/books/' . $image),
        );
    }
}
```

<p>Dari perubahan kode di atas, pertama kita <em>import</em> <em>Cast</em> <code>Attribute</code>.</p>

```
use Illuminate\Database\Eloquent\Casts\Attribute;
```

<p>kemudian kita membuat <em>method</em> baru dengan nama <code>image()</code>. Di dalam <em>method</em> ini kita melakukan <em>return</em> <em>field</em> <code>image</code> agar menghasilkan <em>full</em> <em>path</em> <em>URL</em> dari <em>folder</em> <em>storage</em>. Jadi ketika kita memanggil <em>field</em> <code>image</code> makan akan otomatis menghasilkan <em>output</em> seperti berikut ini :</p>

```
domain.com/storage/books/nama_file_image.png
```

<p>Tapi, jika kita tidak menggunakan fitur <em>Accessor</em>, maka hasilnya akan seperti berikut ini :</p>

```
nama_file_image.png
```

<p><em>sumber</em>  <a href="https://santrikoding.com/tutorial-restful-api-laravel-11-1-cara-install-menjalankan-laravel-11" target="_blank">https://santrikoding.com/tutorial-restful-api-laravel-11-1-cara-install-menjalankan-laravel-11</a>.</p>
