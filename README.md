# Lab5Web
# Tugas Pratikum 05
### Hasil
https://user-images.githubusercontent.com/92742499/232253469-9b19f692-6ea5-4b52-988b-d028a409d33c.mp4

# .htacces
```bash
    <IfModule mod_rewrite.c>
 RewriteEngine On
 RewriteBase /lab5_php_oop/
 RewriteCond %{REQUEST_FILENAME} !-f
 RewriteCond %{REQUEST_FILENAME} !-d
 RewriteRule ^(.*)$ home.php?mod=$1 [L]
</IfModule>
```
# config.php
```bash

<?php
return array(
    'host' => 'localhost',
    'username' => 'root',
    'password' => '',
    'db_name' => 'latihan1'
);
return $config;
?>
```
# database.php
```bash
<?php
class Database {
    protected $host;
    protected $user;
    protected $password;
    protected $db_name;
    protected $conn;
    
    public function __construct() {
        $this->getConfig();
        $this->conn = new mysqli($this->host, $this->user, $this->password, $this->db_name);
        if ($this->conn->connect_error) {
            die("Connection failed: " . $this->conn->connect_error);
        }
    }
    
    private function getConfig() {
        $config = include("config.php");
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
        $columns = array();
        $values = array();
        if (is_array($data) && count($data) > 0) {
            foreach($data as $key => $val) {
                $columns[] = $key;
                $values[] = "'".$this->conn->real_escape_string($val)."'";
            }
            $columns = implode(",", $columns);
            $values = implode(",", $values);
            $sql = "INSERT INTO ".$table." (".$columns.") VALUES (".$values.")";
            $result = $this->conn->query($sql);
            if ($result === false) {
                die("Error: ".$this->conn->error);
            } else {
                return $result;
            }
        } else {
            return false;
        }
    }
    
    
    public function update($table, $data, $where) {
        $update_value = array(); // Ubah dari string kosong menjadi array kosong
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
```
# mobil.php
```bash

<?php
/**
* Program sederhana pendefinisian class dan pemanggilan class.
**/
class Mobil
{
private $warna;
private $merk;
private $harga;

public function __construct($warna, $merk, $harga)
{
$this->warna = $warna;
$this->merk = $merk;
$this->harga = $harga;
}

public function getWarna()
{
return $this->warna;
}

public function setWarna($warna)
{
$this->warna = $warna;
}

public function getMerk()
{
return $this->merk;
}

public function setMerk($merk)
{
$this->merk = $merk;
}

public function getHarga()
{
return $this->harga;
}

public function setHarga($harga)
{
$this->harga = $harga;
}

public function tampilWarna()
{
echo "Warna mobilnya: " . $this->warna;
}
}

// membuat objek mobil
$a = new Mobil("Biru", "BMW", 10000000);
$b = new Mobil("Merah", "Toyota", 5000000);

// memanggil objek
echo "Mobil a: warna " . $a->getWarna() . ", merk " . $a->getMerk() . ", harga " . $a->getHarga() . "<br>";
echo "Mobil b: warna " . $b->getWarna() . ", merk " . $b->getMerk() . ", harga " . $b->getHarga() . "<br>";

// mengubah warna mobil a
$a->setWarna("Hitam");

// memanggil objek setelah diubah warna nya
echo "Mobil a: warna " . $a->getWarna() . ", merk " . $a->getMerk() . ", harga " . $a->getHarga() . "<br>";
?>
```
# TERIMAKASIH