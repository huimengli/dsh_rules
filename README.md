# dsh_rules

给 DeepSeek Harness (DSH) 使用的 Agent Rules 集合。

本仓库只存放 Markdown 规则文件，不是 DSH 插件，不需要 `dsh plugin add`。  
把规则放到 DSH 能读取的位置后，新开会话即可生效。

## 目录结构

```text
dsh_rules/
├── agent_rules/
│   ├── AGENTS.md
│   ├── code-style.md
│   ├── git.md
│   └── ...
└── README.md
