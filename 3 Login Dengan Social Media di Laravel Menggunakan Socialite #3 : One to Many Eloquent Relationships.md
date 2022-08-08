Halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar tentang One to Many Relationships dari Model User ke SocialAccount.



Ini digunakan karena bisa jadi 1 User bisa memiliki lebih dari 1 akun Sosial Media, maka kita akan membuat Relationships dari kedua table ini. Langsung saja kita mulai.



### Langkah 1 - Hash Many

Silahkan teman-teman buka file Model `App/Models/User.php` dan silahkan teman-teman tambahkan kode berikut ini di dalam Model.



```php
/**
* @return \Illuminate\Database\Eloquent\Relations\HasMany
*/
public function socialAccounts()
{
  return $this->hasMany(SocialAccount::class);
}
```



Di atas, kita akan memberitahu aplikasi kita bahawa kita akan melakukan relasi Many ke Model atau table SocialAccount.



Jika file model `App/Models/User.php` di tulis dengan lengkap, kurang lebih seperti berikut ini :

```php
<?php

namespace App\Models;

// use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable;

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
     * The attributes that should be cast.
     *
     * @var array<string, string>
     */
    protected $casts = [
        'email_verified_at' => 'datetime',
    ];

    /**
     * @return \Illuminate\Database\Eloquent\Relations\HasMany
     */
    public function socialAccounts()
    {
        return $this->hasMany(SocialAccount::class);
    }
}

```



### Langkah 2 - BelongsTo

Pada tahap kali ini kita akan deklarasikan bahwa Model atau Table SocialAccount ini terhubung dengan table / Model User.



Silahkan teman-teman tambahkan function dibawah ini di dalam Model `App/Models/SocialAccount.php`



```php
/**
* @return \Illuminate\Database\Eloquent\Relations\BelongsTo
*/
public function user()
{
  return $this->belongsTo(User::class);
}
```

Atau jika di tulis dengan lengkap, kurang lebih seperti berikut ini :



```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class SocialAccount extends Model
{
    use HasFactory;

    protected $guarded = [];

    /**
     * @return \Illuminate\Database\Eloquent\Relations\BelongsTo
     */
    public function user()
    {
        return $this->belongsTo(User::class);
    }
}

```

Sampai disini pembahasan tentang bagaimana cara membuat Relationship Eloquent One to Many di Laravel, di artikel selanjutnya kita semua akan belajar bagaimana cara membuat Auth di Laravel.

Terima Kasih.