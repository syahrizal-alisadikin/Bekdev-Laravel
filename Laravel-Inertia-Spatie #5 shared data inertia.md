Untuk membuat data secara <em>global</em>, kita bisa mendefinisikannya di dalam <em>middleware</em> yang sudah disediakan oleh Inertia.js. Disini kita akan menambahkan beberapa data yang mungkin nantinya akan kita gunakan terus di dalam halaman.

Silahkan buka <em>file</em> <code>app/Http/Middleware/handleInertiaRequest.php</code>, kemudian ubah kode-nya menjadi seperti berikut ini :

```
<?php

namespace App\Http\Middleware;

use Illuminate\Http\Request;
use Inertia\Middleware;

class HandleInertiaRequests extends Middleware
{
    /**
     * The root template that is loaded on the first page visit.
     *
     * @var string
     */
    protected $rootView = 'app';

    /**
     * Determine the current asset version.
     */
    public function version(Request $request): ?string
    {
        return parent::version($request);
    }

    /**
     * Define the props that are shared by default.
     *
     * @return array<string, mixed>
     */
    public function share(Request $request): array
    {
        return [
            ...parent::share($request),
            'auth' => [
                'user' => $request->user(),
                'permissions' => $request->user() ? $request->user()->getUserPermissions() : [],
            ],
        ];
    }
}

```

Pada kode diatas pada method <code>share</code> kita menambahkan beberapa key didalam array auth, yang pertama ada key permissions.

```
'permissions' => $request->user() ? $request->user()->getUserPermissions() : [],
```

Pada kode diatas, key permissions kita lakukan pengecekan apakah user sedang login ? jika <code>true</code> maka kita tampilkan data permissions yang dimiliki oleh user menggunakan method <code>getUserPermissions</code> yang telah kita buat sebelumnya di model users, jika <code>false</code> maka kita akan tampilkan empty array.

### kesimpulan

Kurang lebih seperti itu untuk menambahkan <em>global</em> data / <em>shared</em> data menggunakan Inertia.js, kita bebas menambahkan data apapun disitu dan otomatis akan bersifat <em>global</em>.

<p>Jika teman-teman ada kendala saat belajar, silahkan bisa bertanya melalui kolom komentar atau <em>group</em> <strong>Telegram</strong> <strong>SantriKoding</strong>.</p>

Semoga bermanfaat! 😊
