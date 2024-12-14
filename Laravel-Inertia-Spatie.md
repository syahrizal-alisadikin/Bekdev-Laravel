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



<h3>Langkah 1 - Install Framework Laravel</h3>

Silahkan masuk ke dalam folder dimana teman-teman akan menyimpan projectnya dan jalankan perintah berikut ini di dalam terminal/CMD :
``` 
composer create-project --prefer-dist laravel/laravel:^11.4 inertia-spatie 
```

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

