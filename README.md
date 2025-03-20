# Reflection

## Inside handle_connection method
1. Menerima TcpStream sebagai parameter \
Method handle_connection menerima stream yang merupakan tipe data TcpStream yang digunakan untuk membaca dan menulis data melalui koneksi TCP.
2. Membuat BufReader \
Kemudian, method handle_connection membuat BufReader yang membungkus stream untuk memproses data secara baris demi baris.
3. Membaca HTTP Request secara baris demi baris \
lines() digunakan untuk membaca aliran data dari stream satu baris pada satu waktu. Setelah itu, map() digunakan untuk mengubah hasil pembacaan menjadi String. Hal tersebut dilakukan sampai menemukan baris kosong menggunakan take_while(). Lalu, collect() untuk mengumpulkannya ke dalam Vec\<String> yang berisi baris-baris HTTP Request. Data tersebut disimpan pada variable http_request.
4. Mencetak HTTP Request \
Terakhir, method handle_connection mencetak isi dari http_request