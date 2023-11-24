# Akses Tanpa Kata Sandi Keamanan Tinggi dengan SSH Keys
&nbsp; &nbsp; &nbsp;  SSH keys merupakan pasangan kunci kriptografis yang terdiri dari kunci publik(Public Keys) dan kunci pribadi (Private Keys). Kunci-kunci ini digunakan dalam protokol SSH (Secure Shell) untuk memberikan metode otentikasi yang lebih aman daripada menggunakan kata sandi saja. 
Kemudian apa itu Private Keys dan Public Keys? Private Keys merupakan kunci yang digunakan untuk membuktikan identitas Anda ketika Anda terhubung ke server yang memiliki kunci publik yang sesuai, sedangkan Public Keys merupakan bagian dari pasangan kunci SSH yang dapat dibagikan dan disalin ke server atau mesin yang ingin Anda akses.

Tanpa panjang lebar lagi, mari kita coba terapkan SSH Keys pada layanan KilatVM 2.0 CloudKilat.

### Langkah pertama - Melakukan generate SSH Keys.
Untuk melakukan generate SSH Keys Anda dapat mengikuti langkah berikut:
1. Buka Terminal
2. Ketikan Perintah berikut:
```
$ ssh-keygen -t rsa
```
3. Setelah perintah diatas dijalankan, maka akan muncul beberapa pertanyaan sebagai berikut:

```
Enter file in which to save the key (/root/.ssh/id_rsa):
```

Untuk pertanyaan ini Anda hanya perlu menekan tombol **ENTER**, yang mana file public key dan private key akan disimpan pada folder default service SSH. Namun apabila Anda ingin **menyimpan** ditempat lain Anda dapat **memasukan detail folder** untuk menyimpan kedua keys tersebut dan tekan **ENTER**.

```
Enter passphrase (empty for no passphrase):
```

Dilanjutkan:

```
Enter same passphrase again:
```

Untuk pertanyaan ini, apabila Anda ingin login tanpa memasukan password maka Anda hanya perlu menekan **ENTER** dan begitu pula sebaliknya apabila Anda ingin login dengan memasukan password maka harap Anda memasukan password yang diiginkan dan tekan **ENTER**.

4. Terakhir setelah melewati beberapa pertanyaan, maka generate SSH Keys telah berhasil dilakukan dan berikut screenshot detailnya: 

![SSH Keys](https://internal.s3-id-jkt-1.kilatstorage.id/KB/SSH%20Keys.png)

### Langkah Kedua - Meng-copy Public key ke remote server.
Untuk melakukan copy public key Anda dapat mengikuti langkah berikut:
1. Tetap diterminal
2. Ketikan Perintah berikut:

```
$ ssh-copy-id user@serverip

atau:

$ ssh-copy-id user@serverip -p(port SSH)
```
Perintah diatas merupakan perintah sederhana untuk meyalin public key anda ke folder SSH dari Kilat VM yang akan dilakukan remote dan setelah perintah itu dijalankan, nantinya Anda diminta untuk memasukan password dari akses ssh user root layanan Kilat VM Anda.

3. Setelah poin 2 dilakukan, maka Anda akan diinformasikan detail login KilatVM dengan menggunakan SSH keys sebagaimana lampiran berikut:

![SSH Copy Id](https://internal.s3-id-jkt-1.kilatstorage.id/KB/SSH%20Copy%20Id.png)

### Langkah Ketiga - Melakukan penyesuaian Config SSH.
Setelah menyalin *Public Key*, maka Anda dapat melakukan penyesuaian *config SSH*, yang mana Anda akan membatasi akses *KilatVM* menggunakan *Public Key* saja tanpa memasukan *password* lagi dan berikut langkahnya:
1.  Tetap diterminal dan login menggunakan informasi saat selesai menyalin public keys

```
$ ssh user@serverip

atau:

$ ssh user@serverip -p(port SSH)
```

2. Langkah selanjutnya setelah masuk ke KilatVM ikuti langkah berikut ini:
  - Buka file sshd_config untuk diedit. Lokasinya berbeda tergantung pada distribusi Linux atau sistem operasi yang Anda gunakan. Umumnya, file ini terletak di /etc/ssh/sshd_config dan berikut baris perintahnya:
 
  ```
  $ vi /etc/ssh/sshd_config
  atau
  $ vim /etc/ssh/sshd_config
  atau
  $ nano /etc/ssh/sshd_config
  ```

  - Pastikan bahwa opsi PubkeyAuthentication diatur ke yes. Ini memungkinkan otentikasi menggunakan kunci publik.

  ```
  PubkeyAuthentication yes
  ```  
  - Pastikan bahwa opsi AuthorizedKeysFile ditetapkan dengan benar. Ini menentukan lokasi file yang berisi kunci publik pengguna yang diizinkan.

  ```
  AuthorizedKeysFile      .ssh/authorized_keys
  ```

  - Jika Anda ingin mematikan otentikasi berbasis kata sandi dan hanya memungkinkan otentikasi kunci publik, pastikan bahwa opsi PasswordAuthentication diatur ke no.

  ```
  PasswordAuthentication no
  ```

3. Langkah terakhir setelah melakukan perubahan config SSH, lakukan restart service SSH dengan perintah:
```
$ systemctl restart sshd
```

Sekian informasi yang dapat kami sampaikan dan Apabila Anda mengalami kesuliatan dan kendala saat melakukan penambahan ssh keys, mohon untuk menghubungi tim support kami melalui tiket atau e-mail ke info@cloudkilat.com
