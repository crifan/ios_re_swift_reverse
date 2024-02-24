# IDA定义

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
