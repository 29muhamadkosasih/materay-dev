# 📚 PHP Dasar - Panduan Lengkap & Profesional

## 📌 Daftar Isi

1. [Pengenalan PHP](#pengenalan)
2. [Syntax & Variabel](#syntax)
3. [Tipe Data](#tipe-data)
4. [Operator](#operator)
5. [Control Flow](#control-flow)
6. [Loop/Iterasi](#loop)
7. [Fungsi](#fungsi)
8. [String & Array](#string-array)
9. [OOP - Object Oriented Programming](#oop)
10. [Inheritance & Interface](#inheritance)
11. [Trait & Abstract Class](#trait)
12. [Static & Constants](#static)
13. [Namespace](#namespace)
14. [Error Handling](#error-handling)
15. [File Handling](#file-handling)
16. [Database Connection](#database)
17. [PDO & Query](#pdo)
18. [Security Best Practices](#security)
19. [Performance Optimization](#performance)
20. [Real POS Implementation](#pos-implementasi)

---

## 🎯 Pengenalan PHP

### Apa itu PHP?

**PHP (Hypertext Preprocessor)** adalah bahasa pemrograman server-side yang powerful dan flexible.

```
CLIENT (Browser)          SERVER (PHP)
    │                         │
    └──► HTTP REQUEST ───────►│
         (URL, Form)          │
                              │
                         ┌────▼─────┐
                         │ PHP Code  │
                         │ Execution │
                         └────┬─────┘
                              │
    ◄──────── HTTP RESPONSE ──┘
    (HTML)
```

### Karakteristik PHP:

| Fitur                | Penjelasan                          |
| -------------------- | ----------------------------------- |
| **Server-side**      | Dijalankan di server, bukan browser |
| **Embedded**         | Bisa di-embed dalam HTML            |
| **Open Source**      | Gratis dan bebas digunakan          |
| **Cross-platform**   | Berjalan di Linux, Windows, Mac     |
| **Simple**           | Syntax mudah dipelajari             |
| **Database Support** | MySQL, PostgreSQL, Oracle, dll      |

### Versi PHP:

```
PHP 5.x (Legacy) ─────────────────► Sudah tidak didukung
PHP 7.0-7.4 ───────────────────► Masih didukung
PHP 8.0+ (Current) ────────────► Rekomendasi (fitur terbaru)
```

---

## 📝 Syntax & Variabel

### Struktur Dasar PHP:

```php
<?php
// Kode PHP di sini
?>

// Atau shorthand (jika short_open_tag = On):
<? ... ?>
```

### Penulisan Kode:

```php
<?php

// ✅ BAIK - Semicolon di akhir statement
echo "Hello";
$nama = "Kosasih";

// ❌ BURUK - Tanpa semicolon (error/warning)
$umur = 25

// ✅ BAIK - Whitespace untuk readability
if ($value > 100) {
    echo "Besar";
}

// ✅ BAIK - Comment
// Ini single line comment
/* Ini multi
   line comment */
?>
```

### Variabel PHP:

```php
<?php

// Deklarasi variabel (diawali dengan $)
$nama = "Ahmad Kosasih";      // String
$umur = 25;                    // Integer
$tinggi = 175.5;               // Float
$aktif = true;                 // Boolean
$hobi = array("Coding", "Gaming"); // Array
$data = null;                  // Null

// Cek tipe data
var_dump($nama);   // string(12)
var_dump($umur);   // int(25)
gettype($nama);    // "string"

// Variabel dinamis
$var_name = "nama";
$$var_name = "Rina"; // Sama dengan $nama = "Rina"

// Variabel global
$global_var = "Saya global";

function test() {
    global $global_var;
    echo $global_var; // Bisa diakses
}

// Superglobal (bisa diakses di mana saja)
$_GET;     // GET parameter
$_POST;    // POST parameter
$_REQUEST; // GET + POST
$_SERVER;  // Server info
$_SESSION; // Session
$_COOKIE;  // Cookie
$_FILES;   // Upload file
?>
```

---

## 🎭 Tipe Data PHP

### 1. String

```php
<?php

// Single quote (tidak parse variabel)
$text1 = 'Hello $nama'; // Output: Hello $nama

// Double quote (parse variabel)
$nama = "Ahmad";
$text2 = "Hello $nama"; // Output: Hello Ahmad

// Heredoc (multi-line dengan parse)
$long_text = <<<EOT
Ini adalah text panjang
Nama saya: $nama
EOT;

// Nowdoc (multi-line tanpa parse)
$nowdoc = <<<'EOT'
Ini adalah text panjang
Nama saya: $nama (tidak di-parse)
EOT;

// String manipulation
$str = "Hello World";

strlen($str);              // 11 (panjang)
strtoupper($str);          // HELLO WORLD
strtolower($str);          // hello world
str_replace("World", "PHP", $str); // Hello PHP
substr($str, 0, 5);        // Hello
explode(" ", $str);        // ["Hello", "World"]
implode("-", ["a", "b"]); // a-b
strpos($str, "World");     // 6 (index)
trim("  text  ");          // "text" (hapus spasi)
?>
```

### 2. Integer & Float

```php
<?php

// Integer
$int1 = 123;
$int2 = -456;
$int3 = 0xFF; // Hexadecimal (255)
$int4 = 0b1010; // Binary (10)

is_int($int1);   // true
is_integer($int1); // true
intval("123");   // Convert ke integer

// Float
$float1 = 3.14;
$float2 = 2.5e3; // Scientific notation (2500)
$float3 = 1.5E-2; // 0.015

is_float($float1);  // true
is_double($float1); // true
floatval("3.14");   // Convert ke float

// Arithmetic
$a = 10;
$b = 3;

echo $a + $b;  // 13
echo $a - $b;  // 7
echo $a * $b;  // 30
echo $a / $b;  // 3.333...
echo $a % $b;  // 1 (modulo)
echo $a ** 2;  // 100 (power)
?>
```

### 3. Boolean

```php
<?php

$true_val = true;
$false_val = false;

// Truthy values: 1, "string", [1,2,3], object
// Falsy values: 0, "", "0", [], null, false

if ($true_val) {
    echo "Benar";
}

if (!$false_val) {
    echo "Salah";
}

// Type casting
$convert = (bool) 1; // true
$convert = (bool) ""; // false
?>
```

### 4. Array

```php
<?php

// Indexed array
$fruits = array("Apple", "Banana", "Orange");
$fruits = ["Apple", "Banana", "Orange"]; // Modern syntax

echo $fruits[0]; // Apple
$fruits[3] = "Grape"; // Add

// Associative array
$person = [
    "nama" => "Kosasih",
    "umur" => 25,
    "alamat" => "Bandung"
];

echo $person["nama"]; // Kosasih
$person["email"] = "kosasih@mail.com"; // Add

// Multi-dimensional array
$data = [
    ["name" => "Ali", "score" => 85],
    ["name" => "Budi", "score" => 90],
    ["name" => "Citra", "score" => 88]
];

echo $data[0]["name"]; // Ali
echo $data[2]["score"]; // 88

// Array functions
count($fruits);           // Jumlah element
in_array("Apple", $fruits); // Cek ada
array_push($fruits, "Mango"); // Add element
array_pop($fruits);       // Remove last
array_shift($fruits);     // Remove first
array_unshift($fruits, ...); // Add first
array_merge($arr1, $arr2); // Gabung
array_slice($arr, 1, 2);  // Potong
array_keys($person);      // ["nama", "umur", ...]
array_values($person);    // ["Kosasih", 25, ...]
array_map(fn($x) => $x*2, [1,2,3]); // [2,4,6]
array_filter([1,2,3,4], fn($x) => $x>2); // [3,4]
?>
```

### 5. NULL & Type Casting

```php
<?php

// NULL
$empty = null;
$undefined; // Juga null

is_null($empty); // true
isset($empty);   // false
empty($empty);   // true

// Type casting
$str = "123";
$int = (int) $str;      // 123
$float = (float) "3.14"; // 3.14
$bool = (bool) 1;       // true
$arr = (array) $obj;    // Convert object ke array
$obj = (object) $arr;   // Convert array ke object
?>
```

---

## 🔢 Operator

### Arithmetic Operators

```php
<?php

$a = 10;
$b = 3;

echo $a + $b;   // 13 - Addition
echo $a - $b;   // 7 - Subtraction
echo $a * $b;   // 30 - Multiplication
echo $a / $b;   // 3.333... - Division
echo $a % $b;   // 1 - Modulo
echo $a ** 2;   // 100 - Exponentiation

// Assignment operators
$x = 10;
$x += 5;  // $x = 15
$x -= 3;  // $x = 12
$x *= 2;  // $x = 24
$x /= 4;  // $x = 6
$x %= 2;  // $x = 0
?>
```

### Comparison Operators

```php
<?php

$a = 10;
$b = "10";
$c = 5;

// Loose comparison (type juggling)
$a == $b;   // true (value sama, type beda OK)
$a != $c;   // true

// Strict comparison (harus sama type)
$a === $b;  // false (type beda)
$a === 10;  // true

$a !== $c;  // true
$a !== 10;  // false

// Comparison
$a > $c;    // true
$a < $c;    // false
$a >= 10;   // true
$a <= 10;   // true

// Spaceship (3-way comparison)
$a <=> $b;  // 0 (sama)
$a <=> $c;  // 1 (a lebih besar)
$c <=> $a;  // -1 (c lebih kecil)

// Null coalescing
$value = $undefined ?? "default"; // "default"
$result = $var1 ?? $var2 ?? "fallback";
?>
```

### Logical Operators

```php
<?php

$a = true;
$b = false;

$a && $b;  // false - AND (kedua harus true)
$a || $b;  // true - OR (salah satu true)
!$b;       // true - NOT

// Short-circuit evaluation
$result = true || expensive_function(); // Function tidak dijalankan
$result = false && expensive_function(); // Function tidak dijalankan

// Alternative syntax (untuk readability)
if ($a and $b) { } // Sama dengan &&
if ($a or $b) { }  // Sama dengan ||
if (!$condition) { } // Sama dengan not
?>
```

### String Operators

```php
<?php

$name = "Kosasih";
$greeting = "Hello ";

$message = $greeting . $name; // Hello Kosasih
$message .= "!"; // Append

// String interpolation
echo "Nama saya adalah $name"; // Direktly parse

// Complex interpolation
$person = ["name" => "Ahmad"];
echo "Nama: {$person['name']}";
?>
```

---

## 🔀 Control Flow

### If/Else Statement

```php
<?php

$nilai = 85;

// Simple if
if ($nilai >= 80) {
    echo "Lulus";
}

// If-else
if ($nilai >= 80) {
    echo "Lulus";
} else {
    echo "Tidak Lulus";
}

// If-elseif-else
if ($nilai >= 90) {
    echo "A";
} elseif ($nilai >= 80) {
    echo "B";
} elseif ($nilai >= 70) {
    echo "C";
} else {
    echo "D";
}

// Ternary operator
$status = ($nilai >= 80) ? "Lulus" : "Tidak Lulus";

// Null coalescing ternary
$result = $value ?? "default";

// Short ternary (deprecated - jangan gunakan)
$result = $value ?: "default"; // Buruk

// Switch
switch ($grade) {
    case 'A':
        echo "Excellent";
        break;
    case 'B':
        echo "Good";
        break;
    default:
        echo "Need Improvement";
        break;
}

// Match expression (PHP 8+)
$message = match($status) {
    'pending' => 'Processing',
    'completed' => 'Done',
    'failed' => 'Error',
    default => 'Unknown'
};
?>
```

---

## 🔁 Loop/Iterasi

### For Loop

```php
<?php

// Standard for loop
for ($i = 0; $i < 5; $i++) {
    echo "Iterasi ke-" . $i;
}

// Forward loop
for ($i = 1; $i <= 10; $i++) {
    echo $i;
}

// Backward loop
for ($i = 10; $i >= 1; $i--) {
    echo $i;
}

// Multiple increment
for ($i = 1; $i <= 10; $i += 2) {
    echo $i; // 1, 3, 5, 7, 9
}
?>
```

### While & Do-While

```php
<?php

// While loop
$i = 0;
while ($i < 5) {
    echo $i;
    $i++;
}

// Do-while (minimal 1 kali eksekusi)
$j = 0;
do {
    echo $j;
    $j++;
} while ($j < 5);
?>
```

### Foreach Loop

```php
<?php

$fruits = ["Apple", "Banana", "Orange"];

// Hanya values
foreach ($fruits as $fruit) {
    echo $fruit;
}

// Key dan value
$person = ["nama" => "Kosasih", "umur" => 25];

foreach ($person as $key => $value) {
    echo "$key: $value";
}

// Reference (modifikasi original array)
$numbers = [1, 2, 3];

foreach ($numbers as &$num) {
    $num = $num * 2;
}

// Unset reference untuk menghindari bug
unset($num);

// Destructuring (PHP 7.1+)
foreach ($data as ["name" => $name, "age" => $age]) {
    echo "$name is $age years old";
}
?>
```

### Break & Continue

```php
<?php

// Break - stop loop
for ($i = 0; $i < 10; $i++) {
    if ($i == 5) {
        break; // Stop di i=5
    }
    echo $i;
}

// Continue - skip iterasi
for ($i = 0; $i < 10; $i++) {
    if ($i % 2 == 0) {
        continue; // Skip number genap
    }
    echo $i; // 1, 3, 5, 7, 9
}

// Break multiple levels
for ($i = 0; $i < 3; $i++) {
    for ($j = 0; $j < 3; $j++) {
        if ($j == 1) {
            break 2; // Break dari 2 loop
        }
        echo "$i-$j ";
    }
}
?>
```

---

## 🔧 Fungsi

### Definisi & Pemanggilan

```php
<?php

// Fungsi sederhana
function sayHello() {
    echo "Hello!";
}

sayHello();

// Fungsi dengan parameter
function greet($name) {
    echo "Hello, $name!";
}

greet("Kosasih");

// Fungsi dengan multiple parameter
function add($a, $b) {
    return $a + $b;
}

$result = add(5, 3); // 8

// Parameter dengan default value
function multiply($a, $b = 2) {
    return $a * $b;
}

multiply(5);    // 10 ($b = 2)
multiply(5, 3); // 15

// Return multiple value (array)
function getDimensions() {
    return [100, 200]; // width, height
}

list($width, $height) = getDimensions();
// atau
[$width, $height] = getDimensions(); // PHP 7.1+
?>
```

### Type Hint & Return Type

```php
<?php

// Argument type hint
function processUser(string $name, int $age): void {
    echo "$name is $age years old";
}

// Return type
function calculateTotal(float $price, int $qty): float {
    return $price * $qty;
}

// Return type nullable
function findUser(int $id): ?User {
    // Return User atau null
    return $user;
}

// Union type (PHP 8+)
function process(int|float $number): int|float {
    return $number * 2;
}

// Strict types (di awal file)
declare(strict_types=1);
?>
```

### Variable Functions

```php
<?php

function sayHi() {
    echo "Hi!";
}

$func = 'sayHi';
$func(); // Hi! - Call by variable

// Anonymous function
$greet = function($name) {
    return "Hello, $name";
};

echo $greet("Kosasih"); // Hello, Kosasih

// Arrow function (PHP 7.4+)
$multiply = fn($x) => $x * 2;
echo $multiply(5); // 10

// Use variable dari parent scope
$factor = 2;
$multiply = fn($x) => $x * $factor; // $factor dari parent scope
?>
```

### Recursive Function

```php
<?php

// Factorial
function factorial($n) {
    if ($n <= 1) {
        return 1;
    }
    return $n * factorial($n - 1);
}

echo factorial(5); // 120

// Fibonacci
function fib($n) {
    if ($n <= 1) return $n;
    return fib($n - 1) + fib($n - 2);
}

echo fib(6); // 8
?>
```

---

## 📚 String & Array Functions

### String Functions

```php
<?php

$text = "Hello World";

// Case conversion
echo strtoupper($text);     // HELLO WORLD
echo strtolower($text);     // hello world
echo ucfirst("hello");      // Hello
echo ucwords("hello world"); // Hello World

// String info
strlen($text);              // 11
str_word_count($text);      // 2
substr($text, 0, 5);        // Hello
strpos($text, "World");     // 6

// String replace
str_replace("World", "PHP", $text); // Hello PHP
str_ireplace("hello", "Hi", $text); // Case-insensitive

// String split/join
$parts = explode(" ", $text); // ["Hello", "World"]
implode("-", $parts);          // Hello-World

// Trim/Pad
trim("  hello  ");          // "hello"
ltrim("...hello");          // "hello"
rtrim("hello...");          // "hello"
str_pad("5", 3, "0", STR_PAD_LEFT); // "005"

// Find/Replace
strpos("hello world", "o");   // 4
strrpos("hello world", "o");  // 7 (terakhir)
str_contains("hello", "ll");  // true (PHP 8)
str_starts_with("hello", "he"); // true (PHP 8)
str_ends_with("hello", "lo");   // true (PHP 8)

// Format
sprintf("Harga: Rp%d", 50000); // "Harga: Rp50000"
printf("Umur: %d\n", 25);       // Print langsung
?>
```

### Array Functions

```php
<?php

$arr = [3, 1, 4, 1, 5, 9, 2];

// Count
count($arr);           // 7
sizeof($arr);          // 7 (alias)

// Sorting
sort($arr);            // Ascending [1,1,2,3,4,5,9]
rsort($arr);           // Descending
asort($arr);           // Sort maintain key
arsort($arr);          // Reverse sort maintain key
ksort($arr);           // Sort by key
krsort($arr);          // Reverse sort by key
shuffle($arr);         // Random order

// Search
in_array(5, $arr);     // true
array_key_exists("name", ['name' => 'Kosasih']); // true
array_search(5, $arr); // Index of value

// Add/Remove
array_push($arr, 6);   // Add last
array_unshift($arr, 0); // Add first
array_pop($arr);       // Remove last
array_shift($arr);     // Remove first
array_splice($arr, 2, 1); // Remove 1 element at index 2

// Slice
array_slice($arr, 2, 3); // Elements dari index 2, panjang 3

// Merge/Combine
array_merge($arr1, $arr2); // Combine arrays
array_combine(['key1', 'key2'], [1, 2]); // key => value

// Map/Filter
array_map(fn($x) => $x * 2, [1,2,3]); // [2,4,6]
array_filter([1,2,3,4], fn($x) => $x > 2); // [3,4]
array_reduce([1,2,3], fn($carry, $item) => $carry + $item, 0); // 6

// Chunk
array_chunk([1,2,3,4,5], 2); // [[1,2], [3,4], [5]]

// Keys/Values
array_keys(['a'=>1, 'b'=>2]); // ['a', 'b']
array_values(['a'=>1, 'b'=>2]); // [1, 2]

// Flip
array_flip(['a'=>1, 'b'=>2]); // [1=>'a', 2=>'b']

// Count values
array_count_values(['a','b','a','c']); // ['a'=>2, 'b'=>1, 'c'=>1]
?>
```

---

## 🏛️ OOP - Object Oriented Programming

### Class & Object

```php
<?php

// Define class
class Product {
    // Properties
    public $name;
    public $price;
    private $stock;
    protected $category;

    // Constructor
    public function __construct($name, $price, $stock) {
        $this->name = $name;
        $this->price = $price;
        $this->stock = $stock;
    }

    // Method (public)
    public function getInfo() {
        return "{$this->name} - Rp{$this->price}";
    }

    // Method (private)
    private function calculateDiscount() {
        return $this->price * 0.1;
    }

    // Getter
    public function getStock() {
        return $this->stock;
    }

    // Setter
    public function setStock($stock) {
        if ($stock >= 0) {
            $this->stock = $stock;
        }
    }
}

// Create object
$product = new Product("Laptop", 10000000, 50);

// Access properties
echo $product->name;      // Laptop
echo $product->price;     // 10000000

// Call method
echo $product->getInfo(); // Laptop - Rp10000000

// Getter/Setter
$product->setStock(45);
echo $product->getStock(); // 45

// Access private/protected - ERROR
// echo $product->stock; // Fatal error
?>
```

### Access Modifiers

```php
┌──────────────┬─────────┬──────────┬──────────┐
│ Level        │ Class   │ Child    │ Outside  │
├──────────────┼─────────┼──────────┼──────────┤
│ public       │ ✓       │ ✓        │ ✓        │
│ protected    │ ✓       │ ✓        │ ✗        │
│ private      │ ✓       │ ✗        │ ✗        │
└──────────────┴─────────┴──────────┴──────────┘

<?php

class Example {
    public $public_prop = 'public'; // Bisa diakses dari mana saja
    protected $protected_prop = 'protected'; // Hanya di class & child
    private $private_prop = 'private'; // Hanya di class ini
}
?>
```

### Magic Methods

```php
<?php

class User {
    private $data = [];

    // Called when creating object
    public function __construct($name, $email) {
        $this->data['name'] = $name;
        $this->data['email'] = $email;
    }

    // Called when accessing undefined property
    public function __get($name) {
        return $this->data[$name] ?? null;
    }

    // Called when setting undefined property
    public function __set($name, $value) {
        $this->data[$name] = $value;
    }

    // Called when checking if property exists
    public function __isset($name) {
        return isset($this->data[$name]);
    }

    // Called when unset property
    public function __unset($name) {
        unset($this->data[$name]);
    }

    // Called when calling undefined method
    public function __call($method, $args) {
        return "Method $method called with " . count($args) . " args";
    }

    // Called when converting to string
    public function __toString() {
        return $this->data['name'] . " ({$this->data['email']})";
    }

    // Called before object is unserialized
    public function __sleep() {
        return array_keys($this->data);
    }

    // Called after object is unserialized
    public function __wakeup() {
        // Restore connection, etc.
    }

    // Called when trying to clone
    public function __clone() {
        $this->data['id'] = null; // Reset ID on clone
    }

    // Called for var_dump()
    public function __debugInfo() {
        return $this->data;
    }
}

$user = new User("Kosasih", "kosasih@mail.com");

// __get
echo $user->name; // Kosasih

// __set
$user->phone = "08123456789";

// __isset
if (isset($user->phone)) { }

// __toString
echo $user; // Kosasih (kosasih@mail.com)

// __call
$user->unknownMethod("arg1", "arg2"); // Method unknownMethod called with 2 args
?>
```

---

## 🏛️ Inheritance & Interface

### Inheritance

```php
<?php

// Parent class
class Vehicle {
    protected $brand;
    protected $color;

    public function __construct($brand, $color) {
        $this->brand = $brand;
        $this->color = $color;
    }

    public function start() {
        return "Engine started";
    }

    public function getInfo() {
        return "{$this->color} {$this->brand}";
    }
}

// Child class
class Car extends Vehicle {
    private $doors;

    public function __construct($brand, $color, $doors) {
        parent::__construct($brand, $color); // Call parent constructor
        $this->doors = $doors;
    }

    // Override method
    public function getInfo() {
        $parent_info = parent::getInfo(); // Call parent method
        return "$parent_info - {$this->doors} doors";
    }
}

$car = new Car("Toyota", "Red", 4);
echo $car->getInfo(); // Red Toyota - 4 doors
?>
```

### Interface

```php
<?php

// Define interface
interface PaymentInterface {
    public function pay($amount);
    public function getStatus();
}

// Implement interface
class CreditCardPayment implements PaymentInterface {
    private $status = 'pending';

    public function pay($amount) {
        // Process payment
        $this->status = 'completed';
        return "Payment of $amount via credit card";
    }

    public function getStatus() {
        return $this->status;
    }
}

class BankTransferPayment implements PaymentInterface {
    private $status = 'pending';

    public function pay($amount) {
        $this->status = 'completed';
        return "Payment of $amount via bank transfer";
    }

    public function getStatus() {
        return $this->status;
    }
}

// Multiple interface
interface Loggable {
    public function log($message);
}

class Order implements PaymentInterface, Loggable {
    public function pay($amount) {
        return "Paid $amount";
    }

    public function getStatus() {
        return 'completed';
    }

    public function log($message) {
        file_put_contents('log.txt', $message . "\n", FILE_APPEND);
    }
}
?>
```

### Abstract Class

```php
<?php

// Abstract class
abstract class Animal {
    protected $name;

    public function __construct($name) {
        $this->name = $name;
    }

    // Abstract method - harus di-implement di child
    abstract public function makeSound();

    // Concrete method
    public function sleep() {
        return "{$this->name} is sleeping";
    }
}

// Implement abstract class
class Dog extends Animal {
    public function makeSound() {
        return "{$this->name} says: Woof!";
    }
}

class Cat extends Animal {
    public function makeSound() {
        return "{$this->name} says: Meow!";
    }
}

$dog = new Dog("Buddy");
echo $dog->makeSound(); // Buddy says: Woof!
echo $dog->sleep();     // Buddy is sleeping

// Cannot instantiate abstract class
// $animal = new Animal("Generic"); // Fatal error
?>
```

---

## 🔐 Trait & Static

### Trait

```php
<?php

// Define trait
trait Loggable {
    public function log($message) {
        echo "[LOG] $message";
    }
}

trait Timestampable {
    public function getTimestamp() {
        return date('Y-m-d H:i:s');
    }
}

// Use trait
class User {
    use Loggable, Timestampable;

    private $name;

    public function __construct($name) {
        $this->name = $name;
        $this->log("User {$name} created");
    }
}

$user = new User("Kosasih");
$user->log("User logged in"); // [LOG] User logged in
echo $user->getTimestamp(); // 2024-01-15 10:30:45

// Trait inheritance
trait BaseTrait {
    public function baseMethod() {
        return "Base";
    }
}

trait ExtendedTrait extends BaseTrait {
    public function extendedMethod() {
        return $this->baseMethod() . " Extended";
    }
}
?>
```

### Static Properties & Methods

```php
<?php

class Counter {
    private static $count = 0;
    private $id;

    public function __construct() {
        self::$count++;
        $this->id = self::$count;
    }

    // Static method
    public static function getCount() {
        return self::$count;
    }

    public function getId() {
        return $this->id;
    }
}

$c1 = new Counter();
$c2 = new Counter();
$c3 = new Counter();

echo Counter::getCount(); // 3
echo $c1->getId(); // 1
echo $c2->getId(); // 2

// Static method langsung dari class
class MathHelper {
    public static function add($a, $b) {
        return $a + $b;
    }
}

echo MathHelper::add(5, 3); // 8

// Late static binding
class Parent {
    public static function who() {
        echo "Parent";
    }

    public static function test() {
        static::who(); // Gunakan late static binding
    }
}

class Child extends Parent {
    public static function who() {
        echo "Child";
    }
}

Child::test(); // Output: Child (bukan Parent)
?>
```

### Constants

```php
<?php

// Class constant
class Config {
    const DB_HOST = "localhost";
    const DB_USER = "root";
    const DB_PASS = "";

    // From PHP 8.1: visibility modifier
    public const API_URL = "https://api.example.com";
    private const SECRET_KEY = "very-secret";
}

echo Config::DB_HOST; // localhost

// Global constant
define('APP_NAME', 'My App');
echo APP_NAME;

// Check constant exists
defined('APP_NAME'); // true

// Constant with expression (PHP 5.6+)
const ARRAY_CONST = [1, 2, 3];

// Class constant dari variable (PHP 8.3+)
const class_name = 'ClassName';
?>
```

---

## 📦 Namespace

### Basic Namespace

```php
<?php

// file: app/Models/User.php
namespace App\Models;

class User {
    public function getName() {
        return "User name";
    }
}

// file: app/Controllers/UserController.php
namespace App\Controllers;

use App\Models\User;

class UserController {
    public function index() {
        $user = new User(); // Bisa pakai use
        echo $user->getName();
    }
}

// Fully qualified
class AnotherClass {
    public function method() {
        $user = new \App\Models\User(); // Dengan backslash
    }
}

// Sub-namespace
namespace App\Models\Admin;

class AdminUser {
    // ...
}

// Mengakses dari namespace berbeda
use App\Models\Admin\AdminUser;
$admin = new AdminUser();

// Alias
use App\Models\Admin\AdminUser as AdminModel;
use App\Models\User as UserModel;

$admin = new AdminModel();
$user = new UserModel();
?>
```

---

## ⚠️ Error Handling

### Try-Catch-Finally

```php
<?php

// Basic try-catch
try {
    $file = fopen("nonexistent.txt", "r");
    if (!$file) {
        throw new Exception("File not found");
    }
} catch (Exception $e) {
    echo "Error: " . $e->getMessage();
}

// Multiple catch
try {
    // Process
} catch (FileNotFoundException $e) {
    echo "File error";
} catch (DatabaseException $e) {
    echo "Database error";
} catch (Exception $e) {
    echo "General error";
}

// Finally block (selalu dijalankan)
try {
    // Process
} catch (Exception $e) {
    echo "Error occurred";
} finally {
    // Cleanup, close connections, etc
    echo "Cleanup done";
}

// Custom exception
class PaymentException extends Exception {
    public function __construct($message = "", $code = 0) {
        parent::__construct($message, $code);
    }
}

try {
    if ($amount <= 0) {
        throw new PaymentException("Invalid amount");
    }
} catch (PaymentException $e) {
    echo "Payment error: " . $e->getMessage();
}
?>
```

---

## 📄 File Handling

### File Operations

```php
<?php

// Check file exists
if (file_exists("data.txt")) {
    echo "File exists";
}

// Read file
$content = file_get_contents("data.txt");
$lines = file("data.txt"); // Array of lines

// Write file
file_put_contents("data.txt", "Hello World");
file_put_contents("data.txt", "New line\n", FILE_APPEND); // Append

// Create/Edit file
$fp = fopen("data.txt", "w");
fwrite($fp, "Content here");
fclose($fp);

// File info
filesize("data.txt"); // 100
is_file("data.txt");  // true
is_dir("folder");     // true
is_readable("file");  // true
is_writable("file");  // true
filetype("data.txt"); // "file"

// Delete file
unlink("oldfile.txt");

// Copy file
copy("source.txt", "destination.txt");

// Rename
rename("oldname.txt", "newname.txt");

// Directory
mkdir("new_folder");
rmdir("empty_folder");
scandir("folder"); // Array of files in folder
?>
```

---

## 🗄️ Database Connection

### PDO - Basic Connection

```php
<?php

// Create connection
try {
    $pdo = new PDO(
        "mysql:host=localhost;dbname=myapp",
        "root",
        ""
    );
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    die("Connection failed: " . $e->getMessage());
}

// Connection options
$options = [
    PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
    PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
    PDO::ATTR_EMULATE_PREPARES => false,
];

$pdo = new PDO($dsn, $user, $password, $options);
?>
```

### PDO Queries

```php
<?php

// Simple query
$stmt = $pdo->query("SELECT * FROM users");
$result = $stmt->fetch();     // Single row
$results = $stmt->fetchAll(); // All rows

// Prepared statement (prevent SQL injection)
$stmt = $pdo->prepare("SELECT * FROM users WHERE id = ?");
$stmt->execute([123]);
$user = $stmt->fetch();

// Named parameters
$stmt = $pdo->prepare("SELECT * FROM users WHERE email = :email");
$stmt->execute([':email' => 'user@mail.com']);
$user = $stmt->fetch();

// Insert
$stmt = $pdo->prepare("INSERT INTO users (name, email) VALUES (?, ?)");
$stmt->execute(["Kosasih", "kosasih@mail.com"]);
$lastId = $pdo->lastInsertId();

// Update
$stmt = $pdo->prepare("UPDATE users SET name = ? WHERE id = ?");
$stmt->execute(["New Name", 123]);
$affectedRows = $stmt->rowCount();

// Delete
$stmt = $pdo->prepare("DELETE FROM users WHERE id = ?");
$stmt->execute([123]);

// Fetch modes
$stmt->fetch(PDO::FETCH_ASSOC);      // Array (default)
$stmt->fetch(PDO::FETCH_OBJ);        // Object
$stmt->fetch(PDO::FETCH_NUM);        // Numeric array
$stmt->fetchAll(PDO::FETCH_ASSOC);   // All as array
$stmt->fetchColumn(0);               // Single column
?>
```

---

## 🔒 Security Best Practices

### SQL Injection Prevention

```php
<?php

// ❌ BURUK - SQL Injection vulnerable
$user_input = "'; DROP TABLE users; --";
$query = "SELECT * FROM users WHERE email = '$user_input'";

// ✅ BAIK - Use prepared statement
$stmt = $pdo->prepare("SELECT * FROM users WHERE email = ?");
$stmt->execute([$user_input]);

// ✅ BAIK - Named parameters
$stmt = $pdo->prepare("SELECT * FROM users WHERE email = :email");
$stmt->execute([':email' => $user_input]);
?>
```

### XSS Prevention

```php
<?php

// User input
$user_input = "<script>alert('XSS')</script>";

// ❌ BURUK - Direct output
echo $user_input; // Script will execute

// ✅ BAIK - Escape HTML
echo htmlspecialchars($user_input, ENT_QUOTES, 'UTF-8');
// Output: &lt;script&gt;alert('XSS')&lt;/script&gt;

// ✅ BAIK - Using functions
echo htmlentities($user_input);
?>
```

### Password Hashing

```php
<?php

// ❌ BURUK - Simple hash (MD5/SHA1)
$password = md5($user_password);

// ✅ BAIK - bcrypt
$hashed = password_hash($user_password, PASSWORD_BCRYPT);

// Verify password
if (password_verify($user_password, $hashed)) {
    echo "Password correct";
}

// Check if rehash needed
if (password_needs_rehash($hashed, PASSWORD_BCRYPT)) {
    $new_hash = password_hash($user_password, PASSWORD_BCRYPT);
}

// ✅ BAIK - Argon2 (PHP 7.2+)
$hashed = password_hash($user_password, PASSWORD_ARGON2ID);
?>
```

---

## ⚡ Performance Optimization

### Optimization Tips

```php
<?php

// 1. Use appropriate data structures
$users = []; // Array
$user_set = new SplFixedArray(1000); // Fixed size

// 2. Avoid unnecessary function calls in loops
$count = count($arr);
for ($i = 0; $i < $count; $i++) { // Good
    // ...
}

// Bad
for ($i = 0; $i < count($arr); $i++) { // Count called setiap iterasi
    // ...
}

// 3. Use isset() instead of empty()
// isset() lebih cepat karena tidak evaluate value

// 4. Use single quotes for strings tanpa variable
$str = 'hello'; // PHP tidak parse untuk var

// 5. Use array operations daripada loop
$doubled = array_map(fn($x) => $x * 2, $numbers); // Better than loop

// 6. Cache results
function expensiveOperation() {
    static $result = null;

    if ($result === null) {
        $result = computeValue();
    }

    return $result;
}

// 7. Use prepared statements
$stmt = $pdo->prepare("SELECT * FROM users WHERE id = ?");
$stmt->execute([1]);
// PDO caches prepared statement

// 8. Reduce database queries
// Use JOIN instead of multiple queries
// Use SELECT specific columns, not *

// 9. Use buffering untuk output besar
ob_start();
echo "Large output";
echo "More output";
$content = ob_get_clean();
// Send all at once

// 10. Optimize autoloading
// Use PSR-4 autoloading dengan Composer
// Avoid manually requiring files
?>
```

---

## 🏪 Real POS Implementation

### User Management System

```php
<?php

namespace App\Models;

class User {
    private $pdo;
    public $id;
    public $name;
    public $email;
    public $password;
    public $role; // admin, kasir, manager

    public function __construct($pdo) {
        $this->pdo = $pdo;
    }

    // Register user
    public function register($name, $email, $password, $role = 'kasir') {
        $hashed_password = password_hash($password, PASSWORD_BCRYPT);

        $stmt = $this->pdo->prepare(
            "INSERT INTO users (name, email, password, role)
             VALUES (?, ?, ?, ?)"
        );

        $stmt->execute([$name, $email, $hashed_password, $role]);
        return $this->pdo->lastInsertId();
    }

    // Login
    public function login($email, $password) {
        $stmt = $this->pdo->prepare(
            "SELECT id, name, email, password, role FROM users WHERE email = ?"
        );
        $stmt->execute([$email]);
        $user = $stmt->fetch();

        if ($user && password_verify($password, $user['password'])) {
            $_SESSION['user'] = $user;
            return true;
        }

        return false;
    }

    // Get all kasir
    public function getAllKasir() {
        $stmt = $this->pdo->prepare(
            "SELECT id, name, email FROM users WHERE role = 'kasir' ORDER BY name"
        );
        $stmt->execute();
        return $stmt->fetchAll();
    }
}

// Usage in POS
$userModel = new User($pdo);

// Register
$user_id = $userModel->register("Ahmad Kosasih", "kosasih@mail.com", "password123", "kasir");

// Login
if ($userModel->login("kosasih@mail.com", "password123")) {
    echo "Login successful";
}

// Get kasir list
$kasir_list = $userModel->getAllKasir();
?>
```

### Product Management

```php
<?php

namespace App\Models;

class Product {
    private $pdo;

    public function __construct($pdo) {
        $this->pdo = $pdo;
    }

    // Add product
    public function create($name, $price, $stock, $category_id, $sku) {
        $stmt = $this->pdo->prepare(
            "INSERT INTO products (name, price, stock, category_id, sku)
             VALUES (?, ?, ?, ?, ?)"
        );

        return $stmt->execute([$name, (float)$price, (int)$stock, $category_id, $sku]);
    }

    // Get all products
    public function getAll() {
        $stmt = $this->pdo->query(
            "SELECT p.*, c.name as category_name
             FROM products p
             JOIN categories c ON p.category_id = c.id
             WHERE p.deleted_at IS NULL
             ORDER BY p.name"
        );

        return $stmt->fetchAll();
    }

    // Get product by ID
    public function getById($id) {
        $stmt = $this->pdo->prepare(
            "SELECT * FROM products WHERE id = ? AND deleted_at IS NULL"
        );
        $stmt->execute([$id]);
        return $stmt->fetch();
    }

    // Check stock
    public function getStock($id) {
        $product = $this->getById($id);
        return $product ? $product['stock'] : 0;
    }

    // Update stock
    public function updateStock($id, $quantity) {
        $stmt = $this->pdo->prepare(
            "UPDATE products SET stock = stock - ? WHERE id = ?"
        );

        return $stmt->execute([$quantity, $id]);
    }
}

// Usage
$productModel = new Product($pdo);

// Add product
$productModel->create("Laptop", 10000000, 50, 1, "LAPTOP-001");

// Get all
$products = $productModel->getAll();

// Get stock
$stock = $productModel->getStock(1);

// Update stock
$productModel->updateStock(1, -5); // Decrease 5
?>
```

### Order Processing

```php
<?php

namespace App\Models;

class Order {
    private $pdo;

    public function __construct($pdo) {
        $this->pdo = $pdo;
    }

    // Create order
    public function create($user_id, $total_price) {
        $stmt = $this->pdo->prepare(
            "INSERT INTO orders (user_id, total_price, status)
             VALUES (?, ?, 'pending')"
        );

        $stmt->execute([$user_id, (float)$total_price]);
        return $this->pdo->lastInsertId();
    }

    // Add item to order
    public function addItem($order_id, $product_id, $quantity, $price) {
        $subtotal = $quantity * $price;

        $stmt = $this->pdo->prepare(
            "INSERT INTO order_items (order_id, product_id, quantity, price, subtotal)
             VALUES (?, ?, ?, ?, ?)"
        );

        return $stmt->execute([$order_id, $product_id, $quantity, $price, $subtotal]);
    }

    // Complete order (transaction)
    public function complete($order_id) {
        try {
            $this->pdo->beginTransaction();

            // Update order status
            $stmt = $this->pdo->prepare(
                "UPDATE orders SET status = 'completed' WHERE id = ?"
            );
            $stmt->execute([$order_id]);

            $this->pdo->commit();
            return true;
        } catch (Exception $e) {
            $this->pdo->rollBack();
            throw $e;
        }
    }

    // Get order details
    public function getDetails($order_id) {
        $stmt = $this->pdo->prepare(
            "SELECT o.id, o.total_price, o.status, o.created_at,
                    GROUP_CONCAT(
                        JSON_OBJECT(
                            'product_id', oi.product_id,
                            'product_name', p.name,
                            'quantity', oi.quantity,
                            'price', oi.price,
                            'subtotal', oi.subtotal
                        )
                    ) as items
             FROM orders o
             LEFT JOIN order_items oi ON o.id = oi.order_id
             LEFT JOIN products p ON oi.product_id = p.id
             WHERE o.id = ?
             GROUP BY o.id"
        );

        $stmt->execute([$order_id]);
        return $stmt->fetch();
    }
}

// Usage
$orderModel = new Order($pdo);

// Create order
$order_id = $orderModel->create($user_id, 0);

// Add items
$orderModel->addItem($order_id, 1, 2, 500000); // Product 1, qty 2, Rp500k
$orderModel->addItem($order_id, 2, 1, 200000); // Product 2, qty 1, Rp200k

// Complete order
$orderModel->complete($order_id);

// Get details
$order = $orderModel->getDetails($order_id);
?>
```

### Database Schema - ERD untuk POS

```
┌─────────────────────────────────────────────────┐
│           POS SYSTEM DATABASE SCHEMA            │
└─────────────────────────────────────────────────┘

    ┌──────────────┐
    │   USERS      │
    ├──────────────┤
    │ id (PK)      │
    │ name         │
    │ email (UK)   │
    │ password     │
    │ role         │
    │ created_at   │
    └────────┬─────┘
             │ 1:N
             ▼
    ┌──────────────────────┐
    │    PRODUCTS          │
    ├──────────────────────┤
    │ id (PK)              │
    │ name                 │
    │ price (DECIMAL)      │
    │ stock (INT)          │
    │ category_id (FK)     │
    │ sku (UK)             │
    │ created_at           │
    └──────────┬───────────┘
               │ N:M (through order_items)
               │
    ┌──────────▼────────────┐
    │    ORDERS            │
    ├──────────────────────┤
    │ id (PK)              │
    │ user_id (FK)         │
    │ total_price (DECIMAL)│
    │ status (ENUM)        │
    │ created_at           │
    └──────────┬───────────┘
               │ 1:N
               ▼
    ┌──────────────────────┐
    │  ORDER_ITEMS         │
    ├──────────────────────┤
    │ id (PK)              │
    │ order_id (FK)        │
    │ product_id (FK)      │
    │ quantity (INT)       │
    │ price (DECIMAL)      │
    │ subtotal (DECIMAL)   │
    └──────────────────────┘
```

---

## ✅ Checklist PHP Development yang Baik

- [ ] Use prepared statements untuk mencegah SQL injection
- [ ] Escape output untuk mencegah XSS
- [ ] Hash password dengan bcrypt/argon2
- [ ] Use type hints dan return types
- [ ] Implement proper error handling
- [ ] Organize code dalam namespace dan folder
- [ ] Use design patterns (Repository, Service)
- [ ] Write unit tests
- [ ] Use version control (Git)
- [ ] Implement logging
- [ ] Use environment variables untuk config
- [ ] Optimize database queries
- [ ] Implement caching strategy
- [ ] Use constants untuk magic values
- [ ] Follow PSR standards

---

## 📝 Summary

### PHP Program Lifecycle:

```
REQUEST
   │
   ▼
PARSE PHP CODE
   │
   ▼
EXECUTE CODE
   ├─► Database queries
   ├─► File operations
   ├─► Business logic
   └─► Generate output
   │
   ▼
SEND RESPONSE (HTML/JSON)
   │
   ▼
CLIENT RECEIVES
```

### Key Concepts:

1. **Variables** - Store data dengan $
2. **Operators** - Arithmetic, comparison, logical
3. **Control Flow** - If/else, switch, match
4. **Loops** - For, while, foreach
5. **Functions** - Reusable code blocks
6. **OOP** - Classes, inheritance, interfaces, traits
7. **Namespaces** - Organize code
8. **Error Handling** - Try-catch-finally
9. **Database** - PDO prepared statements
10. **Security** - Prevention of injection, XSS, hashing