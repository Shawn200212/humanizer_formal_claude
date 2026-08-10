# Humanizer — holdout-machine-zh.md

**REWORK** — the median window sits above the 95th percentile of held-out human prose.

| | this draft | held-out human prose |
|---|---|---|
| median window p(machine) | 0.992 | 0.011 (p90 0.238) |
| windows above 0.5 | 100.0% | 4.2% |
| windows scored | 6 | 475 |

Model: zh, 120 boosting rounds kept of 900 trained, held-out test AUC 0.998.

A single high window proves nothing — see the false-positive rate above. Read the passage.

## Passages to rework

### 1. p(machine) = 0.998

> 我们在系统中把这一功能称为现场自洽性提示，它不阻断录入，只给出可忽略的警示。系统在青海某地区1:5万区域地质调查项目中投入试用，共3个填图小组、11名调查员、连续两个野外季。累计采集地质点4218个、界线1073条、产状6890组、样品记录512件。为量化效果，我们与同一项目组前一年采用纸质流程的记录做了对比。单日平均采集地质点数从12.4个提高到19.8个，主要节省来自坐标与高程的自动记录以及属性的下拉选择。室内整理时间从平均每人每天2.6小时降至0.4小时，后者主要是照片归档和文字描述润色。数据错误率方面，我们抽取400个地质点做人工复核，纸质流程转录后的属性错误率为4.8%，本系统为1.1%，其中大部分残余错误是描述性文字的笔误而非结构化字段。产状倾向记反的情况从17例降至2例，说明现场提示确实起了作用。…

- **ttr** = 0.882 (human p10–p90: 0.633–0.803, median 0.727). Unusually high type-token ratio. In this corpus that is a *machine* signal, not a virtue: generated prose keeps reaching for a fresh synonym where published prose repeats the technical term. Repeat the term.
- **rare_token_share** = 0.689 (human p10–p90: 0.351–0.499, median 0.413). Heavy rare-term load. Check the terms are defined on first use.
- **sent_len_cv** = 0.406 (human p10–p90: 0.365–0.839, median 0.569). Sentence lengths are too uniform. Split one long sentence into a short declarative plus a longer one, and let at least one sentence run long. Variance is a consequence of argument: short sentences mark turns, long ones carry qualification.
- **de_rate** = 34.23 (human p10–p90: 33.654–65.116, median 49.881). 「的」密度低于已发表中文论文，通常意味着修饰关系被名词化短语吸收了。检查是否有可以拆开的长定语。

### 2. p(machine) = 0.995

> 离线数据模块把地形高程、影像和已有地质图切成MBTiles瓦片预先下载，一个1:5万标准图幅的全套底图约1.7GB。同步模块在回到有网环境时执行增量上传，冲突以要素级时间戳解决，并保留双版本供人工裁决。三维显示的用法值得单独说明。我们没有把它做成一个炫技式的独立视图，而是与采集流程结合：调查员在露头处记录产状后，系统立即在三维地形上按该产状生成一个半透明的层面片，若同一构造单元内已有多个产状点，还会拟合出推测的层面延伸方向。这让调查员能在现场就发现明显不合理的记录——例如把倾向记反了180度，这类错误在纸质流程中通常要到室内制图时才暴露。我们在系统中把这一功能称为现场自洽性提示，它不阻断录入，只给出可忽略的警示。系统在青海某地区1:5万区域地质调查项目中投入试用，共3个填图小组、11名调查员、连续两个野外季。累…

- **ttr** = 0.895 (human p10–p90: 0.633–0.803, median 0.727). Unusually high type-token ratio. In this corpus that is a *machine* signal, not a virtue: generated prose keeps reaching for a fresh synonym where published prose repeats the technical term. Repeat the term.
- **rare_token_share** = 0.687 (human p10–p90: 0.351–0.499, median 0.413). Heavy rare-term load. Check the terms are defined on first use.
- **hedge_per_1k** = 5.0 (human p10–p90: 0.0–4.866, median 0.0). Hedges above the band. Keep the ones that mark a real limit on the evidence; delete the ones hedging a fact.

### 3. p(machine) = 0.992

> 产状倾向记反的情况从17例降至2例，说明现场提示确实起了作用。设备与续航是野外应用绕不开的问题。在8英寸平板上，三维模式下连续运行的平均功耗为3.9W，配合10000mAh移动电源可支撑约11小时，覆盖典型工作日。低温测试中，环境温度-8℃时设备可正常工作但电池容量衰减约35%，我们据此在系统中加入了低温省电模式，自动关闭三维渲染并降低定位采样率至5秒一次，功耗降至1.6W。调查员的主观反馈集中在两点：三维地形对判断填图路线帮助明显；表单的必填项校验在赶路时略显繁琐，后续版本改为允许暂存草稿。这项工作没有提出新的地质理论或算法，它的价值在于把开源虚拟地球平台真正落到了野外生产流程里，并给出了一套在无网络、长续航、多人协同约束下可行的工程组织方式。仍然存在的不足是：增量同步在两人同时修改同一条界线时仍需人工介入，…

- **ttr** = 0.947 (human p10–p90: 0.633–0.803, median 0.727). Unusually high type-token ratio. In this corpus that is a *machine* signal, not a virtue: generated prose keeps reaching for a fresh synonym where published prose repeats the technical term. Repeat the term.
- **rare_token_share** = 0.66 (human p10–p90: 0.351–0.499, median 0.413). Heavy rare-term load. Check the terms are defined on first use.
- **sent_len_cv** = 0.445 (human p10–p90: 0.365–0.839, median 0.569). Sentence lengths are too uniform. Split one long sentence into a short declarative plus a longer one, and let at least one sentence run long. Variance is a consequence of argument: short sentences mark turns, long ones carry qualification.
- **clause_cv** = 0.8 (human p10–p90: 0.535–1.178, median 0.808). Every sentence has the same number of clauses. Vary the syntactic shape, not just the length.
