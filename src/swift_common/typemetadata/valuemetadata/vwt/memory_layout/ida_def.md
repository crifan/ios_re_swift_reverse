# VWT内存布局的IDA定义

* struct `ValueWitnessTable`

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

* enum `TargetValueWitnessFlags`

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