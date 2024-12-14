Halo teman-teman semuanya, gimana kabarnya ? masih semangat belajarnya kan hehe

pada kesempatan kali ini kita semua akan belajar bagaimana cara membuat Role Permission mengggunakan Spatie di Laravel Inertia React Js.
<h3>Spatie (Laravel - Permission)</h3>
<p><strong>Laravel Spatie Permission</strong> merupakan <em>package</em> yang sangat <em>powerfull</em> dan digunakan untuk membuat <em>role</em> dan <em>permissions</em> di dalam aplikasi berbasis Laravel. Dengan bantuan <em>package</em> ini, kita bisa memberikan hak akses kepada <em>user</em> untuk mengakses <em>menu</em> tertentu secara dinamis.</p>

- **Fitur Utama**:
  - Mengelola Role dan Permission dinamis.
  - Memberikan izin berdasarkan Role atau Permission.
  - Middleware bawaan untuk proteksi route.


<h3> Inertia.js</h3>
 <p><strong>Inertia.js</strong> adalah pendekatan baru untuk membuat aplikasi web berbasis SPA (Single Page Application) menggunakan Laravel di sisi backend dan React atau Vue.js di sisi frontend tanpa memerlukan API REST.</p>

- **Fitur Utama**:
  - Navigasi seperti SPA (Single Page Application).
  - Data dikirim langsung dari server ke komponen React/Vue/Svelte.
  - Tanpa perlu membuat API endpoint untuk setiap aksi.



### Langkah 1 - Install Framework Laravel

Silahkan masuk ke dalam folder dimana teman-teman akan menyimpan projectnya dan jalankan perintah berikut ini di dalam terminal/CMD :
``` 
composer create-project --prefer-dist laravel/laravel:^11.4 inertia-spatie 
```
![](https://i.imgur.com/2udGP0V.png)
> **CATATAN** : `inertia-spatie` adalah nama dari projeck kita.

Perintah di atas akan membuat project Laravel baru dengan versi 11.x Silahkan tunggu proses installasi sampai selesai dan pastikan teman-teman harus terhubung dengan internet, karena semua paket akan diunduh secara online.

### Langkah 2 - Menjalankan Project Laravel

Setelah proses installasi selesai, sekarang kita akan belajar untuk menjalankan project Laravel-nya. Silahkan jalankan perintah berikut ini di dalam terminal/CMD :

```bash
cd inertia-spatie
```

Perintah cd di atas digunakan untuk melakukan navigasi ke dalam sebuah folder, dalam contoh di atas kita akan masuk ke dalam folder inertia-spatie. Setelah berhasil masuk ke dalam folder project Laravel, sekarang silahkan jalankan perintah berikut ini di dalam terminal/CMD :



```php
php artisan serve
```

Jika perintah di atas berhasil dijalankan, maka project Laravel akan dijalankan pada port 8000 di dalam localhost. Kita bisa membukanya di dalam browser dengan [http://localhost:8000](http://localhost:8000/) dan jika berhasil, maka hasilnya seperti berikut ini.

![](https://i.imgur.com/p7dSkFu.png)

### Langkah 3 - Koneksi Database

Pertama, kita akan lakukan konfigurasi untuk koneksi database-nya terlebih dahulu. Silahkan buka project Laravel-nya menggunakan Text Editor dan cari file `.env` dan kode berikut ini.

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
DB_DATABASE=inertia_spatie
DB_USERNAME=root
DB_PASSWORD=
```

Dari perubahan kode di atas, kita melakukan konfigurasi nama dabatase yang akan kita gunakan nanti, yaitu `DB_DATABASE` kita berikan value `inertia_spatie`.

Dan untuk `DB_PASSWORD`, silahkan disesuaikan dengan konfigurasi dari MySQL masing-masing, jika menggunakan **XAMPP**, maka secara default kita tidak perlu mengisinya atau dibiarkan kosong.

Kemudian tidak lupa kita juga akan mengubah TIMEZONE dari aplikasi kita dengan cara mengubah `APP_TIMEZONE=UTC` menjadi `APP_TIMEZONE='Asia/Jakarta'`

### Langkah 4 - Menjalankan Migration

Secara default Laravel sudah memberikan kita sebuah migration yang digunakan untuk melakukan generate table users, disini kita tidak perlu membuat sebuah migration lagi dan kita tinggal menjalankan perintah migrate, maka kita akan mendapatkan beberapa table.

Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam project Laravel.

```
php artisan migrate
```
![](https://i.imgur.com/YuDz0l9.png)

Karena kita belum membuat database inertia_spatie, Laravel secara otomatis akan membuat database tersebut langsung `Enter`

![](https://i.imgur.com/uR1YBtx.png)

### Kesimpulan

<p>Pada artikel ini, kita telah belajar Penjelasan <strong>Spatie</strong>, <strong>Inertia Js</strong> dan Installasi <em>project</em> <strong>Laravel</strong>.</p>
<p>Jika teman-teman ada kendala saat belajar, silahkan bisa bertanya melalui kolom komentar atau <em>group</em> <strong>Telegram</strong> <strong>SantriKoding</strong>.</p>

Semoga bermanfaat! 😊
