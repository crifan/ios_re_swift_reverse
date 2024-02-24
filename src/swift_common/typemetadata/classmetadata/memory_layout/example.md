# 举例

## __DataStorage

具体详见：

[ClassMetadata 内存布局 图](../../../../swift_common/typemetadata/classmetadata/memory_layout/figure.md)

对应调试细节：

### ClassMetadata

```c
(lldb) x/10xg 0x0000000207cb7d90
0x207cb7d90: 0x0000000207cb7d58 0x0000000207c47ed8
0x207cb7da0: 0x00000001c29c8490 0x0000805000000000
0x207cb7db0: 0x0000000281e95702 0x0000000000000002
0x207cb7dc0: 0x0000000700000041 0x00000010000000d0
0x207cb7dd0: 0x00000001b2009838 0x0000000000000000

(lldb) po 0x0000000207cb7d58
Foundation.__DataStorage
(lldb) po 0x0000000207c47ed8
_TtCs12_SwiftObject

(lldb) x/2xh 0x207cb7db8
0x207cb7db8: 0x0002 0x0000

(lldb) x/2xh 0x207cb7dc0
0x207cb7dc0: 0x0041 0x0000

(lldb) x/2xw 0x207cb7db8
0x207cb7db8: 0x00000002 0x00000000

(lldb) x/2xw 0x207cb7dc0
0x207cb7dc0: 0x00000041 0x00000007

(lldb) x/2xh 0x207cb7dc4
0x207cb7dc4: 0x0007 0x0000

(lldb) x/2xw 0x207cb7dc8
0x207cb7dc8: 0x000000d0 0x00000010
```

->

* __DataStorage的Class的TypeMetadata
  * [+0x00] = var kind: Int
    * 0x0000000207cb7d58
      * Foundation.__DataStorage
  * [+0x08] = var superClass: UnsafeMutablePointer<ClassMetadata>
    * 0x0000000207c47ed8
      * _TtCs12_SwiftObject
  * [+0x10] = Int cacheData[2]
    * 0x00000001c29c8490
    * 0x0000805000000000
  * [+0x20] = var data: Int
    * 0x0000000281e95702
  * [+0x28] = var classFlags: Int32
    * 0x00000002
  * [+0x2C] = var instanceAddressPoint: UInt32
    * 0x00000000
  * [+0x30] = var instanceSize: UInt32
    * 0x00000041
  * [+0x34] = var instanceAlignmentMask: UInt16
    * 0x0007
  * [+0x36] = var reserved: UInt16
    * 0x0000
  * [+0x38] = var classSize: UInt32
    * 0x000000d0
  * [+0x3C] = var classAddressPoint: UInt32
    * 0x00000010
  * [+0x40] = var typeDescriptor: UnsafeMutablePointer<TargetClassDescriptor>
    * 0x00000001b2009838
  * [+0x48] = var iVarDestroyer: UnsafeMutablePointer<ClassIVarDestroyer>
    * 0x0000000000000000

以及：

### struct TargetClassDescriptor

```c
(lldb) x/8xg 0x00000001b2009838
0x1b2009838: 0xffffbc3480000050 0xfff49e10ffffffe8
0x1b2009848: 0x0000000000010464 0x0000001800000002
0x1b2009858: 0x000000060000000e 0x000000100000000a
0x1b2009868: 0x0000000100000008 0x00000001fff33690
(lldb) x/16xw 0x00000001b2009838
0x1b2009838: 0x80000050 0xffffbc34 0xffffffe8 0xfff49e10
0x1b2009848: 0x00010464 0x00000000 0x00000002 0x00000018
0x1b2009858: 0x0000000e 0x00000006 0x0000000a 0x00000010
0x1b2009868: 0x00000008 0x00000001 0xfff33690 0x00000001
```

->
* Foundation.__DataStorage TypeDescriptor == struct TargetClassDescriptor
  * [+0x00] = var flags: UInt32
    * 0x80000050
  * [+0x04] = var parent: TargetRelativeDirectPointer<UnsafeRawPointer>
    * 0xffffbc34
  * [+0x08] = var name: TargetRelativeDirectPointer<CChar>
    * 0xffffffe8
  * [+0x0C] = var accessFunctionPointer: TargetRelativeDirectPointer<UnsafeRawPointer>
    * 0xfff49e10
  * [+0x10] = var fieldDescriptor: TargetRelativeDirectPointer<FieldDescriptor>
    * 0x00010464
  * [+0x14] = var superClassType: TargetRelativeDirectPointer<CChar>
    * 0x00000000
  * [+0x18] = Int32 Union
    * var ResilientMetadataBounds: RelativeDirectPointer<TargetStoredClassMetadataBounds>
    * var metadataNegativeSizeInWords: UInt32
      * 估计是：0x00000002
  * [+0x1C] = Int32 Union
    * var extraClassFlags: ExtraClassDescriptorFlags
    * var metadataPositiveSizeInWords: UInt32
    * 暂不确定是哪个
    * 此处值：0x00000018
  * [+0x20] = var numImmediateMembers: UInt32
    * 0x0000000e
  * [+0x24] = var numFields: UInt32
    * 0x00000006
  * [+0x28] = var fieldOffsetVectorOffset: UInt32
    * 0x0000000a
  * [+0x2C] = var Offset: UInt32
    * 0x00000010
  * [+0x30] = var size: UInt32
    * 0x00000008
  * [+0x34] = var firstVtableData: VTableBuffer<VTable>
    * 0x00000001
