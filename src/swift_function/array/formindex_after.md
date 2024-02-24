# Array.formIndex(after:)

## 源码

`swift/stdlib/public/core/Array.swift`

```c
  /// Replaces the given index with its successor.
  ///
  /// - Parameter i: A valid index of the collection. `i` must be less than
  ///   `endIndex`.
  @inlinable
  public func formIndex(after i: inout Int) {
    // NOTE: this is a manual specialization of index movement for a Strideable
    // index that is required for Array performance.  The optimizer is not
    // capable of creating partial specializations yet.
    // NOTE: Range checks are not performed here, because it is done later by
    // the subscript function.
    i += 1
  }
```

## 总结

* Swift函数：Array.formIndex(after:)
  * 定义
    * formIndex(after i: inout Int)
  * 参数
    * 变量名
      * 传入：after
      * 内部：i
      * 类型：inout Int == 指针类型，内部会改变值
  * 返回值
    * 传入的after==i （值已加1）
