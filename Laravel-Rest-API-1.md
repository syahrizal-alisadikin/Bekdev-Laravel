<p>Halo teman-teman semuanya, pada seri artikel kali ini kita semua akan belajar bagaimana cara membuat <em>Restful</em> <em>API</em> di <strong>Laravel</strong> <strong>11</strong> yang ditunjukkan untuk para pemula. Dimana pada seri ini kita akan mulai materi-nya secara step by step dari dasar sampai testing <em>Rest</em> <em>API</em> yang telah dibuat. Pada pembuatan <em>Rest</em> <em>API</em> ini, kita juga akan belajar melakukan <em>upload</em> gambar di dalam <strong>Laravel</strong>.</p>
<p>Dan kita juga akan memanfaatkan fitur bawaan dari <strong>Laravel</strong> yang bernama <em>API</em> <em>Resources</em>. Dengan menggunakan <em>API</em> <em>Resources</em>, kita bisa melakukan transformasi data dari <em>Model</em> menjadi format <em>JSON</em> dengan sangat mudah dan cepat.</p>
<h3>Tools dan hal-hal teknis yang perlu disiapkan</h3>
<h4>- Web server (Xampp/Laragon)</h4>
<h4>- PHP 8.2 </h4>
<h4>- Text Editor (Vscode / Sublime text / Cursor) </h4>
<h4>- Composer 2.7 </h4>
<h4>- Postman </h4>
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

<p>Di atas, pertama kita ubah driver koneksinya yang semula menggunakan <code>sqlite</code> menjadi <code>mysql</code>. Kemudian untuk <code>DB_DATABASE</code> kita ubah menjadi <code>laravel_api</code>,</p>
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

<h3>Laravel Sanctum</h3>
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

<h3>Membuat RESTful API Authentication dan Register User</h3>
<p>Sekarang, kita akan belajar bagaimana cara membuat <em>RESTful</em> <em>API</em> untuk proses <em>Authentication</em> menggunakan <strong>Sanctum</strong>. Disini kita akan membuat beberapa fitur, yaitu : <code>register</code>dan <code>login</code>. Dan untuk <em>tools</em> debug yang akan gunakan adalah <code>postman</code>, jadi silahkan teman-teman bisa <em>install</em> <em>tools</em> tersebut melalui <em>link</em> berikut ini : <a href="https://www.postman.com/downloads/" target="_blank">https://www.postman.com/downloads/</a></p>

<h3>Membuat Controller API Authentication</h3>
<p>Disini kita akan mengelompokan semua <em>controller</em> yang digunakan untuk <em>RESTful</em> <em>API</em> di dalam <em>folder</em> <code>app/Http/Controllers/Api</code>. Silahkan jalankan perintah berikut ini di dalam terminal/CMD :</p>

```
php artisan make:controller Api/AuthController
```
<p>Jika perintah di atas berhasil, maka kita akan mendapatkan 1 <em>file</em> <em>controller</em> baru di dalam <em>folder</em> <code>app/Http/Controllers/Api/AuthController.php</code>. Silahkan buka <em>file</em> tersebut dan ubah kode-nya menjadi seperti berikut ini :</p>

```
<?php

namespace App\Http\Controllers\Api;

use Exception;
use App\Models\User;
use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Hash;
use Illuminate\Support\Facades\Validator;

class AuthController extends Controller
{
    public function register(Request $request)
    {
        $validator = Validator::make($request->all(), [
            'name'     => 'required|string|max:255',
            'email'    => 'required|email|unique:users',
            'password' => 'required|confirmed',
        ]);

        if ($validator->fails()) {
            return response()->json($validator->errors(), 400);
        }

        $user = User::create([
            'name'      => $request->name,
            'email'     => $request->email,
            'password'  => Hash::make($request->password)
        ]);

        $token = $user->createToken('authToken')->plainTextToken;



        if($user) {
            return response()->json([
                'token_type' => 'Bearer',
                'access_token' => $token,
                'user' => $user,
                'message' => "berhasil register"
            ], 201);
        }

        return response()->json([
            'success' => false,
        ], 409);
    }

    public function login(Request $request)
    {
        try {
            // validasi input
            $validator = Validator::make($request->all(), [
                'email' => 'required|email',
                'password' => 'required',
            ]);

            if ($validator->fails()) {
                return response()->json($validator->errors(), 400);
            }

            // cek Credentials Login
            $user = User::where('email', $request->email)->first();
            if (! $user) {
                return response()->json(['message' => 'Email User Salah'], 400);
            }


            // jika hash tidak sesuai muncul alert
            if (!Hash::check($request->password, $user->password, [])) {
                return response()->json([
                    'success' => false,
                    'message' => 'Password salah',
                ], 401);
            }

            // jika berhasil
            $token = $user->createToken('authToken')->plainTextToken;

            return response()->json([
                'token_type' => 'Bearer',
                'access_token' => $token,
                'user' => $user,
                'message' => "berhasil login"
            ]);
        } catch (Exception $error) {
            return response()->json([
                'status' => false,
                'message' => $error->getMessage(),
            ],  500);
        }
    }

    public function logout(Request $request)
    {
        $user = $request->user();
        $user->tokens()->delete();
        return response()->json([
            'user' => $user,
            'message' => "berhasil logout"
        ]);
    }
}
```
<p>Di atas kita <em>import</em> <em>model</em> <code>User</code>, karena kita akan menggunakan <em>model</em> ini untuk beberapa aksi, yaitu untuk proses <em>register</em> dan <em>login</em>.</p>

```
use App\Models\User;
```

<p>Kemudian, kita <em>import</em> <code>Facades Hash</code>, ini kita gunakan untuk proses <em>hash</em> <em>password</em> saat <em>register</em> <em>user</em>.</p>

```
use Illuminate\Support\Facades\Hash;
```

<p>Kemudian, kita <em>import</em> <code>Facades Validator</code>, ini akan kita manfaatkan untuk proses validasi di dalam <em>register</em> dan <em>login</em>.</p>

```
use Illuminate\Support\Facades\Validator;
```

<p>Kemudian, kita <em>import</em> <code>Exception</code>, ini akan kita manfaatkan untuk proses jika ada error.</p>

```
use Exception;
```
<p>Di dalam <em>class</em> <code>AuthController</code> kita membuat 3 <em>function</em>, yaitu <code>function register</code> dan <code>function login</code> dan <code>logout</code>.</p>

<p><em><strong>Function Register</strong></em></p>
<p><code>Function Register</code> digunakan untuk proses registrasi <em>user</em>, disini kita menggunakan validasi untuk memastikan bahwa data yang akan diinputkan benar-benar sesuai.</p>

```
$validator = Validator::make($request->all(), [
            'name'     => 'required|string|max:255',
            'email'    => 'required|email|unique:users',
            'password' => 'required|confirmed',
        ]);
```

<p>Jika validasi di atas tidak terpenuhi, maka kita akan me-<em>return</em> sebuah <em>response</em> validasi <em>error</em> dengan <em>status</em> <em>code</em> <code>400</code>.</p>

```
 if ($validator->fails()) {
            return response()->json($validator->errors(), 400);
        }
```

<p>Jika validasi di atas terpenuhi, maka akan melakukan proses <em>insert</em> data baru ke dalam <em>database</em>. Kurang lebih seperti berikut ini :</p>

```
$user = User::create([
            'name'      => $request->name,
            'email'     => $request->email,
            'password'  => Hash::make($request->password)
        ]);
```

<p>Setelah data berhasil di masukkan ke dalam <em>database</em>, selanjutnya kita akan melakukan <em>authentication</em> dan <em>generate</em> <em>token</em> menggunakan <strong>Sanctum</strong>, dimana datanya di ambil dari <em>user</em> yang sedang melakukan proses <em>registrasi</em>.</p>

```
$token = $user->createToken('authToken')->plainTextToken;
```

<p>Jika berhasil, kita akan membuat <em>response</em> dalam bentuk JSON dengan isi adalah data <em>user</em> itu sendiri dan <em>token</em> yang berhasil di <em>generate</em>. Dan disini kita menggunakan <em>status</em> <em>code</em> <code>201</code>. Artinya kita berhasil membuat <em>resources</em> baru di <em>server</em>.</p>

```
 if($user) {
            return response()->json([
                'token_type' => 'Bearer',
                'access_token' => $token, //token sanctum
                'user' => $user, // data user
                'message' => "berhasil register"
            ], 201);
        }
```

<p><em><strong>Function Login</strong></em></p>
<p><code>Function Login</code> akan digunakan untuk proses <em>authentication</em> <em>user</em>. Disini kita juga menggunakan validasi untuk memastikan bahwa <em>field</em> <code>email</code> dan <code>password</code> benar-benar sesuai dengan yang diharapkan.</p>

```
 $validator = Validator::make($request->all(), [
                'email' => 'required|email',
                'password' => 'required',
            ]);
```

<p>Jika validasi di atas tidak terpenuhi, maka kita akan membuat <em>response</em> dengan <em>message</em> dari validasi <em>error</em> tersebut dan kita akan menggunakan <em>status</em> <em>code</em> <code>400</code>.</p>

```
 if ($validator->fails()) {
                return response()->json($validator->errors(), 400);
            }
```

<p>kemudian kita membuat kondisi untuk proses <em>login</em> <em>gagal</em>. Dan kita akan membuat <em>response</em> dengan <em>status</em> <em>code</em> <code>401</code>.</p>

```
 // cek Credentials Login
    $user = User::where('email', $request->email)->first();
    if (! $user) {
        return response()->json(['message' => 'Email User Salah'], 401);
    }


    // jika hash tidak sesuai muncul alert
    if (!Hash::check($request->password, $user->password, [])) {
        return response()->json([
            'success' => false,
            'message' => 'Password salah',
        ], 401);
    }
```

<p>Jika proses login berhasil, kita akan membuat <em>response</em> <code>success</code> menjadi <code>true</code> dan menampilkan data <em>user</em> beserta token yang di <em>generate</em>. Disini kita menggunakan <em>status</em> <em>code</em> <code>201</code>.</p>

```
 // jika berhasil
    $token = $user->createToken('authToken')->plainTextToken;

    return response()->json([
        'token_type' => 'Bearer',
        'access_token' => $token, //token sanctum
        'user' => $user, //data user
        'message' => "berhasil login"
    ],201);
```

<p><em><strong>Function Logout</strong></em></p>
<p><code>Function logout</code> digunakan untuk menghapus  <em>token</em>/<em>user</em> yang sedang <em>login</em>.</p>

```
public function logout(Request $request)
    {
        $user = $request->user();
        $user->tokens()->delete();
        return response()->json([
            'user' => $user,
            'message' => "berhasil logout"
        ]);
    }
```

<h3>Membuat Route API Authentication</h3>
<p> silahkan buka <em>file</em> <code>routes/api.php</code> dan ubah semua kode-nya menjadi seperti berikut ini :</p>

```
<?php

namespace App\Http\Controllers\Api;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/user', function (Request $request) {
    return $request->user();
})->middleware('auth:sanctum');

Route::post('login', [AuthController::class, 'login']);
Route::post('register', [AuthController::class, 'register']);

```

<p>Di atas, pertama kita set <code>namespace</code> untuk <em>folder</em> <em>controller</em> yang digunakan, yaitu <code>App\Http\Controllers\Api</code>. karena semua <em>controller</em> <em>API</em> kita nanti akan diletakkan di dalam <em>folder</em> tersebut.</p>

```
namespace App\Http\Controllers\Api;
```
<p>Kemudian, kita membuat 3 <em>route</em> dengan <em>method</em> <code>post</code>, yaitu : <code>/login</code>, <code>/register</code> dan <em>method</em> <code>get</code>, yaitu  <code>/user</code>. Untuk route <code>/user</code> kita letakkan di dalam middleware <code>auth:sanctum</code>, artinya route tersebut hanya bisa diakses jika user sudah melakukan proses login / otentikasi.</p>

<p>Secara <em>default</em> jika kita membuat <em>route</em> di dalam <em>file</em> <code>routes/api.php</code> maka <em>prefix</em> yang di gunakan adalah <code>api</code>. Jadi jika di atas kita membuat <em>route</em> <code>/login</code> dan <code>/register</code>, maka hasilnya akan menjadi seperti ini :</p>
<ul>
<li>
<a href="http://127.0.0.1:8000/api/login">http://127.0.0.1:8000/api/login</a>
</li>
<li>
<a href="http://127.0.0.1:8000/api/register">http://127.0.0.1:8000/api/register</a>
</li>
</ul>

<h3>Uji Coba Register User</h3>
<p>Sekarang, kita akan melakukan uji coba <em>RESTful API</em> untuk proses <em>register</em>, silahkan buka <code>postman</code> dan pada bagian <em>URL</em>, silahkan masukkan : <a href="http://127.0.0.1:8000/api/register" target="_blank">http://127.0.0.1:8000/api/register</a> dan jangan lupa untuk <em>method</em>nya menggunakan <code>POST</code>, kemudian klik <code>Send</code>, maka kita akan mendapatkan <em>response</em> kurang lebih seperti berikut ini :</p>

```
{
    "name": [
        "The name field is required."
    ],
    "email": [
        "The email field is required."
    ],
    "password": [
        "The password field is required."
    ]
}
```

![Imgur](https://i.imgur.com/1clJv1d.png)

<p><em>Response</em> tersebut merupakan hasil validasi, karena kita belum memasukkan data apapun di dalam <code>Body</code>, sekarang kita akan coba lagi dengan menggunakan data.</p>
<p>Silahkan klik pada <em>tab</em> <code>Body</code> dan pilih <code>form-data</code>, kemudian isikan beberapa <code>key</code> dan <code>value</code> seperti berikut ini :</p>
<div class="table-responsive"><table>
<thead>
<tr>
<th align="left">KEY</th>
<th align="left">VALUE</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">name</td>
<td align="left">jaka</td>
</tr>
<tr>
<td align="left">email</td>
<td align="left">jaka@gmail.com</td>
</tr>
<tr>
<td align="left">password</td>
<td align="left">password</td>
</tr>
<tr>
<td align="left">password_confirmation</td>
<td align="left">password</td>
</tr>
</tbody>
</table></div>
<p>Di atas, kita membuat contoh data <em>user</em>, sekarang silahkan klik <code>Send</code>. Dan jika berhasil kita akan mendapatkan <em>response</em> kurang lebih seperti berikut ini :</p>


```
{
    "token_type": "Bearer",
    "access_token": "2|6rwgQmklxL1Mj8rrQyr7tIcVVVCZVtoH05ILEs7Ueaa4581c",
    "user": {
        "name": "jaka",
        "email": "jaka@gmail.com",
        "updated_at": "2024-05-10T04:17:00.000000Z",
        "created_at": "2024-05-10T04:17:00.000000Z",
        "id": 2
    },
    "message": "berhasil register"
}
```

![Imgur](https://i.imgur.com/YHPVXTl.png)

<p>Bisa kita lihat, di atas kita berhasil melakukan proses registrasi dan berhasil mendapatkan <em>token</em> dari <strong>Sanctum</strong>, sekarang silahakan cek juga di <code>table users</code>.</p>
<h3>Uji Coba Login User</h3>
<p>Setelah kita berhasil melakukan uji coba di proses <em>register</em>, sekarang kita akan lakukan uji coba untuk proses <em>login</em>. Silahkan klik new <em>tab</em> di <code>postman</code> dan masukkan <em>URL</em> berikut ini : <a href="http://127.0.0.1:8000/api/login" target="_blank">http://127.0.0.1:8000/api/login</a> dan jangan lupa menggunakan method <code>POST</code>, kemudian klik <code>Send</code>. Maka kurang lebih hasilnya seperti berikut ini :</p>

```
{
    "email": [
        "The email field is required."
    ],
    "password": [
        "The password field is required."
    ]
}
```

![Imgur](https://i.imgur.com/NKSwnAX.png)

<p>Di atas kita mendapatkan <em>response</em> <em>message</em> dari validasi, karena kita belum memasukkan data apapun di dalam <em>tab</em> <code>Body</code>. Sekarang silahkan klik tab <code>Body</code> dan pilih <code>form-data</code>, kemudian masukkan beberapa <code>key</code> dan <code>value</code> berikut ini :</p>
<div class="table-responsive"><table>
<thead>
<tr>
<th>KEY</th>
<th>VALUE</th>
</tr>
</thead>
<tbody>
<tr>
<td>email</td>
<td>jaka@gmail.com</td>
</tr>
<tr>
<td>password</td>
<td>password</td>
</tr>
</tbody>
</table></div>
<p>Silahkan isi dengan data tersebut, karena kita tadi menggunakan data tersebut untuk proses registrasi. Kemudian klik <code>Send</code>. Jika berhasil maka kita akan mendpatkan <em>response</em> kurang lebih seperti berikut ini :</p>

```
{
    "token_type": "Bearer",
    "access_token": "3|OA0ilBRoPooEhAZVuannanKMg2A6VvaVn5YWyF1Jc06ae311",
    "user": {
        "id": 2,
        "name": "jaka",
        "email": "jaka@gmail.com",
        "email_verified_at": null,
        "created_at": "2024-05-10T04:17:00.000000Z",
        "updated_at": "2024-05-10T04:17:00.000000Z"
    },
    "message": "berhasil login"
}
```

![Imgur](https://i.imgur.com/aoV3838.png)

<p>Di atas kita telah berhasil melakukan proses <em>login</em> dan mendapatkan <em>token</em> <strong>Sanctum</strong>, <em>token</em> tersebut bisa kita gunakan untuk proses mendapatkan data dari <em>database</em>.</p>
<blockquote>
<p><strong>CATATAN !</strong> : <em>token</em> akan selalu bersifat <em>random</em> saat melakukan proses <em>login</em>.</p>
</blockquote>

<h3> Membuat Model dan Migration Book</h3>
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

<h3> Membuat API Resources</h3>
<p>Silahkan teman-teman jalankan perintah berikut ini di dalam terminal/CMD dan pastikan sudah berada di dalam <em>project</em> <strong>Laravel</strong>-nya.</p>

```
php artisan make:resource BookResource
```

<p>Jika perintah di atas berhasil dijalankan, maka kita akan mendapatkan 1 <em>file</em> baru yang berada di dalam <em>folder</em> <code>app/Http/Resources/BookResource.php</code>.</p>
<h3> Custom Response</h3>
<p>Sebenarnya, secara bawaan <em>API</em> <em>Resource</em> yang telah di<em>generate</em> sudah bisa langsung digunakan, tapi karena kita akan menyesuaikan format <em>JSON</em> yang diinginkan, maka kita perlu melakukan sedikit modifikasi di dalam <em>file</em> tersebut.</p>
<p>Silahkan teman-teman buka <em>file</em> <code>app/Http/Resources/BookResource.php</code>, kemudian ubah semua kode-nya menjadi seperti berikut ini.</p>

```
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\JsonResource;

class BookResource extends JsonResource
{
    //define properti
    public $status;
    public $message;
    public $resource;

    /**
     * __construct
     *
     * @param  mixed $status
     * @param  mixed $message
     * @param  mixed $resource
     * @return void
     */
    public function __construct($status, $message, $resource)
    {
        parent::__construct($resource);
        $this->status  = $status;
        $this->message = $message;
    }

    /**
     * toArray
     *
     * @param  mixed $request
     * @return array
     */
    public function toArray(Request $request): array
    {
        return [
            'success'   => $this->status,
            'message'   => $this->message,
            'data'      => $this->resource
        ];
    }
}
```

<p>Dari perubahan kode di atas, pertama kita menambahkan 3 properti, yaitu:</p>

```
//define properti
public $status;
public $message;
public $resource;
```

<p>Properti ini digunakan untuk menyimpan status, pesan, dan data yang akan dikirimkan dalam respons <em>JSON</em>.</p>
<p>Kemudian kita buat <em>method</em> <code>_construct</code> yang menerima 3 parameter, yaitu <code>$status</code>, <code>$message</code> dan <code>$resource</code>. Konstruktor ini akan dipanggil untuk menginisialisasi nilai dari properti <code>$status</code> dan <code>$message</code> dengan nilai yang diberikan oleh <em>controller</em> nantinya.</p>

```
public function __construct($status, $message, $resource)
{
    parent::__construct($resource);
    $this->status  = $status;
    $this->message = $message;
}
```

<p>Setelah itu, di dalam <em>method</em> <code>toArray</code> kita ubah bagian <em>return</em>-nya, tujuaanya agar mengembalikan format <em>JSON</em> yang sesuai dengan yang dibutuhkan.</p>
<p>Jadi, tujuan dari <code>BookResource</code> ini adalah untuk membuat data dari <em>Model</em> <code>Book</code> ke dalam format <em>JSON</em> yang sesuai dengan kebutuhan, termasuk penambahan informasi status dan pesan tertentu.</p>

<p><em>sumber</em>  <a href="https://laravel.com/docs/11.x/eloquent-resources" target="_blank">https://laravel.com/docs/11.x/eloquent-resources</a>.</p>

<h3> Membuat Controller Book</h3>
<p>Sekarang, silahkan teman-teman jalankan perintah berikut ini di dalam terminal/CMD dan pastikan sudah berada di dalam <em>project</em> <strong>Laravel</strong>-nya.</p>

```
php artisan make:controller Api/BookController
```

<p>Jika perintah di atas berhasil dijalankan, maka kita akan mendapatkan <em>file</em> baru yang berada di dalam <em>folder</em> <code>app/Http/Controllers/Api/BookController.php</code>. Silahkan buka <em>file</em> tersebut, kemudian ubah semua kode-nya menjadi seperti berikut ini.</p>

```
<?php

namespace App\Http\Controllers\Api;

//import model Post
use App\Models\Book;

use App\Http\Controllers\Controller;

//import resource BookResource
use App\Http\Resources\BookResource;

//import Http request
use Illuminate\Http\Request;

//import facade Validator
use Illuminate\Support\Facades\Validator;

//import facade Storage
use Illuminate\Support\Facades\Storage;

class BookController extends Controller
{
    /**
     * index
     *
     * @return void
     */
    public function index()
    {
        //get all books
        $books = Book::latest()->paginate(5);

        //return collection of books as a resource
        return new BookResource(true, 'List Data Books', $books);
    }

    public function store(Request $request)
    {
        //define validation rules
        $validator = Validator::make($request->all(), [
            'name'      => 'required',
            'harga'     => 'required',
            'stock'     => 'required',
            'image'     => 'required|image|mimes:jpeg,png,jpg,gif,svg|max:2048',
        ]);

        //check if validation fails
        if ($validator->fails()) {
            return response()->json($validator->errors(), 422);
        }

        //upload image
        $image = $request->file('image');
        $image->storeAs('public/books', $image->hashName());

        //create books
        $book = Book::create([
            'user_id'   => $request->user()->id,
            'name'      => $request->name,
            'harga'     => $request->harga,
            'stock'     => $request->stock,
            'image'     => $image->hashName(),
        ]);

        //return response
        return new BookResource(true, 'Data Book Berhasil Ditambahkan!', $book);
    }

    public function show($id)
    {
        //find book by ID
        $book = Book::find($id);

        //return single book as a resource
        return new BookResource(true, 'Detail Data Book!', $book);
    }

    public function update(Request $request, $id)
    {
        //define validation rules
        $validator = Validator::make($request->all(), [
            'name'      => 'required',
            'harga'     => 'required',
            'stock'     => 'required',
        ]);

        //check if validation fails
        if ($validator->fails()) {
            return response()->json($validator->errors(), 422);
        }

        //find book by ID
        $book = Book::find($id);

        //check if image is not empty
        if ($request->hasFile('image')) {

            //upload image
            $image = $request->file('image');
            $image->storeAs('public/books', $image->hashName());

            //delete old image
            Storage::delete('public/books/' . basename($book->image));

            //update book with new image
            $book->update([
                'image'     => $image->hashName(),
                'name'      => $request->name,
                'harga'     => $request->harga,
                'stock'     => $request->stock,
            ]);
        } else {

            //update post without image
            $book->update([
                'name'      => $request->name,
                'harga'     => $request->harga,
                'stock'     => $request->stock,
            ]);
        }

        //return response
        return new BookResource(true, 'Data Book Berhasil Diubah!', $book);
    }

    public function destroy($id)
    {

        //find book by ID
        $book = Book::find($id);

        //delete image
        Storage::delete('public/books/'.basename($book->image));

        //delete book
        $book->delete();

        //return response
        return new BookResource(true, 'Data Book Berhasil Dihapus!', null);
    }
}
```

<p>Setelah itu, silahkan teman-teman buka <em>file</em> <code>routes/api.php</code>, kemudian ubah kode-nya menjadi seperti berikut ini.</p>

```
<?php

namespace App\Http\Controllers\Api;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;


Route::middleware(['auth:sanctum'])->group(function () {
    //user
    Route::get('/user', function (Request $request) {
        return $request->user();
    });

    //books
    Route::apiResource('/books',BookController::class);
});

Route::post('login', [AuthController::class, 'login']);
Route::post('register', [AuthController::class, 'register']);
```

<p>Di atas, kita menambahkan <em>route</em> baru dengan jenis <code>apiResource</code> dan memiliki <em>path</em> <code>/books</code>. Untuk memastikan <em>route</em> tersebut berhasil ditambahkan, teman-teman bisa menjalankan perintah berikut ini di dalam terminal/CMD.</p>

```
php artisan route:list
```

![Imgur](https://i.imgur.com/K3xRaQB.png)

<h3> Uji Coba Rest API Book</h3>
<p>Sekarang, silahkan teman-teman buka aplikasi <strong>Postman</strong>, kemudian masukkan <em>URL</em> berikut ini <a href="http://127.0.0.1:8000/api/books" target="_blank">http://127.0.0.1:8000/api/books</a> dan untuk <em>method</em>-nya silahkan pilih <code>GET</code>.</p>
<p>Jangan Lupa Untuk Mendapatkan <code>Token</code> Terlebih dahulu setelah itu simpan di Header.</p>
<p>Jika sudah, silahkan klik <code>Send</code> dan jika berhasil maka teman-teman akan mendapatkan hasil seperti berikut ini.</p>

```
{
    "success": true,
    "message": "List Data Books",
    "data": {
        "current_page": 1,
        "data": [],
        "first_page_url": "http://127.0.0.1:8000/api/books?page=1",
        "from": null,
        "last_page": 1,
        "last_page_url": "http://127.0.0.1:8000/api/books?page=1",
        "links": [
            {
                "url": null,
                "label": "&laquo; Previous",
                "active": false
            },
            {
                "url": "http://127.0.0.1:8000/api/books?page=1",
                "label": "1",
                "active": true
            },
            {
                "url": null,
                "label": "Next &raquo;",
                "active": false
            }
        ],
        "next_page_url": null,
        "path": "http://127.0.0.1:8000/api/books",
        "per_page": 5,
        "prev_page_url": null,
        "to": null,
        "total": 0
    }
}
```

![Imgur](https://i.imgur.com/Wb6HdzK.png)

<p>Di atas, kita masih belum menampilkan data apapun, karena memang kita belum memiliki data di dalam <em>database</em>. Dan yang ditampilkan adalah <em>array</em> kosong.</p>


<p><em>sumber</em>  <a href="https://santrikoding.com/tutorial-restful-api-laravel-11-1-cara-install-menjalankan-laravel-11" target="_blank">https://santrikoding.com/tutorial-restful-api-laravel-11-1-cara-install-menjalankan-laravel-11</a>.</p>
