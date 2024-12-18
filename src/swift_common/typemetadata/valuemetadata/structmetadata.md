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
  * [+0x00] = int64 kind
    * 是**固定的**：`0x200`
  * [+0x08] = StructDescriptor* description
    * StructDescriptor
      * FieldDescriptor
        * FieldRecord

### 举例

#### `ConcreteConfigPrimitiveBox`的`StructDescriptor`

内存值：

```bash
(lldb) x/16gw 0x000000010771431c
0x10771431c: 0x000000d1 0xffffffdc 0xffe75bcc 0xfd6669b4
0x10771432c: 0x00000000 0x00000001 0x00000004 0x00a2c760
0x10771433c: 0xffe75bd4 0x00010001 0x00000002 0x00000080
0x10771434c: 0x00000080 0x000a5870 0xfffffe78 0x000000c2
```

对应着IDA中的汇编：

```asm
__constg_swiftt:0000000102DF831C ; nominal type descriptor for MainAppLibrary.ConcreteConfigPrimitiveBox
__constg_swiftt:0000000102DF831C $s14MainAppLibrary26ConcreteConfigPrimitiveBoxVMn StructDescriptor <0xD1, stru_102DF82FC - ., aConcreteconfig - ., \
__constg_swiftt:0000000102DF831C                                         ; DATA XREF: type metadata accessor for ConcreteConfigPrimitiveBox↑o
__constg_swiftt:0000000102DF831C                                         ; __swift5_types:0000000102EA5EAC↓o ...
__constg_swiftt:0000000102DF831C                                   $s14MainAppLibrary26ConcreteConfigPrimitiveBoxVMa - .,\ ; type metadata accessor for ConcreteConfigPrimitiveBox ...
__constg_swiftt:0000000102DF831C                                   $s14MainAppLibrary26ConcreteConfigPrimitiveBoxVMn.FieldDescriptor - .,\
__constg_swiftt:0000000102DF831C                                   1, 4>
```

->

* StructDescriptor的例子：ConcreteConfigPrimitiveBox
  * struc : sizeof=0x1C == 0x000000010771431c == IDA的 0x0000000102df831c
    * [0x00~0x03] = Flags
      * 0x000000d1
    * [0x04~0x07] = Parent
      * 0xffffffdc
        * stru_102DF82FC - .
        * = 0x102DF82FC - (0x0000000102df831c + 0x4)
        * = 0x102DF82FC - 0x0000000102df8320
        * = 0xFFFFFFDC
    * [0x08~0x0B] = Name
      * 0xffe75bcc
        * aConcreteconfig - .
          * aConcreteconfig == "ConcreteConfigPrimitiveBox"
        * = 0x102C6DEF0 - (0x0000000102df831c + 0x8)
        * = 0xFFE75BCC
    * [0x0C~0x0F] = AccessFunction = AccessFunctionPtr
      * 0xfd6669b4
        * $s14MainAppLibrary26ConcreteConfigPrimitiveBoxVMa - .
        * = 0x10045ECDC - (0x0000000102df831c + 0xC)
        * = 0xFD6669B4
    * [0x10~0x13] = FieldDescriptor == Fields
      * 0x00000000
        * $s14MainAppLibrary26ConcreteConfigPrimitiveBoxVMn.FieldDescriptor - .
        * = 0x0000000102df832c - (0x0000000102df831c + 0x10)
        * = 0
    * [0x14~0x17] = NumFields
      * 0x00000001
        * 这个结构体只有一个字段=属性property=域field ？
        * 好像是：field offset vector length = 1
          * 此处是有1个field offset vector的
    * [0x18~0x1B] = FieldOffsetVectorOffset
      * 0x00000004
        * The offset of the field offset vector for this struct's stored properties in its metadata
          * 此处的field offset vector的offset是4
