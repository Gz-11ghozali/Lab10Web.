# Lab10Web.
### langkah 1


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

### langkah 2 membuat class library untuk membuat form dan file form.php

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

   ### langkah implementasi pemanggilan class library form dan buat file form_input.php


   <?php
    // Menghubungkan dengan file database.php
    include "database.php";
    include "header.php";
    
    if ($_SERVER['REQUEST_METHOD'] === 'POST') {
        $nim = $_POST['txtnim'];
        $nama = $_POST['txtnama'];
        $alamat = $_POST['txtalamat'];
        $db = new Database();
        $data = [
            'nim' => $nim,
            'nama' => $nama,
            'alamat' => $alamat
        ];
    
        if ($db->insert('mahasiswa', $data)) {
            echo "Data berhasil disimpan!";
        } else {
            echo "Gagal menyimpan data.";
        }
    } else {
        echo "<html><head><title>Mahasiswa</title></head><body>";
    
        class Form
        {
            private $fields = [];
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
                echo "<form action='" . $this->action . "' method='POST'>";
                echo '<table width="100%" border="0">';
                for ($j = 0; $j < count($this->fields); $j++) {
                    echo "<tr><td align='right'>" . $this->fields[$j]['label'] . "</td>";
                    echo "<td><input type='text' name='" . $this->fields[$j]['name'] . "'></td></tr>";
                }
                echo "<tr><td colspan='2'>";
                echo "<input type='submit' value='" . $this->submit . "'></td></tr>";
                echo "</table>";
                echo "</form>";
            }
            public function addField($name, $label)
            {
                $this->fields[$this->jumField]['name'] = $name;
                $this->fields[$this->jumField]['label'] = $label;
                $this->jumField++;
            }
        }
        $form = new Form("", "Submit Data");
        $form->addField("txtnim", "Nim");
        $form->addField("txtnama", "Nama");
        $form->addField("txtalamat", "Alamat");
    
        echo "<h3>Silahkan isi form berikut ini :</h3>";
        $form->displayForm();
        echo "</body></html>";
    }
    ?>
    <?php include('footer.php'); ?>


  ![Screenshot 2024-12-08 215324](https://github.com/user-attachments/assets/64418564-f35d-46f9-91e8-1275a9d9fbc0)

### langkah 3 conection query dan membuat file database.php lalu membuat file config.php untuk menyambungkan ke database mysql

<?php
      class Database {
      protected $host;
      protected $user;
      protected $password;
      protected $db_name;
      protected $conn;
      public function __construct() {
      $this->getConfig();
      $this->conn = new mysqli($this->host, $this->user, $this->password,
      $this->db_name);
      if ($this->conn->connect_error) {
      die("Connection failed: " . $this->conn->connect_error);
      }
      }
      private function getConfig() {
      include_once("config.php");
      $this->host = $config['host'];
      $this->user = $config['username'];
      $this->password = $config['password'];
      
      $this->db_name = $config['db_name'];
      }
      public function query($sql) {
      return $this->conn->query($sql);
      }
      public function get($table, $where=null) {
      if ($where) {
      $where = " WHERE ".$where;
      }
      $sql = "SELECT * FROM ".$table.$where;
      $sql = $this->conn->query($sql);
      $sql = $sql->fetch_assoc();
      return $sql;
      }
      public function insert($table, $data) {
      if (is_array($data)) {
      foreach($data as $key => $val) {
      $column[] = $key;
      $value[] = "'{$val}'";
      }
      $columns = implode(",", $column);
      $values = implode(",", $value);
      }
      $sql = "INSERT INTO ".$table." (".$columns.") VALUES (".$values.")";
      $sql = $this->conn->query($sql);
      if ($sql == true) {
      return $sql;
      } else {
      return false;
      }
      }
      public function update($table, $data, $where) {
      $update_value = "";
      if (is_array($data)) {
      foreach($data as $key => $val) {
      $update_value[] = "$key='{$val}'";
      }
      $update_value = implode(",", $update_value);
      }
      $sql = "UPDATE ".$table." SET ".$update_value." WHERE ".$where;
      $sql = $this->conn->query($sql);
      if ($sql == true) {
      return true;
      } else {
      
      return false;
      }
      }
      public function delete($table, $filter) {
      $sql = "DELETE FROM ".$table." ".$filter;
      $sql = $this->conn->query($sql);
      if ($sql == true) {
      return true;
      } else {
      return false;
      }
      }
      }
      ?>


 ### pastikan nama database nya sudah ada dan sama

 ![Screenshot 2024-12-08 215739](https://github.com/user-attachments/assets/0ec9d515-d6a3-4945-8ed5-321389bcd1f6)

 ### selanjutnya membuat create table

 ![image](https://github.com/user-attachments/assets/4e57b740-75ff-4215-8f6f-f9f0003d1385)


### lalu input

![Screenshot 2024-12-08 215324](https://github.com/user-attachments/assets/97562bc5-2753-4d68-8f29-c737c38af8e8)

### dan data berhasil di simpan 

![Screenshot 2024-12-08 215455](https://github.com/user-attachments/assets/b7999bf4-9fa6-43a1-b68b-953958698451)

### tampilkan data yang di input dengan select*from

![image](https://github.com/user-attachments/assets/10ebc4e3-4dba-4bc5-b541-f34f166751a0)

### Lalu langkah membuat file config.php

<?php
$config = [
    'host' => 'localhost',
    'username' => 'root',
    'password' => '', // Sesuaikan jika ada password MySQL
    'db_name' => 'latihan' // Nama database
];
?>

### Pertanyaan dan Tugas
Implementasikan konsep modularisasi pada kode program pada praktukum sebelumnya
dengan menggunakan class library untuk form dan database connection.

<?php
    // Menghubungkan dengan file database.php
    include "database.php";
    include "header.php";
    
    if ($_SERVER['REQUEST_METHOD'] === 'POST') {
        $nim = $_POST['txtnim'];
        $nama = $_POST['txtnama'];
        $alamat = $_POST['txtalamat'];
        $db = new Database();
        $data = [
            'nim' => $nim,
            'nama' => $nama,
            'alamat' => $alamat
        ];
    
        if ($db->insert('mahasiswa', $data)) {
            echo "Data berhasil disimpan!";
        } else {
            echo "Gagal menyimpan data.";
        }
    } else {
        echo "<html><head><title>Mahasiswa</title></head><body>";
    
        class Form
        {
            private $fields = [];
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
                echo "<form action='" . $this->action . "' method='POST'>";
                echo '<table width="100%" border="0">';
                for ($j = 0; $j < count($this->fields); $j++) {
                    echo "<tr><td align='right'>" . $this->fields[$j]['label'] . "</td>";
                    echo "<td><input type='text' name='" . $this->fields[$j]['name'] . "'></td></tr>";
                }
                echo "<tr><td colspan='2'>";
                echo "<input type='submit' value='" . $this->submit . "'></td></tr>";
                echo "</table>";
                echo "</form>";
            }
            public function addField($name, $label)
            {
                $this->fields[$this->jumField]['name'] = $name;
                $this->fields[$this->jumField]['label'] = $label;
                $this->jumField++;
            }
        }
        $form = new Form("", "Submit Data");
        $form->addField("txtnim", "Nim");
        $form->addField("txtnama", "Nama");
        $form->addField("txtalamat", "Alamat");
    
        echo "<h3>Silahkan isi form berikut ini :</h3>";
        $form->displayForm();
        echo "</body></html>";
    }
    ?>
    <?php include('footer.php'); ?>

    
![Screenshot 2024-12-08 214021](https://github.com/user-attachments/assets/2de45722-6e82-460f-a904-a0dd148fb237)
