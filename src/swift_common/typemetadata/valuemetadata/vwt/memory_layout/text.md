# VWT内存布局 文字版

## 概述

* `Swift`的`VWT`=`Value Witness Table` 结构布局=字段=属性
  * [+0x00] = void* `initializeBufferWithCopyOfBuffer`
  * [+0x08] = void* `destroy`
  * [+0x10] = void* `initializeWithCopy`
  * [+0x18] = void* `assignWithCopy`
  * [+0x20] = void* `initializeWithTake`
  * [+0x28] = void* `assignWithTake`
  * [+0x30] = void* `getEnumTagSinglePayload`
  * [+0x38] = void* `storeEnumTagSinglePayload`
  * [+0x40] = void* int64 `size`
  * [+0x48] = void* int64 `stride`
  * [+0x50~0x53] = int32 `flags`
  * [+0x54~0x57] = int32 `extraInhabitantCount`

## 详解

* `Swift`的`VWT`=`Value Witness Table` 结构布局=字段=属性
  * [+0x00] = initializeBufferWithCopyOfBuffer
    * 定义：`T *(*initializeBufferWithCopyOfBuffer)(B *dest, B *src, M *self);`
  * [+0x08] = destroy
    * 定义：`void (*destroy)(T *object, witness_t *self);`
  * [+0x10] = initializeWithCopy
    * 定义：`T *(*initializeWithCopy)(T *dest, T *src, M *self);`
  * [+0x18] = assignWithCopy
    * 定义：`T *(*assignWithCopy)(T *dest, T *src, M *self);`
  * [+0x20] = initializeWithTake
    * 定义：`T *(*initializeWithTake)(T *dest, T *src, M *self);`
  * [+0x28] = assignWithTake
    * 定义：`T *(*assignWithTake)(T *dest, T *src, M *self);`
  * [+0x30] = getEnumTagSinglePayload
    * 定义：`unsigned (*getEnumTagSinglePayload)(const T* enum, UINT_TYPE emptyCases, M *self);`
  * [+0x38] = storeEnumTagSinglePayload
    * 定义：`void (*storeEnumTagSinglePayload)(T* enum, UINT_TYPE whichCase, UINT_TYPE emptyCases, M *self);`
  * [+0x40] = size
    * 定义：`SIZE_TYPE size;`
  * [+0x48] = stride
    * 定义：`SIZE_TYPE stride;`
  * [+0x50~0x53] = flags
    * 定义：`UINT_TYPE flags;`
  * [+0x54~0x57] = extraInhabitantCount
    * 定义：`UINT_TYPE extraInhabitantCount;`
* 说明
  * `SIZE_TYPE` = `StoredSize` = `size_t` ? = `int64`
  * `UINT_TYPE` = `unsigned` = `int32`
