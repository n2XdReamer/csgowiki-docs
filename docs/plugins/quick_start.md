# 快速开始

欢迎来到CSGOWiki-Pack文档:clap:！本章节会告诉你怎么如何**快速安装插件**与**构建本地开发环境**。

## 基础教程

### 安装

1. 【**安装依赖**】插件的所有依赖见[**兼容性→**](./README.md#兼容性)。
2. 【**下载插件包**】插件最新版本见[**csgowiki-pack.zip**](https://github.com/hx-w/CSGOWiki-Plugins/releases/latest)，国内用户可以在[**Gitee**](https://gitee.com/hx-w/CSGOWiki-Plugins)获取更好的体验。
3. 【**部署插件**】解压文件后以[sourcemod的安装方法](https://wiki.alliedmods.net/Installing_SourceMod)安装插件。
4. 【**cfg设置**】根据实际需求，更改[**csgowiki-pack.cfg**](./config.md)中的相关参数。
5. 【**重启并验证**】重启服务器，在控制台中输入`sm plugins list`，
   如果出现
   ```
   "[CSGO Wiki] Plugin-Pack" (v1.x.x) by CarOL
   ```
   即为安装成功。

---

::: warning 注意

- 【**安装依赖**】拓展与环境必须安装正确，包括但不限于版本正确、路径正确和权限正确等。
- 【**cfg设置**】参数须按实际需求填写，其中`sm_csgowiki_token`为必填，须从[CSGOWiki主站](https://csgowiki.top)获取。`sm_qqchat_sv_port`为消息通道的socket端口，如果你开启了QQ聊天功能，那么服务器中该端口需要开放。
- 【**重启并验证**】在验证插件安装成功后，可以对插件功能进行测试，如果发现不正确的现象请保存**屏幕截图**和**插件运行日志**进行反馈。

:::

### 使用


**指令列表：**

| 指令         | 参数           | 功能                                                         | [权限](https://wiki.alliedmods.net/Adding_Admins_(SourceMod)) | 备注                                                         |
| :----------- | -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `sm_bsteam`  | 字符串         | 绑定steam账号至CSGOWiki                                      | 无                                                           | 由于`.top`域名无法使用steam登录服务，所以使用该指令进行绑定。待新域名备案完成，恢复steam登录功能后可能废弃该指令。 |
| `sm_submit`  | 无             | 开启道具上传状态                                             | 无                                                           | 触发该指令后上传的道具会保存在CSGOWiki主站，可根据获取的道具ID前往主站补全图文信息。 |
| `sm_abort`   | 无             | 中止道具上传状态                                             | 无                                                           | 进入道具上传状态后，在没有投掷道具之前可以中止道具上传。     |
| `sm_wiki`    | 道具ID(可选的) | 呼出社区道具合集菜单，如果输入了道具ID参数，则将玩家传送至对应的投掷点 | 无                                                           | 插件会缓存当前地图的社区道具的索引合集，道具的详细数据请求有次数与频率限制，见[**CSGOWiki等级与权限**](https://www.csgowiki.top/profile/exp/)。 |
| `sm_wikipro` | 无             | 呼出职业道具合集菜单，如果没有选择职业比赛场次，则会先呼出职业比赛场次菜单用于选择 | 无                                                           | 该功能请求的服务器源在国外，当切换职业比赛回合数时会请求服务器数据，次数会有较高延时，其他情况下都是用游戏服务器内的缓存数据。 |
| `sm_option`  | 无             | 呼出个性化设置菜单                                           | 无                                                           | 插件提供多种玩家个性化设置，包括`QQ聊天触发方式`、`社区道具合集是否自动重投`和`上传道具快捷按键`等。 |
| `sm_wikiop`  | 无             | 呼出插件管理员菜单                                           | **n**                                                        | 提供多种功能的开关，实现单个功能的即时开关功能等。           |
| `sm_m`       | 无             | 呼出csgowiki功能整合菜单                                     | 无                                                           | 该菜单为以上功能的整合，方便玩家索引对应的功能。             |


::: tip 更多指令
上面提到的是部分常用指令，更多的功能见[**功能细节**](./menu.md)
:::

### QQ机器人

如果你想使用QQ聊天功能，那么需要使用QQ机器人进行消息传递。

1. 添加CSGOWiki官方QQ机器人：**2823195212**，添加好友的验证消息填写：**8802**，填写正确会自动同意好友申请。
2. 邀请QQ机器人进入目标群，理论上机器人会自动接受邀请请求，如果机器人无反应，请在csgowiki交流群中反馈。
3. 配置并重启游戏服务器后即可与QQ机器人[**互动**](./menu.md#QQ消息转发)，实现QQ群与游戏服务器消息互通。

::: tip 使用自定义机器人

如果你需要使用自己的QQ机器人进行消息转述，那么请关注[**消息中继服务**](../message-channel/README.md)，项目正在开发中。

:::
## 进阶指南

::: tip 阅前须知

下面介绍**参与开发CSGOWiki-Pack**所需要了解的内容，如果你只对如何使用插件感兴趣，可以跳过该部分。

:::

### 目录结构

**CSGOWiki-Pack的目录结构如下（只包含功能文件）：**

```
.
├── archive
├── plugins
│   └── csgowiki-pack.smx
└── scripting
    ├── csgowiki-pack.sp
    ├── csgowiki
    │   ├── menus
    │   │   ├── menu_option.sp
    │   │   ├── menu_wikiop.sp
    │   │   ├── menu_wikipro.sp
    │   │   └── menu_wiki.sp
    │   ├── kicker.sp
    │   ├── panel.sp
    │   ├── qqchat.sp
    │   ├── steam_bind.sp
    │   ├── utility_modify.sp
    │   ├── utility_submit.sp
    │   ├── utility_wiki.sp
    │   └── utils.sp
    └── include
        ├── csgowiki.inc
        └── socket.inc
```

| 文件                              | 备注                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| `/archive/`                       | 老旧废弃的插件，包括[**csgo-practice-mode**](https://github.com/splewis/csgo-practice-mode)的手动汉化版 |
| `/plugins/`                       | 该分支编译的最新插件文件，领先于Release版本                  |
| `/scripting/csgowiki-pack.sp`     | 插件的编译入口文件                                           |
| `/scripting/csgowiki`             | 插件的子功能文件                                             |
| `/scripting/include/csgowiki.inc` | 插件的预编译/全局变量定义头文件                              |
| `/scripting/include/socket.inc`   | 适用于该插件的socket头文件，原`socket.inc`不兼容sourcemod v1.10编译器 |

### 构建与编译

构建该插件的编译环境，除了[**安装插件**](#安装)需要的依赖之外，还需要安装以下库用于编译：

- [**sm-json**](https://github.com/clugg/sm-json)，sourcemod的json方法实现。

---

在`/scripting/`目录下安装[**sourcemode编译器**](https://www.sourcemod.net/downloads.php?branch=stable)

以`Ubuntu 20.04LTS`操作系统为例，按sourcemod规范进行编译：

```shell
chmod +x compile.sh spcomp spcompe64
./compile.sh csgowiki-pack.sp
```

编译结果路径为`/scripting/compiled/csgowiki-pack.smx`。

### 预编译参数

在`/scripting/include/csgowiki.inc`中提供了一些预编译参数，不建议修改。

```cpp
#pragma dynamic 131072
```
开辟131072 cells的内存空间，在sourcemod中1 cell = 4 bytes，即512KB内存。

```cpp
#define PREFIX "\x01[\x05CSGO Wiki\x01]"
```
聊天窗口回显信息的前缀。
