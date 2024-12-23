# iOS逆向：Swift逆向

* [前言](README.md)
* [Swift逆向概览](swift_re_overview/README.md)
  * [Swift逆向脑图](swift_re_overview/mindmap.md)
* [Swift基础知识](swift_basic/README.md)
* [Swift函数](swift_function/README.md)
  * [SwiftObject](swift_function/SwiftObject/README.md)
    * [swift_getInitializedObjCClass](swift_function/SwiftObject/swift_getinitializedobjcclass.md)
  * [UnsafeMutableBufferPointer](swift_function/unsafemutablebufferpointer/README.md)
    * [init(start:count:)](swift_function/unsafemutablebufferpointer/init_start_count.md)
  * [Array](swift_function/array/README.md)
    * [formIndex(after:)](swift_function/array/formindex_after.md)
  * [Set](swift_function/set/README.md)
    * [_NativeSet._unsafeInsertNew](swift_function/set/nativeset_unsafeinsertnew.md)
* [Swift逆向相关](swift_re_related/README.md)
  * [静态分析](swift_re_related/static_analysis/README.md)
    * [导出头文件](swift_re_related/static_analysis/export_header.md)
    * [IDA分析](swift_re_related/static_analysis/ida/README.md)
      * [IDA中Swift相关定义](swift_re_related/static_analysis/ida/swift_definitions/REDME.md)
        * [IDA自带Swift相关定义](swift_re_related/static_analysis/ida/swift_definitions/ida_builtin.md)
        * [Crifan新增Swift相关定义](swift_re_related/static_analysis/ida/swift_definitions/crifan_added.md)
  * [动态调试](swift_re_related/dynamic_debug/README.md)
* [Swift通用逻辑](swift_common/README.md)
  * [TypeMetadata](swift_common/typemetadata/README.md)
    * [ValueMetadata](swift_common/typemetadata/valuemetadata/README.md)
      * [VWT](swift_common/typemetadata/valuemetadata/vwt/README.md)
        * [内存布局](swift_common/typemetadata/valuemetadata/vwt/memory_layout/README.md)
          * [图](swift_common/typemetadata/valuemetadata/vwt/memory_layout/figure.md)
          * [文字](swift_common/typemetadata/valuemetadata/vwt/memory_layout/text.md)
          * [IDA定义](swift_common/typemetadata/valuemetadata/vwt/memory_layout/ida_def.md)
          * [举例](swift_common/typemetadata/valuemetadata/vwt/memory_layout/example.md)
        * [Swift源码](swift_common/typemetadata/valuemetadata/vwt/src.md)
        * [和C++对应关系](swift_common/typemetadata/valuemetadata/vwt/vs_cpp.md)
      * [StructMetadata](swift_common/typemetadata/valuemetadata/structmetadata/README.md)
        * [内存布局](swift_common/typemetadata/valuemetadata/structmetadata/memory_layout/README.md)
          * [图](swift_common/typemetadata/valuemetadata/structmetadata/memory_layout/figure.md)
          * [文字](swift_common/typemetadata/valuemetadata/structmetadata/memory_layout/text.md)
          * [举例](swift_common/typemetadata/valuemetadata/structmetadata/memory_layout/example.md)
      * [EnumMetadata](swift_common/typemetadata/valuemetadata/enummetadata.md)
    * [ClassMetadata](swift_common/typemetadata/classmetadata/README.md)
      * [内存布局](swift_common/typemetadata/classmetadata/memory_layout/README.md)
        * [图](swift_common/typemetadata/classmetadata/memory_layout/figure.md)
        * [文字](swift_common/typemetadata/classmetadata/memory_layout/text.md)
        * [Swift源码](swift_common/typemetadata/classmetadata/memory_layout/src.md)
        * [IDA定义](swift_common/typemetadata/classmetadata/memory_layout/ida_def.md)
        * [举例](swift_common/typemetadata/classmetadata/memory_layout/example.md)
  * [MetadataKind](swift_common/metadatakind.md)
* [常用类型](common_type/README.md)
  * [Array数组](common_type/array/README.md)
    * [内存布局](common_type/array/memory_layout/README.md)
      * [图](common_type/array/memory_layout/figure.md)
      * [文字](common_type/array/memory_layout/text.md)
      * [Swift源码](common_type/array/memory_layout/src.md)
      * [IDA定义](common_type/array/memory_layout/ida_def.md)
      * [举例](common_type/array/memory_layout/example.md)
  * [Bool布尔](common_type/bool/README.md)
  * [Data数据](common_type/struct/README.md)
    * [内存布局](common_type/data/memory_layout/README.md)
      * [图](common_type/data/memory_layout/figure.md)
      * [文字](common_type/data/memory_layout/text.md)
      * [Swift源码](common_type/data/memory_layout/src.md)
      * [IDA定义](common_type/data/memory_layout/ida_def.md)
      * [举例](common_type/data/memory_layout/example.md)
  * [Dictionary字典](common_type/dict/README.md)
    * [内存布局](common_type/dict/memory_layout/README.md)
      * [图](common_type/dict/memory_layout/figure.md)
      * [文字](common_type/dict/memory_layout/text.md)
      * [Swift源码](common_type/dict/memory_layout/src.md)
      * [IDA定义](common_type/dict/memory_layout/ida_def.md)
      * [举例](common_type/dict/memory_layout/example.md)
  * [Enum枚举](common_type/enum/README.md)
  * [Int整型](common_type/int/README.md)
  * [Set集合](common_type/set/README.md)
    * [内存布局](common_type/set/memory_layout/README.md)
      * [图](common_type/set/memory_layout/figure.md)
      * [文字](common_type/set/memory_layout/text.md)
      * [Swift源码](common_type/set/memory_layout/src.md)
      * [IDA定义](common_type/set/memory_layout/ida_def.md)
      * [举例](common_type/set/memory_layout/example.md)
  * [String字符串](common_type/string/README.md)
    * [内存布局](common_type/string/memory_layout/README.md)
      * [图](common_type/string/memory_layout/figure.md)
      * [文字](common_type/string/memory_layout/text.md)
      * [Swift源码](common_type/string/memory_layout/src.md)
      * [IDA定义](common_type/string/memory_layout/ida_def.md)
      * [举例](common_type/string/memory_layout/example.md)
  * [Struct结构体](common_type/struct/README.md)
    * [内存布局](common_type/struct/memory_layout/README.md)
      * [图](common_type/struct/memory_layout/figure.md)
      * [文字](common_type/struct/memory_layout/text.md)
      * [Swift源码](common_type/struct/memory_layout/src.md)
      * [IDA定义](common_type/struct/memory_layout/ida_def.md)
      * [举例](common_type/struct/memory_layout/example.md)
  * [Tuple元祖](common_type/tuple/README.md)
* [附录](appendix/README.md)
  * [参考资料](appendix/reference.md)
