# TypeMetadata

* Swift中的`Metadata`
  * **TargetMetadata**
    * **ValueMetadata** ==`TargetValueMetadata`
      * 特点
        * **有VWT**
          * `[-0x8]`是`VWT`=`ValueWitnessTable`
        * 适用于：**Value值** == **非Class** == `Struct`、`Enum`、`Optional`等类型
        * `kind`值的范围：`0 < kind < 0x7FF` 
          * `kind == 0x200` 是 `Struct`
          * `kind == 0x201` 是 `Enum`
          * `kind == 0x202` 是 `Optional`
          * `kind == 0x301` 是 `Tuple`
    * **ClassMetadata** ==`TargetClassMetadata`
      * 特点
        * **没有VWT**
        * 适用于：**Class类**
        * `kind`值的范围：
          * `kind == 0` => （没有继承自`ObjC`的）**纯Swift类**
          * `kind > 0x7FF` => 继承自`ObjC`的`Swift`类 == 此时的值就是`ObjC`中的**isa**
