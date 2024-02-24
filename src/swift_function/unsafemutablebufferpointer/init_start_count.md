# UnsafeMutableBufferPointer.init(start:count:)

## 源码

`swift/stdlib/public/core/UnsafeRawBufferPointer.swift.gyb`

```c
@frozen
public struct Unsafe${Mutable}RawBufferPointer {
  @usableFromInline
  internal let _position, _end: Unsafe${Mutable}RawPointer?


  /// Creates a buffer over the specified number of contiguous bytes starting
  /// at the given pointer.
  ///
  /// - Parameters:
  ///   - start: The address of the memory that starts the buffer. If `starts`
  ///     is `nil`, `count` must be zero. However, `count` may be zero even
  ///     for a non-`nil` `start`.
  ///   - count: The number of bytes to include in the buffer. `count` must not
  ///     be negative.
  @inlinable
  public init(
    @_nonEphemeral start: Unsafe${Mutable}RawPointer?, count: Int
  ) {
    _debugPrecondition(count >= 0, "${Self} with negative count")
    _debugPrecondition(count == 0 || start != nil,
      "${Self} has a nil start and nonzero count")
    _position = start
    _end = start.map { $0 + _assumeNonNegative(count) }
  }
}
```

## 总结

* UnsafeMutableBufferPointer
  * `init(start:count:)`
    * 定义
      * `init(start: UnsafeMutablePointer<Element>?, count: Int)`
    * 参数
      * `start: UnsafeMutablePointer<Element>?`
      * `count: Int`
    * 返回值
      * `指针`
