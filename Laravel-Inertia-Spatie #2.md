<p>Sekarang kita akan belajar bagaimana cara melakukan installasi dan konfigurasi <strong>Laravel Spatie Permission</strong> di dalam aplikasi yang sedang kita kembangkan.</p>

### Langkah 1 - Install Laravel Spatie Permission

<p>Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan sudah beraada di dalam <em>project</em> Laravel-nya.</p>

``` 
composer require spatie/laravel-permission:6.10.1                                                                                                                                                                                       
```
<p>Silahkan tunggu proses installasi-nya sampai selesai. Dan pastikan teman-teman terhubung dengan <em>internet</em>, karena paket ini akan diinstall secara <em>online</em>.</p>

![](https://i.imgur.com/nVafWgz.png)

### Langkah 2 - Publish Konfigurasi

<p>Setelah proses installasi selesai dilakukan, sekarang kita lanjutkan untuk melakukan proses <em>publish</em> konfigurasi dari <strong>Laravel Spatie Permission</strong>. Silahkan jalankan perintah berikut ini di dalam terminal/CMD :</p>

``` 
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"                                                                                                                                                                                      
```

Dari perintah di atas, kita akan mendapatkan `2 file` baru, yaitu:
<ol>
  <li>
  <code>config/permission.php</code>
  </li>
   <li>
  <code>database/migrations/2024_12_14_023311_create_permission_tables.php</code>
  </li>
</ol>

Jika kita lihat, di dalam file <em>migration</em> <strong>Laravel Spatie Permission</strong> akan membuat 5 <em>table</em>, yaitu :

<ol>
  <li>
  <code>roles</code>
  </li>
  <li>
  <code>permissions</code>
  </li>
  <li>
  <code>role_has_permissions</code>
  </li>
  <li>
  <code>model_has_roles</code>
  </li>
  <li>
  <code>model_has_permissions</code>
  </li>
</ol>

### Langkah 3 - Konfigurasi Model User
Kita lanjutkan untuk menambahkan <em>Traits</em> <code>HasRoles</code> milik <strong>Laravel Spatie Permission</strong> di dalam <em>Model</em> <code>User</code>. Ini digunakan agar kita bisa menggunakan fitur dan relasi yang ada di dalam <strong>Laravel Spatie Permission</strong> melalui <em>Model</em> <code>User</code>.

Silahkan buka <em>file</em> <code>app/Models/User.php</code>, kemudian ubah kode-nya menjadi seperti berikut ini :

```
<?php

namespace App\Models;

// use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Spatie\Permission\Traits\HasRoles;

class User extends Authenticatable
{
    /** @use HasFactory<\Database\Factories\UserFactory> */
    use HasFactory, Notifiable, HasRoles;

    /**
     * The attributes that are mass assignable.
     *
     * @var list<string>
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for serialization.
     *
     * @var list<string>
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

Di atas kita <em>import Traits</em> dari <strong> Laravel Spatie</strong>, kurang lebih seperti berikut ini :

```
use Spatie\Permission\Traits\HasRoles;
```

kemudian kita gunakan <em>Traits</em> tersebut di dalam Model Class <strong>User</strong>, kurang lebih seperti berikut ini :

```
use HasFactory, Notifiable, HasRoles;
```

kita menambahkan  <em>method</em> baru dengan nama <code>getPermissionArray</code>, <em>method</em> tersebut akan kita gunakan untuk mendapatkan <em>list</em> <em>permissions</em> berdasarkan <em>user</em> yang sedang <em>login</em>.

```
public function getUserPermissions()
{
    return $this->getAllPermissions()->mapWithKeys(fn($permission) => [$permission['name'] => true]);
}
```

### Langkah 4 - Register Middleware

Agar <strong>Laravel Sptie Permission</strong> dapat digunakan di dalam aplikasi, maka kita perlu melakukan <em>register</em> <em>middleware</em>. Silahkan buka <em>file</em> <code>bootstrap/app.php</code>, kemudian tambahkan kode berikut ini di dalam <code>withMiddleware</code>.

Jika file <code>bootstrap/app.php</code> ditulis secara lengkap, kurang lebih seperti berkut ini :

```
<?php

use Illuminate\Foundation\Application;
use Illuminate\Foundation\Configuration\Exceptions;
use Illuminate\Foundation\Configuration\Middleware;

return Application::configure(basePath: dirname(__DIR__))
    ->withRouting(
        web: __DIR__.'/../routes/web.php',
        commands: __DIR__.'/../routes/console.php',
        health: '/up',
    )
    ->withMiddleware(function (Middleware $middleware) {
        $middleware->alias([
            'role' => \Spatie\Permission\Middleware\RoleMiddleware::class,
            'permission' => \Spatie\Permission\Middleware\PermissionMiddleware::class,
            'role_or_permission' => \Spatie\Permission\Middleware\RoleOrPermissionMiddleware::class,
        ]);
    })
    ->withExceptions(function (Exceptions $exceptions) {
        //
    })->create();
```
