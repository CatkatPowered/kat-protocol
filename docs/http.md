# HTTP部分
> HTTP协议在kat-server中主要用于文件的传输，只是作为websocket协议的辅助
## 过程详解

### IM Provider => kat-server => mosseger
1. kat-server => mosseger发送WebsocketMessagePacket, 其中包含的KatUniMessage包含请求的URL和一次性鉴权token
2. mosseger => kat-server使用GET方式请求HTTP接口, 使用一次性鉴权token拉取资源文件

### mosseger => kat-server => IM Provider
1. mosseger => kat-server使用POST方式请求HTTP统一上传接口, 上传完成后获得一次性鉴权token
2. mosseger => kat-server发送WebsocketMessagePacket, 其中包含的KatUniMessage包含文件的hash以及一次性鉴权token, 此时的KatUniMessage的isDownloadable应为false
