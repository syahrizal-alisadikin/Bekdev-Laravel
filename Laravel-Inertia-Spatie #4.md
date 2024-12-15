<strong>Laravel</strong> secara <em>default</em> menyediakan fitur yang bernama <code>Seeding</code>. Fitur ini memungkinkan kita untuk melakukan proses <em>insert</em> data ke dalam <em>database</em> tanpa perlu <em>form</em> atau yang lainnya.

Fitur ini umumnya digunakan untuk membuat <em>dummy</em> data atau data yang diatur secara <em>default</em> oleh kita. Dan untuk fitur ini berada di dalam <em>folder</em> <code>database/seeders</code>. Secara <em>default</em> kita sudah disediakan sebuah <em>class</em> yang bernama <code>DatabaseSeeder</code>. Di dalam <em>class</em> ini kita bisa memanggil <em>seeder</em> yang nantinya kita buat dan kita juga bisa mengurutkan <em>file</em> <em>seeder</em> saat dieksekusi.

### Langkah 1 - Membuat Seeder Roles

Sekarang kita akan belajar membuat <em>seeder</em> untuk data <em>roles</em>. Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam <em>project</em> Laravel-nya.

```
php artisan make:seed RolesTableSeeder
```
