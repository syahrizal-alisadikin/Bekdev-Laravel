Halo teman-teman semuanya, pada kesempatan kali ini kita semua akan belajar Queue Laravel,

Sebelum Mengikuti Tutorial ini, disarankan mengikuti Tutorial https://santrikoding.com/tutorial-kirim-email-di-laravel-menggunakan-gmail dan  https://santrikoding.com/tips-mengatasi-error-mengirim-email-menggunakan-smtp-gmail-di-laravel ya 😁



Oke tanpa lama-lama kita lanjut aja😁

**Apa itu Queue ?** Yap betul, antrian. Namun pada konteks sebuah aplikasi web biasa dikenal dengan *Task Queue.* Laravel Queue biasanya digunakan untuk memecah beban kerja pada suatu page atau API Endpoint agar response time menjadi lebih cepat. Contoh beban kerja  pada website yang biasanya dipecah dan akan dijalankan oleh Queue adalah :

• Mengirim email notifikasi (contoh : pada resgister user atau pemberitahuan waktu expired subscription)

• Export database dari file csv

• menggunakan **service** dari **third-party** (contoh : mengirim real-time notification)

• dll.



Pada dasarnya Laravel Queue adalah sistem yang menjalankan *Task Queue* ( antrian tugas) dengan beban kerja besar tapi dijalankan oleh *worker* dari background server. Job pada Laravel sendiri dapat diartikan sebagai tugas yang akan dimasukan kedalam antrian tertentu untuk dikerjakan oleh worker. Pada Laravel sendiri ada beberapa drivers yang didukung untuk memproses Task Queue yaitu database, [Amazon SQS](https://aws.amazon.com/sqs/), [Redis](https://redis.io/), dan [Beanstalkd](https://beanstalkd.github.io/).



Sumber : https://techblog.revivaltv.id/laravel-queue/amp/