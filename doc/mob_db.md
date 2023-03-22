# 怪物数据库(mob_db)字段说明

## ID
怪物编号。

## SpriteName
怪物的外观名称 (.act 和 .spc 文件)。

## Name
怪物的名字，当你使用`--en--`时会用到。

## JapaneseName
怪物的日文名字，当你使用`--ja--`时会用到。

## Level
怪物的等级。

## Hp
怪物的生命值。

## Sp
怪物的魔法值。

## BaseExp
怪物的基础经验值。

## JobExp
怪物的职业经验值。

## MvpExp
打败怪物时给予获得MVP奖励的玩家的MVP经验值。 这个经验是怪物给的经验的百分比。

## Attack
* 复兴前：怪物的最小攻击力。
* 复兴后：怪物的基础攻击力。

## Attack2
* 复兴前：怪物的最大攻击力。如果未定义，怪物的攻击力固定为 Attack 指定的值，否则攻击力会在 Attack 和 Attack2 之间随机。
* 复兴后：怪物的基础魔法攻击力。

## Defense
怪物的物理防御，减少近战和远程物理攻击/技能伤害。

## MagicDefense
怪物的魔法防御，减少魔法技能伤害。

## Resistance
怪物的物理抵抗，减少近战和远程物理攻击/技能伤害。

## MagicResistance
怪物的魔法抵抗，减少魔法技能伤害。

## Str
怪物的力量，影响物理攻击力(ATK)。

## Agi
怪物的敏捷，影响闪避率(FLEE)。

## Vit
怪物的体力，增加额外的物理防御(DEF)。

## Int
怪物的智力，增加额外的魔法攻击力(MATK)。

## Dex
怪物的灵巧，影响命中率(HIT rate)。

## Luk
怪物的幸运，影响完美回避/幸运闪避/完美闪避/幸运回避率

## AttackRange
攻击范围。 
* 如果设置为1或2，则为近战攻击。 
* 如果设置为3以上，则为远程攻击。

## SkillRange
最大技能攻击范围。

## ChaseRange

## Size

## Race

## RaceGroups

## Element

## ElementLevel

## WalkSpeed

## AttackDelay

## AttackMotion

## DamageMotion

## DamageTaken

## Ai

## Class

## Modes

## MvpDrops

## Drops
