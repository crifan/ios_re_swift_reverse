# VWT的内存布局的举例

## Builtin.NativeObject的VWT

* `Builtin.NativeObject`的`VWT`内存布局
  * [+0x00] = initializeBufferWithCopyOfBuffer
    * `name="swift::metadataimpl::BufferValueWitnesses<swift::metadataimpl::ValueWitnesses<swift::metadataimpl::SwiftRetainableBox>, true, 8ul, 8ul, (swift::metadataimpl::FixedPacking)1>::initializeBufferWithCopyOfBuffer(swift::TargetValueBuffer<swift::InProcess>*, swift::TargetValueBuffer<swift::InProcess>*, swift::TargetMetadata<swift::InProcess> const*)"`
      * `mangled="_ZN5swift12metadataimpl20BufferValueWitnessesINS0_14ValueWitnessesINS0_18SwiftRetainableBoxEEELb1ELm8ELm8ELNS0_12FixedPackingE1EE32initializeBufferWithCopyOfBufferEPNS_17TargetValueBufferINS_9InProcessEEESA_PKNS_14TargetMetadataIS8_EE"`
  * [+0x08] = destroy
    * `name="swift::metadataimpl::ValueWitnesses<swift::metadataimpl::SwiftRetainableBox>::destroy(swift::OpaqueValue*, swift::TargetMetadata<swift::InProcess> const*)"`
      * `mangled="_ZN5swift12metadataimpl14ValueWitnessesINS0_18SwiftRetainableBoxEE7destroyEPNS_11OpaqueValueEPKNS_14TargetMetadataINS_9InProcessEEE"`
  * [+0x10] = initializeWithCopy
    * `name="swift::metadataimpl::ValueWitnesses<swift::metadataimpl::SwiftRetainableBox>::initializeWithCopy(swift::OpaqueValue*, swift::OpaqueValue*, swift::TargetMetadata<swift::InProcess> const*)"`
      * `mangled="_ZN5swift12metadataimpl14ValueWitnessesINS0_18SwiftRetainableBoxEE18initializeWithCopyEPNS_11OpaqueValueES5_PKNS_14TargetMetadataINS_9InProcessEEE"`
  * [+0x18] = assignWithCopy
    * `name="swift::metadataimpl::ValueWitnesses<swift::metadataimpl::SwiftRetainableBox>::assignWithCopy(swift::OpaqueValue*, swift::OpaqueValue*, swift::TargetMetadata<swift::InProcess> const*)"`
      * `mangled="_ZN5swift12metadataimpl14ValueWitnessesINS0_18SwiftRetainableBoxEE14assignWithCopyEPNS_11OpaqueValueES5_PKNS_14TargetMetadataINS_9InProcessEEE"`
  * [+0x20] = initializeWithTake
    * `name="swift::metadataimpl::ValueWitnesses<swift::metadataimpl::SwiftRetainableBox>::initializeWithTake(swift::OpaqueValue*, swift::OpaqueValue*, swift::TargetMetadata<swift::InProcess> const*)"`
      * `mangled="_ZN5swift12metadataimpl14ValueWitnessesINS0_18SwiftRetainableBoxEE18initializeWithTakeEPNS_11OpaqueValueES5_PKNS_14TargetMetadataINS_9InProcessEEE"`
  * [+0x28] = assignWithTake
    * `name="swift::metadataimpl::ValueWitnesses<swift::metadataimpl::SwiftRetainableBox>::assignWithTake(swift::OpaqueValue*, swift::OpaqueValue*, swift::TargetMetadata<swift::InProcess> const*)"`
      * `mangled="_ZN5swift12metadataimpl14ValueWitnessesINS0_18SwiftRetainableBoxEE14assignWithTakeEPNS_11OpaqueValueES5_PKNS_14TargetMetadataINS_9InProcessEEE"`
  * [+0x30] = getEnumTagSinglePayload
    * `name="swift::metadataimpl::FixedSizeBufferValueWitnesses<swift::metadataimpl::ValueWitnesses<swift::metadataimpl::SwiftRetainableBox>, true, 8ul, 8ul, true>::getEnumTagSinglePayload(swift::OpaqueValue const*, unsigned int, swift::TargetMetadata<swift::InProcess> const*)"`
      * `mangled="_ZN5swift12metadataimpl29FixedSizeBufferValueWitnessesINS0_14ValueWitnessesINS0_18SwiftRetainableBoxEEELb1ELm8ELm8ELb1EE23getEnumTagSinglePayloadEPKNS_11OpaqueValueEjPKNS_14TargetMetadataINS_9InProcessEEE"`
  * [+0x38] = storeEnumTagSinglePayload
    * `name="swift::metadataimpl::FixedSizeBufferValueWitnesses<swift::metadataimpl::ValueWitnesses<swift::metadataimpl::SwiftRetainableBox>, true, 8ul, 8ul, true>::storeEnumTagSinglePayload(swift::OpaqueValue*, unsigned int, unsigned int, swift::TargetMetadata<swift::InProcess> const*)"`
      * `mangled="_ZN5swift12metadataimpl29FixedSizeBufferValueWitnessesINS0_14ValueWitnessesINS0_18SwiftRetainableBoxEEELb1ELm8ELm8ELb1EE25storeEnumTagSinglePayloadEPNS_11OpaqueValueEjjPKNS_14TargetMetadataINS_9InProcessEEE"`
  * [+0x40] = int64 size
    * `0x0000000000000008`
  * [+0x48] = int64 stride
    * `0x0000000000000008`
  * [+0x50~0x53] = int32 flags
    * `0x00010007`
  * [+0x54~0x57] = int32 extraInhabitantCount
    * `0x7fffffff`
