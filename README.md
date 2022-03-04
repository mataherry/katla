# katla
Katla Bebas Minimalis

https://katla.deno.dev/

https://mataherry.github.io/katla/

- Murni Javascript tanpa framework
- Hanya bisa pakai tombol
- Tanpa animasi

Credits:

Wordle https://www.nytimes.com/games/wordle/index.html

Katla https://katla.vercel.app

# Tutorial
1. Tampilan Katla/Wordle sebenarnya cukup sederhana, 6 baris kotak-kotak, dan virtual keyboard QWERTY
![image_2022-03-04_172310](https://user-images.githubusercontent.com/832202/156745862-1bb780c0-641e-42cf-88dc-fd95631512ed.png)

- Pertama kita buat kotak-kotak menggunakan for loop untuk membuat input text dengan size 1 huruf sebanyak 6 x 5
  ```
  function buatKotak() {
    var kotak = document.getElementById('kotak')
    kotak.innerHTML = ''
    for(var i = 0; i < 6; i++) {
      kotak.innerHTML += '<div>'
      for(var j = 0; j < 5; j++) {
        kotak.innerHTML += '<input id="kotak' + i + j + '" type="text" max-length="1" size="1" readonly>'
      }
      kotak.innerHTML += '</div>'
    }
  }
  ```
  > Catatan: kita set attribute input menjadi readonly supaya tidak bisa diisi dengan keyboard fisik/HP secara manual (karena tampilannya akan terpotong dengan keyboard HP)

- Untuk membuat virtual keyboard, kita membuat variable yang berisi huruf-huruf sesuai keyboard, dan sekali lagi menggunakan for loop untuk membuat tombol-tombolnya
  ```
  function buatPapan() {
    var qwe = 'QWERTYUIOP'
    var asd = 'ASDFGHJKL'
    var zxc = 'ZXCVBNM'

    papan.innerHTML = '<div>'
    for(var i = 0; i < qwe.length; i ++) {
      papan.innerHTML += '<input id="papan' + qwe[i] + '" type="button" onclick="inset(this.value)" value="' + qwe[i] + '"></input>'
    }
    papan.innerHTML += '</div>'
    ...
  }

2. Logika permainan Katla/Wordle sebenarnya juga cukup sederhana, yaitu membandingkan huruf-huruf dalam tebakan dengan kunci jawaban
- Kita perlu melakukan 2x for loop, yang pertama untuk menentukan huruf-huruf yang letaknya sudah benar
- Kita menyimpan variable status untuk mengecek apakah sudah benar semua (2 = huruf dan letak sudah benar, 1 = huruf benar tapi letak salah, 0 = huruf tidak ada di jawaban)
- Huruf-huruf yang letaknya sudah benar kita eliminasi untuk loop kedua, jadi di loop kedua kita hanya membandingkan huruf-huruf yang letaknya masih salah (kita simpan dalam variable sisa) 
- Untuk huruf yang letaknya benar kita update background kotak dan papan dengan warna hijau
```
  function periksa() {
    ...
    for(var i = 0; i < tebakan[baris].length; i++) {
      if (jawaban[i] === tebakan[baris][i]) {
        document.getElementById('kotak' + baris + i).style.background = 'darkseagreen'
        document.getElementById('papan' + tebakan[baris][i].toUpperCase()).style.background = 'darkseagreen'
        status += '2'
      }
      else {
        status += '0'
        sisa += jawaban[i]
      }
    }
    ...
```

- Dalam for loop yang kedua, kita abaikan kalau status huruf-nya sudah benar (2), kalau ada huruf yang letaknya salah, kita update status-nya menjadi 1, dan juga menghapusnya dari variable sisa.
  > Catatan: function replace string hanya menghapus huruf yang ditemukan pertama kali, jadi kalau ada huruf yang dipakai berulang maka tetap akan dibandingkan lagi berikutnya   - Untuk huruf yang letaknya salah kita update background kotak dan papan dengan warna orange
- Sementara untuk huruf yang tidak ada dalam tebakan kita warnai dengan lightgrey
```
    for(var i = 0; i < tebakan[baris].length; i++) {
      if (status[i] === '2') {
        continue
      }
      else if (sisa.includes(tebakan[baris][i])) {
        document.getElementById('kotak' + baris + i).style.background = 'orange'
        document.getElementById('papan' + tebakan[baris][i].toUpperCase()).style.background = 'orange'
        status[i] = '1'
        sisa = sisa.replace(tebakan[baris][i], '')
      }
      else {
        document.getElementById('kotak' + baris + i).style.background = 'lightgrey'
        if(document.getElementById('papan' + tebakan[baris][i].toUpperCase()).style.background === '')
          document.getElementById('papan' + tebakan[baris][i].toUpperCase()).style.background = 'lightgrey'
      }
    }
    kolom = 0
    baris++
  }
```

Sekian tutorial singkat membuat Katla/Wordle dengan html, javascript, dan css

Untuk saran, komen, dan pertanyaan bisa add issue di project github ini, atau kontak saya di twitter [@mataherry](http://twitter.com/mataherry) atau e-mail mataherry@yahoo.com
