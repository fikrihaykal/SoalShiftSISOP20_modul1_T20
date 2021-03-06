# SoalShiftSISOP20_modul1_T20
Praktikum Sistem Operasi 2020 - Modul 1
Departemen Teknologi Informasi

Kelompok T20

Anggota :
- Fikri Haykal
- Hana Ghaliyah Azhar


# Soal 1
Whits adalah seorang mahasiswa teknik informatika. Dia mendapatkan tugas praktikum untuk membuat laporan berdasarkan data yang ada pada file "Sample-Superstore". Namun dia tidak dapat menyelesaikan tugas tersebut. Laporan yang diminta berupa : <br />

Whits memohon kepada kalian yang sudah jago mengolah data untuk mengerjakan laporan tersebut. <br />
*Gunakan AWK dan Command pendukung <br />

### Soal 1a
- Tentukan wilayah bagian (region) mana yang memiliki keuntungan (profit) paling sedikit <br />

#### Algoritma Penyelesaian Soal 1a
- Menggunakan <b>gawk</b> untuk mencari seluruh baris/entry file <b>Sample-Superstore.tsv</b>. Kami mengelompokkan row/entry file tsv tersebut berdasarkan <b>Region</b> yang ada pada kolom 13. Setiap entry yang memiliki region yang sama, akan dijumlahkan total profitnya. Setelah itu kami menampilkan region dengan profit paling kecil dengan menggunakan sort by value. Kami juga menyimpan nama region dengan profit paling kecil sebagai variabel baru yang akan digunakan untuk soal 1b 

#### Pembahasan Soal 1a
- Pindah ke directory soal1 <br />
  Syntax : `cd Downloads/Modul1/soal1` <br />
- Membuat file shell "soal1.sh" <br />
  Syntax : `nano soal1.sh` <br />
- Script yang ada di dalam file <b>soal1.sh</b>
  ```
  #!/bin/bash 
  
  declare -a state 
  c=0
  
  read region profit <<< $(gawk -F "\t" 'NR>1 {summm[$13] += $21}
  END { 
  for(i in summm){
  print i, summm[i] 
  } 
  }' Sample-Superstore.tsv | LC_ALL=C sort -nrk2 | tail -1) 
  
  echo "Region dengan profit terendah adalah "$region" dengan profit sebesar "$profit 
  printf "\n\n" 
  ```
- Menjalankan file shell <b>soal1.sh</b> menggunakan bash <br />
  <b>Bash</b> berfungsi sebagai penerjemah antara user dan sistem operasi (kernel). <br />
  Syntax : `bash soal1.sh` <br />
  Tampilan <b>soal1.sh</b> setelah dijalankan
  ![hasil 1a](https://user-images.githubusercontent.com/26424136/75509861-9a71fd80-5a1b-11ea-9e4d-b74825f30c70.jpg) <br />
  Region `Central` memiliki profit paling sedikit yaitu sebesar `39706.4` 

### Soal 1b
- Tampilkan 2 negara bagian (state) yang memiliki keuntungan (profit) paling sedikit berdasarkan poin a <br />

#### Algoritma Penyelesaian Soal 1b
- Hampir sama dengan soal nomor 1a, kami menggunakan <b>gawk</b> untuk mencari seluruh baris/entry file <b>Sample-Superstore.tsv</b>. Kami mengelompokkan row/entry file tsv tersebut berdasarkan <b>State</b> yang ada pada kolom 11. Bedanya disini kami hanya menggunakan row yang regionnya bernilai hasil dari soal nomor 1a. Setiap entry yang memiliki state yang sama, akan dijumlahkan total profitnya. Setelah itu kami menampilkan 2 state dengan profit paling kecil dengan menggunakan sort by value. Kami juga menyimpan 2 nama state dengan profit paling kecil kedalam sebuah file temp <b>temp1.txt</b> yang akan digunakan untuk soal 1c

#### Pembahasan Soal 1b
 - Script yang ada di dalam file <b>soal1.sh</b>
   ```
   gawk -F "\t" -v reg=$region 'NR>1 {if($13==reg){summm[$11] += $21}}
   END {
     for(j in summm){
       print j, summm[j]
     }
   }' Sample-Superstore.tsv | LC_ALL=C sort -nrk2 | tail -2 | cut -d " " -f1 > temp1.txt
   
   echo "2 negara yang memiliki profit terendah di region "$region" adalah"
   while IFS= read -r line; 
   do 
     state+=($line) 
     echo "- "${state[c]} 
     c=$c+1 
   done < temp1.txt 
   printf "\n\n" 
   ```
- Tampilan file shell <b>soal1.sh</b> setelah dijalankan menggunakan syntax `bash` <br />
  ![hasil 1b](https://user-images.githubusercontent.com/26424136/75510834-ccd12a00-5a1e-11ea-970a-b7578c9b7278.jpg) <br />
  2 negara bagian (state) yang memiliki profit terendah diregion <b>Central</b> yaitu `Illinois dan Texas` <br />

### Soal 1c
- Tampilkan 10 produk (product name) yang memiliki keuntungan (profit) paling sedikit berdasarkan 2 negara bagian (state) hasil poin b <br />

#### Algoritma Penyelesaian Soal 1c
- Hampir sama dengan soal nomor 1b, kami menggunakan <b>gawk</b> untuk mencari seluruh baris/entry file <b>Sample-Superstore.tsv</b>. Kami mengelompokkan row/entry file tsv tersebut berdasarkan <b>Product Name</b>. Untuk kali ini kami menggunakan row yang statenya bernilai variabel yang direstore dari file <b>temp1.txt</b>. Setiap entry yang memiliki product name yang sama, akan dijumlahkan total profitnya. Setelah itu kami menampilkan 10 produk dengan profit paling kecil dengan menggunakan sort by value dari 2 state hasil 1b.

#### Pembahasan Soal 1c
- Script yang ada di dalam file <b>soal1.sh</b>
  ```
  echo "10 produk yang memiliki keuntungan paling rendah di negara "${state[0]}" atau negara "${state[1]}" adalah sebagai berikut."
  echo "-------------------------------------------------------------------" 
  
  gawk -F "\t" -v stat1=${state[0]} -v stat2=${state[1]} 'NR>1 {if(($11==stat1) || ($11==stat2)){summm[$17] += $21}} 
  END { 
    for(k in summm){ 
    print "- "k";"summm[k] 
    } 
  }' Sample-Superstore.tsv | LC_ALL=C sort -gk2 -t ";" | head -10 | cut -d ";" -f1
  
  rm temp1.txt
  ```
- Tampilan file shell <b>soal1.sh</b> setelah dijalankan menggunakan syntax `bash` <br />
  ![hasil 1c](https://user-images.githubusercontent.com/26424136/75511699-88935900-5a21-11ea-9269-732599ed7ef6.jpg) <br />

#### Tampilan Nomor 1 keseluruhan pada linux
- syntax `nano soal1.sh`
![1](https://user-images.githubusercontent.com/26424136/75511932-61895700-5a22-11ea-9794-31a33282d23f.PNG)
- syntax `bash soal1.sh`
![hasil 1](https://user-images.githubusercontent.com/26424136/75511880-2c7d0480-5a22-11ea-9925-73001db56f57.PNG) 
<br /> <br />


# Soal 2
Pada suatu siang, laptop Randolf dan Afairuzr dibajak oleh seseorang dan kehilangan data-data penting. Untuk mencegah kejadian yang sama terulang kembali mereka meminta bantuan kepada Whits karena dia adalah seorang yang punya banyak ide. Whits memikirkan sebuah ide namun dia meminta bantuan kalian kembali agar ide tersebut cepat diselesaikan. Idenya adalah kalian <br />

HINT: enkripsi yang digunakan adalah caesar cipher. <br />
*Gunakan Bash Script <br />

### Soal 2a dan 2b 
- membuat sebuah script bash yang dapat menghasilkan password secara acak sebanyak 28 karakter yang terdapat huruf besar, huruf kecil, dan angka. <br />
- Password acak tersebut disimpan pada file berekstensi .txt dengan nama berdasarkan argumen yang diinputkan dan <b>HANYA berupa alphabet</b>. <br />

#### Algoritma Penyelesaian Soal 2a dan 2b
- Mengambil argumen untuk digunakan sebagai penamaan file untuk menyimpan password. Namun, argumen tersebut difilter untuk dihilangkan numeric dan symbol yang ada. Setelah itu dilakukan generating password yang terdiri dari huruf kapital, huruf kecil serta angka dengan panjang 28 karakter. Hasil generate password tersebut disimpan kedalam sebuah file dengan nama dari argumen yang sudah difilter tadi

#### Pembahasan Soal 2a dan 2b
- Pindah ke directory soal2 <br />
  Syntax : `cd ../soal2` <br />
- Membuat file shell <b>soal2.sh</b> <br />
  File ini digunakan untuk generate random password yang membutuhkan argument untuk penamaan file<br />
  Syntax : `nano soal2.sh` <br />
  Script bash yang ada didalam file <b>soal2.sh</b> <br />
  ```
  #!bin/bash
  
  read lowerfile <<< $(echo $1 | tr '[:upper:]' '[:lower:]' | cut -d "." -f1 | sed 's/[0-9]//g')
  lowerfile+=.txt

  head /dev/urandom | tr -dc A-Za-z0-9 | head -c 28 > $lowerfile
  ```
- Menjalankan file <b>soal2.sh</b> dan membuat file berekstensi `.txt` dengan nama <b>sisop.txt</b>. <br /> 
  Syntax : `bash soal2.sh sisop.txt` <br />
- Memastikan file <b>sisop.txt</b> sudah berada di directory `soal2` <br />
  Syntax : `ls` <br />
  <b>ls</b> digunakan untuk menampilkan file-file pada directory tersebut. <br />
- Menampilkan isi file <b>sisop.txt</b> <br />
  Syntax : `cat sisop.txt` <br />
  Password pada file <b>sisop.txt</b> : <br/> 
  ```
  Au8dktuXbekPYUKJK8UpkY1IrqfA
  ```
  File <b>sisop.txt</b> berisi password acak sebanyak 28 karakter yang terdapat huruf besar, huruf kecil, dan angka. <br />
##### Berikut tampilan `nomor 2 a dan b` pada linux:
  ![2a](https://user-images.githubusercontent.com/16980689/75595198-f4c39a80-5abd-11ea-8073-40c6d2a55499.PNG)

### Soal 2c
- Kemudian supaya file .txt tersebut tidak mudah diketahui maka nama filenya akan dienkripsi dengan menggunakan konversi huruf (string manipulation) yang disesuaikan dengan jam(0-23) dibuatnya file tersebut dengan program terpisah dengan (misal: password.txt dibuat pada jam 01.28 maka namanya berubah menjadi qbttxpse.txt dengan perintah ‘bash soal2_enkripsi.sh password.txt’. Karena p adalah huruf ke 16 dan file dibuat pada jam 1 maka 16+1=17 dan huruf ke 17 adalah q dan begitu pula seterusnya. Apabila melebihi z, akan kembali ke a, contoh: huruf w dengan jam 5.28, maka akan menjadi huruf b. <br />

#### Algoritma Penyelesaian Soal 2c
- Mengambil argumen yang merupakan nama file untuk dienkripsi, kami menghapus ekstensi <b>.txt</b> dari argumen tersebut agar tidak ikut terenkripsi. Selain itu, kami juga mengambil <b>time</b> untuk key dari caesar cipher. Setelah itu, dengan formula caesar cipher, kami mengenkripsi argumen tadi dengan key dari jam yang sudah didapat. Setelah proses pengenkripsian selesai, kami menambahkan ekstensi <b>.txt</b> kedalam argumen tadi. Selanjutnya kami merubah nama file yang dienkripsikan

#### Pembahasan Soal 2c
- Membuat file shell <b>soal2_enkripsi.sh</b> <br />
  File ini digunakan untuk mengenkripsi file. Penggunaannya adalah dengan menambahkan argument nama file yang akan dienkripsi<br />
  Syntax : `nano soal2_enkripsi.sh` <br />
  Script bash yang ada didalam file <b>soal2_enkripsi.sh</b> <br />
  ```
  #!/bin/bash

  read lowerfile <<< $(echo $1 | tr '[:upper:]' '[:lower:]' | cut -f1 -d "." | sed 's/[0-9]//g')
  #echo $lowerfile

  jam=$(date +"%I")

  export A=$(echo {a..z} | sed -r 's/ //g')
  export C=$(echo $A | sed -r "s/^.{$jam}//g")$(echo $A | sed -r "s/.{$(expr 26 - $jam)}$//g")
  
  read namafile <<< $(echo $lowerfile | tr '[A-Z]' $A | tr $A $C)
  namafile+=.txt
  
  lowerfile+=.txt
  
  mv $lowerfile $namafile
  ```
- Menjalankan file <b>soal2_enkripsi.sh</b> dan file yang berekstensi `.txt` dengan nama <b>sisop.txt</b>. <br /> 
  Syntax : `bash soal2_enkripsi.sh sisop.txt` <br />
##### Berikut tampilan <b>nomor 2 c</b> pada linux :  
![2b](https://user-images.githubusercontent.com/16980689/75595990-2db13e80-5ac1-11ea-8b4b-4528480c95de.PNG)
- Memastikan file <b>sisop.txt</b> sudah berganti nama pada directory `soal2` <br />
  Syntax : `ls` <br />
  Nama file `sisop.txt` berubah menjadi <b>`brbxy.txt`</b> setelah melakukan enkripsi.

### Soal 2d
- Jangan lupa untuk membuat dekripsinya supaya nama file bisa kembali. <br />

#### Algoritma Penyelesaian Soal 2d
- Hampir sama dengan penyelesaian soal 2c, kami mengambil argumen yang merupakan nama file untuk didekripsi, kami menghapus ekstensi <b>.txt</b> dari argumen tersebut agar tidak ikut terdekripsi. Selain itu, kami juga mengambil <b>time</b> untuk key dari caesar cipher. Setelah itu, dengan formula caesar cipher, kami mengdekripsi argumen tadi dengan key dari jam yang sudah didapat. Setelah proses pengdekripsian selesai, kami menambahkan ekstensi <b>.txt</b> kedalam argumen tadi. Selanjutnya kami merubah nama file yang didekripsikan

#### Pembahasan Soal 2d
- Membuat file shell <b>soal2_dekripsi.sh</b> <br />
  File ini digunakan untuk mendekripsi file. Penggunaannya adalah dengan menambahkan argument nama file yang akan didekripsi<br />
  Syntax : `nano soal2_dekripsi.sh` <br />
  Script bash yang ada didalam file <b>soal2_dekripsi.sh</b> <br />
  ```
  #!/bin/bash

  read lowerfile <<< $(echo $1 | tr '[:upper:]' '[:lower:]' | cut -f1 -d "." | sed 's/[0-9]//g')

  jam=$(date +"%I")

  export A=$(echo {a..z} | sed -r 's/ //g')
  export C=$(echo $A | sed -r "s/^.{$jam}//g")$(echo $A | sed -r "s/.{$(expr 26 - $jam)}$//g")

  read deknama <<< $(echo $lowerfile | tr '[A-Z]' $A | tr $C $A)
  deknama+=.txt
  lowerfile+=.txt

  mv $lowerfile $deknama

  ```
- Menjalankan file <b>soal2_dekripsi.sh</b> dan file yang berekstensi `.txt` dengan nama <b>brbxy.txt</b>. <br /> 
  Syntax : `bash soal2_dekripsi.sh brbxy.txt` <br />
  
##### Berikut tampilan <b>nomor 2 d</b> pada linux :  
![2c](https://user-images.githubusercontent.com/16980689/75596580-c1840a00-5ac3-11ea-958e-ac5534076c30.PNG)
- Memastikan file <b>brbxy.txt</b> sudah berganti nama pada directory `soal2` <br />
  Syntax : `ls` <br />
  Nama file `brbxy.txt` berubah ke nama file awal yaitu <b>`sisop.txt`</b> setelah melakukan dekripsi.


# Soal 3
1 tahun telah berlalu sejak pencampakan hati Kusuma. Akankah sang pujaan hati kembali ke naungan Kusuma? Memang tiada maaf bagi Elen. Tapi apa daya hati yang sudah hancur, Kusuma masih terguncang akan sikap Elen. Melihat kesedihan Kusuma, kalian mencoba menghibur Kusuma dengan mengirimkan gambar kucing. <br />
- Maka dari itu, kalian mencoba membuat script untuk mendownload 28 gambar dari https://loremflickr.com/320/240/cat menggunakan command wget dan menyimpan file dengan nama "pdkt_kusuma_NO" (contoh: pdkt_kusuma_1, pdkt_kusuma_2, pdkt_kusuma_3) serta jangan lupa untuk menyimpan log messages wget kedalam sebuah file wget.log . Karena kalian gak suka ribet, kalian membuat penjadwalan untuk menjalankan script download gambar tersebut. Namun, script download tersebut hanya berjalan <br />
- Setiap 8 jam dimulai dari jam 6.05 setiap hari kecuali hari Sabtu. Karena gambar yang didownload dari link tersebut bersifat random, maka ada kemungkinan gambar yang terdownload itu identik. Supaya gambar yang identik tidak dikira Kusuma sebagai spam, maka diperlukan sebuah script untuk memindahkan salah satu gambar identik. Setelah memilah gambar yang identik, maka dihasilkan gambar yang berbeda antara satu dengan yang lain. Gambar yang berbeda tersebut, akan kalian kirim ke Kusuma supaya hatinya kembali ceria. Setelah semua gambar telah dikirim, kalian akan selalu menghibur Kusuma, jadi gambar yang telah terkirim tadi akan kalian simpan kedalam folder ./kenangan dan kalian bisa mendownload gambar baru lagi. <br />
- Maka dari itu buatlah sebuah script untuk mengidentifikasi gambar yang identik dari keseluruhan gambar yang terdownload tadi. Bila terindikasi sebagai gambar yang identik, maka sisakan 1 gambar dan pindahkan sisa file identik tersebut ke dalam folder ./duplicate dengan format filename duplicate_nomor (contoh : duplicate_200, duplicate_201). Setelah itu lakukan pemindahan semua gambar yang tersisa kedalam folder ./kenangan dengan format filename kenangan_nomor (contoh: kenangan_252, kenangan_253). Setelah tidak ada gambar di current directory , maka lakukan backup seluruh log menjadi ekstensi .log.bak . <br />

Hint : Gunakan wget.log untuk membuat location.log yang isinya merupakan hasil dari grep "Location". <br />

#### Algoritma Penyelesaian Soal 3
- Mengunduh 28 gambar dari link yang disediakan dengan <b>looping wget</b>. Jika sudah terdownload, maka akan membuat folder `kenangan` dan folder `duplicate` jika belum tersedia. Kemudian mengecek setiap gambar `pdkt_kusuma_x.jpg` dengan gambar yang ada di dalam folder `kenangan` untuk dideteksi merupakan gambar yang sama atau bukan (jika folder kenangan kosong, maka langsung dipindah dalam folder kenangan). Gambar yang sama akan dipindah ke dalam folder `duplicate` dengan nama `duplicate_x.jpg`, sedangkan gambar yang berbeda akan disimpan dalam folder `kenangan` dengan nama `kenangan_x.jpg`. Metode yang kami gunakan untuk membandingkan kedua gambar adalah dengan mengkonversikan kedua gambar tersebut kedalam format <b>rgba</b>. Tidak lupa kami menyimpan dan membuat backup gambar yang diunduh tadi kedalam file log.
- Agar program ini dapat dieksekusi otomatis sesuai dengan jam yang diminta, kami menggunakan perintah `crontab -e` dan menyetel command. Tidak lupa menambahkan `chmod +x soal3.sh` agar file dapat dieksekusi

### Pembahasan Soal 3 
- Pindah ke directory soal3 <br />
  Syntax : `cd ../soal3` <br />
- Membuat file shell <b>soal3.sh</b> <br />
  File ini akan mengeksekusi sesuai permintaan semua sub-soal nomor 3<br />
  Syntax : `nano soal3.sh` <br />
  Script bash ada didalam file <b>soal3.sh</b> <br />
  ```
  #!bin/bash/

  for((i=1; i<29; i++))
  do
    wget -cO "pdkt_kusuma_$i.jpg" -a "wget.log" "https://loremflickr.com/320/240/cat"
  done

  if [ ! -d kenangan ]
  then
    mkdir kenangan
    mkdir duplicate
  fi

  for ((j=1; j<29; j++))
  do
    if [ -f "pdkt_kusuma_$j.jpg" ]
    then
      jml_kng=$(ls kenangan | wc -l)
      for ((k=1; k<=jml_kng; k++))
      do
        convert pdkt_kusuma_$j.jpg gambar_asli.rgba
        convert kenangan/kenangan_$k.jpg gambar_checker.rgba

        cmp -s {gambar_asli,gambar_checker}.rgba
        hasil=$?

        if [[ $hasil -eq 0 ]]
        then
          gambar_duplikat=$(ls duplicate | wc -l)
          mv pdkt_kusuma_$j.jpg duplicate/duplicate_$(($gambar_duplikat+1)).jpg
          k=jml_kng+1
        fi
      done
      if [ -f "pdkt_kusuma_$j.jpg" ]
      then
        mv pdkt_kusuma_$j.jpg kenangan/kenangan_$(($jml_kng+1)).jpg
      fi
    fi
  done

  cp wget.log wget.log.bak
  rm *.rgba
  ```
- Menjalankan file <b>soal3.sh</b> <br /> 
  Syntax : `bash soal3.sh` <br />
  
##### Tampilan `nomor 3a` pada linux setelah file `soal3.sh` dijalankan
  ![3a](https://user-images.githubusercontent.com/16980689/75597373-b8953780-5ac7-11ea-84f7-7ac66ff07cea.PNG)
  
##### Tampilan `nomor 3b` pada linux setelah file `soal3.sh` dijalankan</b>
  ![3kenangan](https://user-images.githubusercontent.com/16980689/75597379-be8b1880-5ac7-11ea-97d8-0a27d818a11a.PNG)
  
##### Tampilan `nomor 3c` pada linux setelah file `soal3.sh` dijalankan</b>
  ![3duplicate](https://user-images.githubusercontent.com/16980689/75597378-bd59eb80-5ac7-11ea-8811-780ac79a1f86.PNG)
  
##### Terdapat 2 folder (kenangan dan duplicate) pada directory soal3</b>
  ![3b](https://user-images.githubusercontent.com/16980689/75597377-bc28be80-5ac7-11ea-9f48-1b745774013a.PNG)
  
##### Selain itu terdapat file `crontab` yang berisi perintah crontab sesuai yang diminta soal </b>
![crontab](https://user-images.githubusercontent.com/16980689/75597233-df06a300-5ac6-11ea-9303-1d7bf5e7b6b0.PNG)
- Agar file shell dapat dieksekusi <br />
  Syntax : `chmod +x soal3.sh`
- Untuk menambahkan crontab (disesuaikan dengan file user) <br />
  Syintax : `crontab -e` <br />
  Script yang ada didalam file <b>crontab</b> 
  ```
  5 6,*/8 * * 0-5 /home/hanaghaliyah/Downloads/Modul1/soal3/soal3.sh
  ```
  Penjelasan : <br />
  `5` mendeklarasikan bahwa jadwal  pengunduhan terjadi setiap menit ke-5 <br />
  `6,*/8` mendeklarasikan bahwa jadwal pengunduhan terjadi pada setiap 8 jam dari jam 6 <br /> 
  `*` mendeklarasikan bahwa tanggal bebas <br /> 
  `*` mendeklarasikan bahwa bulan bebas <br />
  `0-5` mendeklarasikan bahwa jadwal pengunduhan terjadi setiap hari minggu hingga jum'at kecuali hari sabtu. <br />
  `/home/hanaghaliyah/Downloads/Modul1/soal3/soal3.sh` digunakan untuk menjalankan script bash pada file soal3.sh yang terdapat di dalam direktori tersebut.

## KENDALA
Laporan pendahuluan mengganggu liburan kami
