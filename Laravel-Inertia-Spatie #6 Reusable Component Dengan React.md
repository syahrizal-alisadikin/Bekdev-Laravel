Pada project kali ini, kita akan membuat beberapa component menggunkan <code>reactjs</code>, tujuannya agar kita dapat membuat reusable UI (User Interface) yang dapat digunakan secara fleksibel. <strong>Component</strong> juga dapat digunakan untuk memisahkan logika tampilan dan logika dari apilkasi utama, sehingga mempermudah dalam pengembangan, pemeliharaan, dan pengujian kode.

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

