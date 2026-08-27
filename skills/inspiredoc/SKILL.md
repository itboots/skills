---
name: inspiredoc
description: 搜索 PIG 官方文档并总结答案。当用户询问 PIG CLOUD、PIG AI、pigx 相关的技术问题、配置方法、功能说明，或明确说“查文档”、“搜一下”、“怎么配置”、“怎么实现”时触发。遇到 PIG 框架相关具体问题时应主动搜索文档后再回答，不要依赖训练数据中可能过期的信息。Use when the user runs /inspiredoc.
---

# PIG 文档搜索

PIG 官方文档有两个检索索引：

- `pig`：PIG Cloud、pigx、网关、权限、微服务、注册中心、代码生成
- `ai`：PIG AI、知识库、RAG、模型、向量库、Embedding、工作流、智能体、图谱、Neo4j

先选索引，再搜索，再按命中结果总结。不要凭记忆回答框架细节。

## 步骤

### 1. 提取 1–3 个关键词

优先中文。去掉口语词，保留模块名和报错特征。

- “多数据源怎么配置” → `多数据源`
- “oauth2 登录失败” → `OAuth2 登录`
- “网关路由 404” → `网关路由`

### 2. 选择索引

- 只涉及 AI 能力 → `ai`
- 只涉及 Cloud / pigx → `pig`
- 两边都沾，或问题本身含糊（如“怎么配置模型”）→ 两个都查

### 3. 调用搜索接口

```bash
curl -s -X POST "https://search.pig4cloud.com/indexes/{index}/search" \
  -H "Content-Type: application/json" \
  -d '{"q": "关键词", "limit": 3}'
```

双查时分别请求 `pig` 和 `ai`，再合并。

响应字段：

| 字段 | 含义 |
|---|---|
| `hits[].lvl0` | 文档标题 |
| `hits[].content` | 正文（Markdown） |
| `hits[].url` | 文档路径 |
| `hits[].lvl2` | 更新时间 |

### 4. 命中差时换词再搜一次

第一次命中少于 2 条，或标题/正文明显不相关时，在同一索引换一个更短或更官方的词再搜，合并后总结。

### 5. 按固定格式回答

**根据 PIG 官方文档，关于「用户问题」：**

**解答：**
综合命中文档给出步骤或说明。保留代码、XML/YAML 和关键注意项。多篇说同一件事时合并，不重复。

**参考文档：**
- [标题](链接) — 一句话说明这篇的作用

链接必须按索引拼：

- `pig`：`https://docs.pig4cloud.com/pig#{url}`
- `ai`：`https://docs.pig4cloud.com/ai#{url}`

双查时先去重。只命中一侧就只基于那一侧回答，并说明来源是 `pig`、`ai` 还是两边。

`hits` 为空时明确说官方文档暂无相关内容，不要编造接口或配置项。关联度低时先说明，再补充已知知识。
