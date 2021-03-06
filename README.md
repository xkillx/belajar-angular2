# Belajar AngularJS 2.0 #

## Langkah-langkah pembuatan Hello World ##

1. [Instalasi NodeJS dan
   NPM](http://software.endy.muhardin.com/javascript/persiapan-coding-nodejs)
2. [Instalasi TypeScript](#instalasi-typescript)
3. [Inisialisasi Struktur Project](#inisialisasi-struktur-project)
4. [Membuat Template HTML](#membuat-template-html)
5. [Implementasi Komponen Angular 2](#implementasi-komponen-ng2)

### <a name="instalasi-typescript">Instalasi TypeScript</a> ###

AngularJS 2.0 menggunakan bahasa pemrograman TypeScript, yaitu pengembangan dari JavaScript. Nama resmi JavaScript adalah ECMAScript, dan versi yang saat ini terinstal di browser pada umumnya adalah versi 5. Saat ini yang sedang dikembangkan adalah versi 6. Berikut adalah beberapa poin penting mengenai TypeScript:

* TypeScript adalah superset dari ES6, sedangkan ES6 adalah superset dari ES5. Artinya, kita bisa menulis ES5 di kode program ES6, dan kita bisa menulis kode program ES6 dan ES5 di dalam TypeScript dan tidak akan error.
* TypeScript dikembangkan oleh Microsoft
* AngularJS versi 2.0 menggunakan TypeScript sebagai bahasa pemrograman utama. Kita juga bisa membuat aplikasi AngularJS 2.0 dengan bahasa ES5, ES6, ataupun Dart.
* Sebelum dirilis, TypeScript juga dikenal dengan istilah AtScript.
* TypeScript harus diubah dulu (transpile) menjadi ES5 supaya bisa dijalankan di browser.
* TypeScript menambahkan beberapa fitur dari ES6, yaitu:

    * static typing : tipe data ditulis secara eksplisit dan diperiksa pada waktu kompilasi
    * annotations / decorator : mirip dengan annotations di Java. Yaitu metadata yang dipasang di kode program dan akan diproses pada waktu aplikasi dijalankan.
    * karena memiliki type system, maka lebih banyak editor yang bisa menawarkan fitur autocomplete dan pemeriksaan error pada waktu kita mengetik

Untuk menginstal TypeScript, terlebih dulu kita harus menginstal NodeJS dan NPM. Selanjutnya, instalasi dilakukan dengan perintah berikut:

```
npm install -g typescript
```

Selanjutnya, untuk mengetes instalasi kita dan melihat cara kerja transpiler TypeScript, buatlah folder kosong dengan nama `belajar-typescript`.

```
mkdir belajar-typescript
cd belajar-typescript
```

Kemudian, buat file halo.ts di dalam folder tersebut dengan isi sebagai berikut:

```ts
var nama: string = "Endy";
```

Selanjutnya, jalankan transpiler untuk mengkompilasi file `halo.ts`

```
tsc halo.ts
```

Dia akan membuat file baru dengan nama `halo.js` yang berisi sebagai berikut:

```js
var nama = "Endy";
```

Seperti kita lihat di atas, kode program JavaScript yang dihasilkan tidaklah istimewa. Kode program JavaScript biasa.

Kita juga bisa menjalankan transpiler untuk selalu memantau file `.ts` sehingga kita tidak perlu menjalankan kompilasi setiap file diedit. Jalankan `tsc` dengan opsi `-w` (watch)

```
tsc -w *.ts
```

Kita bisa melihat fitur type system pada TypeScript dengan cara mengisi nilai angka ke dalam variabel nama (yang sudah dideklarasikan dengan tipe data `string`). Ubah file `halo.ts` menjadi seperti ini:

```ts
var nama: string = "Endy";
nama = 123;
```

Bila kita perhatikan transpiler, dia akan mengeluarkan pesan error seperti ini

```
File change detected. Starting incremental compilation...
halo.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

Dari pesan error di atas, kita bisa lihat bahwa variabel `nama` tidak bisa diisi dengan nilai `123` karena tipe datanya berbeda.

Lebih lanjut mengenai bahasa TypeScript dapat dipelajari pada [tutorial resmi di websitenya](http://www.typescriptlang.org/Tutorial).

### <a name="inisialisasi-struktur-project">Inisialisasi Struktur Project</a> ###

Langkah pertama tentunya kita buat dulu folder projectnya.

```
mkdir belajar-angular2
cd belajar-angular2
```

Ada dua file konfigurasi yang dibutuhkan dalam project TypeScript:

* `package.json` : Konfigurasi dependency management untuk mencatat daftar package/library yang dibutuhkan. File ini digunakan oleh `npm` untuk mengunduh dan memasang library tambahan yang kita butuhkan dalam aplikasi yang akan kita buat.
* `tsconfig.json` : Konfigurasi transpiler TypeScript. Adanya file ini menunjukkan bahwa folder ini merupakan project TypeScript.

Untuk membuat file `package.json`, kita jalankan perintah `npm init`. Berikut hasilnya

```
npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
name: (belajar-angular2) 
version: (1.0.0) 
description: Aplikasi Belajar AngularJS 2.0
entry point: (index.js) js/aplikasi.js
test command: 
git repository: https://github.com/endymuhardin/belajar-angular2
keywords: 
author: Endy Muhardin
license: (ISC) CC-BY-SA-4.0
About to write to /Users/endymuhardin/workspace/belajar/belajar-angular2/package.json:

{
  "name": "belajar-angular2",
  "version": "1.0.0",
  "description": "Aplikasi Belajar AngularJS 2.0",
  "main": "js/aplikasi.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/endymuhardin/belajar-angular2.git"
  },
  "author": "Endy Muhardin",
  "license": "CC-BY-SA-4.0",
  "bugs": {
    "url": "https://github.com/endymuhardin/belajar-angular2/issues"
  },
  "homepage": "https://github.com/endymuhardin/belajar-angular2#readme"
}


Is this ok? (yes) 
```

Untuk saat ini, kita belum melakukan modifikasi terhadap file tersebut. Selanjutnya, kita buat file konfigurasi satu lagi yaitu `tsconfig.json`.

```
tsc --init
```

Dia akan membuatkan file `tsconfig.json`.

Selanjutnya, kita akan melakukan modifikasi terhadap file `package.json`. Tambahkan bagian script sehingga menjadi seperti ini

```js
  "scripts": {
    "tsc": "tsc",
    "tsc:w": "tsc -w",
    "lite": "lite-server",
    "start": "concurrent \"npm run tsc:w\" \"npm run lite\" "
  }
```

Dengan konfigurasi di atas, kita bisa menjalankan perintah `start` dari folder project.

Selanjutnya, tambahkan bagian `dependencies` agar memuat library AngularJS, SystemJS, dan kelengkapan lainnnya

```js
  "dependencies": {
    "angular2": "2.0.0-beta.0",
    "systemjs": "0.19.6",
    "es6-promise": "^3.0.2",
    "es6-shim": "^0.33.3",
    "reflect-metadata": "0.1.2",
    "rxjs": "5.0.0-beta.0",
    "zone.js": "0.5.10"
  }
```

Tambahkan juga `devDependencies` untuk paket yang hanya digunakan selama development.

```js
  "devDependencies": {
    "concurrently": "^1.0.0",
    "lite-server": "^1.3.1",
    "typescript": "^1.7.3"
  }
```

Untuk file `tsconfig.json`, berikut isi filenya

```js
{
  "compilerOptions": {
        "target": "es5",
        "module": "system",
        "moduleResolution": "node",
        "sourceMap": true,
        "emitDecoratorMetadata": true,
        "experimentalDecorator": true,
        "removeComments": false,
        "noImplicitAny": false
    },
    "exclude": [
        "node_modules"
    ]
}
```

Berikut penjelasannya:

* `target` : hasil transpile. Kita output menjadi ES5 agar bisa dijalankan semua browser.
* `module` : ini adalah sistem untuk load beberapa file `js` dalam project. Dalam contoh ini, kita menggunakan SystemJS. Selain SystemJS, ada juga beberapa alternatif lain misalnya: RequireJS, Browserify, WebPack, CommonJS, AMD, dan sebagainya.
* `moduleResolution` : ada dua opsi : classic dan node. Untuk TypeScript versi 1.6 ke atas, gunakan `node`
* `sourceMap` : opsi supaya compiler menghasilkan file `map`. File ini dibutuhkan agar kita bisa menelusuri pesan error di browser ke baris kode TypeScript (bukan ke JavaScript hasil compile)
* `emitDecoratorMetadata` : menulis informasi tentang anotasi di file hasil kompilasi
* `experimentalDecorator` : ini adalah konfigurasi agar Visual Studio Code tidak menampilkan pesan error untuk anotasi / decorator yang masih experimental
* `removeComments` : hilangkan semua komentar dari file hasil kompilasi
* `noImplicitAny` : bila nilainya false, maka compiler akan menganggap variabel yang tidak memiliki deklarasi tipe data menjadi tipe data `any`. Bila nilainya `true`, compiler akan mengeluarkan pesan error.
* `exclude` : file / folder yang tidak akan diproses oleh compiler

### <a name="membuat-template-html">Membuat Template HTML</a> ###
### <a name="membuat-komponen-ng2">Membuat Komponen AngularJS 2.0</a> ###
## Tools ##

* [Visual Studio Code]()
* [Atom Editor]() dengan [package AngularJS 2.0]() 


## Referensi ##

* [AngularJS 2.0 Quickstart](https://angular.io/docs/ts/latest/quickstart.html)
* [Arsitektur Aplikasi AngularJS
  2](https://angular.io/docs/ts/latest/guide/architecture.html)
