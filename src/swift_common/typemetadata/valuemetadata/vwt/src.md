# VWT的Swift源码

[swift-language/include/swift/ABI/ValueWitness.def at master · eaplatanios/swift-language](https://github.com/eaplatanios/swift-language/blob/master/include/swift/ABI/ValueWitness.def)

```c
///   T *(*initializeBufferWithCopyOfBuffer)(B *dest, B *src, M *self);
/// Given an invalid buffer, initialize it as a copy of the
/// object in the source buffer.
FUNCTION_VALUE_WITNESS(initializeBufferWithCopyOfBuffer,
                       InitializeBufferWithCopyOfBuffer,
                       MUTABLE_VALUE_TYPE,
                       (MUTABLE_BUFFER_TYPE, MUTABLE_BUFFER_TYPE, TYPE_TYPE))

BEGIN_VALUE_WITNESS_RANGE(ValueWitness,
                          InitializeBufferWithCopyOfBuffer)
BEGIN_VALUE_WITNESS_RANGE(RequiredValueWitness,
                          InitializeBufferWithCopyOfBuffer)
BEGIN_VALUE_WITNESS_RANGE(RequiredValueWitnessFunction,
                          InitializeBufferWithCopyOfBuffer)

///   void (*destroy)(T *object, witness_t *self);
///
/// Given a valid object of this type, destroy it, leaving it as an
/// invalid object.  This is useful when generically destroying
/// an object which has been allocated in-line, such as an array,
/// struct, or tuple element.
FUNCTION_VALUE_WITNESS(destroy,
                       Destroy,
                       VOID_TYPE,
                       (MUTABLE_VALUE_TYPE, TYPE_TYPE))

///   T *(*initializeWithCopy)(T *dest, T *src, M *self);
///
/// Given an invalid object of this type, initialize it as a copy of
/// the source object.  Returns the dest object.
FUNCTION_VALUE_WITNESS(initializeWithCopy,
                       InitializeWithCopy,
                       MUTABLE_VALUE_TYPE,
                       (MUTABLE_VALUE_TYPE, MUTABLE_VALUE_TYPE, TYPE_TYPE))

///   T *(*assignWithCopy)(T *dest, T *src, M *self);
///
/// Given a valid object of this type, change it to be a copy of the
/// source object.  Returns the dest object.
FUNCTION_VALUE_WITNESS(assignWithCopy,
                       AssignWithCopy,
                       MUTABLE_VALUE_TYPE,
                       (MUTABLE_VALUE_TYPE, MUTABLE_VALUE_TYPE, TYPE_TYPE))

///   T *(*initializeWithTake)(T *dest, T *src, M *self);
///
/// Given an invalid object of this type, initialize it by taking
/// the value of the source object.  The source object becomes
/// invalid.  Returns the dest object.
FUNCTION_VALUE_WITNESS(initializeWithTake,
                       InitializeWithTake,
                       MUTABLE_VALUE_TYPE,
                       (MUTABLE_VALUE_TYPE, MUTABLE_VALUE_TYPE, TYPE_TYPE))

///   T *(*assignWithTake)(T *dest, T *src, M *self);
///
/// Given a valid object of this type, change it to be a copy of the
/// source object.  The source object becomes invalid.  Returns the
/// dest object.
FUNCTION_VALUE_WITNESS(assignWithTake,
                       AssignWithTake,
                       MUTABLE_VALUE_TYPE,
                       (MUTABLE_VALUE_TYPE, MUTABLE_VALUE_TYPE, TYPE_TYPE))

/// unsigned (*getEnumTagSinglePayload)(const T* enum, UINT_TYPE emptyCases)
/// Given an instance of valid single payload enum with a payload of this
/// witness table's type (e.g Optional<ThisType>) , get the tag of the enum.
FUNCTION_VALUE_WITNESS(getEnumTagSinglePayload,
                       GetEnumTagSinglePayload,
                       UINT_TYPE,
                       (IMMUTABLE_VALUE_TYPE, UINT_TYPE, TYPE_TYPE))

/// void (*storeEnumTagSinglePayload)(T* enum, UINT_TYPE whichCase,
///                                   UINT_TYPE emptyCases)
/// Given uninitialized memory for an instance of a single payload enum with a
/// payload of this witness table's type (e.g Optional<ThisType>), store the
/// tag.
FUNCTION_VALUE_WITNESS(storeEnumTagSinglePayload,
                       StoreEnumTagSinglePayload,
                       VOID_TYPE,
                       (MUTABLE_VALUE_TYPE, UINT_TYPE, UINT_TYPE, TYPE_TYPE))

END_VALUE_WITNESS_RANGE(RequiredValueWitnessFunction,
                        StoreEnumTagSinglePayload)

///   SIZE_TYPE size;
///
/// The required storage size of a single object of this type.
DATA_VALUE_WITNESS(size,
                   Size,
                   SIZE_TYPE)

BEGIN_VALUE_WITNESS_RANGE(TypeLayoutWitness,
                          Size)

BEGIN_VALUE_WITNESS_RANGE(RequiredTypeLayoutWitness,
                          Size)

///   SIZE_TYPE stride;
///
/// The required size per element of an array of this type. It is at least
/// one, even for zero-sized types, like the empty tuple.
DATA_VALUE_WITNESS(stride,
                   Stride,
                   SIZE_TYPE)


///   UINT_TYPE flags;
///
/// The ValueWitnessAlignmentMask bits represent the required
/// alignment of the first byte of an object of this type, expressed
/// as a mask of the low bits that must not be set in the pointer.
/// This representation can be easily converted to the 'alignof'
/// result by merely adding 1, but it is more directly useful for
/// performing dynamic structure layouts, and it grants an
/// additional bit of precision in a compact field without needing
/// to switch to an exponent representation.
///
/// The ValueWitnessIsNonPOD bit is set if the type is not POD.
///
/// The ValueWitnessIsNonInline bit is set if the type cannot be
/// represented in a fixed-size buffer or if it is not bitwise takable.
///
/// The ExtraInhabitantsMask bits represent the number of "extra inhabitants"
/// of the bit representation of the value that do not form valid values of
/// the type.
///
/// The Enum_HasSpareBits bit is set if the type's binary representation
/// has unused bits.
///
/// The HasEnumWitnesses bit is set if the type is an enum type.
DATA_VALUE_WITNESS(flags,
                   Flags,
                   UINT_TYPE)

///   UINT_TYPE extraInhabitantCount;
///
/// The number of extra inhabitants in the type.
DATA_VALUE_WITNESS(extraInhabitantCount,
                   ExtraInhabitantCount,
                   UINT_TYPE)

END_VALUE_WITNESS_RANGE(RequiredTypeLayoutWitness,
                        ExtraInhabitantCount)

END_VALUE_WITNESS_RANGE(RequiredValueWitness,
                        ExtraInhabitantCount)

END_VALUE_WITNESS_RANGE(TypeLayoutWitness,
                        ExtraInhabitantCount)

#endif /* WANT_REQUIRED_VALUE_WITNESSES */

#if WANT_ENUM_VALUE_WITNESSES

// The following value witnesses are conditionally present if the witnessed
// type is an enum.

///   unsigned (*getEnumTag)(T *obj, M *self);
///
/// Given a valid object of this enum type, extracts the tag value indicating
/// which case of the enum is inhabited. Returned values are in the range
/// [0..NumElements-1].
FUNCTION_VALUE_WITNESS(getEnumTag,
                       GetEnumTag,
                       INT_TYPE,
                       (IMMUTABLE_VALUE_TYPE, TYPE_TYPE))

BEGIN_VALUE_WITNESS_RANGE(EnumValueWitness,
                          GetEnumTag)

///   void (*destructiveProjectEnumData)(T *obj, M *self);
/// Given a valid object of this enum type, destructively extracts the
/// associated payload.
FUNCTION_VALUE_WITNESS(destructiveProjectEnumData,
                       DestructiveProjectEnumData,
                       VOID_TYPE,
                       (MUTABLE_VALUE_TYPE, TYPE_TYPE))

///   void (*destructiveInjectEnumTag)(T *obj, unsigned tag, M *self);
/// Given an enum case tag and a valid object of case's payload type,
/// destructively inserts the tag into the payload. The given tag value
/// must be in the range [-ElementsWithPayload..ElementsWithNoPayload-1].
FUNCTION_VALUE_WITNESS(destructiveInjectEnumTag,
                       DestructiveInjectEnumTag,
                       VOID_TYPE,
                       (MUTABLE_VALUE_TYPE, UINT_TYPE, TYPE_TYPE))

END_VALUE_WITNESS_RANGE(EnumValueWitness,
                        DestructiveInjectEnumTag)

END_VALUE_WITNESS_RANGE(ValueWitness,
                        DestructiveInjectEnumTag)
```
