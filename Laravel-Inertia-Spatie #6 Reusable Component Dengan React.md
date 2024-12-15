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
