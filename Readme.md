# Dokumentasi alur pembuatan migrasi dan seeder pada database serta membuat API menggunakan metode Token/JWT (jason web token)

## Tech

tools dan bahasa pemrograman yang di pakai dalam pembuatan sistem aplikasi :

- [PHP] - Sebagai bahasa pemrograman yang dipakai dalam pembuatan sistem aplikasi 
- [sublime text Editor] - sebagai media untuk menuliskan source code pada pembuatan sistem aplikasi
- [Laravel v9] - Framework PHP yang mempermudah dalam penggarapan sistem aplikasi  
- [POSTMAN] - untuk pengujian API  
- [CMD] - untuk memberikan perintah dalam membuat dan menginstall komponen-komponen yang terlibat dalam pembuatan aplikasi sistem   
- [JWT] - sebuah library untuk membuat autentikasi melalui token dalam format json
- [MYSQL] - sebagai database dalam penyimpanan data pada aplikasi


# Pembuatan migrasi dan seeder pada laravel V9 menggunakan database mysql
Pada tahapan ini akan menjelaskan mengenai proses langkah dalam membuat migrasi dan seeder pada laravel v9 dengan nama folder migration_and_seeder.
### 1. Pembuatan database mysql 
Tahapan pertama membuat database pada mysql dengan nama database -> " produksi ",  yang berisikan 3 table diantaranya:

- Table User  
- Table Product  
- Table Category Product

### 2. Koneksi database pada file .env di laravel v9 
Pada tahapan ini ialah mengkoneksikan database yang telah dibuat, pada laravel v9 di dalam folder migration_and_seed ---> .env :

    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=produksi
    DB_USERNAME=root
    DB_PASSWORD=
- menginputkan nama database pada DB_DATABASE 
- menginputkan nama localhost pada DB_USERNAME 

### 3. Membuat Migrasi   
tahapan selanjutnya membuat migrasi pada cmd dengan perintah :

    php artisan make:migration migration_and_seed
    
file migrasi telah dibuat dengan command diatas dan tersimpan di dalam folder migration_and_seed ---> database ---> migrations ---> file (migration_and_seed) format lengkap beserta tanggal, bulan dan tahun dari pembuatan file tersebut.

didalam file migrasi ini akan diinputkan source code untuk migrasi data dengan source code sebagai berikut :  


    <?php
    
    
    use Illuminate\Database\Migrations\Migration;
    use Illuminate\Database\Schema\Blueprint;
    use Illuminate\Support\Facades\Schema;
    use Illuminate\Support\Facades\DB;
    use Illuminate\Support\Facades\Hash;


    return new class extends Migration 
    {
    
        public function up() {
        
        /**proses penginputan data kedalam table Users menggunakan Schema  
                Schema::create('User', function (Blueprint $table) {
                    $table -> String('email');
                    $table -> String('password'); 
                }
        );
        /**proses penginputan data kedalam table Category_product
                 Schema::create('Category_product', function(Blueprint $table) {
                    $table -> String('name');
                 }
        );
        
        /**proses penginputan data kedalam table Product
                Schema::create('Product', function(Blueprint $table) {
                    $table -> increments('product_category_id');
                    $table -> String('name');
                    $table -> integer('price');
                    $table -> string('image');
                }
        );
        
         }

   
    public function down()
    {
        Schema::dropIfExists('user');
    }
    };

### 3. Membuat Seeder  
tahapan selanjutnya ialah membuat seeder pada cmd dengan perintah :

    php artisan make:seeder userSeeder
    
file seeder telah dibuat dengan command diatas dan tersimpan di dalam folder migration_and_seed ---> database ---> seeders ---> file (userSeeder).

didalam file seeder tersebut akan diinputkan source code untuk mengisi record dengan source code sebagai berikut :  

    <?php

    namespace Database\Seeders;

    use Illuminate\Database\Console\Seeds\WithoutModelEvents;
    use Illuminate\Database\Seeder;

    class userSeeder extends Seeder
    {

        public function run()
        {
            /**Proses penginputan isi kolom pada table user 
            \DB::table('user')->insert([
                'email' => 'tokoweb@gmail.com',
                'password' => 'tokoweb'
            ]);
            
            /**Proses penginputan isi kolom table category_product
            \DB::table('category_product')->insert([
                'name' => 'Makanan'
            ]);
            
            /**Proses penginputan isi kolom table product
            \DB::table('product')->insert([
                'product_category_id' => '1',
                'name' => 'tango',
                'price' => '20000',
                'image' => 'tango.jpg'
            ]);
        }   
    }





# Memubuat API dengan metode JWT (json web token)

## instalasi JWT

menginstall JWT dengan cmd

```sh
php-open-source-saver/jwt-auth tymondesign/jwt-auth tymondesign/jwt/auth
```

membuat konfigurasi JWT  ...

```sh
php artisan vendor : publish -- provider = "PHPOpenSourceSaver\JWTAuth\providers\LaravelServiceProvider"
```
membuat kunci token 

```sh
php artisan jwt:secret
```
## Konfigurasi auth guard 

membuat perubahan pada file config/auth.php untuk mengonfigurasi laravel agar menggunakan JWT AuthGuard untuk mendukung autentikasi aplikasi 


    'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],
        //** untuk menjadikan API guard sebagai default.
        'api' => [
            'driver' => 'jwt',
            'provider' => 'users',
        ],
    ],

## Membuat RegisterController

membuat RegisterController sebagai pengontrol agar dapat melakukan proses autentikasi. dengan membuat pengontroll pada cmd dengan mengetikan perintah

```sh
php artisan make:controller RegisterController
```

## Menambahkan rute API

menambahkan rute API untuk menentukan rute dari controller yang telah dibuat. pada folder routes/api.php 

    <?php

    use Illuminate\Http\Request;
    use Illuminate\Support\Facades\Route;

    Route::post('/register', App\Http\Controllers\Api\RegisterController::class)->name('register');
    Route::get('/product', App\Http\Controllers\Api\ProductController::class)->name('product');

    Route::middleware('auth:api')->get('/user', function (Request $request) {
        return $request->user();
    });
    
Sebelum memulai POSTMAN tambahkan API pendaftaran di dalam kolom alamat dengan memilih metode permintaan HTTP POST dari dropdown, lalu inputkan email dan password pada form-data untuk akses masuk dalam pengujian endpoint. 






