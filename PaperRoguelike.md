# 纸片地下城

Roguelike类

## 概念：

* 神力：玩家通过鼠标或者触屏点击，对单位、物品进行神力渗透，或者进行神力标记。

* 神力标记：选定一个单位后右键地板一个地方，进行神力标记。（移动单位用）

* 神力等级：体现为数值等级，通过勇者祭献物品/怪物获得经验 或者 进行新一轮地下城轮回时结算获得。每一物品/单位都有其不变的神力等级。

* 神力渗透：对一个单位或者物品进行神力渗透，可修改其内在属性。

* 神力使用规则：1、神力渗透只能作用于等级小于等于玩家神力等级的单位/物品。2、神力等级、神力碎片、神力规则 对同一地下城种子可继承，不随勇者死亡而消失。

* 神力碎片：可填充在神力规则中的言语片段。

> 例：
>
> 属性描述类： 物理的、魔法的、火属性的、风属性的、雷属性的...
>
> 攻击特效类：两连击、三连击、范围性、三名单位、远程、近战
>
> 数值类：1、2、3 ... n（n小于等于神力等级，神力等级提升时自动解锁）、等同于攻击力的值、等同于力量的值、使用者的力量值...
>
> 名称类：勇者、哥布林、史莱姆...
>
> 类别类：敌对生物、人类、魔物、最近的生物...
>
> 装备类：主武器、副武器、头盔、铠甲...
>
> 物品类：生命药水、魔法药水...
>
> 布尔条件类：略
>
> （真名描述：）

* 神力规则：描述在物品/单位的内在属性面板的语句，只有与神力碎片组合才能发挥出功能。
> 例：
>
> 描述类（描述类规则单项唯一）：最大HP：[数值]、攻击力：[数值]、力量：[数值]、本名：[名称]、类型：[类别]、（真名：[真名描述]）、可装备[装备]
>
> 附加值类：提高[数值]点 攻击力、习得技能：治疗术—消耗10MP，治疗[数值]点HP
>
> 攻击类：每[数值]秒用[装备]攻击一次、每秒对[名称]造成[数值]点近战伤害、每[数值]秒用[装备]攻击一次、每秒对[类别]造成[数值]点近战伤害
>
> 移动类：自动寻敌[数值]m范围内的[单位]、跟随[单位]、优先前往神力标记处
>
> 魔法类：当[布尔条件]时，对自己释放[技能]。
>
> 功能类：自动标记攻击自己的单位为“敌对生物”、当[布尔条件]时，对自己使用[物品]。

* 祭献：对怪物尸体或者物品使用，让其消失，获取其中包含的一项神力规则、神力碎片

## 简单描述下游戏流程：

1、输入一个地下城种子，随机根据种子生成地下城（demo可直接默认一个地下城）

2、初始神力等级为1 \ 神力碎片有：数值类：1 \ 无神力规则

3、勇者神力等级为1。

> 没加中括号的为常数固定值。

~~~txt
[属性面板]
本名：[名称：勇者]
类型：[类别：人类]
HP：10
攻击力：3
自动标记[攻击自己]的单位为“敌对生物”
每秒对[敌对生物]造成[攻击力]点近战伤害
优先前往神力标记处
规则槽位----解锁所需神力等级2
规则槽位----解锁所需神力等级3
规则槽位----解锁所需神力等级5
......（质数排列，仅展示2个待解锁的槽位，其他隐藏）
~~~

4、然后第一个房间有一个神力等级为1的哥布林

~~~txt
[属性面板]
本名：[名称：哥布林]
类型：[类别：魔物]
最大HP：[数值：10] + 5
攻击力：[数值：1]
力量：[数值：3]
可装备：[装备：主武器]
自动寻敌[数值：5]m范围内的[名称：勇者]
每[数值：3]秒用[装备：主武器]攻击一次
规则槽位----解锁所需神力等级5
规则槽位----解锁所需神力等级8
......
~~~

~~~txt
[装备面板]
主武器：狼牙棒

[狼牙棒面板]
本名：狼牙棒
类型：主武器
攻击力：1 + [使用者的力量值] （+1）
提高攻击力1点
~~~

5、由于哥布林的神力等级为1，小于等于玩家的神力等级，所以可直接通过神力渗透，渗透它，将他的最大HP：[数值：10] + 5修改为最大HP：[数值：1] + 5，即6点HP，然后勇者两下带走它。

6、哥布林爆出装备狼牙棒，祭献它尸体，可随机获得1～3个神力规则，
