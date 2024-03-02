# 文字

* Swift的Structure内存布局 概述
  * `Pure Structure`
    * 符合对齐标准，挨个字段存放
  * `Protocol Structure`
    * 字段**没超过**`ValueBuffer[3]` == `ValueBuffer[3]`能放得下Structure的所有字段值
      * `ValueBuffer[0~3]`：符合对齐标准，挨个字段存放
      * `[+0x18] = TypeMetadata* metadata`
      * `[+0x20] = ProtocolWitnessTable* pwt`
    * 字段**超过**`ValueBuffer[3]` == `ValueBuffer[3]`放不下Structure的所有字段值
      * `ValueBuffer[0] = void* realStruct` = 真正结构体Structure字段
      * `ValueBuffer[1]` = 没用 = 无效数据
      * `ValueBuffer[2]` = 没用 = 无效数据
      * `[+0x18] = TypeMetadata* metadata`
      * `[+0x20] = ProtocolWitnessTable* pwt`
