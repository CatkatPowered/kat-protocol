# 基本数据包

## 基础包

```
{
    "type": "string"
}
```

该 type 应为内置数据包类型或 IM 桥定义的类型之一，否则将返回[ErrorPacket](#错误包)
**一切数据包都应继承于基础包**

## 错误包

```
{
    "type": "error",
    "msg": "string"
}
```

该包为所有错误的统一数据包

# 描述数据包

## 服务器描述包

```
{
    "type": "server_description",
    "server": "server name",
    "version": "alpha0.1",
    "description": "kat server test server 001",
    "im_bridge_description": [
        {
            "im_bridge_name": "telegram",
            // 此处等待插件系统商议完成补全
        }
    ]
}
```

该包应在连接建立后由逻辑服务端作为首个包发送
