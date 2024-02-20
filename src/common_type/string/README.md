# String字符串

## 空间占用=大小

* 空间占用：`0x10`字节 = `16`字节 = **2**个`int64`

## 分类=类型

### 概述

* Swift中String
  * `Small String`
  * `Large String`
    * 三类
      * `Native`
        * 真正地址
          ```bash
          realStrAddr = objectAddr + 0x20
                      = objectAddr + 32
                      = objectAddr + nativeBias
          ```
      * `Shared`
        * 真正地址
          ```bash
          realStrAddr = objectAddr
          ```
        * 说明
          * 对于Bridge的字符串：objectAddr == NSString的地址
            * 内存布局保存的数据，是NSString中的数据
      * `Foreign`
        * 真正地址
          ```bash
          realStrAddr = objectAddr
          ```

### 详解

* Swift中String
  * `Small String`
  * `Large String`
    * 分3类
      * native
        * Native strings have tail-allocated storage, which begins at an offset of `nativeBias` from the storage object's address. String literals, which reside in the constant section, are encoded as their start address minus `nativeBias`, unifying code paths for both literals ("immortal native") and native strings. Native Strings are always managed by the Swift runtime.
      * shared
        * Shared strings do not have tail-allocated storage, but can provide access upon query to contiguous UTF-8 code units. Lazily-bridged NSStrings capable of providing access to contiguous ASCII/UTF-8 set the ObjC bit. Accessing shared string's pointer should always be behind a resilience barrier, permitting future evolution.
      * foreign
        * Foreign strings cannot provide access to contiguous UTF-8. Currently, this only encompasses lazily-bridged NSStrings that cannot be treated as "shared". Such strings may provide access to contiguous UTF-16, or may be discontiguous in storage. Accessing foreign strings should remain behind a resilience barrier for future evolution. Other foreign forms are reserved for the future.
    * 其他说明
      * 对于Shared和foreign
        * always created and accessed behind a resilience barrier, providing flexibility for the future.
