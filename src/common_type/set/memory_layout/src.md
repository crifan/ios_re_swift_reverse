# Swift源码

## Set.swift

`swift/stdlib/public/core/Set.swift`

```c
@frozen
@_eagerMove
public struct Set<Element: Hashable> {
  @usableFromInline
  internal var _variant: _Variant


  /// Creates an empty set with preallocated space for at least the specified
  /// number of elements.
  ///
  /// Use this initializer to avoid intermediate reallocations of a set's
  /// storage buffer when you know how many elements you'll insert into the set
  /// after creation.
  ///
  /// - Parameter minimumCapacity: The minimum number of elements that the
  ///   newly created set should be able to store without reallocating its
  ///   storage buffer.
  public // FIXME(reserveCapacity): Should be inlinable
  init(minimumCapacity: Int) {
    _variant = _Variant(native: _NativeSet(capacity: minimumCapacity))
  }

  /// Private initializer.
  @inlinable
  internal init(_native: __owned _NativeSet<Element>) {
    _variant = _Variant(native: _native)
  }

#if _runtime(_ObjC)
  @inlinable
  internal init(_cocoa: __owned __CocoaSet) {
    _variant = _Variant(cocoa: _cocoa)
  }

  /// Private initializer used for bridging.
  ///
  /// Only use this initializer when both conditions are true:
  ///
  /// * it is statically known that the given `NSSet` is immutable;
  /// * `Element` is bridged verbatim to Objective-C (i.e.,
  ///   is a reference type).
  @inlinable
  public // SPI(Foundation)
  init(_immutableCocoaSet: __owned AnyObject) {
    _internalInvariant(_isBridgedVerbatimToObjectiveC(Element.self),
      "Set can be backed by NSSet _variant only when the member type can be bridged verbatim to Objective-C")
    self.init(_cocoa: __CocoaSet(_immutableCocoaSet))
  }
#endif
}
```

核心定义是：

```c
public struct Set<Element: Hashable> {
  var _variant: _Variant
```


## BridgeStorage.swift

`swift/stdlib/public/core/BridgeStorage.swift`

```c
import SwiftShims

#if !$Embedded

@frozen
@usableFromInline
internal struct _BridgeStorage<NativeClass: AnyObject> {
  @usableFromInline
  internal typealias Native = NativeClass

  @usableFromInline
  internal typealias ObjC = AnyObject

  // rawValue is passed inout to _isUnique.  Although its value
  // is unchanged, it must appear mutable to the optimizer.
  @usableFromInline
  internal var rawValue: Builtin.BridgeObject

  @inlinable
  @inline(__always)
  internal init(native: Native, isFlagged flag: Bool) {
    // Note: Some platforms provide more than one spare bit, but the minimum is
    // a single bit.

    _internalInvariant(_usesNativeSwiftReferenceCounting(NativeClass.self))

    rawValue = _makeNativeBridgeObject(
      native,
      flag ? (1 as UInt) << _objectPointerLowSpareBitShift : 0)
  }

...
```

核心定义：

```c
struct _BridgeStorage<NativeClass: AnyObject> {
  var rawValue: Builtin.BridgeObject
```

```c
protocol BridgeStorage {
  associatedtype Native : AnyObject
  associatedtype ObjC : AnyObject


  init(native: Native, isFlagged: Bool)
  init(native: Native)
  init(objC: ObjC)


  mutating func isUniquelyReferencedNative() -> Bool
  mutating func isUniquelyReferencedUnflaggedNative() -> Bool
  var isNative: Bool {get}
  var isObjC: Bool {get}
  var nativeInstance: Native {get}
  var unflaggedNativeInstance: Native {get}
  var objCInstance: ObjC {get}
}

extension _BridgeStorage : BridgeStorage {}
```

## SetStorage.swift

`swift/stdlib/public/core/SetStorage.swift`

```c
/// An instance of this class has all `Set` data tail-allocated.
/// Enough bytes are allocated to hold the bitmap for marking valid entries,
/// keys, and values. The data layout starts with the bitmap, followed by the
/// keys, followed by the values.
// NOTE: older runtimes called this class _RawSetStorage. The two
// must coexist without a conflicting ObjC class name, so it was
// renamed. The old name must not be used in the new runtime.
@_fixed_layout
@usableFromInline
@_objc_non_lazy_realization
internal class __RawSetStorage: __SwiftNativeNSSet {
  // NOTE: The precise layout of this type is relied on in the runtime to
  // provide a statically allocated empty singleton.  See
  // stdlib/public/stubs/GlobalObjects.cpp for details.

  /// The current number of occupied entries in this set.
  @usableFromInline
  @nonobjc
  internal final var _count: Int

  /// The maximum number of elements that can be inserted into this set without
  /// exceeding the hash table's maximum load factor.
  @usableFromInline
  @nonobjc
  internal final var _capacity: Int

  /// The scale of this set. The number of buckets is 2 raised to the
  /// power of `scale`.
  @usableFromInline
  @nonobjc
  internal final var _scale: Int8

  /// The scale corresponding to the highest `reserveCapacity(_:)` call so far,
  /// or 0 if there were none. This may be used later to allow removals to
  /// resize storage.
  ///
  /// FIXME: <rdar://problem/18114559> Shrink storage on deletion
  @usableFromInline
  @nonobjc
  internal final var _reservedScale: Int8

  // Currently unused, set to zero.
  @nonobjc
  internal final var _extra: Int16

  /// A mutation count, enabling stricter index validation.
  @usableFromInline
  @nonobjc
  internal final var _age: Int32

  /// The hash seed used to hash elements in this set instance.
  @usableFromInline
  internal final var _seed: Int

  /// A raw pointer to the start of the tail-allocated hash buffer holding set
  /// members.
  @usableFromInline
  @nonobjc
  internal final var _rawElements: UnsafeMutableRawPointer

  // This type is made with allocWithTailElems, so no init is ever called.
  // But we still need to have an init to satisfy the compiler.
  @nonobjc
  internal init(_doNotCallMe: ()) {
    _internalInvariantFailure("This class cannot be directly initialized")
  }

  @inlinable
  @nonobjc
  internal final var _bucketCount: Int {
    @inline(__always) get { return 1 &<< _scale }
  }

  @inlinable
  @nonobjc
  internal final var _metadata: UnsafeMutablePointer<_HashTable.Word> {
    @inline(__always) get {
      let address = Builtin.projectTailElems(self, _HashTable.Word.self)
      return UnsafeMutablePointer(address)
    }
  }

  // The _HashTable struct contains pointers into tail-allocated storage, so
  // this is unsafe and needs `_fixLifetime` calls in the caller.
  @inlinable
  @nonobjc
  internal final var _hashTable: _HashTable {
    @inline(__always) get {
      return _HashTable(words: _metadata, bucketCount: _bucketCount)
    }
  }
}
```

和

```c
extension __RawSetStorage {
  /// The empty singleton that is used for every single Set that is created
  /// without any elements. The contents of the storage must never be mutated.
  @inlinable
  @nonobjc
  internal static var empty: __EmptySetSingleton {
    return Builtin.bridgeFromRawPointer(
      Builtin.addressof(&_swiftEmptySetSingleton))
  }
}
```

## Runtime.swift

```c
@_fixed_layout
@usableFromInline
@objc @_swift_native_objc_runtime_base(__SwiftNativeNSSetBase)
internal class __SwiftNativeNSSet {
  @nonobjc
  internal init() {}
  @objc public init(coder: AnyObject) {}
  deinit {}
}
```
