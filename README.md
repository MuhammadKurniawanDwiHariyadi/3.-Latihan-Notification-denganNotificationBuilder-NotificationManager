# 3. Latihan Notification dengan NotificationBuilder dan NotificationManager

Ini adalah proyek latihan dari kursus **Android Fundamental: Background Task & Scheduler** di platform **Dicoding**.

## Pengenalan Notification
Notification adalah sebuah pesan dari aplikasi untuk memberitahu pengguna ketika ada informasi terbaru, dalam bentuk popup maupun item di status bar

### Membuat Notification
Menentukan informasi user interface dan action dari notifikasi dengan objek `NotificationCompat.Builder`. 
Untuk membuat notifikasi itu sendiri, bisa menggunakan `NotificationCompat.Builder.build()` yang akan **mengembalikan** sebuah objek notifikasi. 
Untuk memberikan notifikasi, Anda bisa menggunakan `NotificationManager.notify()`.

### Required Notification Content
Paramater yang harus ada pada notification
1. Small Icon, dapat diatur dengan `setSmallIcon()`.
2. Title, dapat diatur dengan `setContentTitle()`.
3. Detail Text, dapat diatur dengan `setContentText()`.

### Notification Actions
Tidak wajib ada tapi disarankan. Sebuah action dari notification untuk membuat informasi yang lebih detail atau notification lainya seperti membalas pesan whatsapp
Action di definisikan sebagai `PendingIntent` dan dimasukkan ke dalam `NotificationCompat.Builder`. Contohnya jika ingin membuka activity ketika notifikasi tersebut disentuh, 
bisa menggunakan `PendingIntent` dengan memanggil `setContentIntent()`. 

`var notificationIntent = Intent(this, MainActivity::class.java)` <br>
`var notificationPendingIntent = PendingIntent.getActivity(this, NOTIFICATION_ID, notificationIntent, PendingIntent.FLAG_UPDATE_CURRENT)` <br>
`var builder = NotificationCompat.Builder(this)` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setSmallIcon(R.drawable.notification_icon)` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setContentTitle("Hey, check this app now!")` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setContentText("Something important you need to know.")` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setContentIntent(notificationPendingIntent)` <br>

### Notification Priority
Diatur menjadi 5 tingkatan dari `PRIORITY_MIN (-2)` sampai `PRIORITY_MAX (2)` dengan default prioritas diatur dengan `PRIORITY_DEFAULT (0)`. <br>
Untuk mengatur prioritas dari notifikasi, Bisa dengan `NotificationCompat.Builder.setPriority()` kemudian di masukkan ke dalam `NotificationCompat`. <br>
Lalu yang bisa digunakan adalah 
|                                  Level                                |   Importance (Android 8.0 ke atas)  |   Priority (Android 7.1 ke bawah)   |
| --------------------------------------------------------------------- | ------------- | ------------- |
| **Urgent** <br> Muncul suara dan tampil secara sekilas (heads-up / peeking)  | `IMPORTANCE_HIGH` | `PRIORITY_HIGH` or `PRIORITY_MAX` | 
| **High** <br> Muncul suara | `IMPORTANCE_DEFAULT` | `PRIORITY_DEFAULT` |
| **Medium** <br> Tanpa suara | `IMPORTANCE_LOW` | `PRIORITY_LOW` |
| **Low** <br> Tanpa suara dan tidak muncul di status bar | `IMPORTANCE_MIN` | `PRIORITY_MIN` |

Contoh penggunanya <br>
`builder.setPriority(Notification.PRIORITY_HIGH)`


### Peeking
Sebuah popup sementara pada priority Max atau High yang mana notificaation akan mengintip dan tidak mempengaruhi aplikasi lain yang sedang berjalan, bisa di matikan di pengaturan. <br>
Contoh penggunaanya <br>
`val builder =` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`NotificationCompat.Builder(this)` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setSmallIcon(R.drawable.notification_icon)` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setContentTitle("My notification")` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setContentText("Hello World!")` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setPriority(NotificationCompat.PRIORITY_HIGH)` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setDefaults(NotificationCompat.DEFAULT_ALL)` <br>

### Expanded view layouts
Untuk memperbesar ukuran pada item notification pada status bar. <br>
Beberapa macam yang bisa digunakan yaitu: <br>
1. BigTextStyle, untuk notifikasi dengan isian text yang banyak.
2. InboxStyle, untuk notifikasi yang berisi list berupa text, biasanya diguanakan oleh aplikasi chatting.
3. MediaStyle, untuk notifikasi yang khusus menangani Media Player, di mana sudah tersedia tombol play, pause, maupun stop.
4. BigPictureStyle, untuk notifikasi yang menampilkan gambar di dalamnya, kita sering melihatnya dipakai oleh aplikasi seperti Instagram.<br>
   Contoh Big Picture Style <br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`val androidImage = BitmapFactory.decodeResource(resources, R.drawable.mascot_1)`
 <br> <br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`val notif = NotificationCompat.Builder(this)` <br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setContentTitle("New photo from " + sender.toString())`<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setContentText(subject)`<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setSmallIcon(R.drawable.new_post)`<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setLargeIcon(androidImage)`<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setStyle(NotificationCompat.BigPictureStyle()`<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.bigPicture(androidImage)`<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.setBigContentTitle("Large Notification Title"))`<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`.build()`<br>

### Notification pada Android O ke Atas
Mulai Android Oreo, Anda harus membuat channel agar notifikasi bisa berjalan. Channel di sini berfungsi untuk mengelompokkan notifikasi dalam beberapa grup seperti kanal (channel). 
Kelebihannya yaitu dapat mengatur notifikasi yang dalam satu channel tersebut dalam sekali waktu, seperti mengatur prioritas, suara, maupun getaran. Hal ini tentu akan memudahkan 
pengguna untuk mem-filter notifikasi yang masuk, sehingga meningkatkan kenyamanan user dalam menggunakan aplikasi.

Langkah yang harus dilakukan yaitu dengan menambahkan kode berikut: <br>
`if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`val channel = NotificationChannel(CHANNEL_ID, CHANNEL_NAME, NotificationManager.IMPORTANCE_DEFAULT)` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`builder.setChannelId(CHANNEL_ID)` <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`notificationManager.createNotificationChannel(channel)` <br>
`}`

### Notification pada Android 13 ke Atas
Mulai Android 13, juga ada peningkatan keamanan dengan mengharuskan developer men-request permission khusus untuk menampilkan notifikasi. 
Android 13 memerlukan `android.permission.POST_NOTIFICATIONS` untuk memberi pengguna lebih banyak kontrol atas notifikasi yang mereka terima. 
Jika aplikasi tidak memiliki permission ini, maka aplikasi tidak akan dapat mengirim notifikasi ke pengguna. Sebelumnya, semua aplikasi 
diizinkan untuk mengirim notifikasi tanpa persetujuan pengguna. Namun, dengan Android 13, pengguna harus memberikan persetujuan eksplisit 
sebelum aplikasi dapat mengirim notifikasi.



<br>
<br>
<br>
<br>
<br>
  
## Dalam aplikasi ini terdapat fitur


