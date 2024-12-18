Pada seri tutorial kali ini kita semua akan belajar  Crud (Create, Read, Update, Delete) Pada Modul User . 

### Langkah 1 - Installasi Package Select2

Dikarenakan kita akan menggunakan select2, disini kita akan lakukan installasi terlebih dahulu, Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam project Laravel-nya.

```
npm i --save react-select
```

Sumber : [React Select](https://react-select.com/home#getting-started) 

### Langkah 2 - Membuat Controller User

Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastiksan sudah berada di dalam <em>project</em> Laravel-nya.

```
php artisan make:controller UserController -r
```

Jika perintah di atas berhasil dijalankan, maka kita akan mendapatkan 1 <em>file</em> <em>controller</em> baru dengan nama <code>UserController.php</code> yang berada di dalam <em>folder</em> <code>app/Http/Controllers</code>.

Silahkan buka <em>file</em> tersebut, kemudian ubah kode-nya menjadi seperti berikut ini.

```
<?php

namespace App\Http\Controllers;

use App\Models\User;
use Illuminate\Http\Request;
use Spatie\Permission\Models\Role;
use Illuminate\Routing\Controllers\Middleware;
use Illuminate\Routing\Controllers\HasMiddleware;
class UserController extends Controller implements HasMiddleware
{

    public static function middleware()
    {
        return [
            new Middleware('permission:users index', only : ['index']),
            new Middleware('permission:users create', only : ['create', 'store']),
            new Middleware('permission:users edit', only : ['edit', 'update   ']),
            new Middleware('permission:users delete', only : ['destroy']),
        ];
    }
    /**
     * Display a listing of the resource.
     */
    public function index(Request $request)
    {
        // get all users
        $users = User::with('roles')
            ->when(request('search'), fn($query) => $query->where('name', 'like', '%'.request('search').'%'))
            ->latest()
            ->paginate(6);

        // render view
        return inertia('Users/Index', ['users' => $users,'filters' => $request->only(['search'])]);
    }

    /**
     * Show the form for creating a new resource.
     */
    public function create()
    {
         // get roles
         $roles = Role::latest()->get();
         // render view
         return inertia('Users/Create', ['roles' => $roles]);
    }

    /**
     * Store a newly created resource in storage.
     */
    public function store(Request $request)
    {
         // validate request
         $request->validate([
            'name' => 'required|min:3|max:255',
            'email' => 'required|email|unique:users',
            'password' => 'required|confirmed|min:4',
            'selectedRoles' => 'required|array|min:1',
        ]);

        // create user
        $user = User::create([
            'name' => $request->name,
            'email' => $request->email,
            'password' => bcrypt($request->password),
        ]);

        // attach roles
        $user->assignRole($request->selectedRoles);

        // render view
        return to_route('users.index');
    }

    /**
     * Display the specified resource.
     */
    public function show(string $id)
    {
        //
    }

    /**
     * Show the form for editing the specified resource.
     */
    public function edit(User $user)
    {
        // get roles
        $roles = Role::where('name', '!=', 'super-admin')->get();

        // load roles
        $user->load('roles');

        // render view
        return inertia('Users/Edit', ['user' => $user, 'roles' => $roles]);
    }

    /**
     * Update the specified resource in storage.
     */
    public function update(Request $request, User $user)
    {
        // validate request
        $request->validate([
            'name' => 'required|min:3|max:255',
            'email' => 'required|email|unique:users,email,'.$user->id,
            'selectedRoles' => 'required|array|min:1',
        ]);

        // update user data
        $user->update([
            'name' => $request->name,
            'email' => $request->email,
        ]);

        // attach roles
        $user->syncRoles($request->selectedRoles);

        // render view
        return to_route('users.index');
    }

    /**
     * Remove the specified resource from storage.
     */
    public function destroy(User $user)
    {
        // delete user data
        $user->delete();

        // render view
        return back();
    }
}
```

Dari perubahan kode di atas, pertama kita <em>import</em> <em>Model</em> <code>User</code>dan <code>Role</code>.


```
use App\Models\User;
use Spatie\Permission\Models\Role;
```

Kemudian kita juga melakukan implementasi sebuah interface middleware terbaru yang telah disediakan oleh laravel dengan cara menambahkan kata kunci <code>implements HasMiddleware</code> pada controller kita, kemudian kita import agar tidak error.

```
use Illuminate\Routing\Controllers\HasMiddleware;
use Illuminate\Routing\Controllers\Middleware;
```

Selanjutnya kita bisa menggunakan sebuah method dari <code>interface HasMiddleware</code> seperti kode dibawah ini.

```
public static function middleware()
    {
        return [
            new Middleware('permission:users index', only : ['index']),
            new Middleware('permission:users create', only : ['create', 'store']),
            new Middleware('permission:users edit', only : ['edit', 'update   ']),
            new Middleware('permission:users delete', only : ['destroy']),
        ];
    }
```

### Langkah 3 - Menambahkan Route User

Setelah berhasil membuat sebuah controller Role, sekarang kita akan lanjutkan untuk pembuatan route-nya, silahkan teman - teman buka file <code>routes/web.php</code>, kemudian ubah kode-nya menjadi seperti berikut ini.

```
<?php

use App\Http\Controllers\ProfileController;
use App\Http\Controllers\PermissionController;
use App\Http\Controllers\RoleController;
use App\Http\Controllers\UserController;
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
    // roles route
    Route::resource('roles', RoleController::class)->except('show');
    // users route
    Route::resource('/users', UserController::class);
    Route::get('/profile', [ProfileController::class, 'edit'])->name('profile.edit');
    Route::patch('/profile', [ProfileController::class, 'update'])->name('profile.update');
    Route::delete('/profile', [ProfileController::class, 'destroy'])->name('profile.destroy');
});

require __DIR__ . '/auth.php';
```

Dari perubahan kode diatas, kita menambahkan sebuah route baru dengan nama users, untuk memastikan route yang kita buat telah berfungsi, teman - teman bisa jalankan.

```
php artisan r:l --name=users
```

![](https://i.imgur.com/GvVgBfz.png)

### Langkah 4 - Menambahkan Navigasi User

Silahkan buka <em>file</em> <code>resources/js/layouts/AuthenticatedLayout.jsx</code>, kemudian ubah kode-nya menjadi seperti berikut ini :

```
import ApplicationLogo from "@/Components/ApplicationLogo";
import Dropdown from "@/Components/Dropdown";
import NavLink from "@/Components/NavLink";
import ResponsiveNavLink from "@/Components/ResponsiveNavLink";
import { Link, usePage } from "@inertiajs/react";
import { useState } from "react";
import hasAnyPermission from "@/Utils/Permissions";
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
                                    href={route("dashboard")}
                                    active={route().current("dashboard")}
                                >
                                    Dashboard
                                </NavLink>

                                {hasAnyPermission(["permissions index"]) && (
                                    <NavLink
                                        href={route("permissions.index")}
                                        active={route().current("permissions*")}
                                    >
                                        Permissions
                                    </NavLink>
                                )}
                                {hasAnyPermission(["roles index"]) && (
                                    <NavLink
                                        href={route("roles.index")}
                                        active={route().current("roles*")}
                                    >
                                        Roles
                                    </NavLink>
                                )}
                                {hasAnyPermission(['users index']) &&
                                    <NavLink href={route('users.index')} active={route().current('users*')}>
                                        Users
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
                                            href={route("profile.edit")}
                                        >
                                            Profile
                                        </Dropdown.Link>
                                        <Dropdown.Link
                                            href={route("logout")}
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
                                        (previousState) => !previousState
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
                                                ? "inline-flex"
                                                : "hidden"
                                        }
                                        strokeLinecap="round"
                                        strokeLinejoin="round"
                                        strokeWidth="2"
                                        d="M4 6h16M4 12h16M4 18h16"
                                    />
                                    <path
                                        className={
                                            showingNavigationDropdown
                                                ? "inline-flex"
                                                : "hidden"
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
                        (showingNavigationDropdown ? "block" : "hidden") +
                        " sm:hidden"
                    }
                >
                    <div className="space-y-1 pb-3 pt-2">
                        <ResponsiveNavLink
                            href={route("dashboard")}
                            active={route().current("dashboard")}
                        >
                            Dashboard
                        </ResponsiveNavLink>
                        {hasAnyPermission(["permissions index"]) && (
                            <ResponsiveNavLink
                                href={route("permissions.index")}
                                active={route().current("permissions*")}
                            >
                                Permissions
                            </ResponsiveNavLink>
                        )}

                        {hasAnyPermission(["roles index"]) && (
                            <ResponsiveNavLink
                                href={route("roles.index")}
                                active={route().current("roles*")}
                            >
                                Roles
                            </ResponsiveNavLink>
                        )}

                        {hasAnyPermission(['users index']) &&
                            <ResponsiveNavLink href={route('users.index')} active={route().current('users*')}>
                                Users
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
                            <ResponsiveNavLink href={route("profile.edit")}>
                                Profile
                            </ResponsiveNavLink>
                            <ResponsiveNavLink
                                method="post"
                                href={route("logout")}
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

### Langkah 5 - Menampilkan data User

Setelah berhasil membuat sebuah Route dan Navigasi User, sekarang kita akan lanjutkan untuk pembuatan view-nya, disini kita akan membuat 3 view sekaligus yaitu <code>index</code>, <code>create</code>, dan <code>edit</code>

Silahkan teman - teman buat folder baru dengan nama <em>Users</em> kemudian didalam folder tersebut buat file baru dengan nama <code>Index.jsx</code> yang akan kita letakan di <code>resources/js/Pages</code>, kemudian ubah kodenya menjadi seperti berikut ini.

```
import React from "react";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout";
import Container from "@/Components/Container";
import Table from "@/Components/Table";
import Button from "@/Components/Button";
import Pagination from "@/Components/Pagination";
import { Head, usePage } from "@inertiajs/react";
import Search from "@/Components/Search";
import hasAnyPermission from "@/Utils/Permissions";
export default function Index({ auth }) {
    // destruct users props
    const { users, filters } = usePage().props;

    return (
        <AuthenticatedLayout
            user={auth.user}
            header={
                <h2 className="font-semibold text-xl text-gray-800 leading-tight">
                    Users
                </h2>
            }
        >
            <Head title={"Users"} />
            <Container>
                <div className="mb-4 flex items-center justify-between gap-4">
                    {hasAnyPermission(["users create"]) && (
                        <Button type={"add"} url={route("users.create")} />
                    )}
                    <div className="w-full md:w-4/6">
                        <Search
                            url={route("users.index")}
                            placeholder={"Search users data by name..."}
                            filter={filters}
                        />
                    </div>
                </div>
                <Table.Card title={"users"}>
                    <Table>
                        <Table.Thead>
                            <tr>
                                <Table.Th>#</Table.Th>
                                <Table.Th>User</Table.Th>
                                <Table.Th>Roles</Table.Th>
                                <Table.Th>Action</Table.Th>
                            </tr>
                        </Table.Thead>
                        <Table.Tbody>
                            {users.data.map((user, i) => (
                                <tr key={i}>
                                    <Table.Td>
                                        {++i +
                                            (users.current_page - 1) *
                                                users.per_page}
                                    </Table.Td>
                                    <Table.Td>
                                        {user.name}
                                        <div className="text-sm text-gray-400">
                                            {user.email}
                                        </div>
                                    </Table.Td>
                                    <Table.Td>
                                        <div className="flex items-center gap-2 flex-wrap">
                                            {user.roles.map((role, i) => (
                                                <span
                                                    className="inline-flex items-center px-3 py-1 rounded-full text-sm bg-sky-100 text-sky-700"
                                                    key={i}
                                                >
                                                    {role.name}
                                                </span>
                                            ))}
                                        </div>
                                    </Table.Td>
                                    <Table.Td>
                                        <div className="flex items-center gap-2">
                                            {hasAnyPermission([
                                                "users edit",
                                            ]) && (
                                                <Button
                                                    type={"edit"}
                                                    url={route(
                                                        "users.edit",
                                                        user.id
                                                    )}
                                                />
                                            )}
                                            {hasAnyPermission([
                                                "users delete",
                                            ]) && (
                                                <Button
                                                    type={"delete"}
                                                    url={route(
                                                        "users.destroy",
                                                        user.id
                                                    )}
                                                />
                                            )}
                                        </div>
                                    </Table.Td>
                                </tr>
                            ))}
                        </Table.Tbody>
                    </Table>
                </Table.Card>
                <div className="flex items-center justify-center">
                    {users.last_page !== 1 && (
                        <Pagination links={users.links} />
                    )}
                </div>
            </Container>
        </AuthenticatedLayout>
    );
}
```

Pada kode diatas, pertama - tama kita import beberapa file yang kita butuhkan

```
import React from "react";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout";
import Container from "@/Components/Container";
import Table from "@/Components/Table";
import Button from "@/Components/Button";
import Pagination from "@/Components/Pagination";
import { Head, usePage } from "@inertiajs/react";
import Search from "@/Components/Search";
import hasAnyPermission from "@/Utils/Permissions";
```

Selanjutnya kita membuat sebuah React Function Component dengan props auth.

```
export default function Index({auth})
```

Selanjutnya kita memanggil sebuah utils <code>hasAnyPermissions</code> yang telah kita buat sebelumnya untuk melakukan pengecekan apakah user memiliki hak akses untuk melakukan penambahan data <code>roles</code>.

```
{hasAnyPermission(['users create']) &&
  <Button type={'add'} url={route('users.create')}/>
}
```

### Langkah 6 - Membuat create User

Silahkan teman - teman  buat file baru dengan nama <code>Create.jsx</code> yang akan kita letakan di <code>resources/js/Pages/Users</code>, kemudian ubah kodenya menjadi seperti berikut ini.

```
import React from 'react'
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout';
import Container from '@/Components/Container';
import { Head, useForm, usePage } from '@inertiajs/react';
import Input from '@/Components/Input';
import Button from '@/Components/Button';
import Card from '@/Components/Card';
import Select2 from '@/Components/Select2';
import Swal from 'sweetalert2';
export default function Create({auth}) {

    // destruct roles from usepage props
    const { roles } = usePage().props;

    // define state with helper inertia
    const { data, setData, post, errors } = useForm({
        name : '',
        email: '',
        selectedRoles : [],
        password: '',
        password_confirmation: ''
    });

    // define method handleSelectedroles
    const formattedRoles = roles.map(role => ({
        value: role.name,
        label: role.name
    }));



    const handleSelectedRoles = (selected) => {
        const selectedValues = selected.map(option => option.value);
        setData('selectedRoles', selectedValues);
    }

    // define method handleStoreData
    const handleStoreData = async (e) => {
        e.preventDefault();

        post(route('users.store'), {
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
            header={<h2 className="font-semibold text-xl text-gray-800 leading-tight">Create User</h2>}
        >
            <Head title={'Create Users'}/>
            <Container>
                <Card title={'Create new user'}>
                    <form onSubmit={handleStoreData}>
                        <div className='mb-4'>
                            <Input label={'Name'} type={'text'} value={data.name} onChange={e => setData('name', e.target.value)} errors={errors.name} placeholder="Input name user.."/>
                        </div>
                        <div className='mb-4'>
                            <Input label={'Email'} type={'email'} value={data.email} onChange={e => setData('email', e.target.value)} errors={errors.email} placeholder="Input email user.."/>
                        </div>
                        <div className='mb-4'>
                        <div className='flex items-center gap-2 text-sm text-gray-700'>
                                    Roles
                                </div>
                        <Select2 onChange={handleSelectedRoles} options={formattedRoles} placeholder="Pilih Role..." />
                            {/* {selectedOptions && selectedOptions.length > 0 && (
                                <div>
                                    <h4>Rasa yang Anda pilih:</h4>
                                    <ul>
                                        {selectedOptions.map((option) => (
                                            <li key={option.value}>{option.label}</li>
                                        ))}
                                    </ul>
                                </div>
                            )} */}
                        </div>
                        {/* <div className='mb-4'>
                            <div className={`p-4 rounded-t-lg border bg-white`}>
                                <div className='flex items-center gap-2 text-sm text-gray-700'>
                                    Roles
                                </div>
                            </div>
                            <div className='p-4 rounded-b-lg border border-t-0 bg-gray-100'>
                                <div className='flex flex-row flex-wrap gap-4'>
                                    {roles.map((role, i) => (
                                        <Checkbox label={role.name} value={role.name} onChange={handleSelectedRoles} key={i}/>
                                    ))}
                                </div>
                                {errors.selectedRoles && <div className='text-xs text-red-500 mt-4'>{errors.selectedRoles}</div>}
                            </div>
                        </div> */}
                        <div className='mb-4'>
                            <Input label={'Password'} type={'password'} value={data.password} onChange={e => setData('password', e.target.value)} errors={errors.password} placeholder="Input password user.."/>
                        </div>
                        <div className='mb-4'>
                            <Input label={'Password Confirmation'} type={'password'} value={data.password_confirmation} onChange={e => setData('password_confirmation', e.target.value)} errors={errors.password_confirmation} placeholder="Input password confirmation..."/>
                        </div>
                        <div className='flex items-center gap-2'>
                            <Button type={'submit'} />
                            <Button type={'cancel'} url={route('users.index')}/>
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
import { Head, useForm, usePage } from '@inertiajs/react';
import Input from '@/Components/Input';
import Button from '@/Components/Button';
import Card from '@/Components/Card';
import Select2 from '@/Components/Select2';
import Swal from 'sweetalert2';
```

Kemudian kita membuat sebuah state menggunakan form helper dari inertia.

```
 // define state with helper inertia
    const { data, setData, post, errors } = useForm({
        name : '',
        email: '',
        selectedRoles : [],
        password: '',
        password_confirmation: ''
    });
```

Selanjutnya kita membuat sebuah method baru dengan nama handleSelectedRoles yang kita gunakan untuk menangkap sebuah value dari sebuah component select2.

```
const handleSelectedRoles = (selected) => {
        const selectedValues = selected.map(option => option.value);
        setData('selectedRoles', selectedValues);
    }
```

Selanjutnya kita membuat sebuah method baru dengan nama <code>handleStoreData</code> yang kita gunakan untuk mengirimkan data kita ke server menggunakan form helper yang telah disediakan oleh inertia.

```
const handleStoreData = async (e) => {
        e.preventDefault();

        post(route('users.store'), {
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

Jika kita perhatikan, pada method post kita arahkan ke sebuah route yang bernama <code>users.store</code> dan ketika data berhasil dikirimkan, kita memanggil sebuah <code>sweet alert</code>.

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

### Langkah 7 - Membuat edit User
