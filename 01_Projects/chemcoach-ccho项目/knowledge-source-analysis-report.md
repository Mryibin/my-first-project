# CCHO_REAL_PILOT_V1 教材预入库完整报告

分析日期：2026-09-05。只读扫描、识别、目录、选择与草稿；未正式入库。

## A. Files Discovered

实际为三套、五卷。SHA-256无重复；均可读、未加密、application/pdf、中文扫描内容。

|文件|字节|页数|≥100字符页|
|---|---:|---:|---:|
|无机化学（第六版）.pdf|306014333|620|0|
|基础有机化学(第4版 )上册.pdf|418181315|662|0|
|基础有机化学(第4版)下册.pdf|170524445|618|288|
|物理化学(第七版)上册.pdf|17797119|308|1|
|物理化学(第七版)下册.pdf|22882016|389|1|

编码：扫描页无可检查的正文字符编码；下册混合OCR层存在语义乱码。文件清单见file-inventory.json。

## B. Book Metadata

书名：无机化学；文件名：无机化学（第六版）.pdf

作者/编者：大连理工大学无机化学教研室 编；孟长功 主编

版本：第六版；出版社：高等教育出版社；年份：2018；ISBN：978-7-04-050429-3

证据：PDF 1、3、4；2018年9月第6版，12月第2次印刷

SHA-256：`1299f52a6b28948dfb30fa4e9ee98313a117605e72d4e7b59e442560776b0946`

文件：`D:\chemcoach-ccho\data\intake\knowledge\private\inorganic_chemistry\无机化学（第六版）.pdf`


书名：UNKNOWN；文件名：基础有机化学(第4版 )上册.pdf

作者/编者：UNKNOWN

版本：UNKNOWN；出版社：UNKNOWN；年份：UNKNOWN；ISBN：UNKNOWN

证据：文件名提示基础有机化学第4版上册；开头直接为正文，已查前部未见书名或版权页；未据下册推定身份

SHA-256：`2c5bfaeee3851e718da5402a46b668c4e7ead47800d5e6ed47ad25106006e7ea`

文件：`D:\chemcoach-ccho\data\intake\knowledge\private\organic_chemistry\基础有机化学(第4版 )上册.pdf`


书名：基础有机化学 下册；文件名：基础有机化学(第4版)下册.pdf

作者/编者：邢其毅、裴伟伟、徐瑞秋、裴坚 编著

版本：第4版；出版社：北京大学出版社；年份：2017；ISBN：978-7-301-27943-4

证据：PDF 1版权页；2017年1月第1版、第1次印刷；书名/CIP标第4版

SHA-256：`d4752630d7b7a6d614ba20da334ee53d34eed098f81f3fcd5485a1d56953b2d6`

文件：`D:\chemcoach-ccho\data\intake\knowledge\private\organic_chemistry\基础有机化学(第4版)下册.pdf`


书名：物理化学 上册；文件名：物理化学(第七版)上册.pdf

作者/编者：天津大学物理化学教研室 编；刘俊吉、李松林、冯霞、朱荣娇、陈丽 修订

版本：第七版；出版社：高等教育出版社；年份：2024；ISBN：978-7-04-061817-4

证据：PDF 2、3；2024年4月第7版、第2次印刷

SHA-256：`9554867507d1cdf54f0f9b06241edcd0b259d5f4c9f87366438116401e20db9f`

文件：`D:\chemcoach-ccho\data\intake\knowledge\private\physical_chemistry\物理化学(第七版)上册.pdf`


书名：物理化学 下册；文件名：物理化学(第七版)下册.pdf

作者/编者：天津大学物理化学教研室 编；李松林、冯霞、刘俊吉、孙艳 修订

版本：第七版；出版社：高等教育出版社；年份：2024；ISBN：978-7-04-062060-3

证据：PDF 2、3；2024年5月第7版、第1次印刷

SHA-256：`dd60ae5b2a937bfb0ae070d847f6ac193817f934ac35c8a7c5487891ff1ccadc`

文件：`D:\chemcoach-ccho\data\intake\knowledge\private\physical_chemistry\物理化学(第七版)下册.pdf`

## C. Rights Status

五卷均为RESTRICTED、ALLOWED_WITH_RESTRICTIONS、UNREVIEWED、DRAFT。用途仅PRIVATE_STUDY和INTERNAL_EVALUATION。此分类按用户要求填写，不代表已获得许可。四卷版权页有版权保留信息，有机上册UNKNOWN。未发现可核实的开放或商业许可。各卷rights_evidence见manifests目录，未提交审批；source_id为本地草稿标识，非数据库ID。

## D. TOC Extraction

5/5卷获得至少章级或局部目录结构；完整节级校对0/5，因此整体PARTIAL，不给出无法核验的节级成功率。无机目录页13–23；物化上10–16、下10–17；有机下2–9但OCR多栏错序严重，报告仅保留可靠章名及候选小节；有机上依赖13个章书签和第1章正文。未知终页明确UNKNOWN。每卷独立TOC保留身份与完整SHA。

## E. Recommended Pilot Sections

|ID|教材/节|印刷页（PDF页）|Domain|估算字符/Token/Chunk|优先级|
|---|---|---|---|---:|---|
|S01|无机化学 7.1.1 氧化值|153–155（176–178）|A|1400/1569/11|P0|
|S02|无机化学 7.1.2 氧化还原反应方程式的配平|155–156（178–179）|A|450/505/4|P0|
|S03|无机化学 7.4.1 判断氧化剂、还原剂的相对强弱|172–173（195–196）|A|350/393/3|P0|
|S04|无机化学 7.4.2 判断氧化还原反应进行的方向|173–174（196–197）|A|250/280/2|P0|
|S05|物理化学 上册 1.1 / 项1 理想气体状态方程|6–7（23–24）|B|650/729/5|P0|
|S06|物理化学 上册 2.3 恒容热、恒压热及焓|39–40（56–57）|B|1600/1793/12|P0|
|S07|物理化学 上册 2.8 / 项1 标准摩尔生成焓|61–62（78–79）|B|850/953/7|P0|
|S08|物理化学 上册 5.2 / 项2 理想气体反应的标准平衡常数|195–196（212–213）|B|850/953/7|P0|
|S09|物理化学 下册 7.6 / 项5 能斯特方程|319–320（46–47）|B|900/1009/7|P0|
|S10|基础有机化学 下册 14.5.1 胺的酸性|668–669（17–18）|C|300/337/3|P0|
|S11|基础有机化学 下册 14.5.2 胺的碱性|669–670（18–19）|C|1100/1233/8|P0|
|S12|基础有机化学 下册 14.6.1 胺的成盐反应|671–671（20–20）|C|350/393/3|P0|

选段视觉依赖LOW，取字风险HIGH；小节完整内容可能MEDIUM，故统一SUBSECTION_SELECTION_RECOMMENDED。必须按CSV中reason的起止边界排除例题、图表及解答，不按整页导入。P1为溶度积、身份确认后的上册酸碱电子理论。光谱与相平衡整章EXCLUDE_FOR_V1。

## F. Corpus Size Estimate

推荐12个小节：无机4、物化5、有机3。均为小节内受限选段，不是整节导入。

估算字符 9050；估算tokens 10147；Estimated Raw Chunks: 72（约54–94）；Estimated Reviewable CorpusUnits: 72（规划按一chunk对应一待审单元）；Target Human Verified Chunks: 60–100。实际创建0个。

方法：按抽查页的文字密度及上述排除边界人工估算字符量（非精确OCR字数），tokens暂取字符×1.12覆盖公式/符号开销；沿用项目默认target180、overlap24，以每小节ceil(tokens/156)估算。没有调用chunker。数值置信度LOW，不能保证人工复核后合格数量。上下限为基准的75%–130%，按小节切分、合并、去重与淘汰会改变结果。

当前估算上界低于120，无需扩成整章。若审核后不足60，先审P1溶度积，再审已确认身份的有机上册酸碱电子理论；若不足40必须重做补充选择及估算，不能自动导入。超过100先删重复判据，超过120必须缩小范围。三个有机小节覆盖面较窄，此轮不代表有机全域能力。

## G. Knowledge Point Proposal


只读查询当前knowledge_points返回0条。已有文档中的DEMO.GEN.*为合成示例，不能作为真实知识点复用。仅映射既有domain枚举，不写入数据库，不创建正式ontology节点。A/B/C是Pilot内部分类。

|提议code|已有point|domain映射|类型|状态|
|---|---|---|---|---|
|KP-REDOX-OXIDATION-NUMBER|NONE|INORGANIC|NEW_KNOWLEDGE_POINT_PROPOSAL|PROPOSED|
|KP-REDOX-AGENTS|NONE|INORGANIC|NEW_KNOWLEDGE_POINT_PROPOSAL|PROPOSED|
|KP-REDOX-ELECTRON-CONSERVATION|NONE|INORGANIC|NEW_KNOWLEDGE_POINT_PROPOSAL|PROPOSED|
|KP-REDOX-POTENTIAL-STRENGTH|NONE|INORGANIC|NEW_KNOWLEDGE_POINT_PROPOSAL|PROPOSED|
|KP-REDOX-DIRECTION|NONE|INORGANIC|NEW_KNOWLEDGE_POINT_PROPOSAL|PROPOSED|
|KP-GAS-IDEAL-RELATIONS|NONE|PHYSICAL_CHEMISTRY|NEW_KNOWLEDGE_POINT_PROPOSAL|PROPOSED|
|KP-THERMO-QV-QP-ENTHALPY|NONE|PHYSICAL_CHEMISTRY|NEW_KNOWLEDGE_POINT_PROPOSAL|PROPOSED|
|KP-THERMOCHEM-FORMATION-ENTHALPY|NONE|PHYSICAL_CHEMISTRY|NEW_KNOWLEDGE_POINT_PROPOSAL|PROPOSED|
|KP-EQUILIBRIUM-K|NONE|PHYSICAL_CHEMISTRY|NEW_KNOWLEDGE_POINT_PROPOSAL|PROPOSED|
|KP-ELECTROCHEM-NERNST|NONE|PHYSICAL_CHEMISTRY|NEW_KNOWLEDGE_POINT_PROPOSAL|PROPOSED|
|KP-ORGANIC-AMINE-ACIDITY|NONE|ORGANIC|NEW_KNOWLEDGE_POINT_PROPOSAL|PROPOSED|
|KP-ORGANIC-AMINE-BASICITY|NONE|ORGANIC|NEW_KNOWLEDGE_POINT_PROPOSAL|PROPOSED|
|KP-ORGANIC-AMINE-SALT|NONE|ORGANIC|NEW_KNOWLEDGE_POINT_PROPOSAL|PROPOSED|

## H. Extraction Risks


五卷均不能直接依赖自动纯文本入库。P0的LOW是选段语义对图的依赖，不是OCR难度；扫描取字仍需人工转录/校对。全节与截取片段风险分列于CSV，未选范围不得顺带导入。

|卷|正文文本层统计（≥100字符页/总页）|风险|
|---|---|---|
|无机化学（第六版）.pdf|0/620|HIGH|
|基础有机化学(第4版 )上册.pdf|0/662|HIGH|
|基础有机化学(第4版)下册.pdf|288/618|HIGH|
|物理化学(第七版)上册.pdf|1/308|HIGH|
|物理化学(第七版)下册.pdf|1/389|HIGH|

统计为逐页pypdf抽取量，不等于可用内容比例。有机下册部分页存在OCR，但胺/铵、Li及化学上下标误识别；其他卷极少量可抽取文本主要位于封面/尾页，正文扫描为主。离线中文OCR可辅助找标题，不能取代公式复核。

实测例：物化PDF24的pV=nRT丢失等号；PDF57的Δ、δ、Q下标和非体积功条件混乱；PDF79的ΔfH及求和式严重破损；无机PDF177离子电荷、反应箭头及Zn/Cu字母被错认。有机下册有公众号水印及其OCR噪声，应剔除且不能作为授权证明。

|检查项|本次结论|未来处理|
|---|---|---|
|H₂SO₄、Fe³⁺、SO₄²⁻|未完成逐个字形准确率测试；同类电荷/上下标破损已见|逐式比对原页，不能声称已正确保留|
|ΔG、ΔH、E°|相应推荐热力学/电化学页的原生文本不可用或OCR失真|人工保留Δ、标准态标记与下标|
|Kₐ、Ksp|Ka相关有机页及P1溶度积页需复核，不将OCR当真值|区分a/b/sp及活度、浓度的约定|
|⇌、电荷、希腊字母|丢失或替换风险HIGH|逐式审查反应方向、符号位置|
|跨页公式|能斯特、氧化值等选段跨页|连同条件一起校对，不能仅取公式|
|表格/图片|表7-1、表14-2、焓框图等不收|不得将图表依赖段落伪装成纯文本|
|页眉页脚/水印|样页可见|建立边界并人工确认清除|

所有例题、习题、解答、参考答案、光谱、相图、复杂机理与长合成路线排除。未检查全书每一个公式；质量评价是全页文本层统计加候选页抽查。

## I. Cross-Book Overlap


|主题|PRIMARY_SOURCE|其他来源角色|处理|
|---|---|---|---|
|氧化值、电子守恒|无机7.1|有机氧化态章节 SECONDARY_SOURCE|未选有机重复定义，SKIP_DUPLICATE|
|电势与反应方向|物化7.6项5定量；无机7.4定性|COMPLEMENTARY|无机仅留定性判据，能斯特公式与条件以物化为主，不重复推导|
|平衡与热力学|物化2.3、2.8、5.2|无机热化学/化学平衡 SECONDARY_SOURCE|本轮不再选择无机对应整章|
|酸碱性|有机下册14.5用于胺|上册1.4.4 SECONDARY_SOURCE；P1|通用酸碱理论不重复收，保留有机结构/溶剂条件差异|
|成盐与碱性|有机14.5.2与14.6.1|COMPLEMENTARY|14.6.1仅保留分离原理，重复质子化式SKIP_DUPLICATE|

这是目录与推荐片段层面的概念重叠分析，未进行全文近重复检索或Benchmark答案匹配。不同教材的标准态、符号和适用条件不能未经审核合并。

## J. Human Review Packet

请使用knowledge-source-review-packet.md逐卷审核，确认身份、来源用途、选段IDs、边界及人工校对工作量。所有选择保持空白，未替用户作审批。5份manifest均为YAML 1.2兼容JSON表示的设计草稿，不是可直接执行的intake输入。

## K. Pilot Readiness

```text
Knowledge Source Scan: COMPLETE
Rights Metadata: NEEDS_REVIEW
TOC Extraction: PARTIAL
Pilot Chapter Selection: PROPOSED
Formal Ingestion: NOT STARTED
Human Verification: NOT STARTED
```

安全检查见safety-validation.md；没有读取Benchmark或Gold答案，没有生成Ground Truth，没有执行ingest/index/create-snapshot/human-verify/benchmark-run，没有提交Git。

# WAITING FOR HUMAN APPROVAL OF PILOT CHAPTER SELECTION
