---
title: "Array"
publishDate: "12 January 2025"
description: "struktur data paling fundamental dan simple yang pernah ada"
tags: ["tutorial", "Algoritma dan Struktur Data"]
---

## Intro.

ketika kita punya sekumpulan informasi, misalnya nama teman teman sekelas kita, dan kita ingin mengumpulkan informasi itu jadi satu untuk kemudian digunakan untuk undian, yang kita lakukan adalah

- menulis nama teman kita satu persatu
- menggunting nama kertasnya sebanyak nama
- mengumpulkannya di gelas atau tempat tertentu

proses ini memiliki konsep yang sama dengan array di konteks pmerograman. ketika kita ingin menyimpan sekumpulan informasi dengan satu tipe yang sama, stuktur data yang paling simple yang kita bisa gunakan adalah array.

## jadi apa itu array?

array adalah seurutan memori di komputer yang kita isi dengan data. yang perlu di ingat adalah urutan dan memori. kita perlu ingat bahwa data atau informasi di simpan di dalam memori, bisa dengan data drive, RAM, atau CPU cache.
dan ketika kita membuat array, datanya akan disimpan berurutan dari pososo 0 ke N, tidak tersebar, tidak terpisah. dan karena berurutan, maka harus setipe.

## key attributes dari array

array adalah data struktur atau data collection paling dasar. karena menjadi yang paling dasar, array punya beberapa limitasi.

- array tidak bisa berkembang (kecuali di beberapa bahasa pemrograman seperti javaascript)
- karena tidak bisa berkembang, maka array tidak punya fitur penambahan pengurangan seperti push, pull, add, remove, clear dan lainnya
- mengisi nilai dari satu block array, sebenarnya adalah proses _overwrite_
- menghapus isi dari satu block array, sebenarnya adalah proses mengganti isi memory dengan null

karena array adalah koleksi atau kumpulan dari beberapa data, maka perlu adanya identifier atau penanda untuk tiap data di dalamnya. analoginya seperti sekumpulan siswa dalam satu kelas, karena tiap siswa punya nama yg unik dan cenderung panjang, maka untuk memudahkann identifikasi, dipakailah nomor absen. nomor absen ini adalah identifikasi yang menggantikan nama.

di array, identifikasi tiap item menggunakan nomor absen yang disebut index. di pemrograman, indexing dimulai selalu dari 0 dan bukan 1. indexing di array menggunakan simbol ekspresi angka di dalam kurung kotak [].

## cara mengakses data dalam array

ketika komputer membuat suatu array, yg akan dilakukan adalah mencari block memory di ram yang kosong sepanjang yang di deklarasikan di progam, contoh jika array yang dibutuhkan adalah array of integer 2, maka komputer akan mencari 2 block 32Byte memory karena int membutuhkan 32byte. alamat dari block memory pertama akan diingat sebagai base address.

lalu ketika kemudian array tersebut ingin diakses, entah itu mengambil data, mengisi, atau menghapus, rumus yang digunakan oleh komputer adalah

> expected address = base address + (requested_index \* element_size)

jadi jika misalnya kita kita punya array a dengan base address 0x0000 dan kita ingin mengisi index nomer 2 dari array dengan 1 seperti a[2] = 1, maka yg dilakukan oleh komputer adalah mencari address memori si index ke 2 tersebut dengan formula di atas yaitu 0x0000 + (2\*4) = 0x0008

## cara membuat array dalam beberapa bahasa pemrograman

```JavaScript
const arr = [1, 2, 3, 4, 5];
```

```TypeScript
const arr: number[] = [1, 2, 3, 4, 5];
```

```Python
arr = [1, 2, 3, 4, 5]
```

```GO
var arr = [5]int{1, 2, 3, 4, 5}
```

```Java
int[] arr = {1, 2, 3, 4, 5};
```

C dan C++

```C
int arr[] = {1, 2, 3, 4, 5};
```

```C#
int[] arr = {1, 2, 3, 4, 5};
```
