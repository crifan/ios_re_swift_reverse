# ClassMetadata的内存布局

## 概述

### ClassMetadata = Class的TypeMetadata

* Class的TypeMetadata
  * [+0x00] = var kind: Int
  * [+0x08] = var superClass: UnsafeMutablePointer<ClassMetadata>
  * [+0x10] = Int cacheData[2]
  * [+0x20] = var data: Int
  * [+0x28] = var classFlags: Int32
  * [+0x2C] = var instanceAddressPoint: UInt32
  * [+0x30] = var instanceSize: UInt32
  * [+0x34] = var instanceAlignmentMask: UInt16
  * [+0x36] = var reserved: UInt16
  * [+0x38] = var classSize: UInt32
  * [+0x3C] = var classAddressPoint: UInt32
  * [+0x40] = var typeDescriptor: UnsafeMutablePointer<TargetClassDescriptor>
  * [+0x48] = var iVarDestroyer: UnsafeMutablePointer<ClassIVarDestroyer>

### struct TargetClassDescriptor

* struct TargetClassDescriptor
  * [+0x00] = var flags: UInt32
  * [+0x04] = var parent: TargetRelativeDirectPointer<UnsafeRawPointer>
  * [+0x08] = var name: TargetRelativeDirectPointer<CChar>
  * [+0x0C] = var accessFunctionPointer: TargetRelativeDirectPointer<UnsafeRawPointer>
  * [+0x10] = var fieldDescriptor: TargetRelativeDirectPointer<FieldDescriptor>
  * [+0x14] = var superClassType: TargetRelativeDirectPointer<CChar>
  * [+0x18] = Int32 Union
    * var ResilientMetadataBounds: RelativeDirectPointer<TargetStoredClassMetadataBounds>
    * var metadataNegativeSizeInWords: UInt32
  * [+0x1C] = Int32 Union
    * var extraClassFlags: ExtraClassDescriptorFlags
    * var metadataPositiveSizeInWords: UInt32
  * [+0x20] = var numImmediateMembers: UInt32
  * [+0x24] = var numFields: UInt32
  * [+0x28] = var fieldOffsetVectorOffset: UInt32
  * [+0x2C] = var Offset: UInt32
  * [+0x30] = var size: UInt32
  * [+0x34] = var firstVtableData: VTableBuffer<VTable>

## 详解

### Swift中类的继承关系

* Swift中类的继承关系
  * 详解
    * TargetClassMetadataType == ClassMetadata
      * = TargetClassMetadata<Runtime, TargetAnyClassMetadataType<Runtime>>
    * TargetClassMetadataObjCInterop
      * = TargetClassMetadata<Runtime, TargetAnyClassMetadataObjCInterop<Runtime>>
    * ->
    * TargetAnyClassMetadataType
      * ObjCInterop=true == 兼容ObjC类 == 支持Objective-C类互操作
        * TargetAnyClassMetadataObjCInterop
      * ObjCInterop=false == 不兼容ObjC类
        * TargetAnyClassMetadata
    * TargetAnyClassMetadataObjCInterop
      * TargetAnyClassMetadata
    * TargetClassMetadata
      * TargetAnyClassMetadataVariant
    * ->
    * TargetAnyClassMetadata
      * TargetHeapMetadata == HeapMetadata
        * TargetMetadata
  * 概述
    * ClassMetadata
      * TargetClassMetadata
        * TargetAnyClassMetadataObjCInterop
          * TargetAnyClassMetadata
            * TargetHeapMetadata
              * TargetMetadata

### Swift中的：ClassMetadata 字段定义

* Swift中的：ClassMetadata 字段定义
  * TargetMetadata
    * var kind: Int
      * 在oc中放的就是isa，在swift中kind大于0x7FF表示的就是类
  * TargetHeapMetadata
    * 没属性=字段
  * TargetAnyClassMetadata
    * var superClass: UnsafeMutablePointer<ClassMetadata>
      * 父类的Metadata，如果是null说明是最顶级的root类了
  * TargetAnyClassMetadataObjCInterop
    * Int cacheData[2]
      * 缓存数据用于某些动态查找，它由运行时拥有，通常需要与Objective-C的使用进行互操作。（说到底就是OC的东西）
    * var data: Int
      * 除了编译器设置低位以表明这是Swift元类型（因此存在对应的类型元数据的头信息）外，这个data里存的指针，用于行外元数据，通常是不透明的（应该也是OC的）
  * TargetClassMetadata
    * var classFlags: Int32
      * Swift-specific class flags
    * var instanceAddressPoint: UInt32
      * The address point of instances of this type
    * var instanceSize: UInt32
      * The required size of instances of this type.(实例对象在堆内存的大小)
    * var instanceAlignmentMask: UInt16
      * The alignment mask of the address point of instances of this type. (根据这个mask来获取内存中的对齐大小)
    * var reserved: UInt16
      * Reserved for runtime use.（预留给运行时使用）
    * var classSize: UInt32
      * The total size of the class object, including prefix and suffix extents.
    * var classAddressPoint: UInt32
      * The offset of the address point within the class object.
    * var typeDescriptor: UnsafeMutablePointer<TargetClassDescriptor>
      * 一个对类型的超行的swift特定描述，如果这是一个人工子类，则为null。目前不提供动态创建非人工子类的机制。
    * var iVarDestroyer: UnsafeMutablePointer<ClassIVarDestroyer>
      * 销毁实例变量的函数，用于在构造函数早期返回后进行清理。如果为null，则不会执行清理操作，并且所有的ivars都必须是简单的。

### struct TargetClassDescriptor

* struct TargetClassDescriptor
  * var flags: UInt32
    * 存储在任何上下文描述符的第一个公共标记
  * var parent: TargetRelativeDirectPointer<UnsafeRawPointer>
    * 复用的RelativeDirectPointer这个类型，其实并不是，但看下来原理一样;
    * 父级上下文，如果是顶级上下文则为null
  * var name: TargetRelativeDirectPointer<CChar>
    * 获取类的名称
  * var accessFunctionPointer: TargetRelativeDirectPointer<UnsafeRawPointer>
    * 这里的函数类型是一个替身，需要调用getAccessFunction()拿到真正的函数指针（这里没有封装），会得到一个MetadataAccessFunction元数据访问函数的指针的包装器类，该函数提供operator()重载以使用正确的调用约定来调用它（可变长参数），意外发现命名重整会调用这边的方法（目前不太了解这块内容）。
  * var fieldDescriptor: TargetRelativeDirectPointer<FieldDescriptor>
    * 一个指向类型的字段描述符的指针(如果有的话)。类型字段的描述，可以从里面获取结构体的属性。
  * var superClassType: TargetRelativeDirectPointer<CChar>
    * The type of the superclass, expressed as a mangled type name that can refer to the generic arguments of the subclass type.
  * Int32 Union (下面两个属性在源码中是union类型，所以取size大的类型作为属性（这里貌似一样），具体还得判断是否have a resilient superclass)
    * var ResilientMetadataBounds: RelativeDirectPointer<TargetStoredClassMetadataBounds>
      * 有resilient superclass，用ResilientMetadataBounds，表示对保存元数据扩展的缓存的引用
    * var metadataNegativeSizeInWords: UInt32
      * 没有resilient superclass使用MetadataNegativeSizeInWords，表示该类元数据对象的负大小(用字节表示)
  * Int32 Union
    * var extraClassFlags: ExtraClassDescriptorFlags
      * 有resilient superclass，用ExtraClassFlags，表示一个Objective-C弹性类存根的存在
    * var metadataPositiveSizeInWords: UInt32
      * 没有resilient superclass使用MetadataPositiveSizeInWords，表示该类元数据对象的正大小(用字节表示)
  * var numImmediateMembers: UInt32
    * 此类添加到类元数据的其他成员的数目。默认情况下，这些数据对运行时是不透明的，而不是在其他成员中公开;它实际上只是NumImmediateMembers * sizeof(void*)字节的数据。
    *   这些字节是添加在地址点之前还是之后，取决于areImmediateMembersNegative()方法。
  * var numFields: UInt32
    * 属性个数，不包含父类的
  * var fieldOffsetVectorOffset: UInt32
    * 存储这个结构的字段偏移向量的偏移量（记录你属性起始位置的开始的一个相对于metadata的偏移量，具体看metadata的getFieldOffsets方法），如果为0，说明你没有属性
    * 如果这个类含有一个弹性的父类，那么从他的弹性父类的metaData开始偏移
  * var Offset: UInt32
  * var size: UInt32 //VTable数量
  * var firstVtableData: VTableBuffer<VTable> //VTable
