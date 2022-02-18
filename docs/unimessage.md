<h1 align="center"> UniMessage </h1>
<div align="center">一个尽可能统一聊天消息结构的数据结构</div>

---

# 序言

各大聊天软件的消息类型有着不同的区别，但是都能抽出大概的类型进行整合和抽象。

抽象出基通用消息类型后，UniMessage 允许定义额外的消息类型，这将极大地拓展 UniMessage 的通用性。

服务端对于消息类型是不敏感的，也就是说服务端核心不会对消息类型进行除了转义，储存以外的额外处理。

# 消息类型

消息类型分为`通用消息类型`和`拓展消息类型`。但是这并不意味着他们是两种不同的消息类型，他们其实只是一系列名称的集合而已

由服务核心内置实现的消息类型叫做`通用消息类型`，由拓展外部注册的消息类型叫做`拓展消息类型`。

## 通用消息类型

经过调研，**通用**的聊天软件的消息类型大概是以下几种

| 消息类型 |     消息代码      |                                     描述                                     |
| :------: | :---------------: | :--------------------------------------------------------------------------: |
| 扁平消息 |   PlainMessage    |                        适用于一般消息，例如单纯的文字                        |
| 聚合消息 | CollectionMessage |                       适用于消息集合，例如 QQ 转发消息                       |
| 图片消息 |   ImageMessage    |                          适用于一张图片即是一条消息                          |
| 文件消息 |    FileMessage    | 适用于一个文件即是一条消息，数据类型包含大视频，大图片，不属于图片的一般文件 |
| 语音消息 |   AudioMessage    |                          适用于一条语音即是一条消息                          |
| 混合消息 |   MixedMessage    |   适用于由多种消息组合成的一条消息，比如一条消息中同时包含语音，图片和文字   |

## 拓展消息类型

除了通用类型之外，还可能存在额外的**拓展消息类型**。

这些消息类型能被注册到**消息类型列表**当中以方便处理。同样的，处理者依然不会是服务端核心部分，服务端核心只会处理文件消息的保存和消息的储存。

暂时拟定这些数据由拓展决定数据结构，储存字段为`extended`。

这些数据由拓展编码成为 JSON，经过 base64 后储存内容到`extended`字段即可完成。

至于具体的操作需要由拓展自行处理，这并不是核心的任务。

# 代码实现

以 Java 为例，我们的数据结构是这样的：

```java
public class KatUniMessage {
    public String messageType = KatMessageTypeConstants.KAT_MESSAGE_TYPE_PLAIN_MESSAGE;
    public String messageID;
    public String messageGroup;
    public ArrayList<String> messageContent;
    public ArrayList<KatUniMessage> messageList;
    public ArrayList<String> extended;
    public String resourceHash;
    public String resourceName;
    public String resourceURL;
}
```

具体的代码是在 [KatUniMessage.java](https://github.com/CatkatPowered/kat-server/blob/main/src/main/java/com/catkatpowered/katserver/message/KatUniMessage.java) 当中

其中 `messageID` 为这条消息的全局唯一值，由 KatServer 分配

## 数据结构样式

![KatUniMessage](https://picture.hanbings.com/2022/02/18/a027415ac216f.png)

## KatUniMessage 对消息类型的假设

有了上述的数据结构，我们可以对消息类型进行大概的假设。

### 扁平消息；图片消息；语音消息；文件消息

以上四种消息有一个共同特点，那就是他们是单一元素消息。

#### 扁平消息

作为扁平消息，我们只需要填充`messageType`，`messageID`，`messageContent`即可

#### 图片消息，语音消息，文件消息

这些消息类型属于含资源的消息，他们需要填充`messageType`，并且附带资源的下载地址

### 混合消息：MixedMessage

WIP
