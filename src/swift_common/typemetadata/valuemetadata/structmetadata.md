# StructMetadata

## StructMetadata的内存布局

### 图

* Swift的StructMetadata内存布局图  = Swift ValueMetadata Memory Layout
  * 在线预览
    * [Swift的StructMetadata内存布局图| ProcessOn免费在线作图,在线流程图,在线思维导图](https://www.processon.com/view/link/65d80e3ad609432b5b881dff)
  * 离线查看
    * ![swift_structmetadata_memory_layout](../../../assets/img/swift_structmetadata_memory_layout.jpg)
  * 核心定义
    * ![swift_structmetadata_memory_layout_core](../../../assets/img/swift_structmetadata_memory_layout_core.jpg)


### 文字

* Swift的StructMetadata的内存布局
  * [-0x08] = ValueWitnessTable* vwt;
  * [+0x00] = int64 kind // 是固定的：0x200
  * [+0x08] = StructDescriptor* description
    * StructDescriptor
      * FieldDescriptor
        * FieldRecord
