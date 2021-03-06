# Solidity数据类型

## 值类型

以下类型也称为值类型，因为这些类型的变量将始终按值来传递。 也就是说，当这些变量被用作函数参数或者用在赋值语句中时，总会进行值拷贝

```
- 布尔类型
- 整型
- 定长浮点型
- 地址类型
- 合约类型
- 定长字节数组
- 变长字节数组
- 有理数和整数字面常数
- 字符串字面常数
- 十六进制字面常数
- 枚举类型
- 函数类型
```

## 引用类型

```
- 数组(Array)
- 结构体(Struct)
- 映射(mapping)
```

如果使用引用类型，则始终必须明确提供存储该类型的数据区域：

- `memory` :  其生命周期仅限于外部函数调用
- `storage` : 状态变量的存储位置，生命周期限于或合同有效期
- `calldata` : 包含函数参数的特殊数据位置，仅可用于外部函数调用参数

## 映射(mapping)

- 映射类型在声明时的形式为 
```
mapping(_KeyType => _ValueType)
```
其中 `_KeyType` 可以是除了映射、变长数组、合约、枚举以及结构体以外的几乎所有类型。 `_ValueType` 可以是包括映射类型在内的任何类型。
- 映射可以视作 `哈希表`
- 映射是没有长度的，也没有 key 的集合或 value 的集合的概念。
- 只有状态变量（或者在 internal 函数中的对于存储变量的引用）可以使用映射类型。

- 可迭代映射

您无法遍历映射，即无法枚举其键。但是，可以在它们之上实现数据结构并对其进行迭代。例如

```
struct itmap {
    mapping(uint => IndexValue) data;
    KeyFlag[] keys;
    uint size;
}
```

## 注意点

- 在Solidity中不存在“未定义”或“空”值的概念，但是新声明的变量始终具有取决于其类型的默认值。要处理任何意外的值，您应该使用`revert`函数还原整个事务，或者返回带有第二个bool值表示成功的元组。
- solidity的`if ()`判断条件只能是`bool`类型，即`if (1) { ...} `是不正确的


