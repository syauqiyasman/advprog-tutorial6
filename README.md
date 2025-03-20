# Reflection

## Milestone 1: Inside handle_connection method
1. Menerima TcpStream sebagai parameter \
Method handle_connection menerima stream yang merupakan tipe data TcpStream yang digunakan untuk membaca dan menulis data melalui koneksi TCP.
2. Membuat BufReader \
Kemudian, method handle_connection membuat BufReader yang membungkus stream untuk memproses data secara baris demi baris.
3. Membaca HTTP Request secara baris demi baris \
lines() digunakan untuk membaca aliran data dari stream satu baris pada satu waktu. Setelah itu, map() digunakan untuk mengubah hasil pembacaan menjadi String. Hal tersebut dilakukan sampai menemukan baris kosong menggunakan take_while(). Lalu, collect() untuk mengumpulkannya ke dalam Vec\<String> yang berisi baris-baris HTTP Request. Data tersebut disimpan pada variable http_request.
4. Mencetak HTTP Request \
Terakhir, method handle_connection mencetak isi dari http_request.

## Milestone 2: New handle_connection
Method handle_connection kali ini tidak lagi mencetak http_request ke terminal, tetapi menyusun HTTP Response.
1. Mendefinisikan status response \
Mendefinisikan status response "HTTP/1.1 200 OK" yang menunjukkan bahwa permintaan berhasil diproses.
```rust
let status_line = "HTTP/1.1 200 OK";
```
2. Membaca isi file html ke dalam string.
```rust
let contents = fs::read_to_string("hello.html").unwrap();
```
3. Menghitung panjang konten HTML yang dibaca.
```rust
let length = contents.len();
```
4. Menyusun Respons Lengkap
```rust
let response =
        format!("{status_line}\r\nContent-Length:{length}\r\n\r\n{contents}");
```
5. Mengirim respons ke client \
   Mengirimkan respons yang telah disusun ke klien dengan mengkonversi string respons menjadi byte array menggunakan as_bytes() dan mengirimkannya melalui stream.
```rust
stream.write_all(response.as_bytes()).unwrap();
```
Gambar response di web browser:
![Commit 2 screen capture](/assets/images/commit2.png)

## Milestone 3: how to split between response and why the refactoring is needed
Cara memisahkan respons satu dengan lainnya dapat dilakukan dengan cara melihat request method, path, dan HTTP version pada HTTP Headers.
Ketika request method GET, path /, dan HTTP version 1.1. Maka kita akan mengembalikan response hello.html, sementara untuk path dan method lainnya, akan kita kirim not found atau 404.html.

Refactoring penting karena membantu membuat kode lebih bersih, terstruktur, dan lebih mudah untuk dipelihara serta diperbarui. Ini mengurangi duplikasi kode, meningkatkan kualitas pengujian, dan membuat pengembangan lebih efisien dalam jangka panjang. Dengan refactoring, kita dapat memastikan bahwa aplikasi dapat tumbuh dan berkembang tanpa menjadi lebih rumit dan sulit untuk dipelihara.

![Commit 3 screen capture](/assets/images/commit3.png)

## Milestone 4: Why it works like that
Pada server yang berjalan dengan single-threaded model, server akan memproses permintaan satu per satu. Begitu server mulai memproses permintaan /sleep, permintaan berikutnya, seperti /, akan tertunda sampai permintaan pertama selesai diproses.

## Milestone 5: Multi-threading
ThreadPool ini memungkinkan kita untuk mengelola sekumpulan thread pekerja yang siap untuk mengeksekusi berbagai pekerjaan. Setiap pekerjaan dikirim melalui channel, dan worker thread mengambil pekerjaan tersebut untuk dijalankan. Dengan ini, kita bisa lebih efisien menangani banyak tugas secara paralel tanpa harus membuat dan menghancurkan thread secara berulang-ulang.
