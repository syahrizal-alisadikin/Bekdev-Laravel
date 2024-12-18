Pada seri tutorial kali ini kita semua akan belajar  Crud (Create, Read, Update, Delete) Pada Modul Permission . 

### Langkah 1 - Membuat Controller Permission

Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastiksan sudah berada di dalam <em>project</em> Laravel-nya.

```
php artisan make:controller PermissionController -r
```

Jika perintah di atas berhasil dijalankan, maka kita akan mendapatkan 1 <em>file</em> <em>controller</em> baru dengan nama <code>PermissionController.php</code> yang berada di dalam <em>folder</em> <code>app/Http/Controllers</code>.

Silahkan buka <em>file</em> tersebut, kemudian ubah kode-nya menjadi seperti berikut ini.

```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Routing\Controllers\HasMiddleware;
use Illuminate\Routing\Controllers\Middleware;
use Spatie\Permission\Models\Permission;

class PermissionController extends Controller implements HasMiddleware
{
    public static function middleware()
    {
        return [
            new Middleware('permission:permissions index', only: ['index']),
            new Middleware('permission:permissions create', only: ['create', 'store']),
            new Middleware('permission:permissions edit', only: ['edit', 'update']),
            new Middleware('permission:permissions delete', only: ['destroy']),
        ];
    }

    /**
     * Display a listing of the resource.
     */
    public function index(Request $request)
    {
        //  get permissions
        $permissions = Permission::select('id', 'name')
            ->when($request->search,fn($search) => $search->where('name', 'like', '%'.$request->search.'%'))
            ->latest()
            ->paginate(6)->withQueryString();

        // render view
        return inertia('Permissions/Index', ['permissions' => $permissions,'filters' => $request->only(['search'])]);
    }

    /**
     * Show the form for creating a new resource.
     */
    public function create()
    {
        // render view
        return inertia('Permissions/Create');
    }

    /**
     * Store a newly created resource in storage.
     */
    public function store(Request $request)
    {
        // validate request
        $request->validate(['name' => 'required|min:3|max:255|unique:permissions']);

        // create new permission data
        Permission::create(['name' => $request->name]);

        // render view
        return to_route('permissions.index');
    }

    /**
     * Show the form for editing the specified resource.
     */
    public function edit(Permission $permission)
    {
        // render view
        return inertia('Permissions/Edit', ['permission' => $permission]);
    }

    /**
     * Update the specified resource in storage.
     */
    public function update(Request $request, Permission $permission)
    {
        // validate request
        $request->validate(['name' => 'required|min:3|max:255|unique:permissions,name,'.$permission->id]);

        // update permission data
        $permission->update(['name' => $request->name]);

        // render view
        return to_route('permissions.index');
    }

    /**
     * Remove the specified resource from storage.
     */
    public function destroy(Permission $permission)
    {
        // delete permissions data
        $permission->delete();

        // render view
        return back();
    }
}
```

Dari perubahan kode di atas, pertama kita <em>import</em> <em>Model</em> <code>Permission</code> dari <strong>Laravel Spatie Permission</strong>.

```
use Spatie\Permission\Models\Permission;
```

Kemudian kita juga melakukan implementasi sebuah <code>interface middleware</code> terbaru yang telah disediakan oleh laravel dengan cara menambahkan kata kunci implements HasMiddleware pada controller kita, kemudian kita import agar tidak error.

```
use Illuminate\Routing\Controllers\HasMiddleware;
use Illuminate\Routing\Controllers\Middleware;
```

Selanjutnya kita bisa menggunakan sebuah method dari interface HasMiddleware seperti kode dibawah ini.

```
public static function middleware()
    {
        return [
            new Middleware('permission:permissions index', only: ['index']),
            new Middleware('permission:permissions create', only: ['create', 'store']),
            new Middleware('permission:permissions edit', only: ['edit', 'update']),
            new Middleware('permission:permissions delete', only: ['destroy']),
        ];
    }
```

Dari kode diatas kita menerapkan beberapa middleware yang berfungsi untuk melakukan pembatasan akses method - method yang telah kita generate sesuai dengan hak akses yang dimiliki oleh user.

### Langkah 2 - Menambahkan Route Permission

Setelah berhasil membuat sebuah controller Permission, sekarang kita akan lanjutkan untuk pembuatan route-nya, silahkan teman - teman buka file <code>routes/web.php</code>, kemudian ubah kode-nya menjadi seperti berikut ini.

```
<?php

use App\Http\Controllers\ProfileController;
use App\Http\Controllers\PermissionController;
use Illuminate\Foundation\Application;
use Illuminate\Support\Facades\Route;
use Inertia\Inertia;

Route::get('/', function () {
    return Inertia::render('Welcome', [
        'canLogin' => Route::has('login'),
        'canRegister' => Route::has('register'),
        'laravelVersion' => Application::VERSION,
        'phpVersion' => PHP_VERSION,
    ]);
});

Route::get('/dashboard', function () {
    return Inertia::render('Dashboard');
})->middleware(['auth', 'verified'])->name('dashboard');

Route::middleware('auth')->group(function () {
    // permissions route
    Route::resource('/permissions', PermissionController::class);

    Route::get('/profile', [ProfileController::class, 'edit'])->name('profile.edit');
    Route::patch('/profile', [ProfileController::class, 'update'])->name('profile.update');
    Route::delete('/profile', [ProfileController::class, 'destroy'])->name('profile.destroy');
});

require __DIR__.'/auth.php';
```

Dari perubahan kode diatas, kita menambahkan sebuah route baru dengan nama permissions, untuk memastikan route yang kita buat telah berfungsi, teman - teman bisa jalankan.

```
php artisan r:l --name=permissions
```

![](https://i.imgur.com/t8IeHmz.png)

### Langkah 3 - Menambahkan Navigasi Permission

Silahkan buka <em>file</em> <code>resources/js/layouts/AuthenticatedLayout.jsx</code>, kemudian ubah kode-nya menjadi seperti berikut ini :

```
import ApplicationLogo from '@/Components/ApplicationLogo';
import Dropdown from '@/Components/Dropdown';
import NavLink from '@/Components/NavLink';
import ResponsiveNavLink from '@/Components/ResponsiveNavLink';
import { Link, usePage } from '@inertiajs/react';
import { useState } from 'react';
import hasAnyPermission from '@/Utils/Permissions';
export default function AuthenticatedLayout({ header, children }) {
    const user = usePage().props.auth.user;

    const [showingNavigationDropdown, setShowingNavigationDropdown] =
        useState(false);

    return (
        <div className="min-h-screen bg-gray-100">
            <nav className="border-b border-gray-100 bg-white">
                <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
                    <div className="flex h-16 justify-between">
                        <div className="flex">
                            <div className="flex shrink-0 items-center">
                                <Link href="/">
                                    <ApplicationLogo className="block h-9 w-auto fill-current text-gray-800" />
                                </Link>
                            </div>

                            <div className="hidden space-x-8 sm:-my-px sm:ms-10 sm:flex">
                                <NavLink
                                    href={route('dashboard')}
                                    active={route().current('dashboard')}
                                >
                                    Dashboard
                                </NavLink>

                                {hasAnyPermission(['permissions index']) &&
                                    <NavLink href={route('permissions.index')} active={route().current('permissions*')}>
                                        Permissions
                                    </NavLink>
                                }
                            </div>
                        </div>

                        <div className="hidden sm:ms-6 sm:flex sm:items-center">
                            <div className="relative ms-3">
                                <Dropdown>
                                    <Dropdown.Trigger>
                                        <span className="inline-flex rounded-md">
                                            <button
                                                type="button"
                                                className="inline-flex items-center rounded-md border border-transparent bg-white px-3 py-2 text-sm font-medium leading-4 text-gray-500 transition duration-150 ease-in-out hover:text-gray-700 focus:outline-none"
                                            >
                                                {user.name}

                                                <svg
                                                    className="-me-0.5 ms-2 h-4 w-4"
                                                    xmlns="http://www.w3.org/2000/svg"
                                                    viewBox="0 0 20 20"
                                                    fill="currentColor"
                                                >
                                                    <path
                                                        fillRule="evenodd"
                                                        d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
                                                        clipRule="evenodd"
                                                    />
                                                </svg>
                                            </button>
                                        </span>
                                    </Dropdown.Trigger>

                                    <Dropdown.Content>
                                        <Dropdown.Link
                                            href={route('profile.edit')}
                                        >
                                            Profile
                                        </Dropdown.Link>
                                        <Dropdown.Link
                                            href={route('logout')}
                                            method="post"
                                            as="button"
                                        >
                                            Log Out
                                        </Dropdown.Link>
                                    </Dropdown.Content>
                                </Dropdown>
                            </div>
                        </div>

                        <div className="-me-2 flex items-center sm:hidden">
                            <button
                                onClick={() =>
                                    setShowingNavigationDropdown(
                                        (previousState) => !previousState,
                                    )
                                }
                                className="inline-flex items-center justify-center rounded-md p-2 text-gray-400 transition duration-150 ease-in-out hover:bg-gray-100 hover:text-gray-500 focus:bg-gray-100 focus:text-gray-500 focus:outline-none"
                            >
                                <svg
                                    className="h-6 w-6"
                                    stroke="currentColor"
                                    fill="none"
                                    viewBox="0 0 24 24"
                                >
                                    <path
                                        className={
                                            !showingNavigationDropdown
                                                ? 'inline-flex'
                                                : 'hidden'
                                        }
                                        strokeLinecap="round"
                                        strokeLinejoin="round"
                                        strokeWidth="2"
                                        d="M4 6h16M4 12h16M4 18h16"
                                    />
                                    <path
                                        className={
                                            showingNavigationDropdown
                                                ? 'inline-flex'
                                                : 'hidden'
                                        }
                                        strokeLinecap="round"
                                        strokeLinejoin="round"
                                        strokeWidth="2"
                                        d="M6 18L18 6M6 6l12 12"
                                    />
                                </svg>
                            </button>
                        </div>
                    </div>
                </div>

                <div
                    className={
                        (showingNavigationDropdown ? 'block' : 'hidden') +
                        ' sm:hidden'
                    }
                >
                    <div className="space-y-1 pb-3 pt-2">
                        <ResponsiveNavLink
                            href={route('dashboard')}
                            active={route().current('dashboard')}
                        >
                            Dashboard
                        </ResponsiveNavLink>
                        {hasAnyPermission(['permissions index']) &&
                            <ResponsiveNavLink href={route('permissions.index')} active={route().current('permissions*')}>
                                Permissions
                            </ResponsiveNavLink>
                        }
                    </div>

                    <div className="border-t border-gray-200 pb-1 pt-4">
                        <div className="px-4">
                            <div className="text-base font-medium text-gray-800">
                                {user.name}
                            </div>
                            <div className="text-sm font-medium text-gray-500">
                                {user.email}
                            </div>
                        </div>

                        <div className="mt-3 space-y-1">
                            <ResponsiveNavLink href={route('profile.edit')}>
                                Profile
                            </ResponsiveNavLink>
                            <ResponsiveNavLink
                                method="post"
                                href={route('logout')}
                                as="button"
                            >
                                Log Out
                            </ResponsiveNavLink>
                        </div>
                    </div>
                </div>
            </nav>

            {header && (
                <header className="bg-white shadow">
                    <div className="mx-auto max-w-7xl px-4 py-6 sm:px-6 lg:px-8">
                        {header}
                    </div>
                </header>
            )}

            <main>{children}</main>
        </div>
    );
}
```
Pada kode diatas, kita memanggil 2 buah component dengan nama <code>NavLink</code> dan <code>ResponsiveNavLink</code> dengan props <code>href</code> yang kita set ke route yang bernama <code>permissions.index</code> dan props <code>active</code> kita set ke semua route <code>permissions*</code> dan tidak lupa kita tambahan pengecekan sebuah hak akses yang dimiliki oleh user menggunakan <code>utils hasAnyPermissions</code> yang telah kita buat sebelumnya.

### Langkah 4 - Menampilkan data Permission

Setelah berhasil membuat sebuah Route dan Navigasi Permission, sekarang kita akan lanjutkan untuk pembuatan view-nya, disini kita akan membuat 3 view sekaligus yaitu <code>index</code>, <code>create</code>, dan <code>edit</code>

Silahkan teman - teman buat folder baru dengan nama <em>Permissions</em> kemudian didalam folder tersebut buat file baru dengan nama <code>Index.jsx</code> yang akan kita letakan di <code>resources/js/Pages</code>, kemudian ubah kodenya menjadi seperti berikut ini.

```
import React from 'react'
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout';
import Container from '@/Components/Container';
import Table from '@/Components/Table';
import Button from '@/Components/Button';
import Pagination from '@/Components/Pagination';
import { Head, usePage } from '@inertiajs/react';
import Search from '@/Components/Search';
import hasAnyPermission from '@/Utils/Permissions';
export default function Index({auth}) {

    // destruct permissions props
    const { permissions,filters } = usePage().props;

    return (
        <AuthenticatedLayout
            user={auth.user}
            header={<h2 className="font-semibold text-xl text-gray-800 leading-tight">Permissions</h2>}
        >
            <Head title={'Permissions'}/>
            <Container>
                <div className='mb-4 flex items-center justify-between gap-4'>
                    {hasAnyPermission(['permissions create']) &&
                        <Button type={'add'} url={route('permissions.create')}/>
                    }
                    <div className='w-full md:w-4/6'>
                        <Search url={route('permissions.index')} placeholder={'Search permissions data by name...'} filter={filters}/>
                    </div>
                </div>
                <Table.Card title={'Permissions'}>
                    <Table>
                        <Table.Thead>
                            <tr>
                                <Table.Th>#</Table.Th>
                                <Table.Th>Permissions Name</Table.Th>
                                <Table.Th>Action</Table.Th>
                            </tr>
                        </Table.Thead>
                        <Table.Tbody>
                            {permissions.data.map((permission, i) => (
                                <tr key={i}>
                                    <Table.Td>{++i + (permissions.current_page-1) * permissions.per_page}</Table.Td>
                                    <Table.Td>{permission.name}</Table.Td>
                                    <Table.Td>
                                        <div className='flex items-center gap-2'>
                                            {hasAnyPermission(['permissions edit']) &&
                                                <Button type={'edit'} url={route('permissions.edit', permission.id)}/>
                                            }
                                            {hasAnyPermission(['permissions delete']) &&
                                                <Button type={'delete'} url={route('permissions.destroy', permission.id)}/>
                                            }
                                        </div>
                                    </Table.Td>
                                </tr>
                            ))}
                        </Table.Tbody>
                    </Table>
                </Table.Card>
                <div className='flex items-center justify-center'>
                    {permissions.last_page !== 1 && (<Pagination links={permissions.links}/>)}
                </div>
            </Container>
        </AuthenticatedLayout>
    )
}
```

Pada kode diatas, pertama - tama kita import beberapa file yang kita butuhkan

```
import React from 'react'
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout';
import Container from '@/Components/Container';
import Table from '@/Components/Table';
import Button from '@/Components/Button';
import Pagination from '@/Components/Pagination';
import { Head, usePage } from '@inertiajs/react';
import Search from '@/Components/Search';
import hasAnyPermission from '@/Utils/Permissions';
```

Selanjutnya kita membuat sebuah React Function Component dengan props auth dan filters.

```
export default function Index({auth,filters})
```

Selanjutnya kita memanggil sebuah utils <code>hasAnyPermissions</code> yang telah kita buat sebelumnya untuk melakukan pengecekan apakah user memiliki hak akses untuk melakukan penambahan data <code>permissions</code>.

```
{hasAnyPermission(['permissions create']) &&
  <Button type={'add'} url={route('permissions.create')}/>
}
```

### Langkah 5 - Membuat create Permission

Silahkan teman - teman  buat file baru dengan nama <code>Create.jsx</code> yang akan kita letakan di <code>resources/js/Pages/Permissions</code>, kemudian ubah kodenya menjadi seperti berikut ini.

```
import React from 'react'
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout';
import Container from '@/Components/Container';
import { Head, useForm } from '@inertiajs/react';
import Input from '@/Components/Input';
import Button from '@/Components/Button';
import Card from '@/Components/Card';
import Swal from 'sweetalert2';
export default function Create({auth}) {

    // define state with helper inertia
    const { data, setData, post, errors } = useForm({
        name : ''
    });

    // define method handleStoreData
    const handleStoreData = async (e) => {
        e.preventDefault();

        post(route('permissions.store'), {
            onSuccess: () => {
                Swal.fire({
                    title: 'Success!',
                    text: 'Data created successfully!',
                    icon: 'success',
                    showConfirmButton: false,
                    timer: 1500
                })
            }
        });
    }

    return (
        <AuthenticatedLayout
            user={auth.user}
            header={<h2 className="font-semibold text-xl text-gray-800 leading-tight">Create Permission</h2>}
        >
            <Head title={'Create Permissions'}/>
            <Container>
                <Card title={'Create new permission'}>
                    <form onSubmit={handleStoreData}>
                        <div className='mb-4'>
                            <Input label={'Permission Name'} type={'text'} value={data.name} onChange={e => setData('name', e.target.value)} errors={errors.name} placeholder="Input permission name.."/>
                        </div>
                        <div className='flex items-center gap-2'>
                            <Button type={'submit'} />
                            <Button type={'cancel'} url={route('permissions.index')}/>
                        </div>
                    </form>
                </Card>
            </Container>
        </AuthenticatedLayout>
    )
}

```

Pada kode diatas, pertama - tama kita import beberapa file yang kita butuhkan

```
import React from 'react'
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout';
import Container from '@/Components/Container';
import { Head, useForm } from '@inertiajs/react';
import Input from '@/Components/Input';
import Button from '@/Components/Button';
import Card from '@/Components/Card';
import Swal from 'sweetalert2';
```

Kemudian kita membuat sebuah state menggunakan form helper dari inertia.

```
const { data, setData, post, errors } = useForm({
    name : ''
});
```

Selanjutnya kita membuat sebuah method baru dengan nama <code>handleStoreData</code> yang kita gunakan untuk mengirimkan data kita ke server menggunakan form helper yang telah disediakan oleh inertia.

```
const handleStoreData = async (e) => {
    e.preventDefault();

    post(route('permissions.store'), {
        onSuccess: () => {
            Swal.fire({
                title: 'Success!',
                text: 'Data created successfully!',
                icon: 'success',
                showConfirmButton: false,
                timer: 1500
            })
        }
    });
}
```

Jika kita perhatikan, pada method post kita arahkan ke sebuah route yang bernama <code>permissions.store</code> dan ketika data berhasil dikirimkan, kita memanggil sebuah <code>sweet alert</code>.

```
post(route('permissions.store'), {
    onSuccess: () => {
        Swal.fire({
            title: 'Success!',
            text: 'Data created successfully!',
            icon: 'success',
            showConfirmButton: false,
            timer: 1500
        })
    }
});
```

### Langkah 6 - Membuat edit Permission

Silahkan teman - teman  buat file baru dengan nama <code>Edit.jsx</code> yang akan kita letakan di <code>resources/js/Pages/Permissions</code>, kemudian ubah kodenya menjadi seperti berikut ini.

```
import React from 'react'
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout';
import Container from '@/Components/Container';
import { Head, useForm, usePage } from '@inertiajs/react';
import Input from '@/Components/Input';
import Button from '@/Components/Button';
import Card from '@/Components/Card';
import Swal from 'sweetalert2';
export default function Edit({auth}) {

    // destruct permissions from usepage props
    const { permission } = usePage().props;

    // define state with helper inertia
    const { data, setData, post, errors } = useForm({
        name : permission.name,
        _method: 'put'
    });

    // define method handleUpdateData
    const handleUpdateData = async (e) => {
        e.preventDefault();

        post(route('permissions.update', permission.id), {
            onSuccess: () => {
                Swal.fire({
                    title: 'Success!',
                    text: 'Data updated successfully!',
                    icon: 'success',
                    showConfirmButton: false,
                    timer: 1500
                })
            }
        });
    }

    return (
        <AuthenticatedLayout
            user={auth.user}
            header={<h2 className="font-semibold text-xl text-gray-800 leading-tight">Edit Permission</h2>}
        >
            <Head title={'Edit Permissions'}/>
            <Container>
                <Card title={'Edit permission'}>
                    <form onSubmit={handleUpdateData}>
                        <div className='mb-4'>
                            <Input label={'Permission Name'} type={'text'} value={data.name} onChange={e => setData('name', e.target.value)} errors={errors.name} placeholder="Input permission name.."/>
                        </div>
                        <div className='flex items-center gap-2'>
                            <Button type={'submit'} />
                            <Button type={'cancel'} url={route('permissions.index')}/>
                        </div>
                    </form>
                </Card>
            </Container>
        </AuthenticatedLayout>
    )
}
```

Pada kode diatas, pertama - tama kita import beberapa file yang kita butuhkan.

```
import React from 'react'
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout';
import Container from '@/Components/Container';
import { Head, useForm, usePage } from '@inertiajs/react';
import Input from '@/Components/Input';
import Button from '@/Components/Button';
import Card from '@/Components/Card';
import Swal from 'sweetalert2';
```

Kemudian kita membuat sebuah state menggunakan form helper dari inertia.

```
const { data, setData, post, errors } = useForm({
    name : permission.name,
    _method: 'put'
});
```

Selanjutnya kita membuat sebuah method baru dengan nama <code>handleUpdateData</code> yang kita gunakan untuk mengirimkan data kita ke server menggunakan form helper yang telah disediakan oleh inertia.

```
const handleUpdateData = async (e) => {
    e.preventDefault();

    post(route('permissions.update', permission.id), {
        onSuccess: () => {
            Swal.fire({
                title: 'Success!',
                text: 'Data updated successfully!',
                icon: 'success',
                showConfirmButton: false,
                timer: 1500
            })
        }
    });
}
```
Sumber : [membuat-module-permission-dengan-inertia-react](https://jurnalkoding.com/series/tutorial-laravel-inertia-roles-permissions/tutorial-inertia-roles-permissions-12-membuat-module-permission-dengan-inertia-react) 

### kesimpulan

<p>Pada artikel ini, kita telah belajar membuat <strong>CRUD (Create,Read,Update,Delete) </strong>  <strong>Permission</strong>.</p>

<p>Jika teman-teman ada kendala saat belajar, silahkan bisa bertanya melalui kolom komentar atau <em>group</em> <strong>Telegram</strong> <strong>SantriKoding</strong>.</p>

Semoga bermanfaat! 😊
