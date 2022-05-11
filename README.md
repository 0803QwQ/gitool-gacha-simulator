# GITool原神抽卡模拟器
这是一个完全在前端运行的原神抽卡模拟器，使用jQuery开发❤️<hr>
**项目示例效果 => [GitHub Pages](https://0803qwq.github.io/gitool-gacha-simulator/)**<hr>
## 基本使用方法
将本仓库clone到您的网站目录下即可开始使用<br>
items.json文件是设置抽卡内容的文件，您可以查看 [如何使用items.json](#如何使用itemsjson) 来获取详细使用方法
## 这个项目可以做什么
1.支持模拟抽取原神的角色UP池，武器UP池和常驻池<br>
2.已实现角色池90发小保底和180发大保底机制<br>
3.已实现多角色UP的情况下切换UP池的方法<br>
4.已实现武器池80发保底机制和定轨机制（包括定轨重置）<br>
5.实现了70发之后递增出货概率的平衡机制<hr>
**总而言之，此项目已尽可能复原原神的抽卡核心机制！**<br>
#### 具体概率：
0.1.五星物品不含保底出货概率：0.6%<br>
0.2.四星物品不含保底出货概率：5.1%<br>
0.3.四星物品在每10的整数倍抽数时保底<br>
##### 1.角色池：
1.1.五星物品从上次出五星开始计算的70抽起开始递增出货概率，到第90抽递增到100%<br>
1.2.角色池限定五星和常驻五星两个集合出货概率各占50%<br>
1.3.当出五星时，出的是常驻五星，则下次出五星必为限定五星(即大保底)<br>
1.4.存在多个限定五星角色UP的情况的特殊机制：<br>
(1)出现这种情况时，需要用户设置抽取的五星角色，否则抽卡效果同2.3.(1)<br>
(2)当设置抽取的五星角色后，抽卡效果将与1.1-1.3一致，注意，另一个未被设置的限定五星将不会进入卡池<br>
(3)当抽卡时中途修改抽取五星角色池时，抽卡记录不重置(即保底共享)
##### 2.武器池：
2.1.五星物品从上次出五星开始计算的70抽起开始递增出货概率，到第80抽递增到100%<br>
2.2.武器池限定五星和常驻五星两个集合出货概率各占50%<br>
2.3.定轨机制：<br>
(1)若未设置定轨，则限定五星和常驻五星混合抽取，没有保底<br>
(2)若设置定轨，则在连续出两次非定轨五星武器(包括常驻五星和另一个未定轨的限定五星)后，下次出五星必为被定轨五星<br>
(3)若撤销定轨或重新设置定轨，则抽卡记录会重置
##### 3.常驻池：
3.1.五星物品从上次出五星开始计算的70抽起开始递增出货概率，到第90抽递增到100%<br>
3.2.常驻池常驻五星角色和常驻五星武器两个集合出货概率各占50%
## 如何使用items.json
### items.json的结构
```
{
    "r5": {----------------->五星物品
        "now-chr": [],------>当期限定五星角色
        "now-arm": [],------>当期限定五星武器
        "always-chr": [],--->常驻五星角色
        "always-arm": []---->常驻五星武器
    },
    "r4": {----------------->四星物品
        "now-chr": [],------>当期提高概率的四星角色
        "now-arm": [],------>当期提高概率的四星武器
        "always": []-------->全部四星物品
    },
    "r3": {----------------->三星物品
        "always": []-------->全部三星物品
    }
}
```
注：你可以向对应位置参考下面的添加结构向内添加数据
### 具体添加角色和武器的结构
```
角色：
[
    "角色头像地址（必须）",
    "角色名称（必须）",
    "角色稀有度（必须）",
    "角色使用武器类型",
    "角色属性",
    "角色性别",
    "角色归属",
    "90级生命值",
    "90级防御力",
    "90级攻击力",
    "90级通过角色突破获得的增益效果",
    "角色定位",
    "属性颜色"
]
武器：
[
    "武器图片地址（必须）",
    "武器名称（必须）",
    "武器类型（必须）",
    "武器稀有度（必须）",
    "武器来源",
    "武器初始攻击力",
    "武器初始副词条",
    "90级攻击力",
    "90级副词条",
    "武器被动效果"
]
```
以七七和风鹰剑为例：
```
[
    "https:\/\/patchwiki.biligame.com\/images\/ys\/thumb\/8\/8b\/049fpv6jcr66mln0nmbbfgigfrkgrzo.png\/60px-%E4%B8%83%E4%B8%83%E5%A4%B4%E5%83%8F.png",
    "七七",
    "5星",
    "单手剑",
    "冰",
    "女",
    "璃月",
    "12368",
    "287",
    "922",
    "治疗加成+22.2%",
    "治疗、复活、特产探索、减攻、自身伤害提升、能量恢复",
    "#A6FDFD"
]
[
    "https:\/\/patchwiki.biligame.com\/images\/ys\/thumb\/5\/5d\/0nq1evrhhxelybr3mogpxoouc4s6b98.png\/150px-%E9%A3%8E%E9%B9%B0%E5%89%91.png",
    "风鹰剑",
    "单手剑",
    "5星",
    "祈愿",
    "48",
    "物理伤害加成9.0%",
    "674",
    "物理伤害加成41.3%",
    "西风之鹰的抗争：攻击力提高20%\/25%\/30%\/35%\/40%；受到伤害时触发：高扬抗争旗号的西风鹰之魂苏醒，恢复同等与攻击力的100%\/115%\/130%\/145%\/160%生命值，并对周围的敌人造成200%\/230%\/260%\/290%\/320%攻击力的伤害。该效果每15秒只能触发一次。"
]
```
注：您可以直接从 [限定五星数据一览.txt](https://github.com/0803QwQ/gitool-gacha-simulator/blob/main/限定五星数据一览.txt) 和 [限定四星武器数据一览.txt](https://github.com/0803QwQ/gitool-gacha-simulator/blob/main/限定四星武器数据一览.txt) 复制所需限定物品的部分到items.json的相应位置来使用
### items.json的数据来源
通过解析以下地址的HTML文档获得：<br>
BiliWiki原神角色筛选：https://wiki.biligame.com/ys/%E8%A7%92%E8%89%B2%E7%AD%9B%E9%80%89<br>
BiliWiki原神武器图鉴：https://wiki.biligame.com/ys/%E6%AD%A6%E5%99%A8%E5%9B%BE%E9%89%B4
## 更新记录
### 2022/4/17
1.项目完全前端化<br>
2.修复了武器池可能卡死的Bug<br>
3.修复了切换卡池时，up信息不更新的Bug
### 2022/4/23
移除了items.json中一些在祈愿中抽不到的三星，四星武器
### 2022/5/11
修复了三星武器“沐浴龙血的剑”的缩略图缺失问题
## Todo（画饼）
显示抽到角色/武器的详细信息<br>
...
#### Powered by 0803QwQ, Thanks for Your Using.
