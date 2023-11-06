# **CTFSHOW_Misc_WP**

一个初学CTF的萌新笔记qwq



## 图片篇(基础操作)

### Misc1

很简单的入门题，教你怎么交flag的

### Misc2

解压压缩包得到txt文件，打开后显示乱码，使用010打开文件

发现文件头为PNG，更改扩展名-  

得到flag

### Misc3

解压文件，发现文件拓展名为bpg，查询到BPG容器格式是一种通用图像格式，下载Honeyview进行查看得到flag

#### Honeyview

是我个人特别喜欢用的一款图片查看器，文件出现改动的时候可以实时变化，并且宽容性极强，没看见过他打不开的图片

### Misc4

解压文件发现6个txt，打开后发现ASCII乱码，直接更改为类图片的后缀名使用honeyview打开发现flag，直接按照顺序进行排序

## 图片篇(信息附加)

### Misc5

#### 十六进制编辑器的搜索功能

解压压缩包，打开文件，png直接显示noflag，使用010打开，在16进制尾端发现真flag

### Misc6

打开jpg，发现伪flag，使用010打开，寻找关键字找到flag

### Misc7

好的打开图片之后还是同样的伪flag，不管了直接上010，相同的搜索关键字懒得写了

### Misc8

解压、伪代码、010、搜索...没出来

换一种思路，打开kali，打开shell，使用binwalk分析，发现zlib与png，使用foremost进行分离可见真flag

#### kali

很经典的Linux，对网安方面有极大作用（建议每个网安人都配一个（？

#### Binwalk

linux中一个特别好用的文件解析器，kali中自带，可以清晰的看到图片中是否含有隐藏数据，同时也支持分离文件

```
binwalk -e file --run-as=root
```

#### Foremost

linux中的文件分离器，我一般是在binwalk没法用或者没解析出来的时候才会用，算是一个备用选项吧

`foremost file`
foremost不指定路径的话会自动在当前文件下的目录中新建一个output文件夹

### Misc9

010查找

### Misc10

同样的伪flag，拖入010没发现异常，进入kali中使用binwalk分析图片发现zlib，使用binwalk进行分离发现flag

### Misc11

拖入010，发现可能存在其他文件，进入kali进行binwalk发现两个zlib文件，则说明此图片有隐藏文件，使用foremost与binwalk均没有解析出内容，使用tweakpng排查发现含有2个IDAT块，删除第一个IDAT块发现flag

#### Tweakpng

Tweakpng是一款简单易用的png图像浏览工具，它允许查看和修改一些png图像文件的元信息存储，这道题里可以用它来删除IDAT块

### Misc12

进tweak特么有一群IDAT啊啊啊啊啊啊啊啊

绷不住了一个一个删吧（

删了一堆之后找到flag了

### Misc13

打开文件发现两个IDAT块，但是删除后没有东西，根据题目提示打开010，发现有四段疑似flag的，根据规律隔位删除可得到四个flag，挨个试可以看到第三段flag为真

吗的做完这道题我的眼睛就要瞎了（（（

### Misc14

binwalk直接出

### Misc15

Crtl+F，请

### Misc16

binwalk直接出

### Misc17

使用tweakpng将IDAT块合并，然后binwalk分离得到flag图片

### Misc18

题目提示“**flag在标题、作者、照相机和镜头型号里。”**

有够明显的



#### Magicexif

一个很好用的exif文件查看器，可以分析元数据并且很直观的显示出来，还可以进行图片修复，查看图片是否被修改等操作



### Misc19

拖到exif查看器里就行

### Misc20

#### 我真想锤烂这个出题人的头

拖到exif查看器里

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698369547873-fb5cad8a-4479-438b-8f38-8e307fcf1ba0.png)

牛逼

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698369571171-2c62aba0-003c-4edb-abc9-066dd30e76af.png)ctfshow{c97964b1aecf06e1d79c21ddad593e42}

### Misc21

上难度了奥

根据题目可知，flag藏在图片序号中，使用exif查看器

```plain
Exif Byte Order : Big-endian (Motorola, MM)
X Resolution : 3902939465
Y Resolution : 2371618619
Page Name : https://ctf.show/
X Position : 1082452817
Y Position : 2980145261
Target Printer : ctfshow{}
Serial Number : 686578285826597329
```



serial number后面有一串数字很可疑，将此转换成ASCII码值，得到这个（hex(X&Ys)）。

根据这个提示我们需要依次提取X、Y值（3902939465、2371618619、1082452817、2980145261），可以看出这四串数字都是十进制数，再将这些数字依次转换成十六进制数，然后我们依次拼接也就是

```
ctfshow{e8a221498d5c073b4084eb51b1a1686d}
```

### Misc22

这道题下载好了之后会看到缩略图里面有一串黄色的东西，但是打开之后又不见了

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698377588571-528ab143-9d86-4ade-b8fb-5aad74137e5d.png)（缩略图）

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698377675329-2a02bbcc-323a-4402-bbe1-6f9dda83a55a.png)（本体）

盲猜那串黄色的东西是flag

#### 解法一（常规解法）

JPG文件的文件头为FFD8，在010中发现图片有两个FF08

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698377848307-5c19cd8a-9bc9-4646-890b-0912f1e02283.png)

直接用010分离图片就可以解出（但是我还不会分离所以这种方法没有进行实操）

#### 解法二（取巧解法）

既然缩略图里面有flag，那么直接想办法看缩略图不就可以了嘛，打开Magicexif

##### ![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698378733495-cfe5b7a5-86da-42da-9b7b-748447d55340.png)

直接查看缩略图就好了qwq

### Misc23

是一个psd文件，用photoshop打开，查看文件简介，原始数据中可以看到以下时间戳

```plain
1997-09-22 02:17:02
2055-07-15 12:14:48
2038-05-05 16:50:45
1984-08-03 18:41:46
```

（也可以用Linux里的exiftool解析）

转为16进制

```plain
print(hex(874865822)
[2:]+hex(2699237688)
[2:]+hex(2156662245)
[2:]+hex(460377706)[2:]) 
ctfshow{3425649ea0e31938808c0de51b70ce6a}
```

### Misc41

题目描述

```plain
（本题为Misc入门图片篇和愚人节比赛特别联动题）
H4ppy Apr1l F001's D4y！
愚人节到了，一群笨蛋往南飞，一会儿排成S字，一会儿排成B字。
```

这道题出题人脑洞真大...

文件打不开，进入010，把头文件补充之后还是打不开，src也过不去，我愣是啥都没看出来

回头看题目描述

```
H4ppy Apr1l F001's D4y!
```

看F001像不像16进制数

在010搜索一下

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698379300013-8d6615bc-d257-4afd-afb8-c68a3113081e.png)

好抽象阿...

可能到这里还没看出来，我画两条线你再看看

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698379357508-75680516-ba08-4979-bba1-65f86c1eaa7e.png)

...好出题人你清高

```
ctfshow{fcbd427caf4a52f1147ab44346cd1cdd}
```



## 图片篇(文件结构)

### Misc24

可以看到目前像素是900 x 153=137700，而**文件头**占了53字节，**文件结尾**在675053字节处。又因为每个像素点由三个字节表示，每个字节控制一种颜色，分别为**红、绿、蓝**三种颜色。所以文件真实像素大小为(675053-53)/3=225000。根据提示本题的宽度是没问题的，所以只需要修改高度即可。高度=225000/900=250


### Misc25

题目提示：

```
**flag在图片上面**
```

根据提示来看数据可能被隐写到了没显示到的区域，将png拖入tweakpng中提示crc校验出错

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698464074225-678ffa4a-2016-4e79-98dd-3f535be69f11.png)

可以判断图片二进制可能被修改了分辨率或者crc

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698464544410-5b177e98-2a2c-4f59-bf50-deb0b2fd41a5.png)

使用以下crc爆破脚本

```python
import os
import binascii
import  struct
crcbp = open("x.png",'rb').read()
for i in range(4000) :
    for j in range(4000):
        data = crcbp[12:16] + struct.pack('>i',i) + struct.pack('>i',j) + crcbp[24:29]
        crc32 = binascii.crc32(data) & 0xffffffff
        if crc32 == xxxxxxxxxx : #根据crc校验修改,记得前面加0x
            print(i,j)
            print("hex",hex(i),hex(j))
```

计算`09 DA D1 61`时与原分辨率相同

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698464528148-44cd94cb-6364-4aa5-afab-b7f766cdea85.png)

计算`76 EC 1E 40`时发现分辨率变成了900*250，相对应的HEX值为`0x384 0xfa`

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698464749914-34739636-ce06-4d64-a0fc-76158a694ea2.png)

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698464778382-5f720053-a586-4736-a865-1cfc63eddd73.png)'

成功获得flag

### Misc26

使用tweakpng发现crc校验出错，使用相同方法求解

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698467483175-d0fd0279-f758-4a5a-90cb-a6e24f98a05f.png)

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698478275381-b8b1c4c8-284a-4ee6-9a3c-4de99682f542.png)

### Misc27

根据题目提示可知不出意外会与上一道题的思路差不太多，所以想办法更改文件的高度

FFC0表示JPEG文件正式进入帧块，所以找到FFC0，后面的两个字节表示帧长度，再后面一个表示精度，之后四个字节分别表示宽度和高度，修改高度即可

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698480396215-bd9767f6-0c68-4f49-bee6-289a11c11737.png)

### Misc28

根据题目提示“**flag在图片下面**”可知这道题还是图片宽高隐写题，想办法更改文件的宽高即可

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698650883049-6f45a71f-9a45-4f5d-8640-162c858b6f04.png)

### Misc29

将gif文件拖入分帧器中可以发现共有八帧图片，进去010可以明显地看出来每张图片的分段。原图长宽为900*150，题目提示flag在图片下面，查找宽度150对应的16进制值（03 96）并且全部替换为03 FF再分帧可以看到从第四帧开始出现flag

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698768482136-388100db-b0b2-426d-913a-645355e8f014.png)

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698768529199-60d9ce16-cab5-4b08-ab63-124ad2c80ad1.png)![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698768537846-c53dd11c-a5d4-4398-9cc9-d0ca18ff8cd2.png)

### Misc30

解压后得到bmp文件，根据题目提示可知图片正确的宽度为950，看到图片属性中宽度为900，进入010查找900对应的16进制值（03 84），但是直接查找没有发现宽度，查看文件头部发现有一串十六进制值为84 03，可能这串数字就代表的是文件的宽度，查找950对应的十六进制值为03 B6，于是将84替换为B6，发现图片出现flag。后期查找资料得知bmp文件大多数都是倒向的位图，因此要倒着写。

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698769334102-8619b230-c59f-4ccb-a080-22ef04dee65a.png)

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698769396309-69ea31f2-7d0d-4a06-972a-b0d4f548129a.png)

### Misc31

同样的宽度设置不正确导致bmp文件失真修复题

目前图片是 900*150=135000个像素大小。
**每个像素点由3个字节（十六进制码6位）表示**，**每个字节负责控制一种颜色**，分别为蓝（Blue）、绿（Green）、红（Red），用010打开发现看到共有 487253 个字节，文件头占 53个字节

则正确的宽度应为 **（487253-53） / 3 / 150 = 1082 =0x43a**（结果取整）

将84 03更改为 3A 04即可

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698770218499-96b2706d-8222-42c0-8a6c-c473f85ca011.png)

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698770205303-496fda13-f4dc-45db-bc4d-3519710820ea.png)

### Misc32

简单的png爆破宽高题，直接上脚本爆破即可

### Misc33

不要管是宽改了还是高改了，只要crc没有改就直接可以上脚本爆破

### Misc34

这道题的crc校验也被更改了，但是题目限定范围宽度一定是大于900的，那么就使用脚本将宽度从901开始挨个更改（我限定的范围是1200）

```python
# 导入所需的模块
import os
import zlib
import struct

# 定义源文件名和输出目录名
filename = "misc34.png"
output_dir = "output"

# 如果输出目录不存在，则创建它
os.makedirs(output_dir, exist_ok=True)

# 以二进制模式打开源文件
with open(filename, 'rb') as f:
    # 读取源文件的全部内容
    all_b = f.read()
    
    # 对于每个在901到1200范围内的整数i
    for i in range(901,1200):
        # 创建新文件名，包括输出目录和文件名
        name = os.path.join(output_dir, str(i) + ".png")
        
        # 以二进制写入模式打开新文件
        with open(name,"wb") as f1:
            # 创建新的图片数据，其中包含修改后的宽度信息
            im = all_b[:16]+struct.pack('>i',i)+all_b[20:]
            
            # 将新的图片数据写入到新文件中
            f1.write(im)
```

在output文件夹中就可以看到正确的flag在哪里

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698824027008-3c5c7742-d059-474d-adc1-af0e9c76d009.png)

发现文件无法直接打开，于是用tweakpng检查文件的crc并且将原来文件中错误的crc替换为正确的crc即可

### Misc35

相同的思路，但是文件格式变成了jpg，但是文件本体没有看到任何信息，尝试修改一下高度发现类似flag的图片

​	![misc35.jpg](https://cdn.nlark.com/yuque/0/2023/jpeg/39298680/1698915256041-e775d328-bf6e-4888-adc9-bbd53f8bee46.jpeg)

那么稍微改动一下上面的脚本

```python
import os
import struct

# 设置输出目录
output_dir = "output"

# 如果输出目录不存在，则创建它
os.makedirs(output_dir, exist_ok=True)

# 设置要读取的文件名
filename = "misc35.jpg"

# 以二进制模式打开文件
with open(filename, 'rb') as f:
    # 读取所有的字节
    all_b = f.read()

    # 遍历901到1200的范围
    for i in range(901, 1200):
        # 创建新的文件名，包含输出目录和当前迭代的数字，扩展名为.jpg
        name = os.path.join(output_dir, str(i) + ".jpg")
        
        # 以二进制写入模式打开新文件
        f1 = open(name, "wb")
        
        # 创建新的图像数据，将原始数据的前159个字节与当前迭代数字打包成大端字节序的二进制数据，然后再加上原始数据的第161个字节及以后的所有字节
        im = all_b[:159] + struct.pack('>h', i) + all_b[161:]
        
        # 将新的图像数据写入新文件
        f1.write(im)
        
        # 关闭新文件
        f1.close()

```

当宽度变成993的时候flag出现

### Misc36

先将图片调整到能看到gif下面隐写的类flag字样，然后将宽度在920到950之间进行爆破即可得出答案

### Misc37

分帧

### Misc38

分帧*

### Misc39

题目描述：

**flag就像水，忽快忽慢地流**

说明gif文件的帧速率不同，flag可能就在帧速率当中*（尝试帧间隔隐写）*

那么接下来需要一个新工具

#### **imagemagick**

`sudo apt-get install imagemagick`

随后执行

`identify -format "%T " misc39.gif > 1.txt`

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698990042594-cd1b51bd-abdd-4f0c-bf43-358a39d2f1ba.png)

发现只有37 36两个数字，尝试转换为二进制

由于flag的格式一般都是41字节数据，于是将转化得出的的二进制转换成最后为41字节的字符串即可

情况一：36=0 37=1

```
11000111110100110011011100111101000110111111101111111011011010101100100111000011000101100101100110110011001110010111001011010111001101100010011011111000101100101011001001101100111000110010001110010110110011001111000010111001110010111000101100011110000101100000110100011010101110011111101
```



```python
s="11000111110100110011011100111101000110111111101111111011011010101100100111000011000101100101100110110011001110010111001011010111001101100010011011111000101100101011001001101100111000110010001110010110110011001111000010111001110010111000101100011110000101100000110100011010101110011111101"
flag=""
for i in range(41):
    flag += chr(int(s[7*i:7*(i+1)],2))
print(flag)

```

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698990613694-147d04c7-19f5-4035-aa38-4e9bbf4d0766.png)

情况二：36=1 37=0

```
00111000001011001100100011000010111001000000010000000100100101010011011000111100111010011010011001001100110001101000110100101000110010011101100100000111010011010100110110010011000111001101110001101001001100110000111101000110001101000111010011100001111010011111001011100101010001100000010
```



```python
s="00111000001011001100100011000010111001000000010000000100100101010011011000111100111010011010011001001100110001101000110100101000110010011101100100000111010011010100110110010011000111001101110001101001001100110000111101000110001101000111010011100001111010011111001011100101010001100000010"
flag=""
for i in range(41):
    flag += chr(int(s[7*i:7*(i+1)],2))
print(flag)

```



![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698990474263-44babec0-e8d9-46c0-9424-69c353b03447.png)

炸了捏

### Misc40

先来说一下这个png为什么会动

##### Apng

APNG 是 PNG 格式的一种扩展，可以支持动图。APNG 是普通 png 图片的升级版，它的后缀依然是.png，包含动态的情况下体积会比普通静态 png 打出数倍，可以做到无损的情况展示动态。APNG 是向下兼容的，扩展名也是.png ，不支持 APNG 的解码器会表现为 PNG 的形式，即显示 APNG 的第一帧图片。

之前的Misc38我误以为是因为hoenyview的宽容性所以才能直接以动图的形式打开其实并不是，只是采用了APNG的格式而已

那么再来介绍一个分解APNG的工具

#### [APNG Disassembler](http://sourceforge.net/projects/apngdis/files/latest/download)

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698992703461-46cd35b2-be87-433b-bc49-10356d45636b.png)

这款工具在分离apng的同时也会输出每张图片的delay

然后这个delay文件就是这道题的关键，可以进行十进制转ascii从而得到flag（别问我是怎么知道的我在这里卡了大半天挨个试的）

写个脚本来提取每个文件中的delay然后直接转换为ascii即可得到flag

```python
flag=""
for i in range(1,69):
    f = open("apngframe" + str(i) + ".txt")
    s = f.read()
    flag += chr(int(s.split("/")[0][6:]))
print(flag)
```

![img](https://cdn.nlark.com/yuque/0/2023/png/39298680/1698992860154-6d1375cf-48ae-4943-aecc-8c15c02fadb8.png)

### Misc42

使用010打开文件，发现有许多的IDAT块，放入tweakpng进行解析，发现IDAT块的个数与flag的格式长度相似，于是使用脚本检索所有idat块的字节数并将每一个块的字节数转换为ascii码

```python
# 导入binascii模块，用于处理二进制和ascii数据
import binascii


# 定义一个函数，用于从png文件中提取idat块的字节数
def get_idat_bytes(png_file):
    # 以二进制模式打开png文件
    with open(png_file, "rb") as f:
        # 读取文件内容
        data = f.read()
        # 将二进制数据转换为十六进制字符串
        hex_data = binascii.hexlify(data).decode()
        # 定义一个空列表，用于存储idat块的字节数
        idat_bytes = []
        # 定义一个变量，用于记录当前的位置
        pos = 0
        # 循环遍历十六进制字符串，直到找到所有的idat块
        while True:
            # 查找idat块的标识符，即49444154
            idat_pos = hex_data.find("49444154", pos)
            # 如果没有找到，说明已经到达文件的末尾，退出循环
            if idat_pos == -1:
                break
            # 如果找到了，计算idat块的起始位置，即标识符前面的四个字节，表示块的长度
            start_pos = idat_pos - 8
            # 计算idat块的结束位置，即标识符后面的四个字节，表示块的校验和
            end_pos = idat_pos + 12
            # 截取idat块的十六进制字符串
            idat_hex = hex_data[start_pos:end_pos]
            # 将idat块的十六进制字符串转换为二进制数据
            idat_data = binascii.unhexlify(idat_hex)
            # 获取idat块的长度，即前四个字节
            idat_length = idat_data[:4]
            # 将idat块的长度添加到列表中
            idat_bytes.append(idat_length)
            # 更新当前的位置，继续查找下一个idat块
            pos = end_pos
        # 返回idat块的字节数列表
        return idat_bytes


# 定义一个函数，用于将字节数转换为ascii码
def bytes_to_ascii(bytes_list):
    # 定义一个空字符串，用于存储ascii码
    ascii_str = ""
    # 循环遍历字节数列表
    for b in bytes_list:
        # 将每个字节转换为十进制整数
        n = int.from_bytes(b, "big")
        # 将每个整数转换为ascii字符
        c = chr(n)
        # 将每个字符拼接到字符串中
        ascii_str += c
    # 返回ascii码字符串
    return ascii_str


# 定义一个png文件的路径，你可以根据你的实际情况修改
png_file = "misc42.png"
# 调用get_idat_bytes函数，从png文件中提取idat块的字节数
idat_bytes = get_idat_bytes(png_file)
# 打印idat块的字节数
print("The bytes of IDAT chunks are:")
for b in idat_bytes:
    print(b)
# 调用bytes_to_ascii函数，将idat块的字节数转换为ascii码
ascii_str = bytes_to_ascii(idat_bytes)
# 打印ascii码
print("The ASCII code of IDAT chunks is:")
print(ascii_str)

```

### Misc43

题目描述：`错误中隐藏着通往正确答案的道路`

使用010打开文件，发现有很多IDAT块，然后使用tweakpng打开文件，发现很多的crc都出错了，对应题目描述`错误中隐藏着通往正确答案的道路`，尝试将所有出错的crc校验码提取出来发现可能对应ascii码，于是使用脚本

```python
import binascii
import struct


def calculate_crc(chunk_type, data):
    return binascii.crc32(chunk_type + data) & 0xffffffff


def get_chunks(png_file):
    png_file.seek(8)  # Skip the PNG signature

    while True:
        chunk_length = struct.unpack('!I', png_file.read(4))[0]
        chunk_type = png_file.read(4)
        data = png_file.read(chunk_length)
        crc = struct.unpack('!I', png_file.read(4))[0]

        yield chunk_type, data, crc

        if chunk_type == b'IEND':
            break


def check_crc(png_file):
    incorrect_crcs = []

    for chunk_type, data, crc in get_chunks(png_file):
        calculated_crc = calculate_crc(chunk_type, data)

        if calculated_crc != crc and chunk_type == b'IDAT':
            incorrect_crcs.append(hex(crc)[2:])  # Convert to hex and remove "0x"

    return incorrect_crcs


def crc_to_ascii(crc_values):
    ascii_values = []

    for crc in crc_values:
        # Start from the third character, and take every two characters as a hex number
        for i in range(0, len(crc), 2):
            hex_value = crc[i:i + 2]
            ascii_values.append(chr(int(hex_value, 16)))

    return ''.join(ascii_values)


with open('misc43.png', 'rb') as f:
    incorrect_crcs = check_crc(f)
    ascii_values = crc_to_ascii(incorrect_crcs)

print(ascii_values)

```

得到flag
