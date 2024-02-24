# 文字

## 概述

* Swift的Data（对于64位，暂忽略32位） 的内存布局
  * 根据数据字节个数多少分类
    * `count=0`
      * empty = 空数据
    * `0 < count < 15` == `0 < count <= 14`
      * `inline` == `struct InlineData`
        * `var bytes: Buffer`
          * `@usableFromInline typealias Buffer = (UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8) //len //enum`
        * `var length: UInt8`
    * `14 < count < 2^32`
      * `slice` == `struct InlineSlice`
        * `var slice: Range<Int32>`
        * `var storage: __DataStorage`
    * `2^32 <= count < 2^64`
      * `large` == `struct LargeSlice`
        * `var slice: RangeReference`
          * `var range: Range<Int64>`
        * `var storage: __DataStorage`

### `__DataStorage`

```c
class __DataStorage {
    var isa: objc_class* // NULL for pure Swift class
    var refCount: UInt64
    var _bytes: UnsafeMutableRawPointer?
    var _length: UInt64
    var _capacity: UInt64
    var _offset: UInt64
    var _deallocator: ((UnsafeMutableRawPointer, Int64) -> Void)?
    var _needToZero: Bool
}
```

==

* `class __DataStorage`
  * [+0x00] = objc_class* isa
  * [+0x08] = int64 refCount
  * [+0x10] = void* _bytes
  * [+0x18] = int64 _length
  * [+0x20] = int64 _capacity
  * [+0x28] = int64 _offset
  * [+0x30] = function_pointer* _deallocator
    * 函数定义：`((UnsafeMutableRawPointer, Int64) -> Void)?`
  * [+0x38] = Bool _needToZero

## 详解

* Swift的Data
  * 分4类
    * 空数据：empty
    * inline == struct InlineData
      * 字段
        * var bytes: Buffer
        * var length: UInt8
      * 说明
        * bytes的Buffer的字节=个数
          * 64位架构：不超过14，即 <=14个字节
            * @usableFromInline typealias Buffer = (UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8, UInt8) //len //enum
          * 32位架构：不超过6，即 <=6个字节
            * @usableFromInline typealias Buffer = (UInt8, UInt8, UInt8, UInt8, UInt8, UInt8) //len //enum
    * slice == struct InlineSlice
      * 字段
        * var slice: Range<Int32>
        * var storage: __DataStorage
      * 说明
        * InlineData的（64bit的）14个 < InlineSlice的字节个数 < 2^64的最大值？（storage pointer + range == a signle word）
    * large == struct LargeSlice
      * 字段
        * var slice: RangeReference
          * var range: Range<Int>
        * var storage: __DataStorage
      * 说明
        * a single word个数=2^64？ < LargeSlice的字节个数 < 双字节大小（two-word size）=2^128？
