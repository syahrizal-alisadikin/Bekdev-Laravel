Pada project kali ini, kita akan membuat beberapa component menggunkan <code>react js</code>, tujuannya agar kita dapat membuat reusable UI (User Interface) yang dapat digunakan secara fleksibel. <strong>Component</strong> juga dapat digunakan untuk memisahkan logika tampilan dan logika dari apilkasi utama, sehingga mempermudah dalam pengembangan, pemeliharaan, dan pengujian kode.

### Langkah 1 - Installasi Package 
Dikarenakan kita akan menggunakan <code>sweetAlert</code> dan package lain disini kita akan lakukan installasi terlebih dahulu, Silahkan jalankan perintah berikut ini di dalam terminal/CMD dan pastikan berada di dalam <em>project</em> Laravel-nya.

```
npm install clsx sweetalert2 @tabler/icons-react
```

![](https://i.imgur.com/HeXbhpd.png)

Pada perintah diatas kita melakukan installasi `3` buah package diantaranya sebagai berikut :
<ol>
  <li>
  <code>clsx</code>
  </li>
   <li>
  <code>sweetAlert</code>
  </li>
   <li>
  <code>@tabler/icons-react</code>
  </li>
</ol>

### Langkah 2 - Component Input
Kita lanjutkan untuk menambahkan <em>Component</em> <code>Input</code>.
Silahkan Buat file baru  dengan nama <code>Input.jsx</code> dan silahkan masukan kode berikut :

```
import React from 'react'

export default function Input({label, type, className, errors, ...props}) {
    return (
        <div className='flex flex-col gap-2'>
            <label className='text-gray-600 text-sm'>
                {label}
            </label>
            <input
                type={type}
                className={`w-full px-4 py-2 border text-sm rounded-md focus:outline-none focus:ring-0 bg-white text-gray-700 focus:border-gray-200 border-gray-200 ${className}`}
                {...props}
            />
            {errors && (
                <small className='text-xs text-red-500'>{errors}</small>
            )}
        </div>
    )
}
```
Pada kode diatas, pertama kita lakukan import <code>React</code> terlebih dahulu.

```
import React from 'react'
```

Selanjutnya kita membuat sebuah <strong>React Functional Component</strong> dengan beberapa props diantara-nya <code>label</code>, <code>type</code>, <code>className</code>, <code>errors</code> dan <code>...props</code>.

```
export default function Input({label, type, className, errors, ...props})
```
### Langkah 3 - Component Checkbox
Silahkan buka <em>file</em> <code>app/resources/js/Components/Checkbox.jsx</code>, kemudian ubah kode-nya menjadi seperti berikut ini :

```
export default function Checkbox({ label, ...props }) {
    return (
        <div>
            <div className="flex flex-row items-center gap-2">
                <input
                    {...props}
                    type="checkbox"
                    className={
                        "rounded-md bg-white border-gray-200 checked:bg-teal-500"
                    }
                />
                <label className="text-sm text-gray-700">{label}</label>
            </div>
        </div>
    );
}
```

Di atas kita menambahkan <em>props</em>  <strong> label</strong>.

### Langkah 4 - Component Container
Kita lanjutkan untuk menambahkan <em>Component</em> <code>Container</code>.
Silahkan Buat file baru  dengan nama <code>Container.jsx</code> dan silahkan masukan kode berikut :

```
export default function Container({children}) {
    return (
        <div className="py-12">
            <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                {children}
            </div>
        </div>
    )
}
```

pada kode diatas kita membuat sebuah <strong>React Functional Component</strong> dengan props  <code>children</code>.

### Langkah 5 - Component Textarea

Kita lanjutkan untuk menambahkan <em>Component</em> <code>Textarea</code>.
Silahkan Buat file baru  dengan nama <code>Textarea.jsx</code> dan silahkan masukan kode berikut :

```
export default function Textarea({label, className, errors,...props}) {
    return (
        <div className='flex flex-col gap-2'>
            <label className='text-gray-600 dark:text-gray-500 text-sm'>
                {label}
            </label>
            <textarea
                className={`w-full px-4 py-2 border text-sm rounded-md focus:outline-none focus:ring-0 bg-white text-gray-700 focus:border-gray-200 border-gray-200 ${className}`}
                {...props}
            />
            {errors && (
                <small className='text-xs text-red-500'>{errors}</small>
            )}
        </div>
    )
}
```

pada kode diatas kita membuat sebuah <strong>React Functional Component</strong> dengan beberapa props  <code>label</code>,<code>className</code>,<code>errors</code> dan <code>...props</code>.

### Langkah 6 - Component Search

Kita lanjutkan untuk menambahkan <em>Component</em> <code>Search</code>.
Silahkan Buat file baru  dengan nama <code>Search.jsx</code> dan silahkan masukan kode berikut :

```
import { useForm } from '@inertiajs/react';
import { IconSearch } from '@tabler/icons-react';
import React from 'react'
export default function Search({url, placeholder}) {

    // define use form inertia
    const {data, setData, get} = useForm({
        search : '',
    })

    // define method searchData
    const handleSearchData = (e) => {
        e.preventDefault();

        get(`${url}?search=${data.search}`)
    }

    return (
        <form onSubmit={handleSearchData}>
            <div className='relative'>
                <input
                    type='text'
                    value={data.search}
                    onChange={e => setData('search', e.target.value)}
                    className='py-2 px-4 pr-11 block w-full rounded-lg text-sm border focus:outline-none focus:ring-0 focus:ring-gray-400 text-gray-700 bg-white border-gray-200 focus:border-gray-200'
                    placeholder={placeholder}/>
                <div className='absolute inset-y-0 right-0 flex items-center pointer-events-none pr-4'>
                    <IconSearch size={18} strokeWidth={1.5}/>
                </div>
            </div>
        </form>
    )
}
```

Pada kode diatas, pertama kita import semua yang kita butuhkan.

```
import { useForm } from '@inertiajs/react';
import { IconSearch } from '@tabler/icons-react';
import React from 'react'
```
Selanjutnya kita membuat sebuah <strong>React Functional Component</strong> dengan beberapa props  <code>url</code> dan <code>placeholder</code>.

```
export default function Search({url, placeholder})
```

Didalam <strong>react function component</strong>, kita membuat sebuah state menggunakan form helper yang telah disediakan oleh inerita.

```
const {data, setData, get} = useForm({
    search : '',
})
```

Kemudian kita juga membuat sebuah method baru dengan nama <code>handleSearchData</code>, method ini kita gunakan untuk melakukan pencarian data, dan method ini dijalankan ketika form di submit.

```
// define method searchData
const handleSearchData = (e) => {
    e.preventDefault();

    get(`${url}?search=${data.search}`)
}
```
### Langkah 7 - Component Table 

Kita lanjutkan untuk menambahkan <em>Component</em> <code>Table</code>.
Silahkan Buat file baru  dengan nama <code>Table.jsx</code> dan silahkan masukan kode berikut :

```
import React from 'react'

const Card = ({ title, className, children }) => {
    return (
        <>
            <div className={`p-4 rounded-t-lg border ${className} bg-white`}>
                <div className='flex items-center gap-2 font-semibold text-sm text-gray-700 uppercase'>
                    {title}
                </div>
            </div>
            <div className='bg-white rounded-b-lg border-t-0'>
                {children}
            </div>
        </>

    )
}

const Table = ({ children }) => {
    return (
        <div className="w-full overflow-hidden overflow-x-auto border-collapse rounded-b-lg border border-t-0">
            <table className="w-full text-sm">
                {children}
            </table>
        </div>
    );
};

const Thead = ({ className, children }) => {
    return (
        <thead className={`${className} border-b bg-gray-50`}>{children}</thead>
    );
};

const Tbody = ({ className, children }) => {
    return (
        <tbody className={`${className} divide-y bg-white`}>
            {children}
        </tbody>
    );
};

const Td = ({ className, children}) => {
    return (
        <td
            className={`${className} whitespace-nowrap p-4 align-middle text-gray-700`}
        >
            {children}
        </td>
    );
};

const Th = ({ className, children }) => {
    return (
        <th
            scope="col"
            className={`${className} h-12 px-4 text-left align-middle font-medium text-gray-700`}
        >
            {children}
        </th>
    );
};

const Empty = ({colSpan, message, children}) => {
    return (
        <tr>
            <td colSpan={colSpan}>
                <div className="flex items-center justify-center h-96">
                    <div className="text-center">
                        {children}
                        <div className="mt-5">
                            {message}
                        </div>
                    </div>
                </div>
            </td>
        </tr>
    )
}

Table.Card = Card;
Table.Thead = Thead;
Table.Tbody = Tbody;
Table.Td = Td;
Table.Th = Th;
Table.Empty = Empty;

export default Table;
```

### Langkah 8 - Component Pagination

Kita lanjutkan untuk menambahkan <em>Component</em> <code>Pagination</code>.
Silahkan Buat file baru  dengan nama <code>Pagination.jsx</code> dan silahkan masukan kode berikut :

```
import React from 'react'
import { Link } from '@inertiajs/react';
import { IconChevronRight, IconChevronLeft } from '@tabler/icons-react';
export default function Pagination({ links }) {

    const style = 'p-1 text-sm border rounded-md bg-white text-gray-500 hover:bg-gray-100'

    return (
        <>
            <ul className="mt-2 lg:mt-5 justify-end flex items-center gap-1">
                {links.map((item, i) => {
                    return item.url != null ? (
                        item.label.includes('Previous') ? (
                            <Link className={style} key={i} href={item.url}>
                                <IconChevronLeft size={'20'} strokeWidth={'1.5'}/>
                            </Link>
                        ) : item.label.includes('Next') ? (
                            <Link className={style} key={i} href={item.url}>
                                <IconChevronRight size={'20'} strokeWidth={'1.5'}/>
                            </Link>
                        ) : (
                            <Link className={`px-2 py-1 text-sm border  rounded-md text-gray-500 hover:bg-gray-100 ${item.active ? 'bg-white text-gray-700' : 'bg-white'}`} key={i} href={item.url}>
                                {item.label}
                            </Link>
                        )
                    ) : null;
                })}
            </ul>
        </>
    )
}
```

Pada kode diatas, pertama kita import semua yang kita butuhkan.

```
import React from 'react'
import { Link } from '@inertiajs/react';
import { IconChevronRight, IconChevronLeft } from '@tabler/icons-react';
```

Selanjutnya kita membuat sebuah <strong>React Functional Component</strong> dengan sebuah props <code>links</code>.

```
export default function Pagination({ links })
```

### Langkah 9 - Component Button

Kita lanjutkan untuk menambahkan <em>Component</em> <code>Button</code>.
Silahkan Buat file baru  dengan nama <code>Button.jsx</code> dan silahkan masukan kode berikut :

```
import { Link, useForm } from '@inertiajs/react'
import { IconArrowBack, IconCheck, IconPencilCog, IconPlus, IconTrash } from '@tabler/icons-react';
import React from 'react'
import Swal from 'sweetalert2';
export default function Button({ type, url, className, children, ...props }) {

    const { delete : destroy } = useForm();

    const handleDeleteData = async (url) => {
        Swal.fire({
            title: 'Are you sure you want to delete this?',
            text: 'Data is unrecoverable!',
            icon: 'warning',
            showCancelButton: true,
            confirmButtonColor: '#3085d6',
            cancelButtonColor: '#d33',
            confirmButtonText: 'Yes, delete it!',
            cancelButtonText: 'Cancel'
        }).then((result) => {
            if (result.isConfirmed) {
                destroy(url)

                Swal.fire({
                    title: 'Success!',
                    text: 'Data deleted successfully!',
                    icon: 'success',
                    showConfirmButton: false,
                    timer: 1500
                })
            }
        })
    }

    return (
        <>
            {type === 'add' &&
                <Link href={url} className='px-4 py-2 text-sm border rounded-lg bg-white text-gray-700 flex items-center gap-2 hover:bg-gray-100'>
                    <IconPlus size={18} strokeWidth={1.5}/> <span className='hidden lg:flex'>Create New Data</span>
                </Link>
            }
            {type === 'modal' &&
                <button {...props} type='button' className={`${className} px-4 py-2 text-sm border rounded-lg flex items-center gap-2`}>
                    {children}
                </button>
            }
            {type === 'submit' &&
                <button type='submit' className='px-4 py-2 text-sm rounded-lg border border-teal-100 bg-teal-50 text-teal-500 flex items-center gap-2 hover:bg-teal-100'>
                    <IconCheck size={16} strokeWidth={1.5}/> Save Data
                </button>
            }
            {type === 'cancel' &&
                <Link href={url} className='px-4 py-2 text-sm rounded-lg border border-rose-100 bg-rose-50 text-rose-500 flex items-center gap-2 hover:bg-rose-100'>
                    <IconArrowBack size={16} strokeWidth={1.5}/> Go Back
                </Link>
            }
            {type === 'edit' &&
                <Link href={url} className='px-4 py-2 rounded-lg bg-orange-50 text-orange-500 flex items-center gap-2 hover:bg-orange-100'>
                    <IconPencilCog size={16} strokeWidth={1.5}/>
                </Link>
            }
            {type === 'delete' &&
                <button onClick={() => handleDeleteData(url)} className='px-4 py-2 rounded-lg bg-rose-50 text-rose-500 flex items-center gap-2 hover:bg-rose-100'>
                    <IconTrash size={18} strokeWidth={1.5}/>
                </button>
            }
        </>
    )
}
```

Pada kode diatas, pertama kita import semua yang kita butuhkan.

```
import { Link, useForm } from '@inertiajs/react'
import { IconArrowBack, IconCheck, IconPencilCog, IconPlus, IconTrash } from '@tabler/icons-react';
import React from 'react'
import Swal from 'sweetalert2';
```

Selanjutnya kita membuat sebuah <strong>React Functional Component</strong> dengan beberapa props <code>type</code>,<code>url</code>,<code>className</code>,<code>children</code> dan <code>...props</code>.

```
export default function Button({ type, url, className, children, ...props })
```

Didalam <strong>react function component</strong>, kita membuat sebuah state menggunakan form helper yang telah disediakan oleh inerita.

```
const { delete : destroy } = useForm();
```

Kemudian kita juga membuat sebuah method baru dengan nama <code>handleDeleteData</code> dan method ini kita tambahkan sebuah paramater <code>url> yang diambil dari props <code>url</code> yang kita kirimkan.

```
const handleDeleteData = async (url) => {
    Swal.fire({
        title: 'Are you sure you want to delete this?',
        text: 'Data is unrecoverable!',
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, delete it!',
        cancelButtonText: 'Cancel'
    }).then((result) => {
        if (result.isConfirmed) {
            destroy(url)

            Swal.fire({
                title: 'Success!',
                text: 'Data deleted successfully!',
                icon: 'success',
                showConfirmButton: false,
                timer: 1500
            })
        }
    })
}
```

Didalam method tersebut kita menggunakan <code>SweetAlert</code> untuk menampilkan jendela konfirmasi sebelum data benar-benar dihapus.

```
Swal.fire({
    title: 'Are you sure you want to delete this?',
    text: 'Data is unrecoverable!',
    icon: 'warning',
    showCancelButton: true,
    confirmButtonColor: '#3085d6',
    cancelButtonColor: '#d33',
    confirmButtonText: 'Yes, delete it!',
    cancelButtonText: 'Cancel'
})
```
Sumber : [membuat-reusable-component-dengan-react](https://jurnalkoding.com/series/tutorial-laravel-inertia-roles-permissions/tutorial-inertia-roles-permissions-8-membuat-reusable-component-dengan-react) 
### kesimpulan


<p>Pada artikel ini, kita telah belajar membuat <strong>Component </strong> Di <strong>React Js</strong>.</p>

<p>Jika teman-teman ada kendala saat belajar, silahkan bisa bertanya melalui kolom komentar atau <em>group</em> <strong>Telegram</strong> <strong>SantriKoding</strong>.</p>

Semoga bermanfaat! 😊

