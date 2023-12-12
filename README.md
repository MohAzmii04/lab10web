# Lab10web

Nama : Mohammad Azmi Abdussyukur

NIM  : 312210109

Kelas : TI.22.A1

## DAFTAR ISI <br>
| No | Description | Link |
|-----|------|-----|
|1|Instruksi Praktikum|[Click Here](#instruksi-praktikum)|
|2|Langkah-langkah Praktikum|[Click Here](#langkah-langkah-praktikum)|
|3|Pertanyaan dan Tugas|[Click Here](#pertanyaan-dan-tugas)|

## Instruksi Praktikum
1. Persiapkan text editor misalnya VSCode.
2. Buat folder baru dengan nama `lab10_php_oop` pada docroot webserver (htdocs).
3. Ikuti langkah-langkah praktikum yang akan dijelaskan berikutnya.

## Langkah-langkah Praktikum
1. Buat file php baru dengan nama `mobil.php`
```
<?php
/**
 * Program sederhana pendefinisian class dan pemanggilan class.
 */

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
        echo "Warna mobilnya : ". $this->warna;
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
```
> Output :

<img width="377" alt="mobile" src="https://github.com/MohAzmii04/lab10web/assets/115864496/7cf5f595-2d88-4a2d-b039-f649f245ccb8">


- **Class Library**
Class Library merupakan pustaka kode program yang dapat digunakan bersama pada beberapa file yang berbeda (konsep modularisasi). Class library menyimpan fungsi-fungsi atau class object komponen untuk memudahkan dalam proses development aplikasi.

2. Buat file php baru dengan nama `form.php`
```
<?php
/**
 * Nama Class: Form
 * Deskripsi: Class untuk membuat form inputan text sederhana
 */

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
        echo "<form action='" . $this->action . "' method='POST'>";
        echo '<table width="100%" border="0">';
        for ($j = 0; $j < count($this->fields); $j++) {
            echo "<tr><td
    align='right'>" . $this->fields[$j]['label'] . "</td>";
            echo "<td><input type='text'
    name='" . $this->fields[$j]['name'] . "'></td></tr>";
        }
        echo "<tr><td colspan='2'>";
        echo "<input type='submit' value='" . $this->submit . "'></td></tr>";
        echo "</table>";
    }

    public function addField($name, $label)
    {
        $this->fields[$this->jumField]['name'] = $name;
        $this->fields[$this->jumField]['label'] = $label;
        $this->jumField++;
    }

}
?>
```

3. Buat file php baru dengan nama `form_input.php`
```
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
```
> Output :

<img width="395" alt="forminputmodular" src="https://github.com/MohAzmii04/lab10web/assets/115864496/e15390b5-1cdf-41d2-b630-2d3effacb51f">


4. Buat file dengan nama `database.php`
```
<?php

class Database
{
    protected $host;
    protected $user;
    protected $password;
    protected $db_name;
    protected $conn;

    public function __construct()
    {
        $this->getConfig();
        $this->conn = new mysqli($this->host, $this->user, $this->password, $this->db_name);
        if ($this->conn->connect_error) {
            die("Connection failed: " . $this->conn->connect_error);
        }
    }

    private function getConfig()
    {
        include_once("config.php");
        $this->host = $config['host'];
        $this->user = $config['username'];
        $this->password = $config['password'];
        $this->db_name = $config['db_name'];
    }
    
    public function query($sql)
    {
        return $this->conn->query($sql);
    }

    public function get($table, $where = null)
    {
        if ($where) {
            $where = " WHERE " . $where;
        }
        $sql = "SELECT * FROM " . $table . $where;
        $sql = $this->conn->query($sql);
        $sql = $sql->fetch_assoc();
        return $sql;
    }

    public function insert($table, $data)
    {
        if (is_array($data)) {
            foreach ($data as $key => $val) {
                $column[] = $key;
                $value[] = "'{$val}'";
            }
            $columns = implode(",", $column);
            $values = implode(",", $value);
        }
        $sql = "INSERT INTO " . $table . " (" . $columns . ") VALUES (" . $values . ")";
        $sql = $this->conn->query($sql);
        if ($sql == true) {
            return $sql;
        } else {
            return false;
        }
    }

    public function update($table, $data, $where)
    {
        $update_value = "";
        if (is_array($data)) {
            foreach ($data as $key => $val) {
                $update_value[] = "$key='{$val}'";
            }
            $update_value = implode(",", $update_value);
        }

        $sql = "UPDATE " . $table . " SET " . $update_value . " WHERE " . $where;
        $sql = $this->conn->query($sql);
        if ($sql == true) {
            return true;
        } else {
            return false;
        }
    }


    public function delete($table, $filter)
    {
        $sql = "DELETE FROM " . $table . " " . $filter;
        $sql = $this->conn->query($sql);
        if ($sql == true) {
            return true;
        } else {
            return false;
        }
    }
}
?>
```

## Pertanyaan dan tugas
1. Membuat file `config.php`
   [config.php](lab10_tugas/config.php)

3. Membuat file `database.php`
   [database.php](lab10_tugas/database.php)

5. Membuat file `formlibrary.php`
   [formlibrary.php](lab10_tugas/formlibrary.php)

7. Konfigurasikan dengan praktikum sebelumnya
   [index.php](lab10_tugas/index.php)
   [tambah.php](lab10_tugas/tambah.php)
   [ubah.php](lab10_tugas/ubah.php)
   [hapus.php](lab10_tugas/hapus.php)

-> **Output Tugas :**

> - `Home.php`

<img width="960" alt="index" src="https://github.com/MohAzmii04/lab10web/assets/115864496/e7320c5f-48ae-4d97-8f09-b51ccff4ce08">


> - `About.php`

<img width="960" alt="About" src="https://github.com/MohAzmii04/lab10web/assets/115864496/0ebec8c0-c663-46f3-bb9b-7f4fb84e46d8">


> - `Kontak.php`

<img width="960" alt="kontak" src="https://github.com/MohAzmii04/lab10web/assets/115864496/881db129-273f-4e02-a33f-1d7b9669580f">

> - `tambah.php`

<img width="960" alt="tambah barang" src="https://github.com/MohAzmii04/lab10web/assets/115864496/243352ff-0bd4-4ffa-bb12-844711e1c15e">

> - `ubah.php`

<img width="960" alt="ubah" src="https://github.com/MohAzmii04/lab10web/assets/115864496/1477b1e7-9cf8-4a02-87b6-a85ce8464ce4">


> - `hapus.php`

<img width="960" alt="hapus" src="https://github.com/MohAzmii04/lab10web/assets/115864496/b69b6e86-a959-478e-9a02-efc32a1f0917">



## Selesai, Terima Kasih
