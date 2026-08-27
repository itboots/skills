# Skills

可复用的 Agent Skills，兼容 Grok、Claude Code、Cursor、Codex 等支持 [Agent Skills](https://agentskills.io) 的客户端。

## 安装

```bash
npx skills add https://github.com/itboots/skills --skill <技能名称>
```

只列出仓库里有哪些技能，不安装：

```bash
npx skills add https://github.com/itboots/skills --list
```

全局安装并跳过确认：

```bash
npx skills add https://github.com/itboots/skills --skill pigdoc -g -y
```

## 可用技能

### pigdoc

搜索 PIG 官方文档（PIG Cloud / pigx / PIG AI）并按文档总结答案。

```bash
npx skills add https://github.com/itboots/skills --skill pigdoc
```

触发示例：`多数据源怎么配置`、`知识库接入 Neo4j`、`pigx 网关路由 404`、`/pigdoc`。

## 仓库结构

```text
skills/
└── <skill-name>/
    ├── SKILL.md          # 必须：YAML frontmatter 含 name、description
    ├── scripts/          # 可选
    └── references/       # 可选
```

每个技能一个目录，目录名与 `SKILL.md` 里的 `name` 一致。

## 许可证

MIT
