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
