---
title: 搭建 CS:GO 服务器 (On Windows)
date: 2021-01-01 21:17:08
tags:
---

## 步骤

[SteamCMD](https://steamcdn-a.akamaihd.net/client/installer/steamcmd.zip)

下载`SteamCMD`，**打开**`steamcmd.exe`，启动命令行。

第一次启动需要等待更新。

等到可以输入时，**输入**：

```
login anonymous
app_update 740 validate
```

（复制代码后，在命令窗口右键可以直接粘贴）

![命令行_1](https://i.imgur.com/J6pjONd.png)

等待服务器下载（约29G）

---

等待过程中先去做一些准备工作


**下载**两个mod：

[Metamod](https://mms.alliedmods.net/mmsdrop/1.11/mmsource-1.11.0-git1145-windows.zip)

[SourceMod](https://sm.alliedmods.net/smdrop/1.10/sourcemod-1.10.0-git6528-windows.zip)

分别**解压**待用

---

顺便**检查**一下电脑里是否有命令行工具（建议使用 Windows Terminal）

[Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab)

安装好命令行工具后，在`Windows徽标`右键，可以看到`Windows终端(管理员)`

**输入如下代码**（用来打开电脑的端口）：

```shell=
netsh advfirewall firewall add rule name="CSGO 27015 TCP" dir=in action=allow protocol=TCP localport=27015
netsh advfirewall firewall add rule name="CSGO 27015 UDP" dir=in action=allow protocol=UDP localport=27015
netsh advfirewall firewall add rule name="CSGO 27020 UDP" dir=in action=allow protocol=UDP localport=27020
netsh advfirewall firewall add rule name="CSGO 27015 TCP-o" dir=out action=allow protocol=TCP localport=27015
netsh advfirewall firewall add rule name="CSGO 27015 UDP-o" dir=out action=allow protocol=UDP localport=27015
netsh advfirewall firewall add rule name="CSGO 27020 UDP-o" dir=out action=allow protocol=UDP localport=27020
```

---

尝试启动游戏

在 `SteamCMD` 的目录下，右键**打开命令行工具**：

```shell=
cd '.\steamapps\common\Counter-Strike Global Offensive Beta - Dedicated Server\'

.\srcds.exe -game csgo -console +map de_dust2
```

![命令行_2](https://i.imgur.com/xsnDsVz.png)

初步启动完成，可以暂时关闭窗口

---

在 `csgo/cfg` 文件夹里**创建** `server.cfg`，文件内容复制粘贴如下链接里的内容即可：

[server.cfg](https://hackmd.io/@1099255210/rk7UMX5Vq)

---

接下来**安装**`Metamod`和`Sourcemod`

将 `mmsource-1.11.0-git1145-windows` 里的文件夹 `addons` 拖到`csgo` 文件夹中

![](https://i.imgur.com/5JdYipL.png)

将 `sourcemod-1.10.0-git6528-windows` 里的文件夹 `addons` 和 `cfg` 拖到 `csgo` 文件夹中

![](https://i.imgur.com/gXHPlEr.png)

安装完成

---

接下来**配置服务器管理员**

打开 `\csgo\addons\sourcemod\configs\admins_simple.ini`

[获取SteamID](https://steamid.io/)

搜索到自己，第一个 steamID 就是我们需要的

在文件末尾**加一行**：

```
"STEAM_0:0:......" "99:z"
```

前面替换成你的 steamID

---

启动测试，还是来到 `\Counter-Strike Global Offensive Beta - Dedicated Server\` 文件夹，**打开命令行**：

```
.\srcds -game csgo -console -usercon +game_type 1 +game_mode 2 +mapgroup mg_allclassic +map de_dust
```

这时候服务器应该已经正常启动了

我们**重新打开一个命令行**，输入 `ipconfig`，在结果里找到：

```
# 这是我自己电脑的信息
无线局域网适配器 WLAN:

   连接特定的 DNS 后缀 . . . . . . . :
   本地链接 IPv6 地址. . . . . . . . : fe80::7d80:a5ec:6d19:69a7%18
   IPv4 地址 . . . . . . . . . . . . : 192.168.31.196
   子网掩码  . . . . . . . . . . . . : 255.255.255.0
   默认网关. . . . . . . . . . . . . : 192.168.31.1
```

记住其中的 `IPv4地址`

游玩游戏的电脑需要和服务器连在同一个wifi内

在游戏端**输入** `connect [IPv4地址:27015]` 应该能正常进入服务器

在聊天框内**输入** `!admin` 如果左边弹出选项，说明管理员认证成功

---

## 安装插件

**安装**mysql

[mysql](https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-8.0.28.0.msi)

![](https://i.imgur.com/3jjhOrh.png)

这个页面选择下面一个

其他按照默认选项安装即可，有错误直接跳过

密码设置自己记得住的，后面要使用

**添加**系统环境变量

设置 > 系统 > 设备规格 > 高级系统设置 > 环境变量 > 用户变量 > Path > 编辑 > 新建

粘贴 `C:\Program Files\MySQL\MySQL Server 8.0\bin`

确定即可

重新打开终端，**输入** `mysql -uroot -p` 以及密码，测试是否设置成功。

若可以正常使用 `mysql` 命令行，**执行**如下命令：

```mysql
CREATE DATABASE `sticker`;
```

---

**打开** `\csgo\addons\sourcemod\configs\databases.cfg`

在 `Databases` 下一级**添加**：

```
"csgo_weaponstickers"
{
    "driver"    "mysql"
    "host"      "localhost"
    "database"  "sticker"
    "user"      "root"
    "pass"      ""
}
```

![](https://i.imgur.com/hC4FIhC.png)




## 附加内容

### 开放到公网

首先到 [Steam服务器管理页面](https://steamcommunity.com/dev/managegameservers) **申请一个令牌**：

![令牌_1](https://i.imgur.com/uPAi8Om.png)

App ID 必须填写730，备忘录无所谓

![令牌_2](https://i.imgur.com/1XOFGKW.png)

申请得到的登录令牌复制**保存**好，不要泄露

在 `server.cfg` 中**加入一行**：

```
sv_setsteamaccount "你的令牌"
```

**修改**文件中的 `sv_lan 1` 改为 `sv_lan 0`

重启服务器即可

---

### 启用远程控制台

在 `server.cfg` 中找到 `rcon_password ""` 添加你的密码

然后在游戏中连接到服务器，在控制台输入 `rcon_password "你的密码"`

之后你就可以在你需要使用的指令前加 `rcon ` 来执行

---