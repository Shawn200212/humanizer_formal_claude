# Humanizer — holdout-human-zh.md

**PASS** — median window and above-0.5 share both sit inside the human held-out band.

| | this draft | held-out human prose |
|---|---|---|
| median window p(machine) | 0.007 | 0.011 (p90 0.238) |
| windows above 0.5 | 0.0% | 4.2% |
| windows scored | 25 | 475 |

Model: zh, 120 boosting rounds kept of 900 trained, held-out test AUC 0.998.

A single high window proves nothing — see the false-positive rate above. Read the passage.

## Passages to rework

### 1. p(machine) = 0.194

> 终版本已经应用到实际工作中，并取得较好的效果。系3.4.2SQLite存储数据实现步骤统安装及运行效果如图7所示。根据实际业务需求，系1)完成LitePal的配置工作；统实现了高空鸟瞰图与地平线视角两种显示方式，供2)创建数据库并创建需要的数据表；用户在实际应用中进行选择，运行效果如图8所示。态，通过旋转，可以从多种角度来观察地球上投影的地5总结形地貌；通过“日夜循环”模式，模拟飞行器绕地飞行，本文针对地质数据野外采集与数字化的实际问题，宏观地观察地面的各种复杂地形情况，方便规划野外集成WorldWind地图技术、网络通信技术、多线程  地质勘查路线；也能够实现地标拖拽、纹理分析及模技术和数据存储技术，设计并实现了一种基于World  质录入方法和掌上电脑平面文本录入的方法。基于北导航及投影研究取得了突破性成果…

- **ttr** = 0.834 (human p10–p90: 0.633–0.803, median 0.727). Unusually high type-token ratio. In this corpus that is a *machine* signal, not a virtue: generated prose keeps reaching for a fresh synonym where published prose repeats the technical term. Repeat the term.
- **rare_token_share** = 0.524 (human p10–p90: 0.351–0.499, median 0.413). Heavy rare-term load. Check the terms are defined on first use.
- **hedge_per_1k** = 2.304 (human p10–p90: 0.0–4.866, median 0.0). Hedges above the band. Keep the ones that mark a real limit on the evidence; delete the ones hedging a fact.
- **nominalization_per_1k** = 29.954 (human p10–p90: 4.796–33.482, median 16.746). Nominalised verbs. 'We evaluated' beats 'an evaluation was performed'.

### 2. p(machine) = 0.071

> SQLite数据库文件存放在应用程序根目录下的之处在于不需要用户名和密码等访问权限。Android正databases目录里面，不仅支持标准的SQL语法，还支是通过将其设置为操作系统自带的数据库，进一步促持数据库的ACID事务，相对于其他的数据库，其方便进了数据的本地持久化。3.4.1图幅切片存储到SD卡实现步骤3)继承DataSupport类，实现增、删、改、查1)创建File类，指向图幅切片保存位置；2)创建File类对应的缓存输出流BufferOutputStream;4系统运行效果测试3)将字节流数列转换为Bitmap对象；本系统已在Android系统的设备上进行安装调试，4)调用Bitmap类下的compress方法将图片以并通过大量的测试，对系统的功能进行优化和升级，最png文件格式保存到SD卡对应…

- **ttr** = 0.838 (human p10–p90: 0.633–0.803, median 0.727). Unusually high type-token ratio. In this corpus that is a *machine* signal, not a virtue: generated prose keeps reaching for a fresh synonym where published prose repeats the technical term. Repeat the term.
- **rare_token_share** = 0.522 (human p10–p90: 0.351–0.499, median 0.413). Heavy rare-term load. Check the terms are defined on first use.

### 3. p(machine) = 0.057

> 说对于同一影像，它被分成18°×18°的片段，因此产生序号字段名称字段名类型长度小数单位必填800块9°×9°的瓦片；图层3就是4.5°×4.5°,而且含有1调查点统一编号PKIAA_POINTGUID36必填3200块瓦片，以此类推。2图幅编号MineNOvarchar10必填3路线编号RouteNovarchar10必填WorldWind采用了先进的流传输技术，传统的数4调查点号PointNOvarchar5必填据传输方式是直接传输空间数据，而WorldWind客户  端和服务器采用了传输图片的方式，也就是说当用户MainThread,其余的线程则被称为WorkerThread.当向服务器请求数据时，服务器不用即时生成数据，而是用户在运行某个程序的时候，Android操作系统都会开将先前准备好的图片数据拼接…

- **rare_token_share** = 0.521 (human p10–p90: 0.351–0.499, median 0.413). Heavy rare-term load. Check the terms are defined on first use.
- **ttr** = 0.828 (human p10–p90: 0.633–0.803, median 0.727). Unusually high type-token ratio. In this corpus that is a *machine* signal, not a virtue: generated prose keeps reaching for a fresh synonym where published prose repeats the technical term. Repeat the term.
- **clause_cv** = 0.791 (human p10–p90: 0.535–1.178, median 0.808). Every sentence has the same number of clauses. Vary the syntactic shape, not just the length.
- **hedge_per_1k** = 4.706 (human p10–p90: 0.0–4.866, median 0.0). Hedges above the band. Keep the ones that mark a real limit on the evidence; delete the ones hedging a fact.
