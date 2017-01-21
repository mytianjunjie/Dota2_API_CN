# Dota2 Web API 中文文档

## 一、准备工作相关资料

1. [Valve](https://steamcommunity.com/login/home/?goto=%2Fdev%2Fapikey) 申请一个开发者的 KEY
2. [THINGS YOU SHOULD KNOW BEFORE STARTING](http://dev.dota2.com/showthread.php?t=58317) Dota2 开发者论坛的精华帖

## 二、API 列表

### GetMatchDetails()

#### 接口功能
> 获取单场比赛的详细数据

#### URL
> GET http://api.steampowered.com/IDOTA2Match_<ID>/GetMatchDetails/v1

#### 参数说明
|参数|必选|类型|说明|
|:----- |:-------|:-----|----- |
|match_id |ture |string|比赛的ID |
|key |true |string |申请的API KEY|
|language |false |string |语言，默认为英语|
|format |false |string |格式，默认为 json|

### 返回字段
|返回字段|字段类型|说明 |
|:----- |:------|:----------------------------- |
|players | list |所有参加选手的列表|
|account_id | 32-bit | 帐号ID |
|player_slot | 8-bit | [选手位置](####player_slot)|
|hero_id | int | 英雄的唯一ID |
|item_0~5 | int | 1~6 格物品栏，左上开始为 1  |
|kills | int | 杀人数 |
|deaths | int | 死亡数 |
|assists | int | 助攻数 |
|leaver_status | int | 离开状态：0 正常比赛 1 断线，未放弃 2 断线太久，已放弃 3 断线后放弃 4 秒退 5 NEVER\_CONNECTED 6 NEVER\_CONNECTED\_TOO\_LONG |
|gold | int | 身上有的钱 |
|last_hits | int | 正补 |
|denies | int | 反补 |
|gold\_per\_min | int | GPM |
|xp\_per\_min | int | XPM |
|gold_spent | int | 金钱消费 |
|hero_damage | int | 英雄伤害 |
|hero_healing | int | 英雄治疗 |
|level | int | 比赛结束时选手等级 |
|ability_upgrades | list | 技能升级顺序 |
|ability | int | 技能ID |
|time | int | 升级时的时间，单位为秒 |
|level | int | 升级时的等级 |
|additional_units | list | 额外的物品栏，比如:熊德 |
|season | int | 比赛的季节(应该是职业联赛才有) |
|radiant_win | bool | 天辉获胜为 true，夜魇获胜为 false |
|duration | int | 比赛时长 |
|start_time | int | 比赛开始时间，unix 时间戳 |
|match_id | int | 比赛ID |
|match\_seq\_num | int | 升级时的等级 |
|tower_status_radiant | 16-bit | 天辉[防御塔状态](####tower_status) |
|tower_status_dire | 16-bit | 夜魇[防御塔状态](####tower_status) |
|barracks_status_radiant | 8-bit | 天辉[兵营状态](####barracks_status) |
|barracks_status_dire | 8-bit | 夜魇[兵营状态](####barracks_status) |
|cluster | int | 服务器集群，用于下载录像 |
|first_blood_time | int | 第一滴血的时间 |
|lobby_type | int | 游戏类型：-1 非法 0 Public matchmaking 1 练习赛 2 锦标赛 3 教学 4 合作对抗机器人 5 队伍匹配 6 单人匹配 7 天梯 8 Solo |
|season | int | 比赛的季节(应该是职业联赛才有) |
|human_players | int | 玩家总人数 |
|leagueid | int | 联赛ID |
|positive_votes | int | 比赛的点赞数 |
|negative_votes | int | 比赛被踩的数量 |
|game_mode | int | 游戏模式：0 无 1 全英雄选择 2 队长模式 3 随机征召 4 个人征召 5 全部随机 6 Intro 7 Diretide 8 反转队长模式 9 The Greeviling 10 教学 11 仅中路 12 Least Played 13 New Player Pool 14 Compendium Matchmaking 15 合作对抗电脑 16 队长征召 18 技能征召 20 随机死亡模式 21 1V1 仅中路 22 Ranked Matchmaking|
|flags | int | ? |
|engine | int | ? |
|radiant_score | int | 天辉分数 |
|dire_score | int | 夜魇分数 |

#### 接口示例
http://api.steampowered.com/IDOTA2Match_570/GetMatchDetails/v001/?language=en_us&match_id=MATCHID&key=YOURKEY&format=json

#### player_slot
player_slot 是一个 8-bit 的 unsigned integer：第一位数代表队伍，0 是天辉，1 是夜魇；最后三位数代表选手打的位置 1~5 号位，数值对应 0~4。
> 例如：player_slot 为 0 ，转化为二进制数为 0000000 ，说明这个选手是 天辉，且打 1 号位；player\_slot 为 130，转化为二进制数为 10000010，说明这个选手是 夜魇，且打 2 号位。

#### tower_status
防御塔状态是一个 16-bit 的 unsigned integer：最右边的 11 个数分别对应 11 个防御塔，顺序为门牙下塔、门牙上塔、下路高地塔、下路中塔、下路外塔、中路高地塔、中路中塔、中路外塔、上路高地塔、上路中塔、上路外塔。
> 例如：tower_status_radiant 为 0 ，则所有天辉塔均被拆完；tower_status_dire 为 1828，转化为二进制数为 11100100100，代表门牙塔和三座高地塔全在，中塔和外塔被拆。

#### barracks_status
兵营状态是一个 8-bit 的 unsigned integer：最右边的 6 个数字分别对应 6 个兵营，顺序为：下路远程，下路近战，中路远程，中路近战，上路远程，上路近战。
> 例如：barracks_status_radiant 为 0 ，则所有天辉兵营均被拆完；barracks_status_dire 为 63，转化为二进制数为 111111，代表所有兵营全部还在。


