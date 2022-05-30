---
title: CS:GO中的cfg系统
date: 2020-09-28 17:02:08
tags:
---

## 导语

本人写文章的时候，**CS:GO**游戏时长已经来到了1800小时。这个游戏不仅仅是竞技的快感让我无法自拔，游戏系统与生俱来的“程序员”风格也是让我无法割舍的（毕竟是v社的游戏）。在遇到CS:GO前，我无法想象一个游戏的**控制台系统**能够强大到让我痴迷。离开了控制台，也就离开了这个游戏的特色。那么谈到控制台，就不得不谈谈**cfg**。本文希望能用较为简单易懂的语言来谈谈CS:GO中的cfg系统，希望能给大家一些帮助。

## 知识补充

### 简单设置

**cfg**其实就是设置(config)的缩写。.cfg也是文件的后缀名，我们在游戏根目录里能找到许多这样的文件。这些文件的作用就是储存我们在游戏里的设置参数。打个比方，我的游戏音量是1.0，那么我就能在.cfg的文件里找到这么一行：

```
volume "1.0"
```

前面的volume指的是音量，后面的"1.0"是数值，代表音量的值为1.0。这个例子反映了大部分设置的基本格式，基本都是一个**设置项目名**，后面跟着一个**具体的数值**。一对一的关系还是很易懂的。由此，我们了解了调整设置的基本语法为：

```
[设置项目名] "[具体数值]"
```


双引号在这里不是必须的，但有些时候需要，为了规范我们尽量加上(游戏中控制台不需要)。

我们可以将上面这一行代码称为一个指令。

我们再举几个真实文件中的例子：

```
cl_crosshair_drawoutline 					        "1"
cl_crosshair_dynamic_maxdist_splitratio 	"0.35"
cl_crosshair_dynamic_splitalpha_innermod 	"1"
cl_crosshair_dynamic_splitalpha_outermod 	"0.5"
cl_crosshair_dynamic_splitdist 				    "7"
cl_crosshair_friendly_warning 				    "1"
cl_crosshair_outlinethickness 				    "1.067771"
cl_crosshair_sniper_show_normal_inaccuracy"0"
cl_crosshair_sniper_width 					      "1"
cl_crosshair_t 								            "0"
cl_crosshairalpha 							          "255.000000"
cl_crosshaircolor 							          "5"
cl_crosshaircolor_b	 						          "0.000000"
cl_crosshaircolor_g 						          "255.000000"
cl_crosshaircolor_r 						          "255.000000"
cl_crosshairdot 							            "0.000000"
cl_crosshairgap 							            "0.076290"
cl_crosshairgap_useweaponvalue 				    "0"
cl_crosshairsize 							            "2.917408"
cl_crosshairstyle 							          "4.000000"
cl_crosshairthickness 						        "1.692905"
cl_crosshairusealpha 						          "1"
```

这是控制**游戏准星**相关的代码，由很多行指令组成。crosshair意思是准星，因此我们知道带有crosshair的内容都是和准星有关的，这段代码也是本人在游戏中的所使用的准星设置。我带着大家解读几行：

```
cl_crosshair_drawoutline 					"1"
```

字面意思来看，crosshair_drawoutline显然指的是准星的轮廓，这里后面的数值则应该为1或0，表示有或无轮廓。

```
cl_crosshairalpha 							"255.000000"
```

字面意思来看，crosshairalpha显然指的是准星的透明度，这里后面的数值我设置的是255，表示透明度为100%。

实际上我们可以发现，cfg中的简单设置内容往往都很易懂，用最少的词表达得很清楚，后面的数值也自然容易理解。这一类的设置我们不再多说，后面会详细介绍几个常用的设置内容。

### cfg中指令的类型

上面提到的**\[设置项目名\]+\[一个具体的数值\]**只是指令中的一种，cfg中可不止这一种指令。

下面我们介绍的这些指令是**只有名称，没有值**的。这种指令往往是一个动作，比如：清除血迹、重新投掷上一个投掷物、退出服务器等等：

```
r_cleaardecals;								//清除血迹
sv_rethrow_last_grenade;					//重新投掷上一个投掷物
disconnect;									//断开连接
quit;										//退出游戏(光速白给)
retry;										//重新连接
```

这些指令和上面介绍的设置指令可以说都是具体的指令，下面我们介绍的一些指令类型，则是属于非具体的指令，我们可以将其理解为一些函数，那么我们开始吧。

### bind的作用

cfg中不只是有简单的设置，按键的绑定也是cfg中不可或缺的。按键绑定的基本代码是：

```
bind "[按键]" "[指令]"
```

我列举一些游戏中基础的绑定代码展示一下：

```
bind "w" "+forward"
bind "a" "+moveleft"
bind "s" "+back"
bind "d" "+moveright"
bind "b" "buymenu"
bind "m" "teammenu"
bind "q" "lastinv"
bind "r" "+reload"
bind "SPACE" "+jump"
bind "TAB" "+showscores"
bind "SHIFT" "+speed"
bind "CTRL" "+duck"
bind "MOUSE1" "+attack"
bind "MOUSE2" "+attack2"
```

这些基本是游戏基本按键的绑定了，前四行简单明了，wasd控制前后左右，b键购买菜单(buymenu)，m键选择队伍(teammenu)，q键切回上一个武器(lastinv)，r键换弹，空格(space)跳跃，Tab键看战绩，Shift行走，Ctrl键蹲下，左键(MOUSE1)攻击或者射击，右键(MOUSE2)武器第二操作。这些都是十分易懂的代码行，因此读到这里应该不算困难。

以上所有的代码都是一个按键对应一个指令。那么下面我提出一个问题，如果我想要用一个按键操作两个动作可以实现吗？

答案是肯定的，我们看下面这行：

```
bind "f" "+lookatweapon;r_cleardecals"
```

**f**键本来是检视武器(+lookatweapon)的按钮，这里我们后面加了个分号，又写了一个r_cleardecals，这是什么意思呢？解释一下，后面这个指令是清除血迹和弹孔的指令。这两个指令被双引号括起来，相当于是打了个包，按下f键的时候就会同时执行后面的两个指令。

因此我们将bind指令的用法重新定义一下：

```
bind "[按键]" "[指令或指令集]"
```

指令集里的每个指令之间一定要记得打分号。

又比如我想要在按Shift键的时候同时走路和清除血迹，那么我们应该这样修改代码：

```
bind "SHIFT" "+speed;r_cleardecals"
```

再来一个：

```
bind "CAPSLOCK" "+jump;-attack;-attack2;-jump"
```

这是本人的跳投绑定，这也是最简单的实现跳投的方式。我们可以看到后面的指令集有四个指令。第一个是按下跳跃键，第二第三个是松开攻击键，最后是松开跳跃键，仔细想想是不是很简单呢？

当然，如果你想解绑一个按键，则可以使用unbind指令：

```
unbind "[按键]"
```

比如我想取消上面绑定的跳投键，则可以这么写：

```
unbind "CAPSLOCK"
```

### toggle的作用

你是否碰到这种情况：队友全部牺牲，只剩你一人面对残局，你聚精会神，这时候队友却吵吵闹闹，让你完全没法集中注意力——这时的你是否想要一个按钮，让该死的队友赶紧闭嘴？很幸运，我们有一个指令能够做到这一点。

有的人可能会说，这个不是很简单吗，我直接绑定一个按键（比如"ALT"键）到静音指令上：

```
bind "ALT" "voice_scale 0"
```

这么做固然能达到效果，但是残局打完以后呢？你保持在了静音队友的状态，想要调回去还得去设置里重新设置，较为繁琐。

这里我向大家介绍一个新的指令——**toggle**，指令用法如下：

```
toggle "[设置项目名] [数值1] [数值2] ...[数值n]"
```

这个指令看起来有很多参数，但是用法十分简单。toggle的意思为拨动，这里是switch from的意思。我们大概也能猜到了：toggle就是让指定设置项目的数值在后面的数值1、数值2...中变动。

我们来用toggle实现我们上面的需求：

```
bind "ALT" "toggle voice_scale 0 1"
```

这一行的意思就是，当我按下alt键的时候，voice_scale的值变为0；当我再按一次，值变为1，再按一次，又变为0...这样我们就实现了随时静音和恢复语音的功能。

由我们上面的定义，可知toggle的数值可以不止2个，利用这个我们可以进行一些设置较为细微的调整，比如游戏音量的调整：

```
bind "PGUP" "toggle volume 0 0.1 0.2 0.3 0.5 0.7 0.9"
```

还有准星透明度的调整：

```
bind "PGDN" "toggle cl_crosshairalpha 0 64 128 192 255"
```

这样我们就能够实现一个通过按"Pgdn"键逐渐增加准星透明度的效果。

当然，大部分时候我们不会用到这么细的调整，很多时候设置的数值都只需要在0和1之间切换而已。这时候我们有个简单一些的新指令BindToggle：

```
BindToggle "[按键]" "[指令]"
```

我们举个例子，cl_righthand 1指的是人物右手持枪，cl_righthand 0是左手持枪，我们想要用v键切换左右手：

```
BindToggle "v" "cl_righthand"
```

这时候按下v，cl_righthand的值就会在0和1之间切换了。

注意：这个BindToggle后面的指令只能是一个而不能是指令集，也就是说使用这个指令的话你无法用一个键控制几个设置项的变动。

> ## 练习<sup>1</sup>:
>
> 如果我要把上面这个切换左右手的这个方法用toggle写，该怎么写呢？如果我要用一个键同时控制cl_righthand和volume这两个值呢？

### 加与减的作用

在步入真正高级的内容之前，我们先回顾一下之前讲的知识点，是否有一些没有提及的细节还未涉及呢？

可能有的人早已有了一个疑问：有些指令中的+和-是什么意思？

回到bind那里，我列举了一些指令：

```
bind "w" "+forward"
bind "a" "+moveleft"
bind "s" "+back"
bind "d" "+moveright"
```

这些指令前面的加号，其实是模拟键盘"按下"的这个动作。我们按下w键，一定是在按住的时候人物才会一直向前走，松开以后，这个前进的动作就停止了。

由这个"+"的定义，我们也不难猜测"-"的意思：

```
bind "CAPSLOCK" "+jump;-attack;-attack2;-jump"
```

我们跳投的时候，往往是先拉引信，对准参照物，然后按下跳投键，这里面的"-attack;-attack2"就是模拟松开攻击键的动作；后面的"-jump"则是模拟松开跳跃键的动作。

为什么在这里介绍加与减的概念？因为下一个指令中会涉及加与减的运用，也是十分有意思的内容。

### alias的作用

cfg中还有一种高级的操作，alias，这在普通的cfg中几乎用不到，但是如果你想要拥有一个高端的cfg，这个指令就十分地重要了。

alias基本语法：

```
alias "名称" "指令或指令集"
```

这看上去有点像函数，只不过没有传递参数。它的功能就是打包一个指令集。

没有听明白？我们举个例子来解释就行了：

```
alias "Jumpthrow" "+jump;-attack;-attack2;-jump"
bind "CAPSLOCK" "Jumpthrow"
```

这段代码也是跳投的代码，和上面只用bind指令的效果是一样的，或者说是等价的。这段代码先将跳投的一系列指令放到一起，命名为"Jumpthrow"，然后将"Jumpthrow"绑定在CAPSLOCK键上。

理解了上面的代码，我们来介绍灵性的部分：加与减要来了！

上一栏提到的加与减，是模拟按下与松开的操作。那么alias定义的名称，是不是也能加与减呢？我们看下面的代码：

```
alias "+Jumpthrow" "+jump;-attack;-attack2"
alias "-Jumpthrow" "-jump"
bind "CAPSLOCK" "+Jumpthrow"
```

如果我说这几行代码和之前的跳投是等效的，你能够理解其中加和减的含义吗？

其实，alias的真正内涵在这里有了体现：alias其实就是创造一个新的指令，这个指令和jump、forward、speed等等系统内置的一些指令一样，拥有"按下和松开"的状态。

知道这一点我们再去解读上面的代码，意思便是如果我按下"CAPSLOCK"这个键，那么就进行"+Jumpthrow"里的操作；如果我松开，那么就执行"-Jumpthrow"里的操作。这样是不是就明白了？

那么我们再举一个只能用alias实现的例子来帮助理解：大跳指令(理论上可以有其他方式，但是实战测试只有alias能用，如果有人知道原因欢迎与我交流)

大跳其实是一种按键技巧，由于游戏引擎的特性，在跳跃之前半蹲可以跳得更高。但是也有很多人练习多次依然无法百分百成功。这时候我们就可以通过指令帮助我们达到100%的成功率：

```
alias "+Highjump" "+duck;+jump"
alias "-Highjump" "-duck;-jump"
bind "v" "+Highjump"
```

这个代码也很好理解：按v触发"+Highjump"中的内容：按下蹲和跳；松开v触发"-Highjump"中的内容：松开蹲和跳。(提示：大跳虽然好，但是不建议绑在默认的跳跃键上，因为不需要大跳的时候先蹲再跳会导致跳跃失误，比如从边缘掉下去等等)

alias还有一个常用的功能是将一些难记的代码简化，比如重新投掷上一次的投掷物的指令是sv_rethrow_last_grenade，这个太难记了，那么我简化一下：

```
alias "rethrow" "sv_rethrow_last_grenade"
```

这样一来以后，在控制台输入rethrow即可执行sv_rethrow_last_grenade；或者我们也可以绑定"rethrow"到一个具体的按键上，都是可行的。当然这个简化的例子并不是很好，我们最常用到的简化代码的例子还是在一键买枪中比较多见。

> ## 练习<sup>2</sup>:
>
> 跳投的指令能不能这样写？：
>
> ```
> alias "+Jumpthrow" "+jump"
> alias "-Jumpthrow" "-attack;-attack2;-jump"
> bind "CAPSLOCK" "+Jumpthrow"
> ```
>
> 如果不能，你是否能指出这样操作的问题所在？

- 后续内容待补充

### 总结

显然这不是CS:GO中独有的，但是CS:GO把cfg文件开放给玩家，让玩家可以任意调整任何设置的值，甚至可以在设置里写函数，可以说是把cfg的运用发挥到了极致。这也是我关于这款游戏最喜欢的内容。