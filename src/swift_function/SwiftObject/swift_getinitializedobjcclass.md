# swift_getInitializedObjCClass

## 源码

[swift/stdlib/public/runtime/SwiftObject.mm at main · apple/swift (github.com)](https://github.com/apple/swift/blob/main/stdlib/public/runtime/SwiftObject.mm)

```c
Class swift::swift_getInitializedObjCClass(Class c) {
  // Used when we have class metadata and we want to ensure a class has been
  // initialized by the Objective-C runtime. We need to do this because the
  // class "c" might be valid metadata, but it hasn't been initialized yet.
  // Send a message that's likely not to be overridden to minimize potential
  // side effects. Ignore the return value in case it is overridden to
  // return something different. See
  // https://github.com/apple/swift/issues/52863 for an example.
  [c self];
  return c;
}
```

## 总结

* Swift的`swift_getInitializedObjCClass`函数定义
  * `Class swift::swift_getInitializedObjCClass(Class c)`
    * 参数
      * `Class c`
    * 返回值
      * 类型：Class
