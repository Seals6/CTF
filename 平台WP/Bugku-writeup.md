# Bugku-writeup

## misc

### ping

1. 解压出来流量包，扔进wireshark，发现是ping流量包

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210114212621.png)

2. 发现每个流量包data附带一个字，前面四个包拼接起来就是`flag`，由于是icmp，没法用追踪功能，手撸`flag{dc76a1eee6e3822877ed627e0a04ab4a}`

   如果有其他更快方法，会在更新

   

### 被勒索了

1. 下载解压出来是个火绒的文件夹

2. 将文件夹拖进沙箱或者新建虚拟机(之前安装过，酒托不进去了)，放入`C:\ProgramData`目录下，此目录是火绒默认隔离区的目录

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210111140247.png)

3. 安装火绒 ，完成后打开隔离区，发现flag

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210111140432.png)

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210111140357.png)

4. 导出flag文件`flag{Virus_traceability}`



### 只有黑棋的棋盘

1. 题目提示动手修复，那应该是修复文件了，下载解压一个围棋的图片，一个flag.zip压缩包

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210111141036.png)

2. 将图片拖进010，发现末尾有`50 4B`标识，应该是有压缩包隐写了，手动提取出来

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210111141140.png)

3. 打开提取的压缩包显示，没有分卷，在打开010，查看文件头发现是`50 4B 05 06`这是文件结束记录符，我们改为`50 4B 03 04`进行修复文件头，再将全局加密改为`00`，这样解压出来一个passwd.txt文档

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210111141615.png)

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210111141701.png)

4. 打开文档，发现字母有序排列，并逐渐向前减一，我们将其补全，按照图片上黑棋坐标点，对应txt坐标写出来相应的字母，这里我横着读了一遍，竖着读了一遍，最终发现是竖着从下往上才能正确排列字母顺序，压缩包密码是大写`GOODGOPLAY`

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210111142126.png)

5. 解压flag.zip出来一张图片，拖进010，发现CRC报错，修改高度一把梭`flag{1t_1s_1nterest1n9}`

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210111142453.png)

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210111142519.png)

### 乌云邀请码

1. 解压出来一张图片，拉入010没有发现其他异常，考虑LSB隐写

   <img src="https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2021-01-03%20%E4%B8%8B%E5%8D%886.04.17.png" style="zoom:50%;" />

2. stegsolve发现RGB 0通道上方有东西

   <img src="https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2021-01-03%20%E4%B8%8B%E5%8D%886.05.35.png" style="zoom:50%;" />

3. 提取，调整调整一下顺序，preview一下`flag{Png_Lsb_Y0u_k0nw!}`

   <img src="https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2021-01-03%20%E4%B8%8B%E5%8D%886.06.15.png" alt="" style="zoom:33%;" />



### Photo的自我修养

1. 题目提示flag好像隐藏在图片中，下载下来1个压缩包，解压出来一张图片还有加密的两个压缩包，图片明显下面还有东西，怀疑改了尺寸

   <img src="https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2020-12-31%20%E4%B8%8B%E5%8D%882.34.04.png" style="zoom:50%;" />

2. 拖入010editor，crc直接报错，修改高一把嗦

   <img src="https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2020-12-31%20%E4%B8%8B%E5%8D%882.35.25.png" style="zoom:50%;" />

3. 图片提示`B:One2021@`

   <img src="https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2020-12-31%20%E4%B8%8B%E5%8D%882.36.19.png" style="zoom:50%;" />

4. 第二个压缩包伪加密，图片详细信息中的属性有第二个提示`A:Just`

   <img src="https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2020-12-31%20%E4%B8%8B%E5%8D%882.41.20.png" style="zoom:33%;" />

5. 两个密码拼接一下`JustOne2021@`为第三个压缩包密码，解压即得`flag{Hello2021_Good}`

   <img src="https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2020-12-31%20%E4%B8%8B%E5%8D%882.42.22.png" style="zoom:35%;" />





### 成果狗成果狗

⚠️这道题没十年脑血栓真的做不出来！

1. 解压文件，得到fl.jpg

2. 拖入010观察，发现有两段FFD8，将两个文件进行分离,得到两个图片

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/%E5%9B%BE%E7%89%87%201.png)

3. 继续分析1.jpg（原fl.jpg），文件尾部FFD9后面有一段加密文字，为base64加密

得到一段hex，将hex导入至FFD9之前，保存文件

<img src="https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/%E5%9B%BE%E7%89%87%202.png" style="zoom:48%;" />

4. 找到文件00C007-00C00A这段为文件的宽高，修改至宽高一样得到图片，

  ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/%E5%9B%BE%E7%89%87%203.png)

5. 很明显，图片显示不正常，但是图片下面显示关键词`Nice}`
6. 猜测，刚刚导入的hex为2.jpg中结尾，如上找到文件端00A003-00A006为图片宽高，修改成一致保存,得到正常图片

​     <img src="https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/%E5%9B%BE%E7%89%87%204.png" style="zoom: 50%;" /> 

6. 同理，在2.jpg后面也有段base64，解码导入010之后，修改宽高一致，得到图片

   <img src="https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/%E5%9B%BE%E7%89%87%205.png" style="zoom:48%;" />

7. 稍微拼接一下`flag{JFIF_1s_very_Nice}`



### 有黑白棋的棋盘

1. 下载出来一个压缩包，解压出来三个压缩包，就疯狂套娃呗

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210114213348.png)

2. 第一个压缩包写了`4easynum`应该是爆破了，上工具密码`7760`

3. 解压出来一个图片和一个hint

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210114213552.png)

   ![](C:\Users\Seals6\AppData\Roaming\Typora\typora-user-images\image-20210114213617768.png)

4. 拖进010，末尾有一串base64，解出来翻译出来意思是精灵，结合题目提示应该古精灵语

   ![](https://bkimg.cdn.bcebos.com/pic/f7246b600c338744de6d0bcf550fd9f9d62aa0dc?x-bce-process=image/watermark,image_d2F0ZXIvYmFpa2U5Mg==,g_7,xp_5,yp_5)

5. 解出来`bugkupasswd`，刚刚提示了这个密码是棋盘的压缩包，解压出来，一张围棋图片，拖进010，发现备注里又有一个hint，base58转一下，我确实没看懂

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210114214328.png)

6. 问了出题人，意思是密码表在围棋中，这里因为图片前面很像`canyou???`以为是猜字符卡了很久，后面问了一血Tokeii大佬，说是图片中围棋的提子，吃了不会下围棋的亏解出来`goodctfer`

![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210114214452.png)

7. 解压出来一张图片，压缩包题目height提示很明显，应该是修改了高，拖进010修改一下高度`flag{new_bugku_is_very_cool}`

   ![](https://aliyunpico.oss-cn-chengdu.aliyuncs.com/img/20210114215016.png)

