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


3. Membuat Class Library untuk Form
Deskripsi: Membuat pustaka kelas Form untuk menghasilkan form input sederhana.
Langkah:
Definisikan kelas Form dengan atribut untuk menyimpan field, action, dan submit button.
Buat metode addField untuk menambahkan field input ke form.
Buat metode displayForm untuk menampilkan form.


4. Implementasi Class Library (form_input.php)
Deskripsi: Menggunakan form.php untuk membuat form input.
Langkah:
Tambahkan include "form.php"; untuk memuat pustaka.
Buat objek Form dan tambahkan field (txtnim, txtnama, txtalamat).
Tampilkan form menggunakan displayForm.

Buat file baru dengan nama form.php
<?php
/**
* Nama Class: Form
* Deskripsi: CLass untuk membuat form inputan text sederhan
**/
class Form
{
private $fields = array();
private $action;
private $submit = "Submit Form";
private $jumField = 0;
public function __construct($action, $submit)
{
$this->action = $action;
$this->submit = $submit;
}
public function displayForm()
{
echo "<form action='".$this->action."' method='POST'>";
echo '<table width="100%" border="0">';
for ($j=0; $j<count($this->fields); $j++) {
echo "<tr><td
align='right'>".$this->fields[$j]['label']."</td>";
echo "<td><input type='text'
name='".$this->fields[$j]['name']."'></td></tr>";
}
echo "<tr><td colspan='2'>";
echo "<input type='submit' value='".$this->submit."'></td></tr>";
echo "</table>";
}
public function addField($name, $label)
{
$this->fields [$this->jumField]['name'] = $name;
$this->fields [$this->jumField]['label'] = $label;
$this->jumField ++;

}
}
?>
File tersebut tidak dapat dieksekusi langsung, karena hanya berisi deklarasi class. Untuk
menggunakannya perlu dilakukan include pada file lain yang akan menjalankan dan harus
dibuat instance object terlebih dulu.
Contoh implementasi pemanggilan class library form.php
Buat file baru dengan nama form_input.php
<?php
/**
* Program memanfaatkan Program 10.2 untuk membuat form inputan sederhana.
**/
include "form.php";
echo "<html><head><title>Mahasiswa</title></head><body>";
$form = new Form("","Input Form");
$form->addField("txtnim", "Nim");
$form->addField("txtnama", "Nama");
$form->addField("txtalamat", "Alamat");
echo "<h3>Silahkan isi form berikut ini :</h3>";
$form->displayForm();
echo "</body></html>";
?>

![Screenshot 2024-12-06 193351](https://github.com/user-attachments/assets/1247692d-1dd9-43ee-8b1a-2384c57ffa6e)



5. Membuat Kelas Database (database.php)
Deskripsi: Membuat kelas Database untuk koneksi dan manipulasi database.
Langkah:
Definisikan atribut koneksi (host, user, password, db_name) dan metode untuk koneksi (__construct).
Buat metode:
query: Menjalankan query SQL.
get: Mengambil data berdasarkan tabel dan kondisi.
insert: Menambahkan data ke tabel.
update: Memperbarui data tabel berdasarkan kondisi.
delete: Menghapus data dari tabel.

