# diff-test-risk

面向 AI coding agent 的技能（agents-skills 规范）：对比 git 仓库两个时间点 / commit / 分支 / tag 之间的**代码净差异**，生成面向测试的转测风险报告。

## 功能

- 变更功能点、影响范围（grep 证实的调用方 + 标注【推断】的运行时影响）
- 重点测试模块（高/中/低风险分级，证据落到文件/函数级，不以 commit message 为依据）
- 风险模块提交人（shortlog + 单文件 log 聚合）
- 可选测试用例集分析：重点回归用例 / 需更新用例 / 需补充用例
- 支持**一次分析多个仓库**，合并为一份报告（含跨仓接口契约影响检查）

## 安装

把本目录复制到（任选其一）：

```
~/.agents/skills/diff-test-risk/          # 用户级，所有工作区可用
<项目>/.agents/skills/diff-test-risk/     # 项目级，仅该项目可用
```

## 配置（可选）

在运行目录放 `.test-risk.json`：

```json
{
  "repos": [
    { "name": "lifeos", "path": "D:/ai/code/lifeos",
      "testcases": [{ "path": "qa/testcases.csv" }] },
    { "name": "some-service", "url": "https://github.com/org/some-service" }
  ],
  "report_dir": "test-reports"
}
```

- `repos`：常用仓库清单；用户没点名仓库时默认分析全部；`url` 仓会自动浅克隆重置为完整历史分析
- `repos[].testcases`：该仓的测试用例集（xlsx/csv/md/txt/json，path 相对该仓根）
- `report_dir`：报告输出目录（相对运行目录，默认 `test-reports/`）

## 使用

对 agent 说类似的话即可触发：

- 「对比 lifeos 10月25日和今天的提交，分析影响范围」
- 「对比仓库 HEAD~5 和 HEAD，生成转测风险报告」
- 「分析配置里所有仓库 v1.2 到 v1.3 的变更，出转测报告」（多仓）

## 目录结构

```
diff-test-risk/
├── SKILL.md                            # 工作流（agent 的主指令）
├── references/
│   ├── analysis-guide.md               # 风险分级/影响判定/用例匹配方法
│   └── report-template.md              # 报告模板（单仓/多仓通用）
└── assets/
    └── test-risk.config.example.json   # 配置示例
```
