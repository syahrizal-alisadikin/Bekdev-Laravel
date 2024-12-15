<strong>Laravel</strong> secara <em>default</em> menyediakan fitur yang bernama <code>Seeding</code>. Fitur ini memungkinkan kita untuk melakukan proses <em>insert</em> data ke dalam <em>database</em> tanpa perlu <em>form</em> atau yang lainnya.

Fitur ini umumnya digunakan untuk membuat <em>dummy</em> data atau data yang diatur secara <em>default</em> oleh kita. Dan untuk fitur ini berada di dalam <em>folder</em> <code>database/seeders</code>. Secara <em>default</em> kita sudah disediakan sebuah <em>class</em> yang bernama <code>DatabaseSeeder</code>. Di dalam <em>class</em> ini kita bisa memanggil <em>seeder</em> yang nantinya kita buat dan kita juga bisa mengurutkan <em>file</em> <em>seeder</em> saat dieksekusi.

### Langkah 1 - Membuat Seeder Roles

Sekarang kita akan belajar membuat <em>seeder</em> untuk data <em>roles</em>. Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam <em>project</em> Laravel-nya.

```
php artisan make:seed RolesTableSeeder
```

Jika perintah di atas berhasil, maka kita akan mendapatkan 1 <em>file</em> baru dengan nama <code>RolesTableSeeder.php</code> di dalam <em>folder</em> <code>database/seeders</code>. Silahkan buka <em>file</em> tersebut, kemudian ubah semua kode-nya menjadi seperti berikut ini :

```
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;
use Spatie\Permission\Models\Role;
use Illuminate\Database\Console\Seeds\WithoutModelEvents;

class RolesTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     */
    public function run(): void
    {
        Role::create(['name' => 'admin']);
        Role::create(['name' => 'user']);
    }
}

```

Dari perubahan kode di atas, pertama kita <em>import</em> <em>Model</em> <code>Role</code> dari <strong>Laravel Spatie Permission</strong>.

```
use Spatie\Permission\Models\Role;
```

Setelah itu, di dalam <code>funtion run</code> kita melakukan <em>create</em> data baru, yaitu untuk <code>admin</code> dan <code>user</code>

```
Role::create(['name' => 'admin']);
Role::create(['name' => 'user']);
```

### Langkah 2 - Membuat Seeder Permission

Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam <em>project</em> Laravel-nya.

```
php artisan make:seed PermissionsTableSeeder
```

Jika perintah di atas berhasil dijalankan, maka kita akan mendapatkan 1 <em>file</em> baru dengan nama <code>PermissionsTableSeeder.php</code> yang berada di dalam <em>folder</em> <code>database/seeders</code>. Silahkan buka <em>file</em> tersebut, kemudian ubah semua kode-nya menjadi seperti berikut ini :

```
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;
use Spatie\Permission\Models\Permission;
use Illuminate\Database\Console\Seeds\WithoutModelEvents;

class PermissionsTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     */
    public function run(): void
    {


        //permission users
        Permission::create(['name' => 'users index', 'guard_name' => 'web']);
        Permission::create(['name' => 'users create', 'guard_name' => 'web']);
        Permission::create(['name' => 'users edit', 'guard_name' => 'web']);
        Permission::create(['name' => 'users delete', 'guard_name' => 'web']);

        //permission roles
        Permission::create(['name' => 'roles index', 'guard_name' => 'web']);
        Permission::create(['name' => 'roles create', 'guard_name' => 'web']);
        Permission::create(['name' => 'roles edit', 'guard_name' => 'web']);
        Permission::create(['name' => 'roles delete', 'guard_name' => 'web']);

        //permission permissions
        Permission::create(['name' => 'permissions index', 'guard_name' => 'web']);
        Permission::create(['name' => 'permissions create', 'guard_name' => 'web']);
        Permission::create(['name' => 'permissions edit', 'guard_name' => 'web']);
        Permission::create(['name' => 'permissions delete', 'guard_name' => 'web']);

       
    }
}
```


