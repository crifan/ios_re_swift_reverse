# IDA定义

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
