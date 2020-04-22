# Solidity数组
[TOC]

## 定长数组

## 不定长数组

### 属性方法

- `length` :求数组长度
- `push()` : 可以在数组末尾添加一个零初始化的元素
- `push(x)` : 可以在数组末尾添加给定的元素`x`
- `pop` :从数组末尾删除一个元素

### 数组切片

截取数组`x`从`start`到`end`的一段数据

```
x[start:end]
```

- `end`最大为`x.length -1`
- 两个`start`和`end`是可选的：`start`默认为0与`end`默认为数组的长度。
- 如果`start`大于`end`或`end`大于数组的长度，则引发异常。

数组切片可用于ABI解码函数参数中传递的辅助数据：

```
pragma solidity >=0.6.0 <0.7.0;

contract Proxy {
    /// Address of the client contract managed by proxy i.e., this contract
    address client;

    constructor(address _client) public {
        client = _client;
    }

    /// Forward call to "setOwner(address)" that is implemented by client
    /// after doing basic validation on the address argument.
    function forward(bytes calldata _payload) external {
        bytes4 sig = abi.decode(_payload[:4], (bytes4));
        if (sig == bytes4(keccak256("setOwner(address)"))) {
            address owner = abi.decode(_payload[4:], (address));
            require(owner != address(0), "Address of owner cannot be zero.");
        }
        (bool status,) = client.delegatecall(_payload);
        require(status, "Forwarded call failed.");
    }
}
```