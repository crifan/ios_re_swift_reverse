# IDA定义

加到IDA中的定义：

* 说明
  * IDA已自带：`Swift::String`，其实和此处定义一样，但是自己的类名`SwiftString`，更加简洁好用
  * 且后续已优化为，`SwiftString`是`SwiftLargeString`和`SwiftSmallString`的union联合体结构，更加准确

## SwiftLargeString

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

## SwiftSmallString

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

## SwiftString

```c
union SwiftString
{
    SwiftLargeString largeStr;
    SwiftSmallString smallStr;
}
```
