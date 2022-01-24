## Mirai-api-http(V2.0+) for Python

提供基于 websocket client 的接口

> 需要安装websocket-client(pip install websocket-client)

提供基于 http 轮询的接口

> 需要安装requests(pip install requests)

### 友情链接

<div>
  <a href="https://github.com/mamoe/mirai/"><span>github:mirai</span></a></hr>
  <a href="https://github.com/mamoe/mirai-api-http/"><span>github:mirai-api-http</span></a>
</div>

<div>
    <a href="https://jq.qq.com/?_wv=1027&k=huNiAQ3N"><span>交流QQ群:556766602</span></a>
</div>

### 接口一览

+ **[认证与会话](#认证与会话)**
+ **[接收消息与事件](#接收消息与事件)**
  + [数据格式](#数据格式)
+ **[获取插件信息](#获取插件信息)**
+ **[缓存操作](#缓存操作)**
  + [通过messageId获取消息](#通过messageId获取消息)
+ **[获取账号信息](#获取账号信息)**
  + [获取好友列表](#获取好友列表)
  + [获取群列表](#获取群列表)
  + [获取群成员列表](#获取群成员列表)
  + [获取Bot资料](#获取Bot资料)
  + [获取好友资料](#获取好友资料)
  + [获取群成员资料](#获取群成员资料)
+ **[消息发送与撤回](#消息发送与撤回)**
  + [发送好友消息](#发送好友消息)
  + [发送群消息](#发送群消息)
  + [发送临时会话消息](#发送临时会话消息)
  + [发送头像戳一戳消息](#发送头像戳一戳消息)
  + [撤回消息](#撤回消息)
+ **[文件操作](#文件操作)**
  + [查看文件列表](#查看文件列表)
  + [获取文件信息](#获取文件信息)
  + [创建文件夹](#创建文件夹)
  + [删除文件](#删除文件)
  + [移动文件](#移动文件)
  + [重命名文件](#重命名文件)
  + [上传文件](#上传文件)
+ **[消息类型](#消息类型)**
  + [Source](#Source)
  + [回复](#Quote)
  + [At](#At)
  + [AtAll](#AtAll)
  + [表情](#Face)
  + [文本](#Plain)
  + [图片](#Image)
  + [Flash图片](#FlashImage)
  + [语音](#Voice)
  + [Xml](#Xml)
  + [Json](#Json)
  + [App](#App)
  + [戳一戳](#Poke)
  + [骰子](#Dice)
  + [音乐分享](#MusicShare)
  + [转发](#ForwardMessage)
  + [文件](#File)
+ **[账号管理](#账号管理)**
  + [删除好友](#删除好友)
+ **[群管理](#群管理)**
  + [禁言群成员](#禁言群成员)
  + [解除群成员禁言](#解除群成员禁言)
  + [移除群成员](#移除群成员)
  + [退出群聊](#退出群聊)
  + [全体禁言](#全体禁言)
  + [解除全体禁言](#解除全体禁言)
  + [设置群精华消息](#设置群精华消息)
  + [获取群设置](#获取群设置)
  + [修改群设置](#修改群设置)
  + [获取群员资料](#获取群员设置)
  + [修改群员资料](#修改群员设置)
+ **[事件处理](#事件处理)**
  + [添加好友申请](#添加好友申请)
  + [用户入群申请](#用户入群申请)
  + [Bot被邀请入群申请](#Bot被邀请入群申请)

## 认证与会话

### 认证

```python
import [py_file]            // 这边的py_file为2个文件之一
ws=[py_file].ws(
    "ws://xxxx.xxx:xxxx",   // 服务端url
    "JBSAIYDAS7SAD8BIA",    // 服务端配置文件的密码
    1234567890              // 机器人ID
)
ws.connect()                // 连接
```

>注意在连接后将自动订阅所有的消息和事件

### 释放

断开连接

```python
ws.disconnect()
```

## 接收消息与事件

### 数据格式

 采用 json 文本格式进行数据传输
 
 ```json5
{
  "id":0,           // 状态码, 0正常 , 1被允许的错误 , 2无法修正的错误
  "msg":"",         // 对于状态的文本提示
  "data":{}         // 数据主体
}
```

syncId为消息信道，只限于`websock-client`时使用

#### 推送格式

从消息列表中取出列表的第一个事件或消息

```python
ws.get()
```

> 注意此处如果没有消息或事件将一直运行到有消息或事件被取出为止

## 获取插件信息

使用此方法获取插件的信息，如版本号


```python
ws.about()
```

syncId=2

## 缓存操作

### 通过messageId获取消息

此方法通过 `messageId` 获取历史消息, 历史消息的缓存有容量大小, 在配置文件中设置

```python
ws.messageFromId(message_id: int)
```

syncId=3

## 获取账号信息

### 获取好友列表

使用此方法获取bot的好友列表

```python
ws.friendList()
```

syncId=4

### 获取群列表

使用此方法获取bot的群列表

```python
ws.groupList()
```

syncId=5

### 获取群成员列表

使用此方法获取bot指定群中的成员列表

```python
ws.memberList(group_id: int)
```

syncId=6

### 获取Bot资料

此接口获取 session 绑定 bot 的详细资料

```python
ws.botProfile()
```

syncId=7

### 获取好友资料

此接口获取好友的详细资料

```python
ws.friendProfile(user_id: int)
```

syncId=8

### 获取群成员资料

此接口获取群成员的消息资料

```python
ws.memberProfile(group_id: int, user_id: int)
```

syncId=9

## 消息发送与撤回

### 发送好友消息

使用此方法向指定好友发送消息

```python
ws.sendFriendMessage(user_id: int, messageChain: list)
```
> `messageChain`是列表参数, 能添加很多消息类型，详情请阅读[消息类型](#消息类型)

syncId=10

### 发送群消息

```python
ws.sendGroupMessage(group_id: int, messageChain: list)
```

> `messageChain`是列表参数, 能添加很多消息类型，详情请阅读[消息类型](#消息类型)

syncId=11

### 发送临时会话消息

```python
ws.sendTempMessage(user_id: int, group_id: int, messageChain: list)
```

> `messageChain`是列表参数, 能添加很多消息类型，详情请阅读[消息类型](#消息类型)

syncId=12

### 发送头像戳一戳消息

```python
ws.sendNudge(target: int, subject: int, kind: str)
```

>   target:目标id
>   subject:目标子id(例如群号，或者QQ号)
>   kind:"Group" or "Friend" or "Stranger"

syncId=13

### 撤回消息

```python
ws.recall(messageId: int)
```

syncId=14

## 文件操作

### 查看文件列表

```python
ws.file_list(target_id: int, path_id: str = "")
```

> target_id:      群号
> path_id:        地址id , 默认为空 , 即根目录

syncId=15

### 获取文件信息

```python
ws.file_info(target_id: int, path_id: str)
```

> target_id:      群号
> path_id:        地址id

syncId=16

### 创建文件夹

```python
ws.file_mkdir(target_id: int, path_id: str, directoryName: str = "New Folder")
```

> target_id:      群号
> path_id:        地址id
> directoryName:  文件名称(默认为"New Folder")

syncId=17

### 删除文件

```python
ws.file_delete(target_id: int, path_id: str)
```

> target_id:      群号
> path_id:        地址id

syncId=18

### 移动文件

```python
ws.file_move(target_id: int, path_id: str, move_path_id: str)
```

> target_id:      群号
> path_id:        地址id
> move_path_id:   移动的父目录地址id

syncId=19

### 重命名文件

```python
ws.file_rename(target_id: int, path_id: str, name: str)
```

> target_id:      群号
> path_id:        地址id
> name:           名称

syncId=20

### 上传文件

```python
ws.file_upload(self,group_id:int,file_path:str,file_name)
```

> group_id:      群号
> file_path:     服务器目录
> file_name:     文件名称

只适用于`http轮询`接口

## 消息类型

### Source

```json5
{
    "type": "Source",
    "id": 123456,
    "time": 123456
}
```

| 名字 | 类型 | 说明                                                            |
| ---- | ---- | --------------------------------------------------------------- |
| id   | Int  | 消息的识别号，用于引用回复（Source类型永远为chain的第一个元素） |
| time | Int  | 时间戳                                                          |

### Quote

```json5
{
    "type": "Quote",
    "id": 123456,
    "groupId": 123456789,
    "senderId": 987654321,
    "targetId": 9876543210,
    "origin": [
        { "type": "Plain", text: "text" }
    ] 
}
```

| 名字     | 类型   | 说明                                              |
| -------- | ------ | ------------------------------------------------- |
| id       | Int    | 被引用回复的原消息的messageId                     |
| groupId  | Long   | 被引用回复的原消息所接收的群号，当为好友消息时为0 |
| senderId | Long   | 被引用回复的原消息的发送者的QQ号                  |
| targetId | Long   | 被引用回复的原消息的接收者者的QQ号（或群号）      |
| origin   | Object | 被引用回复的原消息的消息链对象                    |


### At

```json5
{
    "type": "At",
    "target": 123456,
    "display": "@Mirai"
}
```

| 名字    | 类型   | 说明                                           |
| ------- | ------ | ---------------------------------------------- |
| target  | Long   | 群员QQ号                                       |
| dispaly | String | At时显示的文字，发送消息时无效，自动使用群名片 |

### AtAll

```json5
{
    "type": "AtAll"
}
```

| 名字 | 类型 | 说明 |
| ---- | ---- | ---- |
| -    | -    | -    |

### Face

```json5
{
    "type": "Face",
    "faceId": 123,
    "name": "bu"
}
```

| 名字   | 类型   | 说明                           |
| ------ | ------ | ------------------------------ |
| faceId | Int    | QQ表情编号，可选，优先高于name |
| name   | String | QQ表情拼音，可选               |

### Plain

```json5
{
    "type": "Plain",
    "text": "Mirai牛逼"
}
```

| 名字 | 类型   | 说明     |
| ---- | ------ | -------- |
| text | String | 文字消息 |

### Image

```json5
{
    "type": "Image",
    "imageId": "{01E9451B-70ED-EAE3-B37C-101F1EEBF5B5}.mirai",  //群图片格式
    //"imageId": "/f8f1ab55-bf8e-4236-b55e-955848d7069f"      //好友图片格式
    "url": "https://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "path": null,
    "base64": null
}
```

| 名字    | 类型   | 说明                                                                                                                 |
| ------- | ------ | -------------------------------------------------------------------------------------------------------------------- |
| imageId | String | 图片的imageId，群图片与好友图片格式不同。不为空时将忽略url属性                                                       |
| url     | String | 图片的URL，发送时可作网络图片的链接；接收时为腾讯图片服务器的链接，可用于图片下载                                    |
| path    | String | 图片的路径，发送本地图片，路径相对于 JVM 工作路径（默认是当前路径，可通过 `-Duser.dir=...`指定），也可传入绝对路径。 |
| base64  | String | 图片的 Base64 编码                                                                                                   |

### FlashImage

```json5
{
    "type": "FlashImage",
    "imageId": "{01E9451B-70ED-EAE3-B37C-101F1EEBF5B5}.mirai",  //群图片格式
    //"imageId": "/f8f1ab55-bf8e-4236-b55e-955848d7069f"      //好友图片格式
    "url": "https://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "path": null,
    "base64": null
}
```

同 `Image`

> 三个参数任选其一，出现多个参数时，按照imageId > url > path > base64的优先级

### Voice

```json5
{
    "type": "Voice",
    "voiceId": "23C477720A37FEB6A9EE4BCCF654014F.amr",
    "url": "https://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "path": null,
    "base64": null,
    "length": 1024,
}
```

| 名字    | 类型   | 说明                                                                                                                 |
| ------- | ------ | -------------------------------------------------------------------------------------------------------------------- |
| voiceId | String | 语音的voiceId，不为空时将忽略url属性                                                                                 |
| url     | String | 语音的URL，发送时可作网络语音的链接；接收时为腾讯语音服务器的链接，可用于语音下载                                    |
| path    | String | 语音的路径，发送本地语音，路径相对于 JVM 工作路径（默认是当前路径，可通过 `-Duser.dir=...`指定），也可传入绝对路径。 |
| base64  | String | 语音的 Base64 编码                                                                                                   |
| length  | Long   | 返回的语音长度, 发送消息时可以不传                                                                                   |

> 三个参数任选其一，出现多个参数时，按照voiceId > url > path > base64的优先级

### Xml

```json5
{
    "type": "Xml",
    "xml": "XML"
}
```

| 名字 | 类型   | 说明    |
| ---- | ------ | ------- |
| xml  | String | XML文本 |

### Json

```json5
{
    "type": "Json",
    "json": "{}"
}
```

| 名字 | 类型   | 说明     |
| ---- | ------ | -------- |
| json | String | Json文本 |

### App

```json5
{
    "type": "App",
    "content": "<>"
}
```

| 名字    | 类型   | 说明 |
| ------- | ------ | ---- |
| content | String | 内容 |

### Poke

```json5
{
    "type": "Poke",
    "name": "SixSixSix"
}
```

| 名字 | 类型   | 说明         |
| ---- | ------ | ------------ |
| name | String | 戳一戳的类型 |

1. "Poke": 戳一戳
2. "ShowLove": 比心
3. "Like": 点赞
4. "Heartbroken": 心碎
5. "SixSixSix": 666
6. "FangDaZhao": 放大招

### Dice

```json5
{
  "type": "Dice",
  "value": 1
}
```

| 名字  | 类型 | 说明 |
| ----- | ---- | ---- |
| value | Int  | 点数 |

### MusicShare

```json5
{
  "type": "MusicShare",
  "kind": "String",
  "title": "String",
  "summary": "String",
  "jumpUrl": "String",
  "pictureUrl": "String",
  "musicUrl": "String",
  "brief": "String"
}
```

| 名字       | 类型   | 说明     |
| ---------- | ------ | -------- |
| kind       | String | 类型     |
| title      | String | 标题     |
| summary    | String | 概括     |
| jumpUrl    | String | 跳转路径 |
| pictureUrl | String | 封面路径 |
| musicUrl   | String | 音源路径 |
| brief      | String | 简介     |

### ForwardMessage

```json5
{
  "type": "Forward",
  "nodeList": [
    {
      "senderId": 123,
      "time": 0,
      "senderName": "sender name",
      "messageChain": [],
      "messageId": 123
    }
  ] 
}
```

| 名字         | 类型   | 说明                                                  |
| ------------ | ------ | ----------------------------------------------------- |
| nodeList     | object | 消息节点                                              |
| senderId     | Long   | 发送人QQ号                                            |
| time         | Int    | 发送时间                                              |
| senderName   | String | 显示名称                                              |
| messageChain | Array  | 消息数组                                              |
| messageId    | Int    | 可以只使用消息messageId，从缓存中读取一条消息作为节点 |

### File

```json5
{
  "type": "File",
  "id": "",
  "name": "",
  "size": 0
}
```

| 名字 | 类型   | 说明       |
| ---- | ------ | ---------- |
| id   | String | 文件识别id |
| name | String | 文件名     |
| size | Long   | 文件大小   |

## 账号管理

### 删除好友

使用此方法删除指定好友

```python
ws.deleteFriend(user_id)
```

syncId=21

## 群管理

### 禁言群成员

使用此方法指定群禁言指定群员（需要有相关权限）

```python
ws.mute(group_id: int, user_id: int, set_time: int = 600)
```

syncId=22

### 解除群成员禁言

使用此方法指定群解除群成员禁言（需要有相关权限）

```python
ws.unmute(group_id: int, user_id: int)
```

syncId=23

### 移除群成员

使用此方法移除指定群成员（需要有相关权限）

```python
ws.kick(group_id: int, user_id: int, msg: str = "您已被移出群聊")
```

> msg:  会话消息(默认为"您已被移出群聊")
syncId=24

### 退出群聊

使用此方法使Bot退出群聊

```python
ws.quit(group_id: int)
```

syncId=25

### 全体禁言

使用此方法令指定群进行全体禁言（需要有相关权限）

```python
ws.muteAll(group_id: int)
```

syncId=26

### 解除全体禁言

使用此方法令指定群解除全体禁言（需要有相关权限）

```python
ws.unmuteAll(group_id: int)
```

syncId=27

### 设置群精华消息

使用此方法添加一条消息为精华消息（需要有相关权限）

```python
ws.setEssence(messageId: int)
```

syncId=28

### 获取群设置

使用此方法获取群设置

```python
ws.groupConfig_get(group_id: int)
```

syncId=29

### 修改群设置

使用此方法修改群设置（需要有相关权限）

```python
ws.groupConfig_update(group_id: int, config: dict)
```

config:

```json5
{
  "name":"",                  // 群名称
  "announcement":"",          // 群公告
  "confessTalk":False,        // 是否开启坦白说
  "allowMemberInvite":False,  // 是否允许群员邀请
  "autoApprove":False,        // 是否开启自动审批入群
  "anonymousChat":False       // 是否允许匿名聊天
}
```
syncId=30

### 获取群员设置

使用此方法获取群员设置

```python
ws.memberInfo_get(group_id: int, user_id: int)
```

syncId=31

### 修改群员设置

使用此方法修改群员设置（需要有相关权限）

```python
ws.memberInfo_update(group_id: int, user_id: int, info: dict)
```

info:

```json5
{
  "name": "群名片",
  "specialTitle": "群头衔"
}
```
syncId=32

### 修改群员管理员

使用此方法修改群员的管理员权限（需要有群主权限）

```python
ws.memberAdmin(group_id: int, user_id: int, isAdmin: bool)
```

syncId=33

## 事件处理

### 添加好友申请

使用此方法处理添加好友申请

```python
ws.resp_newFriendRequestEvent(event_id: int, bot_id: int, group_id: int, operate: int, msg: str)
```

> `msg`返回给对方的消息

> groupId对应申请人的群号，可能为0

| operate | 说明                                               |
| ------- | -------------------------------------------------- |
| 0       | 同意添加好友                                       |
| 1       | 拒绝添加好友                                       |
| 2       | 拒绝添加好友并添加黑名单，不再接收该用户的好友申请 |

syncId=34

### 用户入群申请

使用此方法处理用户入群申请

```python
ws.resp_memberJoinRequestEvent(event_id:int, bot_id: int, group_id: int, operate: int, msg: str)
```

> `msg`返回给对方的消息

| operate | 说明                                           |
| ------- | ---------------------------------------------- |
| 0       | 同意入群                                       |
| 1       | 拒绝入群                                       |
| 2       | 忽略请求                                       |
| 3       | 拒绝入群并添加黑名单，不再接收该用户的入群申请 |
| 4       | 忽略入群并添加黑名单，不再接收该用户的入群申请 |

syncId=35

### Bot被邀请入群申请

使用此方法处理Bot被邀请入群申请

```python
ws.resp_botInvitedJoinGroupRequestEvent(event_id: int, bot_id: int, group_id: int, operate: int, msg: str)
```

> `msg`返回给对方的消息

| operate | 说明     |
| ------- | -------- |
| 0       | 同意邀请 |
| 1       | 拒绝邀请 |

syncId=36
