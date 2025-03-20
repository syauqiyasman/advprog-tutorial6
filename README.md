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