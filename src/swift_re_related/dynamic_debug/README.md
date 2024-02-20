# Swift逆向之动态调试

调试时，辅助用IDA打开Swift的各种库文件，比如：

* `libswiftCore.dylib`
* `libswiftFoundation.dylib`
* 等等

方便调试内容。

这样，通过动态调试和查看IDA中的分析，函数的伪代码实现等等，就可以有更深入的理解很多函数和变量了。


---


TODO：把下面帖子中有用的内容，整理过来：

* 【未解决】iOS逆向Swift：swift_getAssociatedTypeWitness
* 【未解决】iOS逆向Swift：Data.withUnsafeMutableBytes<A>(_:)
* 【已解决】iOS逆向Swift：Data.withUnsafeBytes<A>(_:)
* 【未解决】iOS逆向WhatsApp：swiftPodCopy_10039A1B4
* 【未解决】iOS逆向Swift：Set.init<A>(_:)
* 
* 【记录】iOS逆向Swift：IDA静态分析libswiftFoundation.dylib
  * 。。。
* 
* 【已解决】iOS逆向Swift：库文件libswiftCore.dylib
  * 【已解决】iOS逆向WhatsApp：ArrayAdoptStorage_2ToW1_CB2C
    * static Swift.Array._adoptStorage(_: __owned Swift._ContiguousArrayStorage<τ_0_0>, count: Swift.Int) -> (Swift.Array<τ_0_0>, Swift.UnsafeMutablePointer<τ_0_0>)
  * 【已解决】iOS逆向：dyld_stub_binder
  * 【已解决】iOS逆向Swift：String的WitnessTable详情
    * destroy value witness for Swift.String
  * 【已解决】iOS逆向Swift：Float对应的Builtin.Int32的VWT的具体值
    * type metadata for Swift.Float
  * 【未解决】iOS逆向Swift：_NativeSet._unsafeInsertNew
    * Swift._NativeSet._unsafeInsertNew(_: __owned τ_0_0, at: Swift._HashTable.Bucket) -> ()
  * 【已解决】iOS逆向Swift：Optional的VWT=ValueWitnessTable
  * 【未解决】iOS逆向Swift：_SetStorage.allocate
    * static Swift._SetStorage.allocate(capacity: Swift.Int) -> Swift._SetStorage<τ_0_0>
  * 【已解决】iOS逆向Swift：Xcode中给Swift函数_NativeSet.init加断点
    * Swift._NativeSet.init(capacity: Swift.Int) -> Swift._NativeSet<τ_0_0>
  * 【未解决】iOS逆向Swift：NativeSet
  * Collection
    * 【未解决】iOS逆向WhatsApp：Collection.map<A>(_:)(void)
    * 【未解决】iOS逆向Swift：swift_getAssociatedTypeWitness
      * Swift.Collection.isEmpty.getter : Swift.Bool
    * 【基本解决】iOS逆向Swift：protocol requirements base descriptor的含义
      * protocol requirements base descriptor for Swift.Collection
  * 【已解决】iOS逆向：Swift的pod_copy
    * pod_copy(swift::OpaqueValue*, swift::OpaqueValue*, swift::TargetMetadata<swift::InProcess> const*)
  * 【已解决】iOS逆向：Swift._ContiguousArrayStorage
    * type metadata accessor for Swift._ContiguousArrayStorage
  * 【已解决】iOS逆向：swift_getTypeByMangledNameInContext
  * 【未解决】iOS逆向：__swift_instantiateConcreteTypeFromMangledName
  * 【已解决】iOS逆向WhatsApp：Swift函数String.append(_:)的字符串拼接的实现逻辑
    * Swift.String.append(Swift.String) -> ()

---

