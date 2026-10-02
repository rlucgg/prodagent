# 学习进度

规则见根目录 `CLAUDE.md`。路线按作者推荐的阅读顺序（`src/__init__.py` 文件头）：
先热身建立直觉，再按 types → command → channels → graph → body → run → eventlog → bus/ports → scheduler
读内核，最后看应用层怎么用同一套原语拼出 ReAct 和多 Agent。每章拆成若干课，每课 ≤10 行代码。

## 课程表

- [ ] 第 0 章 热身：Agent 的真面目
  - [x] 第 1 课：Agent 就是一个 while 循环，问题出在"进度藏在内存里"（`docs/zh/architecture.md:11-18`）
- [ ] 第 1 章 `types.py`：用枚举把状态写死（NodeStatus、RunState、合法转移表、ToolCall/ToolResult）
- [ ] 第 2 章 `command.py`：节点跟调度器说话的唯一词汇（Goto、Send）
- [ ] 第 3 章 `channels.py`：并发写同一个键怎么办（reducer、Channel、WaveWrites、AmbiguousWrite）
- [ ] 第 4 章 `graph.py`：静态蓝图（Node、Edge、Plan、`ready()`、`sweep_skipped()`）
- [ ] 第 5 章 `body.py`：唯一的可组合接口（Outcome、coerce_outcome、NodeContext、四种 body）
- [ ] 第 6 章 `run.py`：一次执行（Run 状态机、`apply()` 唯一咽喉、Interrupt）
- [ ] 第 7 章 `eventlog.py` + `replay.py`：事件是真相，状态是折叠出来的
- [ ] 第 8 章 `bus.py` + `ports.py`：对外旁观 / 裁决 / 订阅，可替换端口
- [ ] 第 9 章 `scheduler.py`：波次引擎（drive、屏障提交、挂起与恢复）
- [ ] 第 10 章 应用层：`runtime/react.py`、`agent.py`、`workflow.py`——ReAct 是拼出来的
- [ ] 第 11 章 多 Agent：委派（子 Run）、交接（Goto）、黑板
- [ ] 第 12 章 收官：trace、blob、回头对照 LangGraph / ADK

## 下一课

第 2 课：框架的第一刀——把"跑到哪了"写成显式的枚举（`src/kernel/types.py` 的 `RunState`）。

## 学习日志

| 课 | 日期 | 一句话要点 |
|---|---|---|
| 1 | 2026-10-02 | Agent = 想 → 做 → 看结果 → 再想 的循环；框架存在的理由是把内存里隐式的进度变成显式、可存盘的数据。 |
