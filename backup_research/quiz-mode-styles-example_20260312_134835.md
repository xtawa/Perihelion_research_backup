---
title: "Perihelion Quiz 组件使用指南：三种预设样式详解与交互示例"
date: 2026-03-11
description: "本文档系统介绍了 Perihelion 网站框架内置的 Quiz 交互组件的三种预设样式——量表问卷（Survey）、知识测验（Correct）与性格测试（Personality）的完整使用方法、数据属性配置及 Markdown 嵌入语法。每种样式均附有可交互的实例演示，涵盖总分计算、正误判定、分类统计与结果解读等核心功能。"
tags: ["Quiz", "交互组件", "使用指南", "Survey", "知识测验", "性格测试", "Perihelion"]
editor: "Perihelion"
lang: zh
justForFun: yes
---

## 概述

Perihelion 的 Quiz 组件支持在 Markdown 文章中直接嵌入交互式问卷与测验。通过 `data-style` 属性，你可以选择三种预设样式：

1. **Survey（量表问卷）**：默认样式。适用于 Likert 量表类心理测评，支持统一刻度、反向计分和分数段结果解读。
2. **Correct（知识测验）**：适用于有标准答案的客观题，选择后即时显示对错，统计正确数。
3. **Personality（性格测试）**：适用于无正误之分的人格/偏好类测试，统计各分类占比并可视化展示。

所有样式均支持 LocalStorage 自动保存、进度条、暗色模式与打印优化。

---

## 样式一：Survey（量表问卷）

这是默认样式，无需指定 `data-style`（或设为 `data-style="survey"`）。每道题共用相同的评分刻度，最终求和计算总分。

### 关键属性

| 属性 | 说明 | 示例 |
|------|------|------|
| `data-title` | 问卷标题 | `data-title="压力感知量表"` |
| `data-description` | 说明文字 | `data-description="回顾过去一周..."` |
| `data-scale` | 评分刻度定义 | `data-scale="1=完全不符,2=不太符合,3=一般,4=比较符合,5=完全符合"` |
| `data-results` | 分数段→结果 | `data-results="5-11=压力较低,12-18=压力中等,19-25=压力较高"` |
| `data-reversed` | 反向计分题号 | `data-reversed="2,4"` |

### 实例：生活满意度简易量表

以下是一个 5 题的生活满意度评估量表（改编自 SWLS），使用 1-5 分制。

<div class="perihelion-quiz"
     data-style="survey"
     data-quiz-id="swls-demo"
     data-title="生活满意度量表 SWLS-5（简版）"
     data-description="请根据你目前的实际感受，对以下每项陈述选择最符合的分数。1 = 完全不符，5 = 完全符合。"
     data-scale="1=完全不符,2=不太符合,3=一般,4=比较符合,5=完全符合"
     data-results="5-9=较低满意度：你对当前的生活状态存在较多不满,10-14=低于平均：生活中有些方面可能需要关注和调整,15-19=平均水平：你对生活的满意程度处于中等,20-22=较高满意度：你对大多数生活领域感到满意,23-25=非常满意：你对当前的生活状态高度认可">
  <div class="quiz-item">在大多数方面，我的生活接近我的理想状态</div>
  <div class="quiz-item">我的生活条件很好</div>
  <div class="quiz-item">我对自己的生活感到满意</div>
  <div class="quiz-item">到目前为止，我已经获得了生活中我想要的重要东西</div>
  <div class="quiz-item">如果可以重来，我几乎不会改变任何事</div>
</div>

### Markdown 写法

```html
<div class="perihelion-quiz"
     data-style="survey"
     data-title="生活满意度量表 SWLS-5"
     data-description="请根据实际感受选择分数。1=完全不符，5=完全符合。"
     data-scale="1=完全不符,2=不太符合,3=一般,4=比较符合,5=完全符合"
     data-results="5-9=较低满意度,10-14=低于平均,15-19=平均水平,20-22=较高满意度,23-25=非常满意">
  <div class="quiz-item">题目文本</div>
  <div class="quiz-item">题目文本</div>
</div>
```

> **提示**：如果需要某些题目反向计分（例如消极题），添加 `data-reversed="2,4"` 即可。系统会自动将这些题目的选项值用 `最大刻度值 - 选中值` 计算。

---

## 样式二：Correct（知识测验）

设置 `data-style="correct"` 启用。适用于每道题有唯一正确答案的客观知识测验。选中后立即反馈对错，并高亮正确答案。

### 关键属性

| 属性 | 说明 | 示例 |
|------|------|------|
| `data-style` | 必须设为 `correct` | `data-style="correct"` |
| `data-title` | 测验标题 | `data-title="天文学常识测验"` |
| `data-answer`（在 quiz-item 上） | 正确答案的 value | `data-answer="B"` |

每个 `quiz-item` 内部通过 `<span class="quiz-option" data-value="X">` 定义选项。

### 实例：天文学常识测验

来测试一下你的天文学知识！每道题只有一个正确答案，选择后会立即显示结果。

<div class="perihelion-quiz"
     data-style="correct"
     data-quiz-id="astro-quiz"
     data-title="天文学常识测验"
     data-description="共 5 题，每题选择一个你认为正确的答案。选中后将即时显示正误。">
  <div class="quiz-item" data-answer="C">
    太阳系中最大的行星是哪一颗？
    <span class="quiz-option" data-value="A">土星</span>
    <span class="quiz-option" data-value="B">天王星</span>
    <span class="quiz-option" data-value="C">木星</span>
    <span class="quiz-option" data-value="D">海王星</span>
  </div>
  <div class="quiz-item" data-answer="B">
    光从太阳到达地球大约需要多长时间？
    <span class="quiz-option" data-value="A">约 1 分钟</span>
    <span class="quiz-option" data-value="B">约 8 分钟</span>
    <span class="quiz-option" data-value="C">约 30 分钟</span>
    <span class="quiz-option" data-value="D">约 1 小时</span>
  </div>
  <div class="quiz-item" data-answer="A">
    "近日点"（Perihelion）指的是什么？
    <span class="quiz-option" data-value="A">行星轨道上离太阳最近的点</span>
    <span class="quiz-option" data-value="B">行星轨道上离太阳最远的点</span>
    <span class="quiz-option" data-value="C">行星自转速度最快的时刻</span>
    <span class="quiz-option" data-value="D">日食发生的位置</span>
  </div>
  <div class="quiz-item" data-answer="D">
    以下哪个不是太阳系的矮行星？
    <span class="quiz-option" data-value="A">冥王星</span>
    <span class="quiz-option" data-value="B">谷神星</span>
    <span class="quiz-option" data-value="C">阋神星</span>
    <span class="quiz-option" data-value="D">灶神星</span>
  </div>
  <div class="quiz-item" data-answer="B">
    银河系的直径大约是多少？
    <span class="quiz-option" data-value="A">约 1 万光年</span>
    <span class="quiz-option" data-value="B">约 10 万光年</span>
    <span class="quiz-option" data-value="C">约 100 万光年</span>
    <span class="quiz-option" data-value="D">约 1000 万光年</span>
  </div>
</div>

### Markdown 写法

```html
<div class="perihelion-quiz" data-style="correct" data-title="测验标题">
  <div class="quiz-item" data-answer="B">
    题目文本
    <span class="quiz-option" data-value="A">选项 A</span>
    <span class="quiz-option" data-value="B">选项 B（正确答案）</span>
    <span class="quiz-option" data-value="C">选项 C</span>
  </div>
</div>
```

> **提示**：`data-answer` 的值必须与某个 `quiz-option` 的 `data-value` 一致。选中后该题即锁定，不可更改。

---

## 样式三：Personality（性格测试）

设置 `data-style="personality"` 启用。适用于无标准答案的性格/偏好测试。每个选项对应一个分类标签，最终统计各分类出现次数并以条形图展示。

### 关键属性

| 属性 | 说明 | 示例 |
|------|------|------|
| `data-style` | 必须设为 `personality` | `data-style="personality"` |
| `data-title` | 测试标题 | `data-title="思维风格测试"` |
| `data-results` | 分类标签→描述 | `data-results="T=思考型：偏好逻辑分析,F=感受型：偏好情感判断"` |

每个 `quiz-option` 的 `data-value` 对应 `data-results` 中的分类 key。

### 实例：思维风格测试

以下测试将帮助你了解自己更倾向于哪种思维风格。没有对错之分，请凭直觉选择。

<div class="perihelion-quiz"
     data-style="personality"
     data-quiz-id="thinking-style"
     data-title="思维风格简易测试"
     data-description="以下每道题描述了两种不同的处事方式，请选择更接近你日常习惯的那一个。没有对错之分。"
     data-results="A=分析型：你倾向于用逻辑和数据来理解世界。在面对问题时，你会系统地拆解、量化和推理。你重视证据和一致性，决策时偏好理性分析而非直觉。,B=直觉型：你倾向于依靠感觉和整体印象来做判断。你善于捕捉隐含的模式和可能性，常常在数据不完整时就能做出准确的判断。你的创造力是你最大的优势。,C=实践型：你更关注具体的行动和可操作的方案。对你来说，一个好的想法必须能落地执行。你脚踏实地，注重效率，喜欢用实际成果来衡量一切。">
  <div class="quiz-item">
    当你需要做一个重要决定时，你通常会：
    <span class="quiz-option" data-value="A">列出利弊清单，逐条分析</span>
    <span class="quiz-option" data-value="B">跟随内心的第一感觉</span>
    <span class="quiz-option" data-value="C">看看别人在类似情况下怎么做的</span>
  </div>
  <div class="quiz-item">
    学习一项新技能时，你更喜欢：
    <span class="quiz-option" data-value="A">先阅读完整的理论和文档</span>
    <span class="quiz-option" data-value="B">先了解整体框架，再按兴趣深入</span>
    <span class="quiz-option" data-value="C">直接上手尝试，边做边学</span>
  </div>
  <div class="quiz-item">
    与朋友讨论一个有争议的话题时，你更可能：
    <span class="quiz-option" data-value="A">引用数据和研究来支持观点</span>
    <span class="quiz-option" data-value="B">从不同角度提出新的可能性</span>
    <span class="quiz-option" data-value="C">分享自己的亲身经历作为例证</span>
  </div>
  <div class="quiz-item">
    面对一个复杂问题，你的第一反应是：
    <span class="quiz-option" data-value="A">将问题拆解为几个小的子问题</span>
    <span class="quiz-option" data-value="B">退后一步，试图看到问题的全貌</span>
    <span class="quiz-option" data-value="C">马上想到的第一个解决方案就去试</span>
  </div>
  <div class="quiz-item">
    选择餐厅吃饭时，你更倾向于：
    <span class="quiz-option" data-value="A">查看评分和评价后做决定</span>
    <span class="quiz-option" data-value="B">选一家看起来有独特氛围的地方</span>
    <span class="quiz-option" data-value="C">去经常去的那家，因为确定好吃</span>
  </div>
  <div class="quiz-item">
    你更欣赏同事的哪种品质？
    <span class="quiz-option" data-value="A">思路清晰，逻辑严密</span>
    <span class="quiz-option" data-value="B">想法独特，富有创意</span>
    <span class="quiz-option" data-value="C">执行力强，靠谱高效</span>
  </div>
</div>

### Markdown 写法

```html
<div class="perihelion-quiz" data-style="personality" data-title="测试标题"
     data-results="A=类型A描述,B=类型B描述,C=类型C描述">
  <div class="quiz-item">
    题目文本
    <span class="quiz-option" data-value="A">对应类型 A 的选项</span>
    <span class="quiz-option" data-value="B">对应类型 B 的选项</span>
    <span class="quiz-option" data-value="C">对应类型 C 的选项</span>
  </div>
</div>
```

> **提示**：`data-results` 中，`=` 号左边是分类 key（对应选项的 `data-value`），右边是该类型的完整描述。如果描述中包含冒号（`：`），冒号前的部分会作为条形图的标签显示。

---

## 通用特性

### 数据持久化

所有样式的答案均自动保存至浏览器 LocalStorage。刷新页面、关闭浏览器后再次打开，之前的选择都会自动恢复。每个问卷的存储 key 格式为：`quiz-{页面路径}-{quizId}`。

### 进度条

三种样式都内置进度条，实时显示已作答题目的百分比。

### 重置功能

每个问卷底部都有"重置"按钮，点击后会清除所有已选答案和 LocalStorage 中的数据。

### 暗色模式

组件会自动适配站点的暗色模式。所有配色方案都有对应的 dark mode 变体。

### 打印优化

在打印/导出 PDF 时，重置按钮会自动隐藏，选中的答案会被清晰标记。

### 自定义 Quiz ID

通过 `data-quiz-id` 属性可以为每个问卷指定唯一 ID。如果同一页面有多个问卷，建议手动设置以避免 ID 冲突。

---

## 样式对比总结

| 特性 | Survey | Correct | Personality |
|------|--------|---------|-------------|
| 适用场景 | 量表测评 | 客观题测验 | 性格/偏好测试 |
| 选项形式 | 统一刻度（radio） | 每题独立选项（button） | 每题独立选项（button） |
| 计分方式 | 求和总分 | 统计正确数 | 统计分类频次 |
| 结果展示 | 分数段解读 | 正确数 / 总题数 | 主导类型 + 分布条形图 |
| 反向计分 | 支持 | 不适用 | 不适用 |
| 作答锁定 | 可修改 | 选后锁定 | 可修改 |
| `data-style` | `survey`（默认） | `correct` | `personality` |
