# 举例

## Small String

### "control"

寄存器中：

```c
(lldb) reg r x0 x1 sp
      x0 = 0x006c6f72746e6f63
      x1 = 0xe700000000000000
```

内存中：

```c
(lldb) po 0x0000000281a4cea0
MainAppLibrary.OfflineAB.BucketInfo
(lldb) x/6xg 0x0000000281a4cea0
0x281a4cea0: 0x000000010415b460 0x0000000000000003
0x281a4ceb0: 0x006c6f72746e6f63 0xe700000000000000
0x281a4cec0: 0x0000000000002710 0x0000000281a4c930
(lldb) p/c 0x006c6f72746e6f63
(long) control\0
```

->字符串本身 = realString ：

`0xe700000000000000 0x006c6f72746e6f63`

->

```c
(lldb) p/c 0x006c6f72746e6f63
(unsigned long) control\0
```

->字符串是："control"

第15个字节=8位，分2部分：

* 最高4位=discriminator
  * 0xe
    * = 0b 1110
      * b63 = isImmortal -> 1
        * isImmortal=True
      * b62 = (large) isBridged / (small) isASCII -> 1
        * Small String：ASCII=True
      * b61 = isSmall -> 1
        * 是Small string
      * b60 = isForeign = isSlow -> 0
        * isForeign=False
* 最低4位=count
  * 0x7
    * 字符串长度是：7
      * = 7个字符
        * 对应着：control字符串的长度=7

## Large String

### Large String - Native

#### "ios_prod_latam_tos_reg_universe"

```c
(lldb) po 0x00000002800f5e00
MainAppLibrary.OfflineAB.UniverseInfo

(lldb) x/6gx 0x00000002800f5e00
0x2800f5e00: 0x0000000105d3c398 0x0000000200000003
0x2800f5e10: 0xd00000000000001f 0x8000000105110930
0x2800f5e20: 0x6469725f72657375 0xe800000000000000
```

根据定义：

* MainAppLibrary.OfflineAB.UniverseInfo
  * [+0x10~0x18] = name
    * 类型：Swift的Large String（真正字符串地址是：name值+0x20）

此处值：

* [+0x10~0x18] = name
  * 0x2800f5e10: 0xd00000000000001f 0x8000000105110930

-》真正字符串地址是：

* 0x8000000105110930 + 0x20 = 0x8000000105110950

```c
(lldb) x/s 0x8000000105110950
0x105110950: "ios_prod_latam_tos_reg_universe"
```

##### 额外说明

（1）如果不加0x20，则看到的字符串是别的（错位后的）值：

```c
(lldb) x/s 0x8000000105110930
0x105110930: "os_reg_experiment"
```

（2）去掉最开始的0x8，也是可以用x/s看到字符串的:

```c
(lldb) x/s 0x0000000105110930
0x105110930: "os_reg_experiment"
(lldb) x/s 0x0000000105110950
0x105110950: "ios_prod_latam_tos_reg_universe"
```

类似的：去掉0x8前缀，可以用po查看出字符串的值：

```c
(lldb) po (char*)0x0000000105110950
"ios_prod_latam_tos_reg_universe"
```

（3）不论是否加0x20，直接po查看，则都是（异常的，不是我们要的）时间类型的值：

```c
(lldb) po 0x8000000105110930
2001-01-01 00:00:00 +0000
(lldb) po 0x8000000105110950
2001-01-01 00:00:00 +0000

(lldb) po (char*)0x8000000105110950
2001-01-01 00:00:00 +0000
```

后记：

另外某次去po，却连异常的时间类型的值都看不到，而是：就是数字：

```c
(lldb) reg r x0 x1
      x0 = 0x000000000000007c
      x1 = 0xe100000000000000
(lldb) reg r x20 sp
     x20 = 0x000000016d658c20
      sp = 0x000000016d658c20
(lldb) x/8gx 0x000000016d658c20
0x16d658c20: 0xd000000000000021 0x80000001051946f0
0x16d658c30: 0x0000000000000002 0x00000002833c1140
0x16d658c40: 0x0000000105da1628 0x0000000105dbbcd0
0x16d658c50: 0x00000002833c1140 0x00000002828d0780
(lldb) po 0x80000001051946f0
9223372041235285744
```

而换x/s字符串查看，是可以看出：

（虽然是错误的，但是是）字符串的值的

```c
(lldb) x/s 0x80000001051946f0
0x1051946f0: "dummy_aa_offline_user_rid_ios"
```

当然，真正地址+0x20的字符串：

```c
(lldb) p/x 0x80000001051946f0 + 0x20
(unsigned long) 0x8000000105194710
```

->但是po也还是看不出:

```c
(lldb) po 0x8000000105194710
9223372041235285776
```

只能用字符串查看：

```c
(lldb) x/s 0x8000000105194710
0x105194710: "dummy_aa_offline_rid_universe_ios"
```

### Large String - Shared

#### "2.23.25.85"

此处：

```c
0xc00000000000000a 0x400000028143c6c0
```

其中：

* Large String
  * 0xc00000000000000a
    * 0xC = 12 = 0b 1100
      * b63 = ASCII = isASCII
        * True
      * b62 = NFC = isNFC
        * True
      * b61 = native = isNativelyStored
        * False
      * b60 = tail = isTailAllocated
        * False
      * b59 = UTF8 = isForeignUTF8
        * False
    * 0xa = 10 ：字符串长度是10
  * 0x400000028143c6c0
    * discriminator = 0x4 = 0b 0100
      * b63 = isImmortal
        * False
      * b62 = (large) isBridged / (small) isASCII
        * True
      * b61 = isSmall
        * False
      * b60 = isForeign = isSlow
        * False
    * objectAddr = 0x28143c6c0

->

* Large String
  * length = 10
  * flag
    * isASCII = True
    * isNFC = True
    * isNativelyStored = False
    * isTailAllocated = False
    * isForeignUTF8 = False
  * discriminator
    * isImmortal = False
    * isBridged = True
    * isSmall = False
    * isForeign == isSlow = False
  * objectAddr = 0x28143c6c0

->

* Swift中的字符串
  * 是bridged桥接的
  * 字符串地址是：0x28143c6c0
  * 其他细节
    * 是ASCII的
    * 是NFC（Normal Form C）的
    * 不是尾部分配的TailAllocated

-> 此处对应着：从`ObjC`的`NSString`：`"2.23.25.85"`

```c
(lldb) reg r x0
      x0 = 0x000000028143c6c0
(lldb) po $x0
2.23.25.85
(lldb) po [$x0 class]
__NSCFString
```

调用

* `libswiftFoundation.dylib`
  * `static Swift.String._unconditionallyBridgeFromObjectiveC(Swift.Optional<__C.NSString>) -> Swift.String`

而Bridge桥接过来的

->

此处：想要查看出字符串的值：

* （方式1）加上NSString强制类型转换去打印字符串

```c
(lldb) po (NSString*)0x000000028143c6c0
2.23.25.85
```

* （方式2）自己查看ObjC的NSString的内存值，手动打印出字符串

此处（NSString类型的字符串的）内存值是：

```c
(lldb) x/8xg 0x00000028143c6c0
0x28143c6c0: 0x000021a1efdfa941 0x000000030000078c
0x28143c6d0: 0x35322e33322e320a 0x000000000035382e
0x28143c6e0: 0x00004fcde1ddc6e0 0x0000000000000041
0x28143c6f0: 0x0000000000000000 0x0000000000000000
```

其中核心数据是：

* `0x28143c6d0: 0x35322e33322e320a 0x000000000035382e`

->可以调试查看到具体字符串的值和长度：

```c
(lldb) p/c 0x35322e33322e320a
(long) \n2.23.25
(lldb) p/c 0x000000000035382e
(int) .85\0

(lldb) x/8xb 0x28143c6d0
0x28143c6d0: 0x0a 0x32 0x2e 0x32 0x33 0x2e 0x32 0x35
(lldb) x/8xb 0x28143c6d8
0x28143c6d8: 0x2e 0x38 0x35 0x00 0x00 0x00 0x00 0x00
(lldb) x/s 0x28143c6d0
0x28143c6d0: "\n2.23.25.85"
(lldb) x/s 0x28143c6d8
0x28143c6d8: ".85"

(lldb) p/d 0xa
(int) 10
(lldb) x/s 0x28143c6d1
0x28143c6d1: "2.23.25.85"
```

即：

* 字符串：
  * 长度：`10`
  * 值：`2.23.25.85`
