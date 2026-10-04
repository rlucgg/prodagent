# 学习进度

规则见根目录 `CLAUDE.md`。路线按作者推荐的阅读顺序（`src/__init__.py` 文件头）：
先热身建立直觉，再按 types → command → channels → graph → body → run → eventlog → bus/ports → scheduler
读内核，最后看应用层怎么用同一套原语拼出 ReAct 和多 Agent。每章拆成若干课，每课 ≤10 行代码。

## 课程表

- [ ] 第 0 章 热身：Agent 的真面目
  - [x] 第 1 课：Agent 就是一个 while 循环，问题出在"进度藏在内存里"（`docs/zh/architecture.md:11-18`）
- [ ] 第 1 章 `types.py`：用枚举把状态写死（NodeStatus、RunState、合法转移表、ToolCall/ToolResult）
  - [x] 第 2 课：`RunState` 四块状态牌，SUSPENDED = 主动放手并已落盘（`src/kernel/types.py:24-30`）
  - [x] 第 3 课：合法转移表 `_ALLOWED_TRANSITIONS`，16 种跳法只放行 5 种（`src/kernel/types.py:36-41`）
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

第 4 课：守门员 `Run._transition`——转移表由谁执法、非法跳转怎么当场炸（`src/kernel/run.py:197-202`）。先点评学员对第 3 课思考题（终点为什么不能复活、想重跑怎么办）的回答。

## 学习日志

| 课 | 日期 | 一句话要点 |
|---|---|---|
| 1 | 2026-10-02 | Agent = 想 → 做 → 看结果 → 再想 的循环；框架存在的理由是把内存里隐式的进度变成显式、可存盘的数据。 |
| 2 | 2026-10-02 | 第 1 课答题：存账（messages）+ 待执行的工具调用；补充：循环位置、工具是否已执行（副作用，at-least-once）。RunState 用 StrEnum：防拼错、防非法组合、天然可序列化。 |
| 3 | 2026-10-04 | 第 2 课答题：学员以为"一切皆有可能"；实际 16 种跳法（含原地跳）只放行 5 种。终点 = 空 frozenset（死胡同）；SUSPENDED 不能直接 COMPLETED，必须先复工；SUSPENDED→FAILED 真实路径：同一波一个节点挂起、另一个节点炸了。 |
