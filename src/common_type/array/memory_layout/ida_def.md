# IDA定义

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
