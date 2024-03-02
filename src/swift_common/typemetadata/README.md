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
    * **ClassMetadata**
      * ==`TargetClassMetadata`
      * 适用于：**Class类**
    * **ValueMetadata**
      * ==`TargetValueMetadata`
      * 适用于：**非Class** == `Struct`、`Enum`、`Optional`等**Value**
