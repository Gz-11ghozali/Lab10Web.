# Lab10Web.
1. Membuat Program mobil.php
Deskripsi: Membuat program sederhana dengan kelas Mobil.
Langkah:
Definisikan kelas Mobil dengan atribut privat warna, merk, dan harga.
Buat konstruktor (__construct) untuk memberikan nilai awal atribut.
Tambahkan metode gantiWarna untuk mengubah warna dan tampilWarna untuk menampilkan warna.
Buat objek Mobil ($a dan $b) dan panggil metode untuk menampilkan dan mengubah warna.

<?php
/**
* Program sederhana pendefinisian class dan pemanggilan class.
**/
class Mobil
{
private $warna;
private $merk;
private $harga;
public function __construct()
{
$this->warna = "Biru";
$this->merk = "BMW";
$this->harga = "10000000";
}
public function gantiWarna ($warnaBaru)
{
$this->warna = $warnaBaru;
}
public function tampilWarna ()
{
echo "Warna mobilnya : " . $this->warna;
}
}
// membuat objek mobil
$a = new Mobil();
$b = new Mobil();
// memanggil objek
echo "<b>Mobil pertama</b><br>";
$a->tampilWarna();
echo "<br>Mobil pertama ganti warna<br>";
$a->gantiWarna("Merah");

$a->tampilWarna();
// memanggil objek
echo "<br><b>Mobil kedua</b><br>";
$b->gantiWarna("Hijau");
$b->tampilWarna();
?>

![image](https://github.com/user-attachments/assets/e12ac9d1-69d2-49d4-bf23-3cd318a3419c)
