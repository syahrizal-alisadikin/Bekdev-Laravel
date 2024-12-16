Disini kita akan membuat 1 buah <em>Utils</em> <code>Permission </code>  yang digunakan untuk pembatasan akses sesuai dengan roles & permissions yang dimiliki oleh user.

### Langkah 1 - Membuat Utils Permission

Silahkan buat <em>folder</em> baru dengan nama <code>Utils</code> di dalam <em>folder</em> <code>app/Resources/Js</code>, setelah itu di dalam <em>folder</em> <code>app/Resources/Js/Utils</code> silahkan buat <em>file</em> baru dengan nama <code>Permissions.jsx</code> dan masukkan kode berikut ini :

```
import { usePage } from "@inertiajs/react";

export default function hasAnyPermission(permissions){

    // destruct auth from usepage props
    const { auth } = usePage().props

    // get all permissions from props auth
    let allPermissions = auth.permissions;

    // get super-admin role from props auth
    let superAdmin = auth.super;

    // define has permission is false
    let hasPermission = false;

    // loop permissions
    permissions.forEach(function(item){
        // do it if permission is match with key
        if(allPermissions[item])
            // assign hasPermission to true
            hasPermission = true;
    });

   
}
```

Pada kode diatas pertama kita import <code>usePage</code> dari Inertia.

```
import { usePage } from "@inertiajs/react";
```

Selanjutnya kita membuat sebuah <strong>React Functional Component</strong> dengan nama <code>hasAnyPermissions</code> pada react functional component tersebut kita juga mendefinisikan sebuah props <code>permissions</code>.

```
export default function hasAnyPermission(permissions)
```

Berikutnya kita melakukan destructing sebuah props <code>auth</code> yang kita dapatkan dari share data global yang telah kita definisikan sebelumnya.

```
const { auth } = usePage().props
```

Selanjutnya kita mendefinisikan sebuah variabel baru dengan nama <code>allPermissions</code>, yang berisi data permissions yang diambil dari props <code>auth.permissions</code>.

```
let allPermissions = auth.permissions;
```

Berikutnya kita lakukan looping data <code>permissions</code> yang kita terima dari props, jika data ditemukan maka kita set <code>hasPermissions</code> dengan <code>true</code>

```
let hasPermission = false;
permissions.forEach(function(item){
    if(allPermissions[item])
        hasPermission = true;
});
```

Sumber : https://jurnalkoding.com/series/tutorial-laravel-inertia-roles-permissions/tutorial-inertia-roles-permissions-9-membuat-utils-permissions-dengan-inertia-react

### Kesimpulan

<p>Pada artikel ini, kita telah belajar  <strong>Utils Permission </strong> Di <strong>Laravel Inertia React</strong>.</p>
<p>Jika teman-teman ada kendala saat belajar, silahkan bisa bertanya melalui kolom komentar atau <em>group</em> <strong>Telegram</strong> <strong>SantriKoding</strong>.</p>

Semoga bermanfaat! 😊
