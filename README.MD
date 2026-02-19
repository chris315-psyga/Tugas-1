Nama : Chris Maruli Siagian
NIM : 2481004

Latihan 1
Eksperimen 1: @Controller vs @RestController
/test/view = yang muncul adalah tulisan yang bertuliskan “Ini dari @Controller”
/test/text = yang muncul adalah tulisan yang bertuliskan “Ini dari @Controller + @ResponseBody”

Apa perbedaannya? Yang tanpa @ResponseBody itu menampilkan halaman web (HTML) karena mencari file template, sedangkan yang menggunakan @ResponseBody hanya menampilkan teks biasa secara langsung tanpa menggunakan file HTML.

Kesimpulan: @Controller tanpa @ResponseBody → return nama template (nama template/data langsung). Dengan @ResponseBody → return data langsung (nama template/data langsung).



Eksperimen 2: Apa Terjadi Jika Template Tidak Ada?
Apakah berhasil? [Ya/Tidak] = Ya
HTTP Status code: 500
Error message: Internal Server Error

Kesimpulan: Jika Controller return nama view yang tidak ada, Spring akan mengembalikan error 500 (Internal Server Error) karena file template HTML product/halaman-tidak-ada tidak ditemukan.


Eksperimen 3: Perbedaan @RequestParam dan @PathVariable
URL : /greet
Hasil di halaman : Selamat Pagi, Mahasiswa!

URL : /greet?name=Budi
Hasil di halaman : Selamat Pagi, Budi!

URL : /greet?name=Budi&waktu=Siang
Hasil di halaman : Selamat Siang, Budi!

URL : /greet/Ani
Hasil di halaman : Halo, Ani!

URL : /greet/Ani/detail
Hasil di halaman : Halo, Ani!

URL : /greet/Ani/detail?lang=EN
Hasil di halaman : Hello, Ani!

Pertanyaan:
•	URL mana yang pakai @RequestParam? /greet, /greet?name=Budi, /greet?name=Budi&waktu=Siang
•	URL mana yang pakai @PathVariable? /greet/Ani, /greet/Ani/detail
•	URL mana yang pakai keduanya? /greet/Ani/detail?lang=EN


Pertanyaan Refleksi
1. Apa perbedaan antara @Controller dan @RestController? Dalam kasus apa kamu pakai masing-masing?
Jawab
@Controller: Digunakan untuk membuat halaman website (HTML). Ia bekerja sama dengan template (Thymeleaf) untuk menyusun tampilan yang cantik di browser.
@RestController: Digunakan untuk memberikan data mentah. Biasanya dipakai untuk API yang dipanggil oleh aplikasi lain.
Pakai @Controller kalau buat website biasa (kayak latihan yang melelahkan ini). Pakai @RestController kalau cuma mau kirim data, bukan tampilan.

2. Perhatikan bahwa template product/list.html dipakai oleh 3 endpoint berbeda (list all, filter by category, search). Apa keuntungannya membuat template yang reusable seperti ini?
Jawab
- Untuk menghemat tenaga karena kita tidak perlu membuat 3 file HTML berbeda untuk fitur “Lihat Semua”, “Cari Kategori”, dan “pencarian”. Kita hanya perlu 1 file saja.
- Mudah dirawat karena kalau kita mau mengubah desain tabel (kayak ganti warna atau tambah kolom), kita cukup ubah di 1 tempat, dan perubahan itu otomatis berlaku untuk 3 fitur tersebut.

3. Kenapa Controller inject ProductService (bukan langsung akses data di ArrayList)? Apa yang terjadi kalau Controller langsung manage data?
Jawab
- Agar pekerjaan lebih rapi yang dimana tugas Controller itu hanya menerima permintaan user dan mengatur tampilan. Tugas mengelola data (CRUD) diserahkan ke Service.
- kalau controller langsung manage data maka kodenya akan berantakan ("Fat Controller"). Nanti kalau kita mau ganti cara menyimpan data (misalnya dari ArrayList ke Database), kita harus bongkar pasang Controller satu per satu. Kan capek ya dan nambah nambah kerjaan aja. Kalau pakai Service, Controller tidak perlu diubah sama sekali.

4. Apa perbedaan model.addAttribute("products", products) dengan return products langsung seperti di @RestController?
Jawab
- model.addAttribute: Kita menaruh data ke dalam "Baki Pengantar" (Model) agar bisa dibawa ke dapur (Template HTML) untuk dijadikan halaman web.
- Return Data Langsung: Data dikirim mentah-mentah ke browser/user tanpa dikemas ke dalam HTML. Biasanya ini terjadi di @RestController.

5. Jika kamu buka http://localhost:8080/products/abc (ID bukan angka), apa yang terjadi? Kenapa?
Jawab
Yang terjadi adalah Tuhan Yesus datang >_< yang terjadi adalah akan muncul error. Kenapa ? karena untuk menyelamatkan manusia dari kerajaan maut. Karena di Controller, kita menulis Long id (angka). Saat kamu memasukkan huruf "abc", Spring bingung karena tidak bisa mengubah huruf menjadi angka (Type Mismatch).

6. Apa keuntungan pakai @RequestMapping("/products") di level class dibanding menulis full path di setiap @GetMapping?
Jawab
- Supaya tidak capek mengetik karena kita tidak perlu menulis /products berkali kali sampai Tuhan Yesus datang di setiap method nya. Kita cukup tulis sekali saja di atas class dan di bawahnya tulis /search atau /categories. 
- Mudah untuk diganti karena kalau suatu saat kita mau mengganti alamat utama dari /products menjadi /items kita hanya perlu ubah 1 baris di atas class saja tanpa perlu ubah satu satu di bawahnya. 

7. Dalam lab ini, kata "Model" muncul dalam beberapa konteks berbeda. Sebutkan minimal 2 arti yang berbeda dan jelaskan perbedaannya. 
Hint: perhatikan Model di parameter method Controller, folder model/, dan class Product.
- Model di Parameter Controller: Ini adalah "Baki Pengantar". Objek ini fungsinya membawa data dari Java ke HTML agar bisa ditampilkan.
- model/ (Folder Package): Ini adalah "Kerangka Data" (POJO). Berisi class Java seperti Product.java yang mendefinisikan bentuk data apa saja yang dimiliki sebuah produk (ID, Nama, Harga, dll). Ini adalah blueprint datanya.



Latihan 2
Eksperimen 1: Fragment yang Tidak Ada
Di template mana saja, ubah referensi fragment menjadi nama yang tidak ada:
<div th:replace="~{fragments/layout :: navbar-yang-salah}"></div>
Jalankan dan buka halaman tersebut.

Apakah error? [Ya/Tidak] Ya
Error message: Internal Server Error

Kesimpulan: Jika nama fragment salah, Thymeleaf akan memberitahu bahwa ada error karena tidak dapat menemukan fragment tersebut.


Eksperimen 2: Static Resource Path
Coba hapus th: dari href CSS:
<!-- Dari: -->
<link rel="stylesheet" th:href="@{/css/style.css}">

<!-- Menjadi: -->
<link rel="stylesheet" href="/css/style.css">
Jalankan dan buka halaman. Apakah CSS masih ter-load?

CSS masih bekerja? [Ya/Tidak] Ya

Sekarang coba path yang salah:
<link rel="stylesheet" th:href="@{/css/tidak-ada.css}">

Apakah halaman error?      [Ya/Tidak] Tidak
Apakah CSS diterapkan?     [Ya/Tidak] Ya

Hal ini bisa terjadi karena adanya status 404 pada tidak-ada.css yang dimana file tersebut tidak ditemukan. Halaman masih terlihat bagus dan tidak ada error karena ada file CSS lain yakni bootstrap.min.css yang berstatus 200 dan berhasil dimuat sehingga menutupi kekurangan tersebut.
Kesimpulan: th:href="@{}" lebih baik karena tautannya lebih pintar dan fleksibel yakni mampu menyesuaikan folder aplikasi secara otomatis. Jika file CSS tidak ada, halaman web tidak akan error atau rusak hanya saja tampilannya saja yang akan berubah menjadi kurang rapih atau bisa dibilang berantakan.


Pertanyaan Refleksi
1. Apa keuntungan menggunakan Thymeleaf Fragment untuk navbar dan footer?
jawab
- Tidak capek mengetik karena kita hanya perlu membuat navbar dan footernya di satu file saja.
- Bisa dipanggil kemana saja yang dimana pada file html lain kita bisa panggil file yang berisi navbar dan footer.
- Lalu mudah untuk diedit karena kalau mau ubah menu navbar atau footernya maka kita hanya perlu perlu ubah di satu file fragmentnya saja dan itu akan otomatis berubah pada semua halaman yang memanggil file fragment tersebut.

2. Apa bedanya file di `static/` dan `templates/`? Kenapa CSS ada di `static/` bukan `templates/`?
jawab
- di templates, file file html nya harus dimasak dulu oleh server alias Thymeleaf. Server mengganti kode kode khusus seperti ${title} menjadi data asli, baru kemudian dikirim ke browser. 
- kalau di static, file file nya seperti CSS, gambar, dan JS itu sudah jadi. Browser bisa mengambilnya langsung tanpa perlu diolah oleh Server.
- Kenapa CSS ada di static dan bukan di templates hal itu dikarenakan CSS itu hanya file teks biasa yang isinya cuman warna dan tampilan doang sehingga browser tidak butuh server untuk mengubah isinya. Browser tinggal meminta file style.css dan kemudian server akan langsung memberikannya. 

3. Apa yang dimaksud dengan `th:replace` dan bagaimana bedanya dengan `th:insert`?
    - Hint: coba ganti `th:replace` jadi `th:insert` dan inspect element di browser
jawab
- Pada th:replace itu Thymeleaf akan menghapus total tag elemen pembungkus (host tag) yang memanggil fragment, dan menggantinya dengan seluruh isi tag Fragment. Contohnya kita punya <div th:replace="....navbar">. Pada browser, hasilnya adalah tag <div> itu hilang dan yang muncul adalah tag <nav> yang berasal dari fragment

- Pada th:insert itu Thymeleaf akan mempertahankan tag elemen pembungkus (host tag), dan memasukkan isi Fragment di dalamnya. Contohnya kita ada <div th:insert="...navbar">. Pada browser, hasilnya adalah tag <div> itu tetap ada. Nah di dalam <div> itulah baru muncul tag <nav> dari fragment. 

4. Kenapa kita pakai `@{}` untuk URL di Thymeleaf, bukan langsung tulis path?
jawab
karena @{} itu adalah URL expression yang memungkinkan Thymeleaf melakukan URL Rewriting (penulisan ulang URL) secara dinamis. Kalau kita tulis manual <a href="/products">, dan suatu hari sebelum Tuhan Yesus datang dan website kamu pindah server (misalnya jadi localhost:8080/akhir-zaman), maka linknya bisa putus.
tapi kalau kita pakai <a th:href="@{/products}">, Thymeleaf otomatis menyesuaikan alamatnya. Ia tahu kalau lagi di mana, jadi linknya akan selalu benar.

5. Perhatikan bahwa `ProductController` inject `ProductService` melalui Constructor Injection (konsep dari Week 3). Apa jadinya kalau Controller tidak pakai DI dan langsung `new ProductService()` di dalam Controller?
yang terjadi adalah = 
- Jadi penambah kerjaan yakni pekerjaan controllernya jadi lebih berat karena harus membuat data sendiri. Hidup udah susah malah dipersusah lagi. 
- Jadi sulit melakukan tes karena Saat melakukan Unit Testing, kita mau mengganti ProductService dengan "Palsu" (Mock Service) untuk menguji logika Controller tanpa perlu Database asli. Kalau kita pakai new, kita tidak bisa menukar Service tersebut karena dia sudah "dikunci keras" di dalam kode Controller. Dengan adanya DI memungkinkan kita untuk menyuntikkan "Palsu" tersebut dengan mudah.
- adanya masalah scope yang dimana Spring secara default mengelola Service sebagai Singleton (hanya dibuat SATU kali untuk seluruh aplikasi) agar hemat memori. Nah jika kita pakai new ProductService() pada setiap kali halaman diakses maka Controller akan membuat object Service baru secara berulang-ulang. Hal ini dapat memboros memori dan membuat data di dalam Service tidak konsisten (data di halaman A bisa beda dengan halaman B karena object-nya berbeda).
