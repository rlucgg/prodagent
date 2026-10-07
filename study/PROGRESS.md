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
  - [x] 第 4 课（借道 `run.py`）：守门员 `Run._transition`，全仓库唯一改 RunState 的地方（`src/kernel/run.py:194-202`）
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

第 5 课：`NodeStatus`——每个步骤自己的状态牌（`src/kernel/types.py:14-21`），对比 RunState 多了 PENDING / SKIPPED。先点评学员对第 4 课思考题（`complete()` 里两行顺序导致非法调用也会改掉 final_output）的回答。

## 学习日志

| 课 | 日期 | 一句话要点 |
|---|---|---|
| 1 | 2026-10-02 | Agent = 想 → 做 → 看结果 → 再想 的循环；框架存在的理由是把内存里隐式的进度变成显式、可存盘的数据。 |
| 2 | 2026-10-02 | 第 1 课答题：存账（messages）+ 待执行的工具调用；补充：循环位置、工具是否已执行（副作用，at-least-once）。RunState 用 StrEnum：防拼错、防非法组合、天然可序列化。 |
| 3 | 2026-10-04 | 第 2 课答题：学员以为"一切皆有可能"；实际 16 种跳法（含原地跳）只放行 5 种。终点 = 空 frozenset（死胡同）；SUSPENDED 不能直接 COMPLETED，必须先复工；SUSPENDED→FAILED 真实路径：同一波一个节点挂起、另一个节点炸了。 |
| 4 | 2026-10-07 | 合并上游 3 个提交（types.py / run.py 未变）。第 3 课思考题学员跳过，给了参考答案：FAILED 不复活，同一张 Plan 新开一个 Run；崩溃 ≠ FAILED，崩溃的 Run 账上还是 RUNNING，可以 resume。第 4 课：`_transition` 先查表后赋值，是全仓库唯一一处 `self.state =`；埋了个思考题：`complete()` 先写 final_output 再查表。 |
