# Swift的Dictionary内存布局 文字

* Swift的Dictionary内存布局 = 字段 = 属性
  * `[+0x00]` = `Metadata* type`
  * `[+0x08]` = `Int refCount`
  * `__RawDictionaryStorage`
    * `[+0x10]` = `var _count: Int`
    * `[+0x18]` = `var _capacity: Int`
    * `[+0x20]` = `Int`
      * `[+0x20~0x20]` = `var _scale: Int8`
      * `[+0x21~0x21]` = `var _reservedScale: Int8`
      * `[+0x22~0x23]` = `var _extra: Int16`
      * `[+0x24~0x27]` = `var _age: Int32`
    * `[+0x28]` = `var _seed: Int`
    * `[+0x30]` = `var _rawKeys: UnsafeMutableRawPointer`
      * 指向`Key`=`键`的数组列表
    * `[+0x38]` = `var _rawValues: UnsafeMutableRawPointer`
      * 指向`Value`=`值`的数组列表
    * `[+0x38]` = `int _metadata`
      * 指定了**哪几个**index元素是**有效数据**
