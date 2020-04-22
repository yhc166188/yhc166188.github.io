# Solidity中的delete操作

Solidity中有个特殊的操作符`delete`用于释放空间，因为区块链做为一种公用资源，为避免大家滥用。且鼓励主动对空间的回收，释放空间将会返还一些`gas`

`delete`关键字的作用是对某个类型值a赋予初始值。比如如果删除整数`delete a`等同于`a = 0`



## 删除数组(array)

- 对于定长数组，删除时，是将数组内所有元素置为初值。
- 而对于变长数组时，则是将长度置为0。
- 删除数组的一个元素

我们也可以删除数组的一个元素，有一点违反直觉的是，删除一个元素后，数组会留个空隙在那里。比如三个元素的数组，删除了第二个元素，只是将第二个元素置为了初始值，其它没变

## 对于映射(mapping)

- `delete`对映射没有影响（因为映射的键可能是任意的，并且通常是未知的）
- 但是，可以删除单个键及其映射的对象：如果`a`是映射，则将删除存储在的值。`delete a[x]x`

## 对于结构(struct)

如果删除一个结构，它将重置所有非映射成员，并且递归到这些成员中，除非它们是映射

```
pragma solidity >=0.4.0 <0.7.0;

contract DeleteExample {
    uint data;
    uint[] dataArray;

    function f() public {
        uint x = data;
        delete x; // sets x to 0, does not affect data
        delete data; // sets data to 0, does not affect x
        uint[] storage y = dataArray;
        delete dataArray; // this sets dataArray.length to zero, but as uint[] is a complex object, also
        // y is affected which is an alias to the storage object
        // On the other hand: "delete y" is not valid, as assignments to local variables
        // referencing storage objects can only be made from existing storage objects.
        assert(y.length == 0);
    }
}
```

## 参考文章

- [Solidity的delete操作符深入详解(十三)|入门系列](https://me.tryblockchain.org/solidity-delete.html)
- [Operators Involving LValues](https://solidity.readthedocs.io/en/v0.6.6/types.html#operators-involving-lvalues)

**注意**: `delete`更像是对一个对象重置初始值，比如删除一个引用变量，它将仅重置自身，而不重置其先前引用的值