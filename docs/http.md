# HTTP 部分

> HTTP 协议在 kat-server 中主要用于文件的传输，只是作为 websocket 协议的辅助

## 过程详解

### IM Provider => kat-server => mosseger

1. kat-server => mosseger 发送 WebsocketMessagePacket, 其中包含的 KatUniMessage 包含请求的 URL 和一次性鉴权 token
2. mosseger => kat-server 使用 GET 方式请求 HTTP 接口, 使用一次性鉴权 token 拉取资源文件

### mosseger => kat-server => IM Provider

1. mosseger => kat-server 使用 POST 方式请求 HTTP 统一上传接口, 上传完成后获得一次性鉴权 token
2. mosseger => kat-server 发送 WebsocketMessagePacket, 其中包含的 KatUniMessage 包含文件的 hash 以及一次性鉴权 token, 此时的 KatUniMessage 的 isDownloadable 应为 false
