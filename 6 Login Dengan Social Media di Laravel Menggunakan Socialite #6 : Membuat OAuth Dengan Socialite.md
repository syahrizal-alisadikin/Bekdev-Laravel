Halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar bagaimana cara membuat OAuth menggunakan Laravel Socialite.



Pada seri tutorial kali ini kita semua akan belajar dengan 2 provider saja yaitu google dan GitHub, meskipun begitu untuk implementasinya dengan provider lain sama saja.



### Langkah 1 - Membuat Controller SocialAccount

Silahkan teman-teman jalankan perintah dibawah ini untuk membuat Controller Socialite :



```
php artisan make:controller Auth/SocialiteController
```

Jika berhasil maka akan dibuatkan `1 file` controller baru di dalam `app/Http/Controllers/Auth/SocialiteController.php` dan kemudian silahkan masukkan kode berikut ini :



```php
<?php

namespace App\Http\Controllers\Auth;

use App\Http\Controllers\Controller;
use App\Models\SocialAccount;
use App\Models\User;
use Illuminate\Http\Request;
use Laravel\Socialite\Facades\Socialite;

class SocialiteController extends Controller
{
    public function redirectToProvider($provider)
    {
        return Socialite::driver($provider)->redirect();
    }

    public function handleProvideCallback($provider)
    {
        try {

            $user = Socialite::driver($provider)->stateless()->user();
        } catch (Exception $e) {
            return redirect()->back();
        }
        // find or create user and send params user get from socialite and provider
        $authUser = $this->findOrCreateUser($user, $provider);

        // login user
        Auth()->login($authUser, true);

        // setelah login redirect ke dashboard
        return redirect()->route('dashboard');
    }

    public function findOrCreateUser($socialUser, $provider)
    {
        // Get Social Account
        $socialAccount = SocialAccount::where('provider_id', $socialUser->getId())
            ->where('provider_name', $provider)
            ->first();

        // Jika sudah ada
        if ($socialAccount) {
            // return user
            return $socialAccount->user;

            // Jika belum ada
        } else {

            // User berdasarkan email 
            $user = User::where('email', $socialUser->getEmail())->first();

            // Jika Tidak ada user
            if (!$user) {
                // Create user baru
                $user = User::create([
                    'name'  => $socialUser->getName(),
                    'email' => $socialUser->getEmail()
                ]);
            }

            // Buat Social Account baru
            $user->socialAccounts()->create([
                'provider_id'   => $socialUser->getId(),
                'provider_name' => $provider
            ]);

            // return user
            return $user;
        }
    }
}

```



### Langkah 2 - Menambahkan Route

Setelah kita berhasil membuat Controller beserta fungsi-fungsinya, sekarang kita lanjutkan untuk membuat sebuah route.



Silahkan teman-teman buka file `routes/web.php` dan silahkan teman-teman tambahkan kode dibawah ini :



```php
/**
 * socialite auth
 */
Route::get('/auth/{provider}', [SocialiteController::class, 'redirectToProvider']);
Route::get('/auth/{provider}/callback', [SocialiteController::class, 'handleProvideCallback']);
```

Jangan lupa import Controller di atas seperti berikut



```php
use App\Http\Controllers\Auth\SocialiteController;
```

### Langkah 3 - Menambahkan Button Login Google & GitHub

Selanjutnya kita akan menambahkan button Google & GitHub di dalam Auth Project kita. Silahkan teman-teman buka file `resources/views/auth/login.blade.php` dan ubahlah menjadi seperti berikut ini :



```php
<x-guest-layout>
    <x-auth-card>
        <x-slot name="logo">
            <a href="/">
                <x-application-logo class="w-20 h-20 fill-current text-gray-500" />
            </a>
        </x-slot>

        <!-- Session Status -->
        <x-auth-session-status class="mb-4" :status="session('status')" />

        <!-- Validation Errors -->
        <x-auth-validation-errors class="mb-4" :errors="$errors" />

        <form method="POST" action="{{ route('login') }}">
            @csrf

            <!-- Email Address -->
            <div>
                <x-label for="email" :value="__('Email')" />

                <x-input id="email" class="block mt-1 w-full" type="email" name="email" :value="old('email')" required autofocus />
            </div>

            <!-- Password -->
            <div class="mt-4">
                <x-label for="password" :value="__('Password')" />

                <x-input id="password" class="block mt-1 w-full"
                                type="password"
                                name="password"
                                required autocomplete="current-password" />
            </div>

            <!-- Remember Me -->
            <div class="block mt-4">
                <label for="remember_me" class="inline-flex items-center">
                    <input id="remember_me" type="checkbox" class="rounded border-gray-300 text-indigo-600 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50" name="remember">
                    <span class="ml-2 text-sm text-gray-600">{{ __('Remember me') }}</span>
                </label>
            </div>

            <div class="flex items-center justify-end mt-4">
                @if (Route::has('password.request'))
                    <a class="underline text-sm text-gray-600 hover:text-gray-900" href="{{ route('password.request') }}">
                        {{ __('Forgot your password?') }}
                    </a>
                @endif

                <x-button class="ml-3">
                    {{ __('Log in') }}
                </x-button>

               
            </div>

            <div class="flex items-center justify-center mt-4">

                <a href="/auth/google" class="mr-2 inline-block items-center px-4 py-2 bg-blue-600 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-blue-400 active:bg-gray-900 focus:outline-none focus:border-gray-900 focus:ring ring-gray-300 disabled:opacity-25 transition ease-in-out duration-150">
                    Login  Google
                </a>
                <br>
                <a href="/auth/github" class="px-4 py-2 bg-gray-800 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-gray-500">
                    Login  GitHub
                </a>
            </div>
        </form>
    </x-auth-card>
</x-guest-layout>

```



kemudian teman-teman juga buka file `resources/views/auth/register.blade.php` dan ubahlah menjadi seperti berikut ini :



```php
<x-guest-layout>
    <x-auth-card>
        <x-slot name="logo">
            <a href="/">
                <x-application-logo class="w-20 h-20 fill-current text-gray-500" />
            </a>
        </x-slot>

        <!-- Validation Errors -->
        <x-auth-validation-errors class="mb-4" :errors="$errors" />

        <form method="POST" action="{{ route('register') }}">
            @csrf

            <!-- Name -->
            <div>
                <x-label for="name" :value="__('Name')" />

                <x-input id="name" class="block mt-1 w-full" type="text" name="name" :value="old('name')" required autofocus />
            </div>

            <!-- Email Address -->
            <div class="mt-4">
                <x-label for="email" :value="__('Email')" />

                <x-input id="email" class="block mt-1 w-full" type="email" name="email" :value="old('email')" required />
            </div>

            <!-- Password -->
            <div class="mt-4">
                <x-label for="password" :value="__('Password')" />

                <x-input id="password" class="block mt-1 w-full"
                                type="password"
                                name="password"
                                required autocomplete="new-password" />
            </div>

            <!-- Confirm Password -->
            <div class="mt-4">
                <x-label for="password_confirmation" :value="__('Confirm Password')" />

                <x-input id="password_confirmation" class="block mt-1 w-full"
                                type="password"
                                name="password_confirmation" required />
            </div>

            <div class="flex items-center justify-end mt-4">
                <a class="underline text-sm text-gray-600 hover:text-gray-900" href="{{ route('login') }}">
                    {{ __('Already registered?') }}
                </a>

                <x-button class="ml-4">
                    {{ __('Register') }}
                </x-button>

              
            </div>
            <div class="flex items-center justify-center mt-4">

                <a href="/auth/google" class="mr-2 inline-block items-center px-4 py-2 bg-blue-600 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-blue-400 active:bg-gray-900 focus:outline-none focus:border-gray-900 focus:ring ring-gray-300 disabled:opacity-25 transition ease-in-out duration-150">
                    Register  Google
                </a>
                <br>
                <a href="/auth/github" class="px-4 py-2 bg-gray-800 border border-transparent rounded-md font-semibold text-xs text-white uppercase tracking-widest hover:bg-gray-500">
                    Register   GitHub
                </a>
            </div>
        </form>
    </x-auth-card>
</x-guest-layout>

```

### Langkah 4 - Uji Coba Tampilan



Sekarang Silahkan teman-teman jalankan projectnya dengan perintah dibawah ini :



```
php artisan serve
```

Jika berhasil teman-teman akan melihat kurang lebih tampilannya seperti berikut ini :



![](https://i.imgur.com/cXnkyoA.png)



![](https://i.imgur.com/eIF6T31.png)



> **CATATAN** ! : disini kita belum bisa menggunakan fungsi Auth dengan Socialite, karena kita belum mendapatkan `CLIENT ID` dan `SECRET ID` dari masing-masing provider.



Di artikel selanjutnya kita akan belajar bagaimana cara mendapatkan `CLEINT ID` dan `SECRET ID` tersebut dari masing-masing provider seperti Google & GitHub.

Terima Kasih