# EviM 评测结果同步

本仓库用于两台服务器交换评测结果，并保留统一汇总报告的版本记录。

- [最新汇总 Word](docs/EviM_三组底座实验与统一资源开销_封尾计时修订版_新增评测数据_20261123.docx)
- [对应 PDF](docs/EviM_三组底座实验与统一资源开销_封尾计时修订版_新增评测数据_20261123.pdf)
- [报告日期与文件校验值](docs/report_manifest.json)
- [已安排评测及状态定义](docs/evaluation_status.json)
- [服务器1本次结果快照](results/server1/latest.json)

## 本次更新：2026年11月27日

补齐 B2、C0 的 StreamingBench 非实时七任务，并保留 C2 的完整结果。C7（基于 C3、去除事件文字补偿的 OVOBench 消融）正在运行，当前已尝试 1937/3035 题，尚未发布部分准确率。B1 Stage3 EviM 的 StreamingBench 实时视觉 2500 题已经接在 C7 之后排队，配置快照与依赖状态已同步；既有 OVOBench 成绩保持原值。

服务器2本次合入：C5全量OVOBench已完成3035/3035、答对1638题、12任务宏平均57.23%、失败0；C6在2026-11-27T11:13:38+08:00快照为819/3035、答对467题、失败0。C5的新增结果与对应分析已写入Word/PDF并用蓝色标记；C6仍标S2。\n\n准确率先算各子任务，再对任务等权平均。完整范围缺少任一任务时不发布该范围总分；仍待重试的失败题不提前作为最终结果。耗时按成功问题逐问平均，本次新增耗时包含题后清理。

StreamingBench非实时七任务已完成或安排 B0、B1、B2、C0、C2、C3；B1 的实时视觉 2500 题已明确排队，依赖 C7 完成。B3、C1、C4 未安排 StreamingBench。C7 是基于 C3 去文字补偿的全量 OVOBench 消融。

## 两台服务器如何同步

首次获取：

```bash
git clone git@github.com:Camellia-zsc/evaluation-handoff.git
cd evaluation-handoff
```

每次更新前执行 `git pull --rebase`。服务器1维护 `results/server1/`，服务器2维护 `results/server2/`；提交各自目录，避免互相覆盖。结果至少记录实验编号、底座/配置、数据集、固定题单与分母、已完成/排队状态、分任务正确数、准确率与资源统计口径。

```bash
git pull --rebase
# 服务器1写入自己的结果；服务器2将 server1 改为 server2。
git add results/server1/
git commit -m "Update server1 evaluation results"
git push origin main
```

汇总报告统一放在 `docs/`，本次由服务器1更新。先拉取两台服务器的最新结果，再由一台服务器更新Word、PDF及report_manifest.json并提交；Word是二进制文件，不同时编辑后自动合并。服务器2后续成绩按现有C5/C6等编号填写，不复用或重命名其他实验编号。

S1/S2仅表示明确安排且尚未完成的评测，不是成绩，也不参与平均；缺数据不代表已经安排。A组未安排的任务标为未安排，范围不完整的总分标为未覆盖全量，未开展的独立资源测量标为未测；未支持和未采集分别保留其含义。当前 S1 包含 C7 的全量 OVOBench 消融及其之后的 B1 实时视觉 2500 题，S2 对应 C5/C6 全量 OVOBench，详见 evaluation_status.json。`latest.json` 是本次提交的核验快照；B1 的排队配置另见 `results/server1/pending_b1_streamingbench_rt_20261127.json`。完成后需再次提交最终成绩。
