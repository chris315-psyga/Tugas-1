Nama : Chris Maruli Siagian
NIM : 2481004

Eksperimen 1: @Controller vs @RestController
Buka /test/view  → Apa yang muncul?   = yang muncul adalah tulisan yang bertuliskan “Ini dari @Controller”
Buka /test/text  → Apa yang muncul?   = yang muncul adalah tulisan yang bertuliskan “Ini dari @Controller + @ResponseBody”
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
1.	Apa perbedaan antara @Controller dan @RestController? Dalam kasus apa kamu pakai masing-masing?
Jawab
@Controller: Digunakan untuk membuat halaman website (HTML). Ia bekerja sama dengan template (Thymeleaf) untuk menyusun tampilan yang cantik di browser.
@RestController: Digunakan untuk memberikan data mentah. Biasanya dipakai untuk API yang dipanggil oleh aplikasi lain.
Pakai @Controller kalau buat website biasa (kayak latihan yang melelahkan ini). Pakai @RestController kalau cuma mau kirim data, bukan tampilan.

2.	Perhatikan bahwa template product/list.html dipakai oleh 3 endpoint berbeda (list all, filter by category, search). Apa keuntungannya membuat template yang reusable seperti ini?
Jawab
1. Untuk menghemat tenaga karena kita tidak perlu membuat 3 file HTML berbeda untuk fitur “Lihat Semua”, “Cari Kategori”, dan “pencarian”. Kita hanya perlu 1 file saja.
2. Mudah dirawat karena kalau kita mau mengubah desain tabel (kayak ganti warna atau tambah kolom), kita cukup ubah di 1 tempat, dan perubahan itu otomatis berlaku untuk 3 fitur tersebut.

3.	Kenapa Controller inject ProductService (bukan langsung akses data di ArrayList)? Apa yang terjadi kalau Controller langsung manage data?
Jawab
1. Agar pekerjaan lebih rapi yang dimana tugas Controller itu hanya menerima permintaan user dan mengatur tampilan. Tugas mengelola data (CRUD) diserahkan ke Service.
2. kalua controller langsung manage data maka kodenya akan berantakan ("Fat Controller"). Nanti kalau kita mau ganti cara menyimpan data (misalnya dari ArrayList ke Database), kita harus bongkar pasang Controller satu per satu. Kan capek ya dan nambah nambah kerjaan aja. Kalau pakai Service, Controller tidak perlu diubah sama sekali.

4.	Apa perbedaan model.addAttribute("products", products) dengan return products langsung seperti di @RestController?
Jawab
1. model.addAttribute: Kita menaruh data ke dalam "Baki Pengantar" (Model) agar bisa dibawa ke dapur (Template HTML) untuk dijadikan halaman web.
2. Return Data Langsung: Data dikirim mentah-mentah ke browser/user tanpa dikemas ke dalam HTML. Biasanya ini terjadi di @RestController.

5.	Jika kamu buka http://localhost:8080/products/abc (ID bukan angka), apa yang terjadi? Kenapa?
Jawab
Yang terjadi adalah Tuhan Yesus datang >_< yang terjadi adalah akan muncul error. Kenapa ? karena untuk menyelamatkan manusia dari kerajaan maut. Karena di Controller, kita menulis Long id (angka). Saat kamu memasukkan huruf "abc", Spring bingung karena tidak bisa mengubah huruf menjadi angka (Type Mismatch).

6.	Apa keuntungan pakai @RequestMapping("/products") di level class dibanding menulis full path di setiap @GetMapping?
Jawab
1. Supaya tidak capek mengetik karena kita tidak perlu menulis /products berkali kali sampai Tuhan Yesus datang di setiap method nya. Kita cukup tulis sekali saja di atas class dan di bawahnya tulis /search atau /categories. 
2. Mudah untuk diganti karena kalau suatu saat kita mau mengganti alamat utama dari /products menjadi /items kita hanya perlu ubah 1 baris di atas class saja tanpa perlu ubah satu satu di bawahnya. 

7.	Dalam lab ini, kata "Model" muncul dalam beberapa konteks berbeda. Sebutkan minimal 2 arti yang berbeda dan jelaskan perbedaannya. 
o	Hint: perhatikan Model di parameter method Controller, folder model/, dan class Product.
1. Model di Parameter Controller: Ini adalah "Baki Pengantar". Objek ini fungsinya membawa data dari Java ke HTML agar bisa ditampilkan.
2. model/ (Folder Package): Ini adalah "Kerangka Data" (POJO). Berisi class Java seperti Product.java yang mendefinisikan bentuk data apa saja yang dimiliki sebuah produk (ID, Nama, Harga, dll). Ini adalah blueprint datanya.

