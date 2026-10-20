# 待讨论: 许愿式编程的新想法 (2026-10-20)

## 1. 难点一: 愿望天生模糊, agent 无法判断清晰度

- 人的主观行动特点: 有想法时往往很模糊, 没有想清楚具体怎么做, 需要逐步才清晰
- 有些团队有产品文档, 配合 agent 时可能直接把整个产品文档作为 "愿望" 交给 agent, 希望它按文档全部实现, 这很常见, 也很自然
- agent 拿到愿望后一般直接按指令执行, 这是 agent 和大模型被规定的方式: 听话
- agent 不会强制让人把大目标拆成小目标, 而且 agent 无法判断哪些东西对当前用户来说是明确的, 哪些不明确, 它默认拿到的目标就是明确的

## 2. 难点二: 需要强制的目标拆解机制

- 需要一个机制, 强制 agent 辅助用户先做愿望和目标的拆解, 作为第一步
- 目前做得比较好的是 Matt Pocock (做 TypeScript 教学的人) 出的技能 GrillMe: 让 agent 不断向用户提问, 达到对齐, 过程中目标也连带明确
- 分工判断: GrillMe 适合前半段, 目标不明确时, 持续问到你知道自己要什么, Pstack 的 how/why 没有解决 "我想要的是什么"

## 3. 难点三: 完全自主的团队做法 (Pstack 团队)

- 有些团队不依赖人在旁边监督: 提出特别多的愿望, 可能上千个, 愿望较具体, 直接交给 agent
- agent 可能自己分解, 也可能不分解, 分解后的小愿望全部实现
- 实现过程中没有使用语音, agent 自主完成
- 可能最开始人和 agent 一起把小愿望拆解, 把目标明确, 之后人不再跟, 让 agent 自主消化, 处理, 验证
- 前提: 项目是 agent 可了解的, 所有历史文档, 代码, 聊天 agent 都能拿到, 了解项目背景和演进过程中的决策
- 项目有很好的单元测试和集成测试, 新的小愿望做完后, agent 会写新的端到端测试 (自动或按要求), 自己跑, 根据测试结果判断是否符合最初的明确目标
- agent 自主验证后, 把一步步怎么做, 怎么验的记录成文档或网页 (含截图), 人统一回来一次性快速过一遍, 大多数情况只看截图
- 这引入了图片作为第三种反馈通道, 之前只有文本 (输入和阅读) 和语音 (说和听), 流程因此拆成两段: 前半段语音对齐, 后半段 agent 自主做, 再用图片反馈
- 背景:
  - Pstack 是 Grokbot 团队使用的技能, 团队成员原来在 Cursor, 现在做 Grokbot 产品
  - 作者加入 Cursor 时很多产品已在线上, 他先做性能工作, 需要了解已有项目, 因此做了 how/why 拆分: how 是当前项目怎么跑, why 是项目怎么演进过来的, 有哪些决策点
  - 后来做新需求, 也要先了解 how/why
- 有访谈称 Pstack 团队一个人一个月提交 2500 个 PR

## 4. 两个技能的分工 (我的判断)

- GrillMe: 前半段, 目标不明确时, 持续提问直到对齐
- Pstack: 后半段, 目标已明确后, 驱动 agent 自主的开发, 实施, 验证循环, 产出证据 (截图, 文档) 供人快速批量验收
- Pstack 的 how/why 解决的是 "项目是什么, 怎么来的", 不解决 "我想要什么"

## 5. 演进路径: 从语音结对到 Pstack 式自主

- 最传统的是纯文本输入, 现在已经用上语音
- 问题: 如何从 "语音对话 + agent 结对处理任务" 演进到 "前一段语音, 后一段完全自主" (Pstack 式)
- 建议的演进过程:
  - 一开始纯语音
  - 通过纯语音积累经验, 把测试, 验证, 多段多案, 留证据这些先通过语音一起实现, 跑通 Pstack 相关流程
  - 跑通后把稳定的部分固化, 逐步尝试后续流程
- 例如: 把一个大愿望拆成十个小愿望, 一开始十个都跟语音一起全做完, 之后流程不断固化稳定, 跟语音一起做的只剩前八个, 再到前六个, 能交出去的逐渐往后放, 让 agent 自主完成, 人做最终基于图片的验收
- 最终前半段的分解仍需与语音助手一起做明确界定, 不可能完全自主: 先跟语音 agent 把该聊的聊明白, 聊具体, 之后才可能自主

## 6. 查证结果 (2026-10-20)

- 名字更正: 技能名是 GrillMe (仓库里写作 `grill-me`), 之前记成了 GroomMe, Pstack 中间没有横线 (仓库里写作 `pstack`), 之前记成了 P-Stack, 上面各节已经改过来
- GrillMe:
  - 作者是 Matt Pocock, 做 TypeScript 教学的那位, 之前记成了 "Mate"
  - 仓库: https://github.com/mattpocock/skills, 默认分支 `main`, 约 191 个文件
  - 技能路径: `skills/productivity/grill-me/SKILL.md`, 这个文件只有一行, 作用是调用 `grilling` 技能, 真正的提问逻辑在 `skills/productivity/grilling/SKILL.md`, 工程场景用的版本是 `skills/engineering/grill-with-docs/SKILL.md`
  - README 里说 `/grill-me` 和 `/grill-with-docs` 是他 "most popular skills", 用来 "align with the agent before you get started"
  - wr-fenglei 账号下没有它的 fork
- Pstack:
  - 作者是 Lauren Tan, 网名 poteto
  - 在 Cursor 官方仓库 https://github.com/cursor/plugins, 默认分支 `main`, 整个仓库约 1158 个文件, `pstack/` 下约 172 个
  - 它是一个完整插件, 约 60 个技能, 根目录 `pstack/`, 配置文件 `pstack/.cursor-plugin/plugin.json` (版本 0.15.29)
  - how 和 why 是两个独立技能: `pstack/skills/how/SKILL.md` 讲子系统现在怎么运行, `pstack/skills/why/SKILL.md` 从 git 历史, 工单, 文档和聊天记录里查它为什么这样设计
  - 总入口是 `pstack/skills/poteto-mode/SKILL.md`, 从 23 套做法里选一套
- 2500 个 PR 有出处:
  - poteto 在 X 上发帖说上个月合了 2500 个 PR: https://x.com/i/status/2106134336705843554
  - Matt Pocock 做过直播 "Poteto on shipping 1,000's of PR's a month": https://www.youtube.com/watch?v=MN9dGgmLyso
  - 总结文章说其中很多是维护和代码质量方面的 PR, 新功能不多: https://intelligentartifact.com/posts/how-lauren-tan-ships-thousands-of-prs-a-month/
  - 她早期在 Cursor 先做性能工作, 跟第 3 节记的背景一致
- 同一个 cursor/plugins 仓库里还有 `grok-voice` 插件, 给应用加语音模式, 听写和朗读, 它只是功能组件, 跟文章里 "语音模式是提速的关键" 讲的不是一回事, 关系不大

## 7. 探索阶段的前置验证 (2026-10-20 通话补充)

- 实现阶段的验证针对新功能, 有完成标准可以对照. 探索 (research) 阶段要验证的是 agent 对现有系统行为的理解是否正确, 这一步容易被忽略
- 对项目不熟, 文档可能出错或缺失, 资料零散的时候, agent 探索后给出的结论需要验证, 只靠人的经验判断不够
- 验证思路: 让 agent 把相关功能真正跑一遍, 可以加日志或断点, 观察实际执行行为和它梳理的逻辑是否一致, 复现当前逻辑, 验证和梳理结果是否一致, 这就是前置验证. 这一步验证的是现有系统的行为, 做法和后面对实现的验证一样
- 如果项目有良好的文档, 单测和历史功能的回归测试集, 已有测试就是现成的参照. 如果没有, 大愿望拆成小愿望后, 前几个愿望如果是 research (梳理现状, 明确愿望是什么), research 结束后, 也要让 agent 跑一遍验证流程
- 如果跑出来和梳理不一致, 差异本身就是信号: 让 agent 重新研究差异的原因, 修正 research 结果, 再重新跑整体流程, 确认一致, 形成迭代闭环
