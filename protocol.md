<h1 align="center">Kat Protocol</h1>

# 协议

Kat Protocol 尝试定义一套**通用的 IM 通讯协议**

Kat Protocol 仅仅是一个文字协议，**并非具体的程序实现**

Kat Project 是 Kat Protocol 协议的标准实现

# 通信

协议采用 http 与 websocket 以及它们的 ssl / tls 安全协议 https 与 websockets 作为传输协议

http 信道用于传输媒体，如音乐 视频 语音 文件等内容

websocket 信道用于传输文本消息，通常它们都是符合本协议的 json 文本

# 安全

这里的 http 与 websocket _仅表达没有通过 ssl / tls 加密的协议_

如果**不局限于本地环路**可以访问到服务端，那么应该使用 https 以及 websockets 提高传输数据的安全

否则，使用 http 与 websocket 通过网络传输的数据**容易遭受窃取**
