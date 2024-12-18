Pada seri tutorial kali ini kita semua akan belajar  Crud (Create, Read, Update, Delete) Pada Modul Permission . 

### Langkah 1 - Membuat Controller Role

Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastiksan sudah berada di dalam <em>project</em> Laravel-nya.

```
php artisan make:controller RoleController -r
```


Jika perintah di atas berhasil dijalankan, maka kita akan mendapatkan 1 <em>file</em> <em>controller</em> baru dengan nama <code>RoleController.php</code> yang berada di dalam <em>folder</em> <code>app/Http/Controllers</code>.

Silahkan buka <em>file</em> tersebut, kemudian ubah kode-nya menjadi seperti berikut ini.

```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Routing\Controllers\HasMiddleware;
use Illuminate\Routing\Controllers\Middleware;
use Spatie\Permission\Models\Role;
use Spatie\Permission\Models\Permission;
class RoleController extends Controller implements HasMiddleware // Implement Middleware Spatie
{
    public static function middleware()
    {
        return [
            new Middleware('permission:roles index', only: ['index']),
            new Middleware('permission:roles create', only: ['create', 'store']),
            new Middleware('permission:roles edit', only: ['edit', 'update']),
            new Middleware('permission:roles delete', only: ['destroy']),
        ];
    }
    /**
     * Display a listing of the resource.
     */
    public function index(Request $request)
    {
        // get roles
        $roles = Role::select('id', 'name')
            ->with('permissions:id,name')
            ->when($request->search,fn($search) => $search->where('name', 'like', '%'.$request->search.'%'))
            ->latest()
            ->paginate(6);

        // render view
        return inertia('Roles/Index', ['roles' => $roles,'filters' => $request->only(['search'])]);
    }

    /**
     * Show the form for creating a new resource.
     */
    public function create()
    {
        // get permissions
        // $permissions = Permission::all();
        $data = Permission::orderBy('name')->pluck('name', 'id');
        $collection = collect($data);
        $permissions = $collection->groupBy(function ($item, $key) {
            // Memecah string menjadi array kata-kata
            $words = explode(' ', $item);

            // Mengambil kata pertama
            return $words[0];
        });
        // return $permissions;
        // render view
        return inertia('Roles/Create', ['permissions' => $permissions]);
    }

    /**
     * Store a newly created resource in storage.
     */
    public function store(Request $request)
    {
         // validate request
         $request->validate([
            'name' => 'required|min:3|max:255|unique:roles',
            'selectedPermissions' => 'required|array|min:1',
        ]);

        // create new role data
        $role = Role::create(['name' => $request->name]);

        // give permissions to role
        $role->givePermissionTo($request->selectedPermissions);

        // render view
        return to_route('roles.index');
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
    public function edit(Role $role)
    {
        // get permissions
        $data = Permission::orderBy('name')->pluck('name', 'id');
        $collection = collect($data);
        $permissions = $collection->groupBy(function ($item, $key) {
            // Memecah string menjadi array kata-kata
            $words = explode(' ', $item);

            // Mengambil kata pertama
            return $words[0];
        });

        // load permissions
        $role->load('permissions');

        // render view
        return inertia('Roles/Edit', ['role' => $role, 'permissions' => $permissions]);
    }

    /**
     * Update the specified resource in storage.
     */
    public function update(Request $request, Role $role)
    {
        // validate request
        $request->validate([
            'name' => 'required|min:3|max:255|unique:roles,name,'.$role->id,
            'selectedPermissions' => 'required|array|min:1',
        ]);

        // update role data
        $role->update(['name' => $request->name]);

        // give permissions to role
        $role->syncPermissions($request->selectedPermissions);

        // render view
        return to_route('roles.index');
    }
    /**
     * Remove the specified resource from storage.
     */
    public function destroy(Role $role)
    {
        // delete role data
        $role->delete();

        // render view
        return back();
    }
}
```

Dari perubahan kode di atas, pertama kita <em>import</em> <em>Model</em> <code>Role</code>, <code>Permission</code>dari <strong>Laravel Spatie Permission</strong>.

```
use Spatie\Permission\Models\Permission;
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
            new Middleware('permission:roles index', only: ['index']),
            new Middleware('permission:roles create', only: ['create', 'store']),
            new Middleware('permission:roles edit', only: ['edit', 'update']),
            new Middleware('permission:roles delete', only: ['destroy']),
        ];
    }
```

### Langkah 2 - Menambahkan Route Role

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
    
    Route::get('/profile', [ProfileController::class, 'edit'])->name('profile.edit');
    Route::patch('/profile', [ProfileController::class, 'update'])->name('profile.update');
    Route::delete('/profile', [ProfileController::class, 'destroy'])->name('profile.destroy');
});

require __DIR__ . '/auth.php';
```

Dari perubahan kode diatas, kita menambahkan sebuah route baru dengan nama roles, untuk memastikan route yang kita buat telah berfungsi, teman - teman bisa jalankan.

```
php artisan r:l --name=roles
```

![](https://i.imgur.com/3ASVJyB.png)

### Langkah 3 - Menambahkan Navigasi Role

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

### Langkah 4 - Menampilkan data Roles

Setelah berhasil membuat sebuah Route dan Navigasi Role, sekarang kita akan lanjutkan untuk pembuatan view-nya, disini kita akan membuat 3 view sekaligus yaitu <code>index</code>, <code>create</code>, dan <code>edit</code>

Silahkan teman - teman buat folder baru dengan nama <em>Roles</em> kemudian didalam folder tersebut buat file baru dengan nama <code>Index.jsx</code> yang akan kita letakan di <code>resources/js/Pages</code>, kemudian ubah kodenya menjadi seperti berikut ini.

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
    // destruct permissions props
    const { roles ,filters} = usePage().props;

    return (
        <AuthenticatedLayout
            user={auth.user}
            header={
                <h2 className="font-semibold text-xl text-gray-800 leading-tight">
                    Roles
                </h2>
            }
        >
            <Head title={"Roles"} />
            <Container>
                <div className="mb-4 flex items-center justify-between gap-4">
                    {hasAnyPermission(["roles create"]) && (
                        <Button type={"add"} url={route("roles.create")} />
                    )}
                    <div className="w-full md:w-4/6">
                        <Search
                            url={route("roles.index")}
                            placeholder={"Search roles data by name..."}
                            filter={filters}
                        />
                    </div>
                </div>
                <Table.Card title={"Roles"}>
                    <Table>
                        <Table.Thead>
                            <tr>
                                <Table.Th>#</Table.Th>
                                <Table.Th>Role Name</Table.Th>
                                <Table.Th>Permissions</Table.Th>
                                <Table.Th>Action</Table.Th>
                            </tr>
                        </Table.Thead>
                        <Table.Tbody>
                            {roles.data.map((role, i) => (
                                <tr key={i}>
                                    <Table.Td>
                                        {++i +
                                            (roles.current_page - 1) *
                                                roles.per_page}
                                    </Table.Td>
                                    <Table.Td>{role.name}</Table.Td>
                                    <Table.Td>
                                        <div className="flex items-center gap-2 flex-wrap">
                                            {role.name == "super-admin" ? (
                                                <span className="inline-flex items-center px-3 py-1 rounded-full text-sm bg-sky-100 text-sky-700">
                                                    all-permissions
                                                </span>
                                            ) : (
                                                role.permissions.map(
                                                    (permission, i) => (
                                                        <span
                                                            className="inline-flex items-center px-3 py-1 rounded-full text-sm bg-sky-100 text-sky-700"
                                                            key={i}
                                                        >
                                                            {role.name ==
                                                            "super-admin"
                                                                ? "all-permissions"
                                                                : permission.name}
                                                        </span>
                                                    )
                                                )
                                            )}
                                        </div>
                                    </Table.Td>
                                    <Table.Td>
                                        <div className="flex items-center gap-2">
                                            {hasAnyPermission([
                                                "roles edit",
                                            ]) && (
                                                <Button
                                                    type={"edit"}
                                                    url={route(
                                                        "roles.edit",
                                                        role.id
                                                    )}
                                                />
                                            )}
                                            {hasAnyPermission([
                                                "roles delete",
                                            ]) && (
                                                <Button
                                                    type={"delete"}
                                                    url={route(
                                                        "roles.destroy",
                                                        role.id
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
                    {roles.last_page !== 1 && (
                        <Pagination links={roles.links} />
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
{hasAnyPermission(['roles create']) &&
  <Button type={'add'} url={route('roles.create')}/>
}
```

### Langkah 5 - Membuat create Roles

Silahkan teman - teman  buat file baru dengan nama <code>Create.jsx</code> yang akan kita letakan di <code>resources/js/Pages/Roles</code>, kemudian ubah kodenya menjadi seperti berikut ini.

```
import React from "react";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout";
import Container from "@/Components/Container";
import { Head, useForm, usePage } from "@inertiajs/react";
import Input from "@/Components/Input";
import Button from "@/Components/Button";
import Card from "@/Components/Card";
import Checkbox from "@/Components/Checkbox";
import Swal from "sweetalert2";
export default function Create({ auth }) {
    // destruct permissions from usepage props
    const { permissions } = usePage().props;

    // define state with helper inertia
    const { data, setData, post, errors, processing } = useForm({
        name: "",
        selectedPermissions: [],
    });

    // define method handleSelectedPermissions
    const handleSelectedPermissions = (e) => {
        let items = data.selectedPermissions;

        items.push(e.target.value);

        setData("selectedPermissions", items);
    };

    // define method handleStoreData
    const handleStoreData = async (e) => {
        e.preventDefault();

        post(route("roles.store"), {
            onSuccess: () => {
                Swal.fire({
                    title: "Success!",
                    text: "Data created successfully!",
                    icon: "success",
                    showConfirmButton: false,
                    timer: 1500,
                });
            },
        });
    };

    return (
        <AuthenticatedLayout
            user={auth.user}
            header={
                <h2 className="font-semibold text-xl text-gray-800 leading-tight">
                    Create Role
                </h2>
            }
        >
            <Head title={"Create Roles"} />
            <Container>
                <Card title={"Create new role"}>
                    <form onSubmit={handleStoreData}>
                        <div className="mb-4">
                            <Input
                                label={"Role Name"}
                                type={"text"}
                                value={data.name}
                                onChange={(e) =>
                                    setData("name", e.target.value)
                                }
                                errors={errors.name}
                                placeholder="Input role name.."
                            />
                        </div>
                        <div className="mb-4">
                            {/* <div className={`p-4 rounded-t-lg border bg-white`}>
                                <div className="flex items-center gap-2 text-sm text-gray-700">
                                    Permissions
                                </div>
                            </div> */}
                            <div className="grid grid-cols-2 gap-4">
                                {Object.entries(permissions).map(
                                    ([group, permissionItems], i) => (
                                        <div
                                            key={i}
                                            className="p-4 bg-white rounded-lg shadow-md"
                                        >
                                            <h3 className="font-bold text-lg mb-2">
                                                {group}
                                            </h3>
                                            <div className="flex flex-wrap gap-2">
                                                {permissionItems.map(
                                                    (permission) => (
                                                        <Checkbox
                                                            label={permission}
                                                            value={permission}
                                                            onChange={
                                                                handleSelectedPermissions
                                                            }
                                                            key={permission}
                                                        />
                                                    )
                                                )}
                                            </div>
                                            {errors?.selectedPermissions && (
                                                <div className="text-xs text-red-500 mt-4">
                                                    {errors.selectedPermissions}
                                                </div>
                                            )}
                                        </div>
                                    )
                                )}
                            </div>
                        </div>
                        <div className="flex items-center gap-2">
                            <Button type={"submit"}  />
                            <Button
                                type={"cancel"}
                                url={route("roles.index")}
                            />
                        </div>
                    </form>
                </Card>
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
import { Head, useForm, usePage } from "@inertiajs/react";
import Input from "@/Components/Input";
import Button from "@/Components/Button";
import Card from "@/Components/Card";
import Checkbox from "@/Components/Checkbox";
import Swal from "sweetalert2";
```
Kemudian kita membuat sebuah state menggunakan form helper dari inertia.

```
const { data, setData, post, errors } = useForm({
        name: "",
        selectedPermissions: [],
    });
```

Selanjutnya kita membuat sebuah method baru dengan nama handleSelectedPermissions yang kita gunakan untuk menangkap sebuah value dari sebuah component checkbox.

```
const handleSelectedPermissions = (e) => {
        let items = data.selectedPermissions;

        items.push(e.target.value);

        setData("selectedPermissions", items);
    };
```

Selanjutnya kita membuat sebuah method baru dengan nama <code>handleStoreData</code> yang kita gunakan untuk mengirimkan data kita ke server menggunakan form helper yang telah disediakan oleh inertia.

```
const handleStoreData = async (e) => {
        e.preventDefault();

        post(route("roles.store"), {
            onSuccess: () => {
                Swal.fire({
                    title: "Success!",
                    text: "Data created successfully!",
                    icon: "success",
                    showConfirmButton: false,
                    timer: 1500,
                });
            },
        });
    };
```

Jika kita perhatikan, pada method post kita arahkan ke sebuah route yang bernama <code>roles.store</code> dan ketika data berhasil dikirimkan, kita memanggil sebuah <code>sweet alert</code>.

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

### Langkah 6 - Membuat edit Roles

Silahkan teman - teman  buat file baru dengan nama <code>Edit.jsx</code> yang akan kita letakan di <code>resources/js/Pages/Roles</code>, kemudian ubah kodenya menjadi seperti berikut ini.

```
import React from "react";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout";
import Container from "@/Components/Container";
import { Head, useForm, usePage } from "@inertiajs/react";
import Input from "@/Components/Input";
import Button from "@/Components/Button";
import Card from "@/Components/Card";
import Checkbox from "@/Components/Checkbox";
import Swal from "sweetalert2";
export default function Edit({ auth }) {
    // destruct permissions from usepage props
    const { permissions, role } = usePage().props;

    // define state with helper inertia
    const { data, setData, post, errors } = useForm({
        name: role.name,
        selectedPermissions: role.permissions.map(
            (permission) => permission.name
        ),
        _method: "put",
    });

    // define method handleSelectedPermissions
    const handleSelectedPermissions = (e) => {
        let items = data.selectedPermissions;

        if (items.includes(e.target.value))
            items.splice(items.indexOf(e.target.value), 1);
        else items.push(e.target.value);
        setData("selectedPermissions", items);
    };

    // define method handleUpdateData
    const handleUpdatedata = async (e) => {
        e.preventDefault();

        post(route("roles.update", role.id), {
            onSuccess: () => {
                Swal.fire({
                    title: "Success!",
                    text: "Data created successfully!",
                    icon: "success",
                    showConfirmButton: false,
                    timer: 1500,
                });
            },
        });
    };
    return (
        <AuthenticatedLayout
            user={auth.user}
            header={
                <h2 className="font-semibold text-xl text-gray-800 leading-tight">
                    Edit Role
                </h2>
            }
        >
            <Head title={"Edit Roles"} />
            <Container>
                <Card title={"Edit role"}>
                    <form onSubmit={handleUpdatedata}>
                        <div className="mb-4">
                            <Input
                                label={"Role Name"}
                                type={"text"}
                                value={data.name}
                                onChange={(e) =>
                                    setData("name", e.target.value)
                                }
                                errors={errors.name}
                                placeholder="Input role name.."
                            />
                        </div>
                        <div className="mb-4">
                            <div className="grid grid-cols-2 gap-4">
                                {Object.entries(permissions).map(
                                    ([group, permissionItems], i) => (
                                        <div
                                            key={i}
                                            className="p-4 bg-white rounded-lg shadow-md"
                                        >
                                            <h3 className="font-bold text-lg mb-2">
                                                {group}
                                            </h3>
                                            <div className="flex flex-wrap gap-2">
                                                {permissionItems.map(
                                                    (permission) => (
                                                        <Checkbox
                                                            label={permission}
                                                            value={permission}
                                                            onChange={
                                                                handleSelectedPermissions
                                                            }
                                                            defaultChecked={data.selectedPermissions.includes(
                                                                permission
                                                            )}
                                                            key={permission}
                                                        />
                                                    )
                                                )}
                                            </div>
                                            {errors?.selectedPermissions && (
                                                <div className="text-xs text-red-500 mt-4">
                                                    {errors.selectedPermissions}
                                                </div>
                                            )}
                                        </div>
                                    )
                                )}
                            </div>
                        </div>

                        <div className="flex items-center gap-2">
                            <Button type={"submit"}  />
                            <Button
                                type={"cancel"}
                                url={route("roles.index")}
                            />
                        </div>
                    </form>
                </Card>
            </Container>
        </AuthenticatedLayout>
    );
}
```

Pada kode diatas, pertama - tama kita import beberapa file yang kita butuhkan.

```
import React from "react";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout";
import Container from "@/Components/Container";
import { Head, useForm, usePage } from "@inertiajs/react";
import Input from "@/Components/Input";
import Button from "@/Components/Button";
import Card from "@/Components/Card";
import Checkbox from "@/Components/Checkbox";
import Swal from "sweetalert2";
```

Kemudian kita membuat sebuah state menggunakan form helper dari inertia.

```
const { data, setData, post, errors } = useForm({
        name: role.name,
        selectedPermissions: role.permissions.map(
            (permission) => permission.name
        ),
        _method: "put",
    });
```

Selanjutnya kita membuat sebuah method baru dengan nama handleSelectedPermissions yang kita gunakan untuk menangkap sebuah value dari sebuah component checkbox.

```
const handleSelectedPermissions = (e) => {
        let items = data.selectedPermissions;

        if (items.includes(e.target.value))
            items.splice(items.indexOf(e.target.value), 1);
        else items.push(e.target.value);
        setData("selectedPermissions", items);
    };
```

Selanjutnya kita membuat sebuah method baru dengan nama <code>handleUpdateData</code> yang kita gunakan untuk mengirimkan data kita ke server menggunakan form helper yang telah disediakan oleh inertia.

```
 const handleUpdatedata = async (e) => {
        e.preventDefault();

        post(route("roles.update", role.id), {
            onSuccess: () => {
                Swal.fire({
                    title: "Success!",
                    text: "Data created successfully!",
                    icon: "success",
                    showConfirmButton: false,
                    timer: 1500,
                });
            },
        });
    };
```

Sumber : [membuat-module-role-dengan-inertia-react](https://jurnalkoding.com/series/tutorial-laravel-inertia-roles-permissions/tutorial-inertia-roles-permissions-13-membuat-module-role-dengan-inertia-react) 

### kesimpulan

<p>Pada artikel ini, kita telah belajar membuat <strong>CRUD (Create,Read,Update,Delete) </strong>  <strong>Roles</strong>.</p>

<p>Jika teman-teman ada kendala saat belajar, silahkan bisa bertanya melalui kolom komentar atau <em>group</em> <strong>Telegram</strong> <strong>SantriKoding</strong>.</p>

Semoga bermanfaat! 😊


