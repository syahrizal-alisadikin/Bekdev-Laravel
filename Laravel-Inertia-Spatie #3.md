Sekarang kita akan belajar bagaimana cara melakukan installasi dan konfigurasi <strong>Laravel Breeze</strong> sebagai starter-kit  project kita

<strong>Laravel Breeze</strong> adalah starter kit yang ringan dan sederhana untuk memulai proyek Laravel dengan fitur otentikasi bawaan. Breeze menyediakan fitur dasar seperti login, registrasi, lupa kata sandi, dan verifikasi email yang sudah siap digunakan.

Tampilan default <strong>Laravel Breeze</strong> terdiri dari template Blade, Vue, React dan Inertia yang dibuat dengan Tailwind CSS

Jika ingin tampilan lebih interaktif, Laravel Breeze juga mendukung <strong>Inertia.js </strong> dan <strong>Vue.js</strong> atau <strong>React</strong> untuk membangun antarmuka yang dinamis.

### Langkah 1 - Install Laravel Breeze

Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan sudah beraada di dalam <em>project</em> Laravel-nya.

```
composer require laravel/breeze:2.2.6 --dev

```

![](https://i.imgur.com/lIwsD1h.png)

### Langkah 2 - Publish Authentication

Setelah proses installasi selesai dilakukan, sekarang kita lanjutkan untuk melakukan proses <em>publish</em> Authentication dari <strong>Laravel Breeze</strong>. Silahkan jalankan perintah berikut ini di dalam terminal/CMD :

```
php artisan breeze:install

```
Ketika kita menjalankan perintah di atas, maka kita akan mendapatkan beberapa pertanyaan, silahkan ikuti langkah-langkahnya seperti berikut ini :

![](https://i.imgur.com/HD2YAWt.png)

Pilih <strong>React with Inertia  </strong> kemudian <em>ENTER</em>

![](https://i.imgur.com/PjJh3uG.png)

Pilih <strong>Inertia SSR </strong> kemudian <em>ENTER</em>

![](https://i.imgur.com/VxAE3vK.png)

Terakhir kita pilih  <strong>PHPUnit </strong> kemudian <em>ENTER</em>

![](https://i.imgur.com/DxJObip.png)

### Kesimpulan

<p>Pada artikel ini, kita telah belajar Installasi  <strong>Laravel Breeze</strong>.</p>
<p>Jika teman-teman ada kendala saat belajar, silahkan bisa bertanya melalui kolom komentar atau <em>group</em> <strong>Telegram</strong> <strong>SantriKoding</strong>.</p>

Semoga bermanfaat! 😊

