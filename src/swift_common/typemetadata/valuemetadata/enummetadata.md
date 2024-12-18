# kind=0x201的EnumMetadata

* Swift的EnumMetadata的内存布局
  * [-0x08] = ValueWitnessTable* vwt;
  * [+0x00] = int64 kind
    * 是**固定的**：`0x201`
  * [+0x08] = EnumDescriptor* description
