# EviM 评测结果同步

本仓库用于两台服务器交换评测结果，并保留统一汇总报告的版本记录。

- [最新汇总 Word](docs/EviM_三组底座实验与统一资源开销_封尾计时修订版_新增评测数据_20261123.docx)
- [对应 PDF](docs/EviM_三组底座实验与统一资源开销_封尾计时修订版_新增评测数据_20261123.pdf)
- [报告日期与文件校验值](docs/report_manifest.json)
- [已安排评测及状态定义](docs/evaluation_status.json)
- [服务器1本次结果快照](results/server1/latest.json)

## 本次更新：2026年11月26日

补齐 B2、C0 的 StreamingBench 非实时七任务。结合已有实时视觉结果，17任务宏平均分别为 **63.36%**、**60.80%**；B0为61.42%。补入两组完整耗时及显存。C2仅发布已完成的ER、SCU、SD、MCR，MA仍待重试。既有OVOBench和另一服务器提供的成绩保持原值。

准确率先算各子任务，再对任务等权平均。完整范围缺少任一任务时不发布该范围总分；仍待重试的失败题不提前作为最终结果。耗时按成功问题逐问平均，本次新增耗时包含题后清理。

StreamingBench非实时七任务只安排B0、B1、B2、C0、C2、C3。B1未安排实时视觉；B3、C1、C4未安排StreamingBench。C7为基于C3去文字补偿的全量OVOBench消融。

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

S1/S2仅表示明确安排且尚未完成的评测，不是成绩，也不参与平均；缺数据不代表已经安排。A组未安排的任务标为未安排，范围不完整的总分标为未覆盖全量，未开展的独立资源测量标为未测；未支持和未采集分别保留其含义。当前S1只对应C2/C3的StreamingBench后续任务及C7全量OVOBench，S2对应C5/C6全量OVOBench，详见evaluation_status.json。`latest.json`是提交时的核验快照，运行进度不会自动上传；后续执行结果需要再次提交。
