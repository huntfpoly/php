Tiếp tục với series Hướng đối tượng trong Php. Hôm nay mình sẽ giới thiệu các bạn về 4 tính chất quan trọng của OOP

## Table of Content 📃

- [1. Tính kế thừa](#1-inheritance)
- [2. Tính trừu tượng](#-2abstraction)
  - [2.1. Abstract class](#21-abstract-class)
  - [2.2. Interface](#22-interface)
  - [2.3. Khách nhau giữa Abstract và Interface](#23-different)
  - [2.4. Khi nào dùng Abstract class và khi nào dùng Interface](#24-how-to-use)
- [3. Tính đóng gói ](#3-encapsulation)
- [4. Tính đa hình](#4-polymorphism)
- [5. Bài tập thực hành](#5-exercise)

Lập trình hướng đối tượng có 4 tính chất chính:

- Tính kế thừa (inheritance).
- Tính đóng gói (encapsulation).
- Tính trìu tượng (abstraction).
- Tính đa hình (polymorphism).

# 1. Inheritance

**Tính kế thừa** trong OOP cho phép một class có thể kế thừa các thuộc tính và phương thức từ các class khác đã được định nghĩa. Lớp được kế thừa còn được gọi là class cha và lớp kế thừa được gọi là class con. Mục đích chính của việc `Kế thừa` là:

- Khả năng tái sử dụng: một số class con, có thể thừa kế từ cùng một class cha. Điều này rất hữu ích khi ta cung cấp các chức năng thông thường như thêm, sửa và xóa dữ liệu trong database.

- Trong PHP để khai báo kế thừa từ một lớp cha chúng ta sử dụng từ khóa `extends` theo cú pháp:

  ```php
  class parentClass{
    public $name = "Parent class";
    public function getName() {
      echo $this->name;
    }
  }
  class childClass extends parentClass{
      //code
  }
  $obj = new childClass();
  echo $obj->name; // Parent class
  $obj->getName(); // Parent class
  ```

  - ta dịnh nghĩa class **childClass** kế thừa từ class **parentClass** đã được định nghĩa trước.
  - trong class **childClass** ta để trống và khởi tạo đối tượng **\$obj** và truy cập tới thuộc tính **\$name** và function **getName()** của class cha thì nó in ra là "Parent class"
  - Như vậy class con có thể sử dụng thuộc tính và phương thức của class cha

- Tính chất **Kế thừa** cho phép ta ghi đề (override) các thuộc tính và phương thức của class cha với [visibility](https://www.php.net/manual/en/language.oop5.visibility.php) là `puclic` và `protected` (_đừng lo, ta sẽ tìm hiểu ngay phần dưới thôi_)

  ```php
  class parentClass {
    public $name = "parent class";
    public function getName() {
      echo $this->name;
    }
  }

  // Strawberry is inherited from Fruit
  class childClass extends parentClass {
    public $name = "child class";
  }

  $obj = new childClass();
  $obj->getName(); // child class
  ```

  - class **childClass** kế thừa class **parentClass** và định nghĩa lại thuộc tính **\$name** và gán giá trị "child class"
  - Trong function **getName()** ta **echo** ra thuộc tính **\$name** với keyword **\$this->name** (trỏ tới thuộc tính trong class gọi tới nó)
  - đối tượng **\$obj** khởi tạo từ class **childClass** nên sử dụng được các thuộc tính và phương thức của class Cha của nó, mà thuộc tính **\$name** đã được class **childClass** định nghĩa lại nên **getName()** sẽ gọi tới thuộc tính ở **trong class gọi tới nó**. (chính là class **childClass**)
  - Vì thế ta mới in ra kết quả là "child class"

---

# 2. Abstraction

**Trìu tượng hóa** (Abstract) là quá trình đơn giản hóa một đối tượng mà trong đó chỉ bao gồm những đặc điểm quan tâm và bỏ qua những đặc điểm chi tiết nhỏ (hay là không sử dụng).

- Quá trình trừu tượng hóa dữ liệu giúp ta xác định được những thuộc tính, hành động nào của đối tượng cần thiết sử dụng cho chương trình.
- **Lớp Abstract** sẽ định nghĩa các hàm (phương thức) mà từ đó các lớp con sẽ kế thừa nó và Overwrite lại (tính đa hình).
- Tất cả các phương thức của **lớp abstract** đều phải được khai báo là `abstract` và visibility là `protected` hoặc `public`, không được là `private`. Lớp Abstract có thể có thuộc tính nhưng **thuộc tính không được khai báo là abstract**, và bạn **không thể khởi tạo một biến của lớp Abstract** được.

Để hiểu rõ về tính trìu tượng chúng ta sẽ tìm hiểu về `Abstract class` và `Interface`.

### 2.1 Abstract class

Để khai báo một lớp Abstract ta dùng cú pháp sau:

```php
abstract class BaseClass
{
    // phương thức với protected
    abstract protected function hello();

    // Phương thức với public
    abstract public function hi();
}
```

Trong lớp Abstract các phương thức bạn khai báo ở dạng Abstract đều phải tuân theo cú pháp trên, tức là bạn không được định nghĩa thêm dòng lệnh nào bên trong nó. Như ví dụ dưới này là sai.

```php
abstract class BaseClass
{
    // Phương thức này sai vì hàm hello là
    // hàm abstract nên không được code bên trong nó
    abstract protected function hello()
    {
        echo 1;
    }

}
// PHP Fatal error:  Abstract function BaseClass::hello() cannot contain body
```

Trong `abstract class` nếu không phải là phương thức abstract thì vẫn khai báo và viết code được như bình thường.

```php
abstract class BaseClass
{
     protected function hello()
    {
        echo "hello";
    }

}
```

Bạn không thể tạo một biến đối tượng mới của lớp Abstract, như ví dụ dưới này là sai:

```php
abstract class BaseClass
{
    abstract protected function hello();
}

// Sai vì BaseClass là lớp Abstract nên không
// khởi tạo mới được
$base = new BaseClass();
// PHP Fatal error:  Uncaught Error: Cannot instantiate abstract class BaseClass
```

Visibility các hàm của Abstract phải ở public hoặc protected để lớp kế thừa có thể định nghĩa lại và các thuộc tính của lớp Abstract không được khai báo Abstract. Các bạn xem ví dụ dưới đây:

```php
abstract class BaseClass
{
    // Đúng
    public $name;

    // Sai vì các thuộc tính không được để ở dạng abstract
    abstract public $title;

    // Đúng
    abstract protected function hello();

    // Sai vì hàm abstract không thể ở private
    abstract private function hi_there();
}
```

Lớp kế thừa từ lớp Abstracth phải Rewrite lại tất cả các hàm Abstract trong lớp Abstract, nếu không sẽ bị báo sai. Ví dụ:

```php
abstract class Person
{
    protected $ten;
    protected $cmnd;
    protected $namsinh;

    abstract public function showInfo();
}

// Lớp này sai vì chưa viết lại hàm showInfo
class CongNhan extends Person
{

}

// Lớp này đúng vì ta đã khai báo, viết lại
// đầy đủ các hàm abstract
class SinhVien extends Person
{
    public function showInfo(){

    }
}

```

_Ta có ví dụ cụ thể sau:_

```php

 <?php
// Parent class
// Định nghĩa class Car là 1 lớp trừu tượng
abstract class Car {
  // Định nghĩa ra thuộc tính name
  public $name;

  // Hàm khởi tạo __construct có 1 tham số $name
  // hàm này giúp ta khởi tạo đối tượng mới từ class Car
  public function __construct($name) {
    $this->name = $name;
  }

  // Khai báo hàm abstract
  abstract public function intro();
}

// Child classes
// Định nghĩa class Audi kế thừa từ abstract class Car
// và ta phải định nghĩa lại (override) các phương thức abstract có trong class cha
class Audi extends Car {
  public function intro() {
    return "Tôi là $this->name!";
  }
}

class Volvo extends Car {
  public function intro() {
    return "Tôi là $this->name!";
  }
}

class Mercedes extends Car {
  public function intro() {
    return "Tôi là $this->name!";
  }
}

// Tạo đối tượng từ các class con
$audi = new audi("Audi");
echo $audi->intro(); // Tôi là Audi!

$volvo = new volvo("Volvo");
echo $volvo->intro(); // Tôi là Volvo!

$citroen = new Mercedes("Mercedes G63");
echo $citroen->intro(); // Tôi là Mercedes G63!
?>

```

_Giải thích:_

- Các lớp **Audi**, **Volvo** và **Mercedes** được kế thừa từ lớp **Car**. Nghĩa là các lớp **Audi**, **Volvo** và **Mercedes** có thể sử dụng thuộc tính **public \$name** cũng như phương thức **public \_\_construct ()** từ lớp **Car** vì **tính kế thừa**.
- Function **intro()** là một phương thức trừu tượng nên được định nghĩa trong tất cả các lớp con và chúng phải trả về một chuỗi.

### 2.2 Interface

- `Interface` không phải là 1 lớp. Nó được mô tả như là 1 bản thiết kế cho các class có chung cách thức hoạt động.
- Vì không phải là 1 lớp nên không thể định nghĩa các thuộc tính, khởi tạo đối tượng mà chỉ khai báo các phương thức.
- Các phương thức chỉ khai báo tên hàm và không viết nội dung hàm trong đó.
- Không có khái niệm visibility của phương thức, tất cả đều là `public`.
- Lớp con kế thừa từ interface sẻ phải override tất cả các phương thức trong đó.
- Một lớp có thể **kế thừa từ nhiều interface** khác nhau bằng từ khóa `implements`
- Các `interface` có thể kế thừa lẫn nhau.

```php

// interface Move
// bản thiết kế hình dung hành động di chuyển
interface Move
{
    function run();
}

// Dog có thể di chuyển nên ta cho lớp Dog kế thừa interface Move
class Dog implements Move
{
    public function run () //phương thức này được override lại và viết code xử lý vào trong thân hàm cho nó
    {
        echo "Con chó chạy bằng 4 chân";
    }
}

// lớp Car kế thừa interface Move
class Car implements Move
{
    public function run () //phương thức này được override lại và viết thân hàm cho nó
    {
        echo "Xe hơi chạy bằng 4 bánh";
    }
}
$dog = new Dog();
$dog->run(); // Con chó chạy bằng 4 chân
$car = new Car();
$car->run(); // Xe hơi chạy bằng 4 bánh
```

### 2.3 Different

| Interface                                                                                                                          | Abstract class                                                                                                                                                                                          |
| ---------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Chỉ có thể kế thừa nhiều interface khác.                                                                                           | Có thể kế thừa từ 1 lớp và nhiều interface.                                                                                                                                                             |
| Chỉ chứa các khai báo và không có phần nội dung                                                                                    | Có thể chứa các thuộc tính thường và các phương thức bình thường bên trong.                                                                                                                             |
| Không có constructor và cũng không có destructor.                                                                                  | Có constructor và destructor.                                                                                                                                                                           |
| Phạm vi truy cập mặc định là public.                                                                                               | Có thể xác định modifier.                                                                                                                                                                               |
| Dùng để định nghĩa 1 khuôn mẫu hoặc quy tắc chung.                                                                                 | Dùng để định nghĩa cốt lõi của lớp, thành phần chung của lớp và sử dụng cho nhiều đối tượng cùng kiểu.                                                                                                  |
| Cần thời gian để tìm phương thức thực tế tương ứng với lớp dẫn đến thời gian chậm hơn 1 chút.                                      | Nhanh hơn so với interface.                                                                                                                                                                             |
| Khi ta thêm mới 1 khai báo. Ta phải tìm hết tất cả những lớp có thực thi interface này để định nghĩa nội dung cho phương thức mới. | Đối với abstract class, khi đĩnh nghĩa 1 phương thức mới ta hoàn toàn có thể định nghĩa nội dung phương thức là rỗng hoặc những thực thi mặc định nào đó. Vì thế toàn bộ hệ thống vẫn chạy bình thường. |

### 2.4 How to use?

- Nhìn chung `abstract class` và `interface` đều được coi như 1 "bản thiết kế" cho các class con kế thừa nó.
  > Vì sao lại gọi là "bản thiết kế"?
- Gọi là "bản thiết kế" vì ví dụ như trong 1 dự án, bạn muốn ép buộc người lập trình phải tuân thủ theo một số các phương thức

  - các phương thức này đã được định nghĩa sẵn những thứ cơ bản, để giúp cho lập trình viên có thể kế thừa các phương thức này và phát triển lớp con của họ. Thì bạn tạo ra các `abstract class` hoặc `interface` và liệt kê các phương thức cần thiết trong đó.
  - Do đó `abstract class` và `interface` chỉ chứa các khai báo mà không quan tâm bên trong các hàm thực hiện những gì.

- `Abstract Class` là "bản thiết kế" cho **Class**:

  - Về bản chất thì `abstract class` là 1 class nên nó có thể khai báo thêm các thuộc tính và phương thức khác không phải trừu tượng.
  - Nó được xem như "bản thiết kế" cho class vì những class `extends` lại từ nó ngoài việc `override` lại các phương thức trừu tượng của nó thì còn có thể sử dụng các thuộc tính của nó.
  - Trong ví dụ `abstract class` ở trên, 2 class kế thừa **abstract class Car** đều có cùng bản chất là **Xe**. Chúng có thể sử dụng được biến **\$name** trong **abstract class Car**.

- `Interface` là "bản thiết kế" cho **Method**:

  - `Interface` không phải là class nên chỉ dùng để khai báo các phương thức. Nó được xem như "bản thiết kế" cho method vì những class implements lại nó đều phải override lại các phương thức của nó.
  - Trong ví dụ `interface` ở trên, 2 class kế thừa từ `interface` **Move** không có cùng bản chất. 1 class thuộc **động vật** và 1 class thuộc **phương tiện đi lại**. Nhưng chúng có chung hành động là run().

> - `Abstract` thường được sử dụng trong trường hợp các class kế thừa từ nó có cùng bản chất (thuộc 1 nhóm đối tượng)
> - `Interface` thường được sử dụng trong trường hợp các class kế thừa không có cùng bản chất (nhóm đối tượng) nhưng chúng có thể thực hiện các hành động giống nhau.

---

# Encapsulation

**Tính Đóng gói** - điều này liên quan đến việc ẩn các thuộc tính và chỉ để lộ các phương thức. Mục đích chính của việc đóng gói là:

- Giảm độ phức tạp khi phát triển phần mềm - bằng cách ẩn các thuộc tính và chỉ để lộ các phương thức, việc sử dụng một lớp trở nên dễ dàng.
- Bảo vệ trạng thái bên trong của một đối tượng - quyền truy cập vào các thuộc tính lớp thông qua các phương thức như **_get_** và **_set_**, điều này làm cho lớp linh hoạt và dễ bảo trì.
- Việc triển khai nội bộ của lớp có thể được thay đổi mà không cần lo lắng về việc phá vỡ code khi sử dụng lớp.

Trong PHP việc đóng gói được thực hiện nhờ sử dụng các từ khoá `public`, `private` và `protected`:

- `Private` là giới hạn hẹp nhất của thuộc tính và phương thức trong hướng đối tượng.

  - Khi các thuộc tính và phương thức khai báo với visibility là `private` thì các thuộc tính phương thức đó **chỉ có thể sử dụng được trong class đó**
  - Bên ngoài class **không thể** nào có thể sử dụng được nó kể cả lớp kế thừa nó cũng không sử dụng được
  - Nếu muốn lấy giá trị hoặc gán giá trị ở bên ngoài class thì chúng ta phải thông qua hai hàm **SET** và **GET**.

- `protected` là được sử dụng trong class đó và các class con kết thừa từ nó, nhưng bên ngoài class thì không sử dụng được.

- `Public ` có mức độ truy cập toàn cục

  - khi khai báo với visibility `public` thì trong hay ngoài class đều sử dụng được.
  - Thông thường khi không khai báo visibility thì chương trình dịch tự nhận nó là `public` nhưng để cho đúng chuẩn thì mọi người lên khai báo từ khóa này vào thay vì bỏ trống.

  _Để hiểu rõ hơn thì ta có ví dụ sau:_
  **_Ví dụ 1: không kế thừa_**

  ```php
    class Parent
  {
    public $public = 'Tôi là Public';
    protected $protected = 'Tôi là Protected';
    private $private = 'Tôi là Private';

    function printHello()
    {
        echo $this->public;
        echo $this->protected;
        echo $this->private;
    }
  }
  $obj = new Parent();
  // phần 1
  echo $obj->public; // Tôi là Public
  // phân 2
  echo $obj->protected; // Fatal Error
  // phần 3
  echo $obj->private; // Fatal Error
  // phần 4
  $obj->printHello(); // Tôi là PublicTôi là ProtectedTôi là Private
  ```

_Giải thích:_

- Đầu tiên ta tạo ra 1 object **$obj** từ class **Parent**
- **Phần 1** ta truy cập tới thuộc tính **public** với visibility `public` và nó in ra màn hình giá trị **public**
- Với **Phần 2** và **Phần 3** thì nó lại báo lỗi **falal error** bởi vì nó k cho phép **bên ngoài class** truy cập vào thuộc tính có visibility `protected` và `private`
- Nhưng với `Phần 4` ta gọi phương thức **printHello()** thì nó lại in ra tất cả giá trị của thuộc tính trong class. Vì phương thức có trạng thái public và có thể truy cập từ bên ngoài, **printHello()** bên trong class nên gọi tới được tới tất cả các thuộc tính.

**_Ví dụ 2: có kế thừa_**

```php
class ParentClass
{
  public $public = 'Public Parent';
  protected $protected = 'Protected Parent';
  private $private = 'Private Parent';

  function printHello()
  {
      echo $this->public;
      echo $this->protected;
      echo $this->private;
  }
}
// Định nghĩa class  Kế thừa
class Child extends ParentClass
{
  // Khai báo lại thuộc tính public và protected
  public $public = 'Public Child';
  protected $protected = 'Protected Child';

  // Khai báo lại (override) function printHello
  function printHello()
  {
      echo $this->public;
      echo $this->protected;
      echo $this->private;
  }
}

$obj2 = new Child();
// Phần 1
echo $obj2->public; // Public Child
// Phần 2
echo $obj2->protected; // Fatal Error
// Phần 3
echo $obj2->private; // Undefined Property
$obj2->printHello(); // Public Child, Protected Child, Undefined Property
```

_Giải thích:_

- Khởi tạo đối tượng obj2 từ class **Child** được kế thừa từ class **ParentClass** với các thuộc tính được override trong class **Child** là **$public** và **$protected**
- **echo** các thuộc tính thì ta thấy chỉ hiện thị thuộc tính có visibility là `public` (giống với ví dụ trên) và giá trị được ghi đề bởi tính chất **Kế Thừa**.
- **Phần 1** và **Phần 2** hiển thị kết quả đều giống _ví dụ 1_, còn **Phần 3** nó báo **Undefined Property** (là thuộc tính chưa được định nghĩa).
- Vây rõ ràng là đối với kế thừa thì ta cũng k thể truy cập vào được thuộc tính và phương thức của class Cha với visibility là `protected`
- **Phần 4** thì cũng tương tự như ví dụ trên thôi nhé
- bạn thử cho function **printHello()** thực thi từng **echo** bên trong với các thuộc tính thì rõ nha.;)

---

# Polymorphism

- `Tính đa hình` trong oop là sự đa hình của mỗi hành động cụ thể ở những đối tượng khác nhau. Ví dụ hành động tính chu vi, diện tích của các hình trong hình học là khác nhau...
- `Đa hình` là quá trình sử dụng một toán tử hoặc hàm theo những cách khác nhau để nhập dữ liệu khác nhau. Về mặt thực tế, tính đa hình có nghĩa là nếu lớp con kế thừa từ lớp cha, thì nó không cần thiết phải kế thừa mọi thứ từ lớp cha
- Hiểu đơn giản thì `tính đa hình` (polymorphism) trong lập trình hướng đối tượng cho phép các lớp con có thể viết lại (override) các thuộc tính hoặc phương thức từ lớp cha.

_Để hình dung rõ hơn thì ta có ví dụ sau:_

```php

// Ta thấy diện tích các hình có công thức khác nhau,
// như hình chữ nhật cần số liệu 2 cạnh (chiều rộng, dài)
// hình tròn thì chỉ cần số liệu bán kính.
// Như vậy với hành động tính diện tích thì các hình thể hiện hành dộng đó theo các cách khác nhau

// tạo lớp trừu tượng hình dạng (trừu tượng lại các hình chữ nhật, hình vuông, hình tròn...)
abstract class Shape {

  private $x = 0;
  private $y = 0;

  // vì các hình đều có diện tích nên ta
  // tạo phương thức trừu tượng tính diện tích
  public abstract function area();
}

// tạo lớp hình chữ nhật kế thừa từ lớp Hình Dạng
class Rectangle extends Shape {

  // tạo phương thức khởi tạo truyền có 2 tham số truyền vào
  function __construct($x, $y) {
      $this->x = $x;
      $this->y = $y;
  }
  // override phương thức area và viết code tính diện tính hình chữ nhật
  function area() {

      return $this->x * $this->y;
  }
}

// tạo lớp hình tròn kế thừa từ lớp Hình Dạng
class Square extends Shape {

  // tạo phương thức khởi tạo truyền có 1 tham số truyền vào
  function __construct($x) {
      $this->x = $x;
  }
  // override phương thức area và viết code tính diện tính hình tròn
  function area() {

      return $this->x * $this->x;
  }
}
// Khởi tạo đối tượng rectangle với 2 tham số truyền vào là 2 cạnh dài và rộng
$rectangle = new Rectangle(4,5);
// Khởi tạo đối tượng square với 1 tham số truyền vào là bán kính r
$square = new Square(4);

echo $rectangle->area() . "\n"; // 20
echo $square->area() . "\n"; // 16

```

---

# 5. Exercise

**Bài 1**: Tạo class **Product** và khởi tạo đối tượng **Product1** từ class **Product**

**Bài 2**: Thêm các thuộc tính cho class **Product** như **\$name**, **\$color**, **\$price**, **\$description**,... Khởi tạo các đối tượng từ **Product** có giá trị thuộc tính khác nhau và in ra màn hình các thuộc tính đó.

**Bài 3**: Thêm phương thức khởi tạo cho **Product** (_\_\_construct_) với các tham số truyền vào là **\$name**... (các thuộc tính ở bài 2). Sau đó khởi tạo 2 đối tượng **\$product1**, **\$product2**

**Bài 4:** Tạo class **Phone**, **Car**... kế thừa từ class **Product**. Thay đổi Visibility của các thuộc tính và phương thức, sử dụng các phương thức **get** **set** để thay đổi, xuất các giá trị của các thuộc tính có visibility là `protected` và `private`

**Bài 5**: thêm phương thức tính **discount**, **getInfo** (xuất thông tin của đối tượng) cho các **Phone**, **Car**... Nếu **price** lớn hơn 1000 giảm 5%, lớn hơn 200 giảm 10%, lớn hơn 3000 giảm 15%. Khởi tạo các đối tượng từ class **Phone**, **Car** và xuất thông tin ra màn hình

**Bài 6\*** Chuyển class **Product** thành **_abstract class_** và xuất thông tin ra màn hình
