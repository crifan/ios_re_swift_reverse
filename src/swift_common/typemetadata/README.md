# TypeMetadata

* Swift中有几种Metadata
  * 主要有2类
    * Class的：TargetClassMetadata == ClassMetadata
    * 非Class的=（Struct、Enum、Optional等）Value的：TargetValueMetadata = ValueMetadata
  * 通用逻辑
    * TargetClassMetadata和TargetValueMetadata，都继承自：TargetMetadata
      * TargetMetadata
        * 属性=字段
          * Kind

另外一种表述：

* Swift中的`Metadata`
  * **TargetMetadata**
    * `0 < kind < 0x7FF` 是 **ValueMetadata**
      * ==`TargetValueMetadata`
      * 适用于：**非Class** == `Struct`、`Enum`、`Optional`等**Value**
      * 举例
        * `kind == 0x200` 是 `Struct`
        * `kind == 0x201` 是 `Enum`
        * `kind == 0x202` 是 `Optional`
        * `kind == 0x301` 是 `Tuple`
    * `kind == 0` 或 `kind > 0x7FF`(==`ObjC`中的`isa`) 是 **ClassMetadata**
      * ==`TargetClassMetadata`
      * 适用于：**Class类**
