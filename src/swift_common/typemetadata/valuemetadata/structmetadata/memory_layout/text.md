# StructMetadata的内存布局文字版

* Swift的StructMetadata的内存布局
  * [-0x08] = ValueWitnessTable* `vwt`;
  * [+0x00] = int64 `kind`
    * 是**固定的**：`0x200`
  * [+0x08] = **StructDescriptor*** `description`
    * StructDescriptor
      * FieldDescriptor
        * FieldRecord
