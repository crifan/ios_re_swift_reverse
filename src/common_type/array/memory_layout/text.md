# 文字

* Array的内存布局=字段=属性
  * [+0x00] = HeapMetadata* metadata
  * [+0x08] = int64 refCount
  * [+0x10] = int64 count
  * [+0x08] = int64 _capacityAndFlags
  * [+0x20] = AnyObject* firstElementAddress
