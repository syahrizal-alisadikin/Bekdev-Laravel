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
Dari perubahan kode di atas, pertama kita <em>import</em> <em>Model</em> <code>Permission</code> dari <strong>Laravel Spatie Permission</strong>.

```
use Spatie\Permission\Models\Permission;
```

Kemudian di dalam <em>method</em> <code>run</code> kita menambahkan banyak sekali <em>list</em> <em>permissions</em> yang nantinya kita gunakan di dalam aplikasi.

### Langkah 3 - Membuat Seeder User

Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam <em>project</em> Laravel-nya.

```
php artisan make:seed UserTableSeeder
```

Jika perintah di atas berhasil dijalankan, maka kita akan mendapatkan 1 <em>file</em> baru dengan nama <code>UserTableSeeder.php</code> yang berada di dalam <em>folder</em> <code>database/seeders</code>. Silahkan buka <em>file</em> tersebut, kemudian ubah kode-nya menjadi seperti berikut ini :

```
<?php

namespace Database\Seeders;

use App\Models\User;
use Illuminate\Database\Seeder;
use Spatie\Permission\Models\Role;
use Spatie\Permission\Models\Permission;
use Illuminate\Database\Console\Seeds\WithoutModelEvents;

class UserTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     */
    public function run(): void
    {
        //create user
        $user = User::create([
            'name'      => 'Syahrizaldev',
            'email'     => 'izaldev@gmail.com',
            'password'  => bcrypt('password'),
        ]);

        //get all permissions
        $permissions = Permission::all();

        //get role admin
        $role = Role::find(1);

        //assign permission to role
        $role->syncPermissions($permissions);

        //assign role to user
        $user->assignRole($role);
    }
}
```

Dari perubahan kode di atas, pertama kita <em>import</em> <em>Model</em> <code>User</code> terlebih dahulu, karena kita akan gunakan untuk melakukan <em>insert</em> data <em>user</em> baru ke dalam <em>database</em>.

```
use App\Models\User;
```
Setelah itu, kita <em>import</em> <em>Model</em> <code>Role</code> dan <code>Permission</code> dari <strong>Laravel Spatie Permission</strong>.

```
use Spatie\Permission\Models\Role;
use Spatie\Permission\Models\Permission;
```

Di dalam <em>method</em> <code>run</code>, pertama kita melakukan <em>insert</em> data <em>user</em> baru ke dalam <em>database</em> menggunakan <strong>Eloquent</strong>.

```
public function run(): void
    {
        //create user
        $user = User::create([
            'name'      => 'Syahrizaldev',
            'email'     => 'izaldev@gmail.com',
            'password'  => bcrypt('password'),
        ]);

        //get all permissions
        $permissions = Permission::all();

        //get role admin
        $role = Role::find(1);

        //assign permission to role
        $role->syncPermissions($permissions);

        //assign role to user
        $user->assignRole($role);
    }
```

Setelah data <em>user</em> berhasil di <em>insert</em> ke dalam <em>database</em>, maka selanjutnya kita akan melakukan <em>get</em> semua data <em>permissions</em> yang ada.

```
//get all permissions
$permissions = Permission::all();
```

Kemudian kita cari <em>role</em> dengan ID <code>1</code>.

```
//get role admin
$role = Role::find(1);
```

Setelah itu, kita melakukan <em>assign</em> semua <em>permissions</em> yang ada ke dalam <em>role</em> tersebut menggunakan <em>method</em> <code>syncPermissions</code>.

```
//assign permission to role
$role->syncPermissions($permissions);
```

Setelah <em>role</em> tersebut sudah memiliki semua hak akses, maka selanjutnya kita tinggal melakukan <em>assign</em> <em>role</em> tersebut ke dalam user yang kita <em>insert</em> di atas.

```
//assign role to user
$user->assignRole($role);
```
### Langkah 4 - Register Seeder

Setelah berhasil membuat <em>seeder</em> <em>user</em>, sekarang kita akan <em>register</em> di dalam <em>class</em> <code>DatabaseSeeder</code>. Silahkan buka <em>file</em> <code>database/seeders/DatabaseSeeder.php</code>, kemudian ubah kode-nya menjadi seperti berikut ini :

```
<?php

namespace Database\Seeders;

use App\Models\User;
// use Illuminate\Database\Console\Seeds\WithoutModelEvents;
use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    /**
     * Seed the application's database.
     */
    public function run(): void
    {
        $this->call(RolesTableSeeder::class);
        $this->call(PermissionsTableSeeder::class);
        $this->call(UserTableSeeder::class);
    }
}
```

Di atas, kita telah memanggil <em>class</em> <code>RolesTableSeeder</code>, <code>PermissionsTableSeeder</code>,  <code>UserTableSeeder</code> di dalam <em>method</em> <code>run</code>. Dan untuk urutannya jangan sampai keliru ya. Karena prosesnya akan dieksekusi secara berurutan.

### Langkah 5 - Menjalankan Migration dan Seeder

Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam <em>project</em> Laravel-nya.

```
php artisan migrate --seed
```

Perintah di atas akan menjalankan <em>migration</em> <em>table</em> dan kita tambahkan <em>flag</em> <code>-- seed</code> yang artinya sekaligus menjalankan perintah <em>database</em> <em>seeder</em>.

![](https://i.imgur.com/eJIGhHQ.png)
