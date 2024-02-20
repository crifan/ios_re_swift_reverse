# Swift源码

## StringObject.swift

* StringObject.swift
  * [swift/stdlib/public/core/StringObject.swift at main · apple/swift (github.com)](https://github.com/apple/swift/blob/main/stdlib/public/core/StringObject.swift)

```c
extension _StringObject {
  @inlinable @inline(__always)
  internal init(_ small: _SmallString) {
    // Small strings are encoded as _StringObjects in reverse byte order
    // on big-endian platforms. This is to match the discriminator to the
    // spare bits (the most significant nibble) in a pointer.
    let word1 = small.rawBits.0.littleEndian
    let word2 = small.rawBits.1.littleEndian
#if _pointerBitWidth(_64)
    // On 64-bit, we copy the raw bits (to host byte order).
    self.init(rawValue: (word1, word2))
#elseif _pointerBitWidth(_32)
    // On 32-bit, we need to unpack the small string.
    let smallStringDiscriminatorAndCount: UInt64 = 0xFF00_0000_0000_0000


    let leadingFour = Int(truncatingIfNeeded: word1)
    let nextFour = UInt(truncatingIfNeeded: word1 &>> 32)
    let smallDiscriminatorAndCount = word2 & smallStringDiscriminatorAndCount
    let trailingTwo = UInt16(truncatingIfNeeded: word2)
    self.init(
      count: leadingFour,
      variant: .immortal(nextFour),
      discriminator: smallDiscriminatorAndCount,
      flags: trailingTwo)
#else
#error("Unknown platform")
#endif
    _internalInvariant(isSmall)
  }


  @inlinable
  internal static func getSmallCount(fromRaw x: UInt64) -> Int {
#if os(Android) && arch(arm64)
    return Int(truncatingIfNeeded: (x & 0x000F_0000_0000_0000) &>> 48)
#else
    return Int(truncatingIfNeeded: (x & 0x0F00_0000_0000_0000) &>> 56)
#endif
  }


  @inlinable @inline(__always)
  internal var smallCount: Int {
    _internalInvariant(isSmall)
    return _StringObject.getSmallCount(fromRaw: discriminatedObjectRawBits)
  }


  @inlinable
  internal static func getSmallIsASCII(fromRaw x: UInt64) -> Bool {
#if os(Android) && arch(arm64)
    return x & 0x0040_0000_0000_0000 != 0
#else
    return x & 0x4000_0000_0000_0000 != 0
#endif
  }
  @inlinable @inline(__always)
  internal var smallIsASCII: Bool {
    _internalInvariant(isSmall)
    return _StringObject.getSmallIsASCII(fromRaw: discriminatedObjectRawBits)
  }


  @inlinable @inline(__always)
  internal init(empty:()) {
    // Canonical empty pattern: small zero-length string
#if _pointerBitWidth(_64)
    self._countAndFlagsBits = 0
    self._object = Builtin.valueToBridgeObject(Nibbles.emptyString._value)
#elseif _pointerBitWidth(_32)
    self.init(
      count: 0,
      variant: .immortal(0),
      discriminator: Nibbles.emptyString,
      flags: 0)
#else
#error("Unknown platform")
#endif
    _internalInvariant(self.smallCount == 0)
    _invariantCheck()
  }
}
```

## StringBridge.swift

`swift/stdlib/public/core/StringBridge.swift`

```c
extension String {
  @_spi(Foundation)
  public init(_cocoaString: AnyObject) {
    self._guts = _bridgeCocoaString(_cocoaString)
  }
}
```

->

```c
@usableFromInline
@_effects(releasenone) // @opaque
internal func _bridgeCocoaString(_ cocoaString: _CocoaString) -> _StringGuts {
  switch _KnownCocoaString(cocoaString) {
  case .storage:
    return _unsafeUncheckedDowncast(
      cocoaString, to: __StringStorage.self).asString._guts
  case .shared:
    return _unsafeUncheckedDowncast(
      cocoaString, to: __SharedStringStorage.self).asString._guts
#if _pointerBitWidth(_64)
  case .tagged:
    // Foundation should be taking care of tagged pointer strings before they
    // reach here, so the only ones reaching this point should be back deployed,
    // which will never have tagged pointer strings that aren't small, hence
    // the force unwrap here.
    return _StringGuts(_SmallString(taggedCocoa: cocoaString)!)
#if arch(arm64)
  case .constantTagged:
    let taggedContents = getConstantTaggedCocoaContents(cocoaString)!
    return _StringGuts(
      cocoa: taggedContents.untaggedCocoa,
      providesFastUTF8: false, //TODO: if contentsPtr is UTF8 compatible, use it
      isASCII: true,
      length: taggedContents.utf16Length
    )
#endif
#endif
  case .cocoa:
    // "Copy" it into a value to be sure nobody will modify behind
    // our backs. In practice, when value is already immutable, this
    // just does a retain.
    //
    // TODO: Only in certain circumstances should we emit this call:
    //   1) If it's immutable, just retain it.
    //   2) If it's mutable with no associated information, then a copy must
    //      happen; might as well eagerly bridge it in.
    //   3) If it's mutable with associated information, must make the call
    let immutableCopy
      = _stdlib_binary_CFStringCreateCopy(cocoaString)


#if _pointerBitWidth(_64)
    if _isObjCTaggedPointer(immutableCopy) {
      // Copying a tagged pointer can produce a tagged pointer, but only if it's
      // small enough to definitely fit in a _SmallString
      return _StringGuts(
        _SmallString(taggedCocoa: immutableCopy).unsafelyUnwrapped
      )
    }
#endif


    let (fastUTF8, isASCII): (Bool, Bool)
    switch _getCocoaStringPointer(immutableCopy) {
    case .ascii(_): (fastUTF8, isASCII) = (true, true)
    case .utf8(_): (fastUTF8, isASCII) = (true, false)
    default:  (fastUTF8, isASCII) = (false, false)
    }
    let length = _stdlib_binary_CFStringGetLength(immutableCopy)


    return _StringGuts(
      cocoa: immutableCopy,
      providesFastUTF8: fastUTF8,
      isASCII: isASCII,
      length: length)
  }
}
```
