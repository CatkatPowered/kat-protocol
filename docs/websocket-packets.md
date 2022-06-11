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

# 消息数据包

## 新消息发送数据包

```
{
	"type": "websocket_message",
	"message": {
		"message_type": "testExtension",
		"extension_id": "PlainMessage",
		"message_group": "151505562",
		"message_id": "uuid",
		"message_content": "欸嘿~",
		"message_timestamp": 1652881882
	}
}
```

该包作用为**逻辑客户端(如 mosseger)和逻辑服务端消息通讯**
方向由接收方判断，即相同格式进行双向通讯
`message`中格式应严格按照`KatUniMessage`进行序列化和反序列化

## 消息查询数据包

```
{
    "type": "websocket_message_query",
    "extension_id": "testExtension",
    "message_group": "151505562",
    "messages": [],
    "start_timestamp": 1652881882,
    "end_timestamp": 1652881890,
}
```

注意**extension_id此处为相应 IM 桥的标识符，应在连接建立时的服务器描述包中存在，若不存在，应在客户端进行拦截处理**
在服务器返回查询到的纪录时，`messages`中将会被由`KatUniMessage`数组进行序列化的格式进行填充
