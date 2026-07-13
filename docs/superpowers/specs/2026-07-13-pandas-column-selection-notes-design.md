# Pandas 选列语法笔记设计

## 目标

参考 Codex 会话 `019f4580-796f-7b63-8a63-449e92cec315`，在 `Python学习笔记.md` 中补充一节适合初学者回看的 Pandas 选列语法笔记。

## 位置与结构

- 在 `## 工程化相关` 之前新增四级标题 `#### Pandas 的选列语法`。
- 使用“概念说明 + 对照示例”的形式，与现有笔记中的短章节风格保持一致。
- 不新增 frontmatter、标签或外部依赖。

## 内容范围

笔记说明以下三种写法及其返回类型：

```python
df["node_code"]
df[["node_code"]]
df[["node_code", "simulate_node_type"]]
```

内容包括：

- `df["node_code"]` 选择一列并返回 `pandas.Series`。
- `df[["node_code"]]` 选择一列但保持二维表结构，返回 `pandas.DataFrame`。
- `df[["node_code", "simulate_node_type"]]` 选择多列并返回 `pandas.DataFrame`。
- 解释双层方括号：内层是普通 Python 列表，外层是 DataFrame 的 `[]` 取列操作。
- 展示先定义 `columns` 再执行 `df[columns]` 的等价写法。
- 用 SQL `SELECT` 作一句简短类比。

## 排除范围

本节不介绍布尔索引、条件筛选、`loc`、`iloc`、列修改或行选择。

## 验收

- 新章节位于通用 Python 内容与“工程化相关”之间。
- Markdown 标题、代码围栏和表格语法完整。
- 示例列名与参考会话一致。
- 文中准确区分 `Series` 和 `DataFrame`，且没有超出约定范围。
