# IDA自带Swift相关定义

* Update: `20241217`

此处发现，最新版IDA（`IDA v8.2.230124`），去分析某个Swift的Mach-O后，已经自带（=自动分析出）一些Swift相关定义：

## Swift::UInt

```c
typedef unsigned __int64 Swift::UInt;
```

## Swift::Double

```c
typedef double Swift::Double;
```

## Swift::String

```c
struct Swift::String
{
  __int64 _countAndFlagsBits;
  void *_object;
};
```

## _TtCs12_SwiftObject

```c
struct _TtCs12_SwiftObject;
```

## ClassDescriptor

```c
struct ClassDescriptor
{
  int Flags;
  __int32 Parent;
  __int32 Name;
  __int32 AccessFunction;
  __int32 FieldDescriptor;
  __int32 SuperclassType;
  int MetadataNegativeSizeInWords;
  int MetadataPositiveSizeInWords;
  int NumImmediateMembers;
  int NumFields;
};
```

## ModuleDescriptor

```c
struct ModuleDescriptor
{
  int Flags;
  __int32 Parent;
  __int32 Name;
};
```

## StructDescriptor

```c
struct StructDescriptor
{
  int Flags;
  __int32 Parent;
  __int32 Name;
  __int32 AccessFunction;
  __int32 FieldDescriptor;
  int NumFields;
  int FieldOffsetVectorOffset;
};
```

## FieldDescriptorKind

```c
enum FieldDescriptorKind : __int16
{
  FDK_Struct = 0,
  FDK_Class = 1,
  FDK_Enum = 2,
  FDK_MultiPayloadEnum = 3,
  FDK_Protocol = 4,
  FDK_ClassProtocol = 5,
  FDK_ObjCProtocol = 6,
  FDK_ObjCClass = 7,
};
```

## FieldDescriptor

```c
struct FieldDescriptor
{
  __int32 MangledTypeName;
  __int32 Superclass;
  FieldDescriptorKind Kind;
  __int16 FieldRecordSize;
  int NumFields;
};
```

## FieldRecord

```c
struct FieldRecord
{
  int Flags;
  __int32 MangledTypeName;
  __int32 FieldName;
};
```

## EnumDescriptor

```c
struct EnumDescriptor
{
  int Flags;
  __int32 Parent;
  __int32 Name;
  __int32 AccessFunction;
  __int32 FieldDescriptor;
  int NumPayloadCasesAndPayloadSizeOffset;
  int NumEmptyCases;
};
```

## MetadataKind

```c
enum MetadataKind : __int32
{
  MK_Class = 0x0,
  MK_Struct = 0x200,
  MK_Enum = 0x201,
  MK_Optional = 0x202,
  MK_ForeignClass = 0x203,
  MK_ForeignReferenceType = 0x204,
  MK_Opaque = 0x300,
  MK_Tuple = 0x301,
  MK_Function = 0x302,
  MK_Existential = 0x303,
  MK_Metatype = 0x304,
  MK_ObjCClassWrapper = 0x305,
  MK_ExistentialMetatype = 0x306,
  MK_ExtendedExistential = 0x307,
  MK_HeapLocalVariable = 0x400,
  MK_HeapGenericLocalVariable = 0x500,
  MK_ErrorObject = 0x501,
  MK_Task = 0x502,
  MK_Job = 0x503,
  MK_LastEnumerated = 0x7FF,
};
```

## ValueMetadata

```c
struct ValueMetadata
{
  __int64 kind;
  void *description;
};
```

## ValueWitnessTable

```c
struct ValueWitnessTable
{
  void *initializeBufferWithCopyOfBuffer;
  void *destroy;
  void *initializeWithCopy;
  void *assignWithCopy;
  void *initializeWithTake;
  void *assignWithTake;
  void *getEnumTagSinglePayload;
  void *storeEnumTagSinglePayload;
  __int64 size;
  __int64 stride;
  int flags;
  int extraInhabitantCount;
};
```

## ProtocolDescriptor

```c
struct ProtocolDescriptor
{
  int Flags;
  __int32 Parent;
  __int32 Name;
  int NumRequirementsInSignature;
  int NumRequirements;
  __int32 AssociatedTypeNames;
};
```

## AnonymousContextDescriptor

```c
struct AnonymousContextDescriptor
{
  int Flags;
  __int32 Parent;
};
```
