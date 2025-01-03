# Crifan新增Swift相关定义

* Update: `20241217`

虽然IDA中已自带一些定义，但是不够全，自己额外又去新增了一些Swift相关定义：

## SwiftObject

说明：虽然IDA已自带`_TtCs12_SwiftObject`，但是没有具体大小的定义，且名字不够简洁，所以自己还是加上自己的定义

```c
struct SwiftObject // _TtCs12_SwiftObject
{
  unsigned __int8 opaque[16];
};
```

## ClassMetadata

```c
struct ClassMetadata
{
  __int64 kind;
  void *superClass;
  __int64 cacheData[2];
  void *data;
  __int32 classFlags;
  __int32 instanceAddressPoint;
  __int32 instanceSize;
  __int16 instanceAlignmentMask;
  __int16 reserved;
  __int32 classSize;
  __int32 classAddressPoint;
  void *typeDescriptor;
  void *iVarDestroyer;
};
```

## TargetValueWitnessFlags

```c
enum TargetValueWitnessFlags : __int32
{
  AlignmentMask = 0xFF,
  IsNonPOD = 0x10000,
  IsNonInline = 0x20000,
  HasSpareBits = 0x80000,
  IsNonBitwiseTakable = 0x100000,
  HasEnumWitnesses = 0x200000,
  Incomplete = 0x400000,
  IsNonCopyable = 0x800000,
};
```

## ValueWitnessTable

IDA虽然自带`ValueWitnessTable`，但是细节不够好。所以还是加上自己更新后的，内容更全的：

```c
struct __cppobj ValueWitnessTable
{
  void (__fastcall *initializeBufferWithCopyOfBuffer)(void *dst, void *src, void *metadataSelf);
  void (__fastcall *destroy)(void *object, void *witnessSelf);
  void (__fastcall *initializeWithCopy)(void *dst, void *src, void *metadataSelf);
  void (__fastcall *assignWithCopy)(void *dst, void *src, void *metadataSelf);
  void (__fastcall *initializeWithTake)(void *dst, void *src, void *metadataSelf);
  void (__fastcall *assignWithTake)(void *dst, void *src, void *metadataSelf);
  unsigned __int64 (__fastcall *getEnumTagSinglePayload)(void *enumPtr, __int64 emptyCases, void *metadataSelf);
  void (__fastcall *storeEnumTagSinglePayload)(void *enumPtr, __int64 whichCase, void *metadataSelf);
  __int64 size;
  __int64 stride;
  TargetValueWitnessFlags flags;
  __int32 extraInhabitantCount;
};
```

## SwiftString

* 说明
  * IDA已自带：`Swift::String`，其实和此处定义一样，但是自己的类名`SwiftString`，更加简洁好用
  * 且后续已优化为，`SwiftString`是`SwiftLargeString`和`SwiftSmallString`的union联合体结构，更加准确

### SwiftLargeString

```c
struct SwiftLargeString
{
    // 1st int64: b0-b63
    // __int64 flagsAndCount;
    __int64 count: 48; // b0-b47

    __int64 reserved: 11; // b48-b58

    __int64 isForeignUTF8 : 1; // b59
    __int64 isTailAllocated : 1; // b60
    __int64 isNativeStored : 1; // b61
    __int64 isNFC : 1; // b62
    __int64 isASCII : 1; // b63

    // 2nd int64: b0-b63

    // 2nd int64: b0-b59
    __int64 objectAddr : 60;

    // 2nd int64: b60-b63
    __int64 isForeign:  1;
    __int64 isSmall:  1;
    __int64 isBridged:  1;
    __int64 isImmortal:  1;
};
```

注：

* 对比，旧的定义是
  ```c
  struct SwiftLargeString
  {
    __int64 flagsAndCount;
    void *objAddr;
  }
  ```

### SwiftSmallString

```c
struct SwiftSmallString
{
    char smallStr[15];

    // 2nd int64: b56-b59
    __int8 count: 4;
    // 2nd int64: b60-b63
    __int8 isForeign:  1;
    __int8 isSmall:  1;
    __int8 isASCII:  1;
    __int8 isImmortal:  1;
}
```

### SwiftString

```c
union SwiftString
{
    SwiftLargeString largeStr;
    SwiftSmallString smallStr;
}
```

## SwiftArray

```c
struct SwiftArray
{
  void *heapMetadata;
  __int64 refCount;
  __int64 count;
  __int64 _capacityAndFlags;
  void* firstElement;
};
```

## Struct

### SwiftProtocolStructure

```c
struct SwiftProtocolStructure
{
    __int64 valueBuffer0; // case1: field1, case2: void* realStruct
    __int64 valueBuffer1; // case1: field2, case2: int64 unused1
    __int64 valueBuffer2; // case1: field3, case2: int64 unused2
    void* typeMetadata;
    void* pwt;
};
```

## SwiftSet

```c
struct SwiftSet
{
  void *type;
  __int64 refCount;
  __int64 _count;
  __int64 _capacity;
  __int8 _scale;
  __int8 _reservedScale;
  __int16 _extra;
  __int32 _age;
  __int64 _seed;
  __int64 _rawElements;
  __int64 _metadata;
  __int64 _firstElement;
};
```

## Swift_DataStorage

```c
struct Swift_DataStorage
{
  void *isa;
  __int64 refCount;
  void *_bytes;
  __int64 _length;
  __int64 _capacity;
  __int64 _offset;
  void *_deallocator;
  bool _needToZero;
};
```

## SwiftData_InlineSlice

```c
struct SwiftData_InlineSlice
{
  __int32 slice;
  Swift_DataStorage *storage;
};
```
