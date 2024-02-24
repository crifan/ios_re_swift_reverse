# IDA定义

* struct `Swift_DataStorage`

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

* struct `SwiftData_InlineSlice`

```c
struct SwiftData_InlineSlice
{
  __int32 slice;
  Swift_DataStorage *storage;
};
```
