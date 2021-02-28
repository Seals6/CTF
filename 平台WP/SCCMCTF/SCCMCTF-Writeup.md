# SCCMCTF

## 前言

### Web

#### 签到题

这不🈶️👋9⃣️🌟 `flag{798e21282436e246360d79a37d42ab3e}`

<img src="https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210225235115280.png" style="zoom:50%;" />





### misc

#### txt

1. 一看题目就是`域名txt解析`

![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228124741666.png)

2. 找一个[在线网站](http://dbcha.com/)

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228125026718.png)

3. base64套娃解码一下，处理下url编码`ti0s{090b4ef8-8092-9b36-7772-a0927bbaebd0}`

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228125339203.png)



#### Windows

1. 下载解压出来一个`缓存.dmp`，直接上工具`mimikatz`

2. 将缓存放置在目录中，cmd调用`mimikatz`

3. 输入`sekurlsa::minidump 1.dmp`切换成缓存

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228130420072.png)

4. 输入`sekurlsa::logonpasswords`，通过缓存文件获取口令`ti0s{910dc511-4c00-8219-ef9f-cfeb912e3c42}`

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228130636578.png)



#### desktop

1. 解压出来一张图片，因为是`png`，优先想到了`LSB隐写`

2. 使用`stegsolve`，发现0，1通道有二维码

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228131221881.png)

3. 扫码发现好像是hex

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228131340777.png)

4. 导入`010editor`看看，发现是python编译的文件，保存为`1.pyc`

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228131613935.png)
   
5. 在线找一个[python反编译网站](https://tool.lu/pyc/)，导入进去
   
   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228131912295.png)
   
6. 保存为py文件，执行发现失败，后面发现没有调用函数，添加函数
   
   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228132208331.png)
   
7. 执行获得`flag{38a57032085441e7}`
   
   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228132316525.png)
   

#### 流量分析

1. 题目`hint:提示：webshell下载了一张图片！`，应该是找这个照片了

2. 解压出来，拖入Wireshark，过滤http流，在最后发现下载图片的信息

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228133139873.png)

3. 追踪流`fl4g:{ftop_Is_Waiting_4_y}`

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228133102990.png)

4. 还可以用linux `strings webshell.pcapng | grep fl`

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228133843551.png)

   

#### 43

1. 下载解压出来一张图片，`010editor`检查无果尝试，其他隐写也没用

2. 考虑`LSB隐写`拖进`stegsolve`，因为图片太大需要仔细看，我第一次就没有看到，经过提示发现了0通道大指拇下面有二维码

   <img src="https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228135216519.png" style="zoom: 33%;" />

3. 扫码发现是莫斯密码`...--.----...--..`，仔细观察下，没有空格或者斜杠分隔，突然发现文件夹名字是`443`分隔下`...- -.-- --. ..--..`，发现还是解不出来，后面尝试了一下`44333`解出来`VYGUD`

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228140118831.png)

4. 题目提示`看看你解密的答案，觉得像哪句话？大写`，百度下发现`VY=VERY GUD=GOOD`
5. 拼接下`flag{VERYGOOD}`   
   
   
   
#### 细节

1. 下载解压出来，扔进`010editor`发现是个jpg，被倒序了，做个脚本转回来

   <img src="https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228141647735.png" style="zoom:33%;" />

   ```python
   with open('reverseMe','rb') as f:
   
      with open('flag','wb') as g:
   
         g.write(f.read()[::-1])
   ```
   
2. 打开图片，翻转一下`flag{4f7548f93c7bef1dc6a0542cf04e796e}`

   ![image-20210228141835405](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/image-20210228141835405.png)



### Crypto








