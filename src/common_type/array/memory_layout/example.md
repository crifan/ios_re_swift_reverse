# 举例

## String的Array

```c
(lldb) x/10gx 0x0000000280c2e2c0
0x280c2e2c0: 0x00000001efe3c008 0x0000000000000003
0x280c2e2d0: 0x0000000000000002 0x0000000000000004
0x280c2e2e0: 0x0000656e6f687069 0xe600000000000000
0x280c2e2f0: 0x0000000069626d73 0xe400000000000000
0x280c2e300: 0x00000001efdf52d0 0x7fffffff7fffffff
(lldb) po 0x00000001efe3c008
_TtGCs23_ContiguousArrayStorageSS_$
(lldb) p/c 0x0000656e6f687069
(long) iphone\0\0
(lldb) p/c 0x0000000069626d73
(int) smbi
```

->

* Array的内存布局=字段
  * HeapMetadata* metadata
    * 0x00000001efe3c008
      * `_TtGCs23_ContiguousArrayStorageSS_$`
        * String的Array
  * int64 refCount
    * 0x0000000000000003
  * int64 count
    * 0x0000000000000002
  * int64 _capacityAndFlags
    * 0x0000000000000004
  * 第一个 = [0x280c2e2e0]
    * 0x0000656e6f687069 0xe600000000000000
      * "iphone"
  * 第二个 = [0x280c2e2f0]
    * 0x0000000069626d73 0xe400000000000000
      * "smbi"
