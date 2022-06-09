---
description: 本文使用1.12格式，适用于全版本
---

# execute

### 先导

首先确保你已掌握`execute`。在本章中将会讲解使用`execute`在多个实体之间执行，达到某些想要的效果。

### 辗转相除法

就是不停a %= b, b %= a...直至一方为0。

按照一般的思路，我们需要两条指令实现这个功能，但是如果我们要一条指令实现该功能该如何做呢？

`execute @e[tag=a] ~ ~ ~ scoreboard players operation @e[c=1,tag=b] score %= @e[c=-1,tag=a] score`

这样就能把两条指令压缩到一条指令进行实现了。

### 随机分队

如何在1t中把所有实体进行分队呢？

首先你需要创建计分板并初始化分数

> scoreboard objectives add team dummy
>
> scoreboard players add @a team 0
>
> \#给盔甲架分别赋予它对应的队伍数
>
> execute @a\[score_team=0] \~ \~ \~ execute @a\[score_team=0,c=1] \~ \~ \~ execute @r\[type=armor_stand,tag=teams,c=#队伍数#] \~ \~ \~ scoreboard players operation @r\[score_team=0] team = @a\[tag=teams,c=1,type=armor_stand] team

### 嵌套批量执行

假设世界中有两个marker执行下面指令时会执行几次指令？

`execute @e[tag=marker] ~ ~ ~ execute @e[tag=marker] ~ ~ ~ say 1`

很简单，会执行4次指令。当世界上有三个marker时会执行9次，所以嵌套两个execute会执行marker数量的二次方次(2^2)，以此类推所以下面指令会执行8次(2^3)

`execute @e[tag=marker] ~ ~ ~ execute @e[tag=marker] ~ ~ ~ execute @e[tag=marker] ~ ~ ~ say 1`

总结执行次数为 **实体数^嵌套数** 实体数量这也用于一些简单的次方运算。

