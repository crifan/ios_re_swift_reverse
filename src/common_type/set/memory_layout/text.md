# 文字

## 概述

* Swift的Set集合的内存布局=字段=属性
  * [+0x00] = Metadata* type
  * [+0x08] = int64 refCount
  * [+0x10] = int64 _count
  * [+0x18] = int64 _capacity
  * [+0x20] = Int64
    * [+0x20~0x20] = int8 _scale 
    * [+0x21~0x21] = int8 _reservedScale
    * [+0x22~0x23] = int16 _extra
    * [+0x24~0x27] = int32 _age
  * [+0x28] = int64 _seed
  * [+0x30] = void* _rawElements
  * [+0x38] = int64 _metadata

## 详解

* Swift的Set集合的内存布局=字段=属性
  * [+0x00] = Metadata* type
  * [+0x08] = Int refCount
  * __RawSetStorage类型
    * [+0x10] = var _count: Int
    * [+0x18] = var _capacity: Int
    * [+0x20] = Int
      * [+0x20~0x20] = var _scale: Int8
      * [+0x21~0x21] = var _reservedScale: Int8
      * [+0x22~0x23] = var _extra: Int16
      * [+0x24~0x27] = var _age: Int32
    * [+0x28] = var _seed: Int
      * 往往是 == 当前Set指针
    * [+0x30] = var _rawElements: UnsafeMutableRawPointer
      * 指向真正数据的开始的地址 == realDataAddress
    * [+0x38] = var _metadata: UnsafeMutableRawPointer
      * == flag：哪些组数据是有效数据
        * （根据count去mask后的）最后一些bit位中的1，决定了对应位置（对应组）数据是有效数据
          * 可以写成
            * validElementIndexMask
              * count < 2：0b 11 = 0x3
              * 2<= count <= 8：0b 1111 1111 = 0xFF
              * ...
      * 举例
        * 0xfffffffffffffff9
          * 注：此处count=2
            * mask后的值：只需要最后1个byte=8个bit的值
          * 0xfffffffffffffffe & 0xFF = 0x09 = 0b 0000 1001
            * = bit0、bit3（的位置是1）
            * = index0、index3是有效数据
        * 0xfffffffffffffffe
          * 注：此处count=1 
            * （此时count<2）mask后，只需要：2个bit位
              * 对应如果有mask的话，可以理解为：
                * validBitMask = 0x3 = 0b 11
              * mask后的值：只需要最后2位=2个bit
          * 0xfffffffffffffffe & 0x3 = 0x2 = 0b 10
            * = bit1（的位置是1）
            * = index1 是有效数据
  * 说明：
    * 此处Int是64bit==int64
      * Int8=8bit
      * Int32=32bit
