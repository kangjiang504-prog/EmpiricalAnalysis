# EmpiricalAnalysis
Stata 实证分析 GUI 工具：傻瓜式操作，一键实证，一键完成常见计量回归，并同步生成可复现的 Stata `.do` 文件。  A GUI toolkit for Stata empirical analysis: simple point-and-click workflow with automatic, reproducible Stata `.do` file generation.
【使用时直接运行EmpiricalAnalysis_v1.13.1\dist文件夹里的exe文件】
中文简介

EmpiricalAnalysis 是一款面向经济学、管理学及其他社会科学研究者的 GUI 实证分析软件。项目的核心目标，是把常见的计量回归、稳健性检验和机制分析整理成可视化、低代码、流程化的操作界面。

用户无需反复编写和修改 Stata 命令，只需要按照页面提示选择数据、变量、固定效应、聚类方式和模型参数，即可完成分析并自动整理结果。

核心特色

1. 傻瓜式操作 / Point-and-click workflow

通过图形化界面完成数据导入、变量设置、模型选择和结果导出。对于常见的实证任务，用户无需从头编写复杂的 Stata 代码。

2. Stata .do 文件同步输出 / Automatic Stata .do generation

这是本项目最重要的特色之一。每次分析不仅可以得到 Python 侧的估计结果和表格，还会同步生成对应的 Stata .do 文件，方便：

在 Stata 中复核和重新运行；

修改模型设定；

将软件操作转化为可保存、可追踪的代码；

在论文复现、审稿回复和后续研究中保留完整分析路径。

3. 每个回归模块独立设置“标准误 / t 值”

各回归模块均可以独立选择结果表括号显示：

聚类稳健标准误；

t 值。

该选项只影响结果表的显示方式，不改变原有回归估计方法。软件仍根据当前模型计算相应的 HC1、单向聚类或双向聚类稳健协方差，并据此得到 t 值。

4. 同时输出 Word、Excel 和 Stata .do 文件

分析完成后，可自动整理为适合进一步修改和论文使用的结果文件，同时保留对应的 Stata 代码。

主要功能

当前版本支持以下分析模块：

模块

主要内容

基准回归

9 种 OLS / 固定效应 / 聚类模型；模型 4–9 支持渐进式规格输出

IV / Lewbel / Bartik

外部工具变量、Lewbel 内生工具变量、Bartik 工具变量及排除自身地区的 Bartik

Heckman

两阶段 Heckman 选择模型，并支持多种 IV 构造思路

Oster

基于 psacalc 思路的遗漏变量偏误稳健性检验

控制变量组合

对不同控制变量组合进行稳健性比较

PSM

1:1–1:5 最近邻、Kernel、半径/卡尺匹配等

变量替换

更换被解释变量或核心解释变量的衡量方式

Logit / Probit / Poisson

常见非线性及计数型模型

DML

Random Forest、Lasso、GBM 等学习器结合交叉拟合的双重机器学习流程

机制检验

两步法、三步法、Sobel、Bootstrap

异质性分析

分组回归及组间系数差异 Bootstrap 检验

**说明：**不同模型的具体设定、样本处理和标准误处理可能存在差异。建议在正式论文中将软件结果与 Stata 结果进行交叉核对，并根据研究设计进行必要的人工检查。

一个典型的使用流程

导入 Excel / CSV 数据
        ↓
选择 Y、X、控制变量、ID、Year
        ↓
选择固定效应与 Cluster
        ↓
选择具体回归 / 稳健性模块
        ↓
选择结果括号：标准误 or t 值
        ↓
运行分析
        ↓
┌────────────────────────────┐
│ Python 估计结果             │
│ Word 结果表                 │
│ Excel 结果表                │
│ Stata .do 文件              │
└────────────────────────────┘

软件的目标不是完全替代 Stata，而是把常见的实证分析流程做成更低门槛的图形化入口，同时始终保留 Stata .do 文件这一可复现接口。

基准回归的特色设计

基准回归目前提供 9 种模型规格。其中部分模型支持按固定效应数量自动生成渐进式结果表。

例如，当模型包含多个固定效应时，可以按照：

第 1 列：Y ~ X
第 2 列：加入第 1 个固定效应
第 3 列：继续加入第 2 个固定效应
...
最后 1 列：全部固定效应 + 全部控制变量

自动生成结果结构，减少手工重复运行回归的工作量。

结果表括号：标准误 / t 值

从 v1.13.1 开始，每个回归模块都可以单独设置：

表格括号：
[聚类稳健标准误]
[t 值]

例如：

选择“聚类稳健标准误”
β = -0.1424**
   (0.0671)

或：

选择“t 值”
β = -0.1424**
   (-2.123)

**注意：**选择 t 值并不会把原来的稳健协方差改成普通标准误；t 值仍然来自相应稳健/聚类稳健标准误。

输出文件

每次分析可根据对应模块生成结果文件，主要包括：

Word (.docx)
Excel (.xlsx)
Stata (.do)

其中 .do 文件会保存软件当前分析对应的 Stata 命令，方便进一步人工修改、复核和复现。

Stata .do 文件的可复现性

自动生成的 .do 文件是本项目的重要设计目标。

例如，软件的图形化操作：

Y = outcome
X = treatment
Controls = size lev roa growth
Firm FE = Yes
Year FE = Yes
Cluster = firm_id

会同步转化为相应的 Stata 分析代码。这样，GUI 操作并不是“黑箱”：研究者仍然可以在 Stata 中看到、保存、检查和修改最终命令。

部分模块会调用 Stata 社区扩展命令，因此重新运行对应 .do 文件时，需要根据模块安装相应的 Stata package，例如：

reghdfe
ivreghdfe
outreg2
psmatch2
ddml
pystacked
psacalc

具体以生成的 .do 文件为准。
