# SoalShiftSISOP20_modul1_T20
Praktikum Sistem Operasi 2020 - Modul 1
Departemen Teknologi Informasi

Anggota :
- Fikri Haykal
- Hana Ghaliyah Azhar


# Soal 1
Whits adalah seorang mahasiswa teknik informatika. Dia mendapatkan tugas praktikum untuk membuat laporan berdasarkan data yang ada pada file "Sample-Superstore". Namun dia tidak dapat menyelesaikan tugas tersebut. Laporan yang diminta berupa : <br />
- Tentukan wilayah bagian (region) mana yang memiliki keuntungan (profit) paling sedikit <br />
- Tampilkan 2 negara bagian (state) yang memiliki keuntungan (profit) paling sedikit berdasarkan poin a <br />
- Tampilkan 10 produk (product name) yang memiliki keuntungan (profit) paling sedikit berdasarkan 2 negara bagian (state) hasil poin b <br />

Whits memohon kepada kalian yang sudah jago mengolah data untuk mengerjakan laporan tersebut. <br />
*Gunakan AWK dan Command pendukung <br />

### Jawaban
1a. Region yang memiliki Profit paling sedikit
- Pindah ke directory soal1 <br />
  Syntax : <br />
  `cd Downloads/Modul1/soal1`
- Membuat file shell "soal1.sh" <br />
  Syntax : <br />
  `nano soal1.sh` <br />
- Isi file soal1.sh
  > #!/bin/bash 
  >
  > declare -a state <br />
  > c=0
  >
  > read region profit <<< $(gawk -F "\t" 'NR>1 {summm[$13] += $21} <br />
  > END { <br />
  > for(i in summm){ <br />
  > print i, summm[i] <br />
  > } <br />
  > }' Sample-Superstore.tsv | LC_ALL=C sort -nrk2 | tail -1) <br />
  >
  > echo "Region dengan profit terendah adalah "$region" dengan profit sebesar "$profit <br />
  > printf "\n\n" <br />
- Menjalankan file shell "soal1.sh" menggunakan bash <br />
  <b>Bash</b> berfungsi sebagai penerjemah antara user dan sistem operasi (kernel). <br />
  Syntax : <br />
  `bash soal1.sh` <br />
  Tampilan "soal1.sh" setelah dijalankan
  ![hasil 1a](https://user-images.githubusercontent.com/26424136/75509861-9a71fd80-5a1b-11ea-9e4d-b74825f30c70.jpg) <br />
  Region `Central` memiliki profit paling sedikit yaitu sebesar `9706.4` <br />
 
 1b. 2 state yang memiliki profit paling sedikit berdasarkan poin a 
 - Isi file shell "soal1.sh" <br />
   > echo "2 negara yang memiliki profit terendah di region "$region" adalah" <br />
   > while IFS= read -r line; <br />
   > do <br />
   >   state+=($line) <br />
   >   echo "- "${state[c]} <br />
   >   c=$c+1 <br />
   > done < temp1.txt <br />
   > printf "\n\n" <br />
- Tampilan file shell "soal1.sh" setelah dijalankan menggunakan syntax `bash` <br />
  ![hasil 1b](https://user-images.githubusercontent.com/26424136/75510834-ccd12a00-5a1e-11ea-970a-b7578c9b7278.jpg) <br />
  2 negara bagian (state) yang memiliki profit terendah diregion <b>Central</b> yaitu `Illinois dan Texas` <br />
  
1c. 10 product name yang memiliki profit paling sedikit berdasarkan 2 state hasil poin b  
- Isi file shell "soal1.sh" <br />
  > echo "10 produk yang memiliki keuntungan paling rendah di negara "${state[0]}" atau negara "${state[1]}" adalah sebagai berikut." <br />
  > echo "-------------------------------------------------------------------" <br />
  >
  > gawk -F "\t" -v stat1=${state[0]} -v stat2=${state[1]} 'NR>1 {if(($11==stat1) || ($11==stat2)){summm[$17] += $21}} <br />
  > END { <br />
  >  for(k in summm){ <br />
  >    print "- "k";"summm[k] <br />
  >  } <br />
  > }' Sample-Superstore.tsv | LC_ALL=C sort -gk2 -t ";" | head -10 | cut -d ";" -f1 <br />
  >
  > rm temp1.txt <br />
- Tampilan file shell "soal1.sh" setelah dijalankan menggunakan syntax `bash` <br />
  ![hasil 1c](https://user-images.githubusercontent.com/26424136/75511699-88935900-5a21-11ea-9269-732599ed7ef6.jpg) <br />

##### Tampilan Nomor 1 keseluruhan pada linux
- syntax `nano soal1.sh`
![1](https://user-images.githubusercontent.com/26424136/75511932-61895700-5a22-11ea-9794-31a33282d23f.PNG)
- syntax `bash soal1.sh`
![hasil 1](https://user-images.githubusercontent.com/26424136/75511880-2c7d0480-5a22-11ea-9925-73001db56f57.PNG) 
<br /> <br />


# Soal 2
Terdiri dari 3 file shell, yaitu :
1. <b>soal.sh</b><br />
   File ini digunakan untuk generate random password yang membutuhkan argument untuk penamaan file<br />
   Contoh : <i>$</b> bash soal2.sh password</i>
2. <b>soal2_enkripsi.sh</b><br />
   File ini digunakan untuk mengenkripsi file. Penggunaannya adalah dengan menambahkan argument nama file yang akan dienkripsi<br />
   Contoh : <i>$</b> bash soal2_enkripsi.sh password.txt</i>
3. <b>soal2_dekripsi.sh</b><br />
   File ini digunakan untuk mendekripsi file. Penggunaannya adalah dengan menambahkan argument nama file yang akan didekripsi<br />
   Contoh : <i>$</b> bash soal2_dekripsi.sh ufxxbtwi.txt</i>
   


# Soal 3
Terdiri dari 1 file shell, yaitu :
1. <b>soal3.sh</b><br />
   File ini akan mengeksekusi sesuai permintaan semua sub-soal nomor 3<br />
   Contoh : <i>$</b> bash soal3.sh</i>
   
Selain itu terdapat file <b>crontab</b> yang berisi perintah crontab sesuai yang diminta soal
