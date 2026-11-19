# How To Use

日常使用时，可以按下面这个判断来决定信息应该放在哪一层。凡是每次生成单期内容都必须知道的，比如页数、真实性标签、LaTeX 模板不能重构，放在 `AGENTS.md`。凡是某类任务才需要的，比如“先做来源整理再写正文”“最后要跑一次 QA 清单”，放在 `workflows/`。凡是需要 Codex 大量复用的高价值提示词，比如“单期生成器”“LaTeX 精修器”，放在 `prompts/`。凡是结构化输入输出，比如期数 brief、来源记录、分页规划、质检清单，放在 `templates/`。而那些解释为什么这样设计、以后怎么迁移、怎么维护整个流水线的长文，则放到 `docs/`。

推荐的日常流程是这样的：先填写 `templates/episode-brief.md`，明确这一期主题、目标受众、难度、是否偏基础还是偏进阶；再填写 `templates/source-log.md`，把题目来源和知识点来源分开记录；然后用 `templates/page-plan.md` 先做一个 5–6 页的分页草图；接着调用 `prompts/00-master-system-prompt.md` 和 `prompts/02-episode-generator.md` 生成 LaTeX 正文草稿；如果语言风格或版式密度不够稳定，再用 `prompts/04-latex-polisher.md` 做二次精修；最后按 `workflows/review-gate.md` 和 `templates/qa-checklist.md` 收口。

维护节奏上，最实用的方法不是不断加新提示词，而是每次做完一期后，回头看这次哪些地方你重复纠正了两次以上。凡是重复纠正两次以上的约束，就应该升级成长期资产：高频硬约束进 `AGENTS.md`，复用流程进 `workflows/`，结构化信息进 `templates/`。这样这套自动化体系会越来越稳，而不是越来越杂。
