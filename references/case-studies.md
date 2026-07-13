# Notion 产品设计案例库

本文整理 Notion 在协作、信息架构、检索、性能与 AI 上的真实产品案例，供“跨页面共享内容并同步编辑”等设计任务复用。事实以 Notion 官方发布说明、帮助中心和工程/设计文章为准；“可迁移原则”是基于案例的设计推导，不代表 Notion 官方原话。

## 目录

- [1. Comments：把异步讨论留在内容旁边（2022）](#1-comments把异步讨论留在内容旁边2022)
- [2. Synced Blocks：复制内容，还是复制引用（2021 → 2026）](#2-synced-blocks复制内容还是复制引用2021--2026)
- [3. 页面间距与邻接规则：用关系规则替代逐块特判（2026）](#3-页面间距与邻接规则用关系规则替代逐块特判2026)
- [4. Teamspaces 与侧边栏：规模增长后仍让每个人只看相关内容](#4-teamspaces-与侧边栏规模增长后仍让每个人只看相关内容)
- [5. Search 与 Command Search：从页面导航到跨应用召回](#5-search-与-command-search从页面导航到跨应用召回)
- [6. Android 启动性能：把“速度感”定义成可测量的用户旅程](#6-android-启动性能把速度感定义成可测量的用户旅程)
- [7. Contextual AI：让 AI 成为横向能力，而非孤立聊天框](#7-contextual-ai让-ai-成为横向能力而非孤立聊天框)
- [8. 跨案例结论](#8-跨案例结论)

---

## 1. Comments：把异步讨论留在内容旁边（2022）

> 时间说明：评论体验的主体重设计发布于 2021 年 Notion 2.12；这里的“Comments 2022”重点指 2022 年在此基础上补上的表态与跨页面引用能力。

### 问题

团队的反馈容易散落在聊天、邮件和会议里；即使评论已经进入文档，如果所有讨论都平铺在正文旁，也会压迫阅读空间。仅有文字回复也不足以表达“已阅”“同意”“已完成”等轻量协作信号，更难在另一页引用一段已有讨论及其上下文。

### 关键设计动作

- 2021 年 Notion 2.12 将一页内的评论线程集中到右侧评论栏，同时保留锚定到正文位置的能力。
- 已结束的线程可 `Resolve`，并可筛选查看已解决评论，使历史决策可追溯而不持续占据注意力。
- 2022 年 8 月加入评论 emoji reaction，把低成本确认从“再写一条回复”变成一次点击。
- 同期加入 `Copy link to discussion`；粘贴后可选择 `Paste as preview` 或 `Paste as mention`，预览包含作者、时间和所在页面等上下文。
- 当前帮助中心还提供页面级讨论、文字/块级评论、评论面板、按人员或状态筛选，以及 `Default` / `Minimal` 两种页内评论呈现密度。

### 给用户的感觉

讨论“贴着工作发生”，但不会永远盖住工作本身。小反馈可以很轻，重要讨论又能被保存、恢复和跨页引用。

### 信号 / 机制

- 正文旁的评论标记说明讨论锚点。
- 右侧面板聚合一页内全部线程。
- 未读红点、@mention 和 Inbox 把“内容存在”转化为“有人需要你处理”。
- `Resolve` 代表当前任务状态，而不是删除历史。
- comment preview 通过作者、时间、来源页维持引用上下文，避免只复制一句脱离语境的话。

### 权衡

- 聚合面板提升可扫描性，但用户可能看见讨论却失去它与正文位置的空间关系，因此点击评论必须能回到锚点。
- reaction 降低回应成本，也可能把需要明确结论的讨论简化成含义模糊的 emoji。
- `Minimal` 降低页面噪声，但会降低评论可发现性。
- 跨页 comment preview 保留了来源身份，却引入来源页权限、链接失效与可见性问题。

### 可迁移原则

1. **把协作状态附着于内容对象，而不是另建脱离内容的消息系统。**
2. **高频、低风险动作应一击完成；高语义动作仍需明确文字。** reaction 适合“已阅”，不适合代替审批结论。
3. **“解决”与“删除”必须分开。** 前者改变工作状态，后者改变历史记录。
4. **跨页面复用时应复制引用而非剥离上下文。** 预览必须带来源、作者和时间。

### 来源 URL

- Notion 2.12, now with better comments（2021-09-21）：https://www.notion.com/releases/2021-09-21
- Comment reactions, progress bars & more（2022-08-11）：https://www.notion.com/releases/2022-08-11
- Comments & mentions（当前帮助中心）：https://www.notion.com/help/comments-mentions-and-reminders

---

## 2. Synced Blocks：复制内容，还是复制引用（2021 → 2026）

### 问题

同一段信息常需出现在团队主页、会议记录、入职文档或不同 teamspace 中。普通复制会立即产生独立副本；内容变化后，维护者必须逐页寻找并更新，最终出现多个互相矛盾的“真相”。另一方面，如果所有复制都自动同步，用户又会因一次本地修改意外影响多页。

### 关键设计动作

#### 2021 年发布行为：在粘贴时做语义分流

- 2021 年 6 月 29 日，Notion 2.11 发布 Synced Blocks。
- 官方首发流程是：复制内容，在另一页粘贴，然后在出现的下拉菜单中选择 `Paste & sync`。
- 选了同步后，对任一 Synced Block 内容或格式的修改都会自动更新其他副本。
- 发布示例包括：把 OKR 同步到团队主页、周会记录和 1:1 页面。

#### 2026 年当前行为：先创建共享对象，再复制其实例

- 当前帮助中心的主流程是：选中一个或多个 block，打开 `⋮⋮`，选择 `Turn into` → `Synced block`，再点复制图标并粘贴到页面、同页其他位置或其他 workspace。
- 编辑任一实例会更新所有实例；编辑时 block 周围出现边框，提示它不是普通本地内容。
- 同步块顶部显示 `Editing in ↙ # other pages`，可查看并跳转到其他实例；创建它的页面带 `ORIGINAL` 标记。
- 若用户无权访问 original block 所在页面，则看不到同步块内容，并可请求访问。编辑同步副本也要求对 original block 有编辑权限。
- `Unsync` 只断开一个副本；`Unsync all` 断开全部实例，使各 block 变成独立内容。
- 当前帮助中心特别说明：若同步块超过 10 个副本，执行 `Unsync all` 或删除 original 会移除所有副本；删除 original 后再 Undo 也不能恢复这些副本。

### 为什么“普通复制”与“同步复制”并不违背直觉

关键不是让用户记住两个长得一样的命令，而是维持两个早已存在、彼此不同的对象直觉：

- **普通复制承诺“从现在开始独立”。** 用户复制一段草稿、模板或旧方案，通常是为了改出新版本；粘贴完成后不期待原文继续控制副本。
- **同步复制承诺“仍是同一个内容对象的另一个位置”。** 用户复制政策、使命宣言、导航块或 OKR，是为了多处展示同一真相，而不是制造新版本。
- Notion 没有偷偷改变 `Copy` / `Paste` 的历史语义。2021 年把 `Paste & sync` 作为粘贴后的显式选择；当前流程则要求先 `Turn into → Synced block`。两条路径都在建立共享身份之前安排了一次明确承诺。
- 同步后的红/强调边框、其他页面数量和 `ORIGINAL` 是持续的身份信号；它们让后续编辑者即使没参与最初创建，也能意识到影响范围。
- `Unsync` 提供可逆出口：用户可以把“同一对象的一个实例”重新变成“独立副本”。这进一步巩固了两种语义，而不是混淆它们。

因此，双选项并不违背直觉；真正违背直觉的是**普通粘贴被默认远程联动**，或者同步关系建立后在编辑点完全不可见。

### 给用户的感觉

常用信息像一个可复用组件：放到哪里都保持最新；但是否建立共享关系由用户显式决定。进入编辑时，系统会提醒“这里的修改不只发生在这里”。

### 信号 / 机制

- 建立关系前：`Paste & sync` 或 `Turn into → Synced block`。
- 关系存在时：编辑边框、复制图标、其他页面计数、`ORIGINAL`。
- 权限不足时：不展示内容，提供请求访问路径。
- 退出关系时：`Unsync` / `Unsync all`，而不是模糊的“复制”。
- 后台机制：多个页面位置引用同一共享 block 身份，而不是各自保存一份需要广播覆盖的文本副本。

### 权衡

- 默认普通复制更安全，却让同步能力较难发现；默认同步更高效，却会制造隐性副作用。Notion 选择显式建立同步。
- `ORIGINAL` 帮助解释来源和权限，却可能让用户误以为其他实例是低一等的镜像；实际上任一可编辑实例都会改动共享对象。
- 原页权限守住了单一访问边界，但会出现“目标页可访问、嵌入内容不可见”的断裂体验。
- 跨 workspace 复用扩展能力边界，也让成员、ACL、离职交接与删除后果更复杂。
- 超过 10 个副本时的删除/撤销限制是高风险边界，说明“删除一个放置位置”和“删除共享对象”若不分开，规模越大越危险。

### 可迁移原则

1. **复制值与复制引用必须是两个显式动作。** 默认复制值，只有用户明确承诺时才建立同步。
2. **在副作用发生点显示影响范围。** 创建时提示一次不够；每次进入编辑或删除状态都应重新提示。
3. **共享对象必须有稳定身份、来源入口和反向实例列表。**
4. **“移除当前实例”“断开同步”“删除所有实例”是三个不同意图。** 不应压缩成一个 `Delete`。
5. **权限应跟共享身份走，并在目标位置解释缺失原因。** 不能用无提示空白掩盖 ACL 冲突。
6. **同步关系要可逆，但批量破坏性动作需要更强确认与恢复机制。**

### 来源 URL

- Notion 2.11, now with Synced Blocks（2021-06-29）：https://www.notion.com/releases/2021-06-29
- Synced blocks（2026 当前帮助中心行为）：https://www.notion.com/help/synced-blocks
- Move & duplicate content（普通复制在跨 workspace 后仍保留原内容）：https://www.notion.com/help/transfer-content-to-another-account

---

## 3. 页面间距与邻接规则：用关系规则替代逐块特判（2026）

### 问题

Notion 的页面由多年累积的不同 block 类型组成。若每种 block 各自定义 margin、padding 与行高，单个组件看起来合理，混排后却会失去统一节奏。简单地给所有 block 相同间距也不行：段落需要呼吸感，列表项却需要紧密成组。

### 关键设计动作

- 2026 年 3 月，产品设计师 Paul Velleux 公开了页面设计更新。
- 团队没有强求传统基线网格，因为浏览器、分辨率、操作系统、用户偏好和可变高度 block 使严格基线对齐不现实；改用标准化 spacing 建立阅读节奏。
- 团队增大段落间距，使段落边界更易扫描。
- 对列表不使用统一大间距，而是引入 adjacency rules：block 会检查上、下相邻对象。
- checklist、bulleted list、toggle list、numbered list 和页面列表都被视作“list”；当相邻对象也是 list item 时，减少 padding，使它们视觉成组。
- 该邻接系统被视为基础能力，不只服务于 spacing，随后覆盖列表、标题和混合内容页面。

### 给用户的感觉

用户不需要设置每个 margin，也能感到段落有呼吸、列表是一个整体、混排页面稳定而自然。规则存在，但工具退到背景里。

### 信号 / 机制

- 段落之间更大的垂直空间提供语义断点。
- 同类列表项之间更紧的 padding 提供“属于同一组”的 Gestalt 邻近信号。
- block 不只读取自身类型，也读取邻居类型后决定最终样式。
- 统一规则由系统自动执行，用户不需要看到额外控制面板。

### 权衡

- 上下文规则比固定 token 更符合阅读语义，但测试矩阵显著扩大：每种 block 都可能与多种邻居组合。
- 自动邻接减少手工控制，也可能让希望精确排版的用户无法覆盖系统判断。
- 规则越“聪明”，越要保证拖拽、类型转换、嵌套和跨端渲染后结果一致，否则会产生不可预测跳动。

### 可迁移原则

1. **复杂组合界面不要只设计组件，还要设计组件之间的关系。**
2. **用少量可解释的邻接规则替代大量局部像素特判。**
3. **视觉距离可以编码语义：紧表示成组，松表示新段落。**
4. **自动规则应覆盖 90% 常态，并为异常组合建立系统测试，而不是把复杂度推给用户。**
5. **“工具退后”不是没有机制，而是机制在用户无需操作时自动给出正确结果。**

### 来源 URL

- Updating the design of Notion pages（Paul Velleux，2026-03-18）：https://www.notion.com/blog/updating-the-design-of-notion-pages

---

## 4. Teamspaces 与侧边栏：规模增长后仍让每个人只看相关内容

### 问题

小团队可以把所有页面放在一个共享侧边栏；当公司增长到数百或数千人时，同一结构会变成长列表。全员可见提升透明度，却增加搜索和导航噪声；完全拆成多个 workspace 又会破坏跨团队链接、共享和检索。

### 关键设计动作

- 2022 年 3 月，Notion 在 Block by Block 公布 Teams 侧边栏方向；同年 8 月以 Notion 2.18 正式发布 teamspaces。
- Teamspace 被设计成 workspace 内的“mini-workspace”，可对应部门、办公室、长期小组或跨职能项目。
- 用户通过 `All teamspaces` 浏览和加入相关空间，只把自己加入的 teamspace 放进侧边栏；可离开或隐藏不相关空间。
- teamspace 可拥有自己的成员、所有者、安全设置和权限；公开、封闭、私有提供不同的发现与加入边界。
- 页面链接、搜索和分享仍可跨 teamspace 工作，避免组织结构变成信息孤岛。
- 2024 年 Notion 2.40 继续清理侧边栏：`Private` 和 `Shared` 进入专门面板，可排序、搜索，并限制默认展示数量，避免无限滚动。

### 给用户的感觉

公司知识仍在同一栋“数字办公室”里，但每个人的入口只突出与自己相关的楼层和房间；必要时仍能跨团队寻找信息。

### 信号 / 机制

- 侧边栏章节和嵌套表达组织归属。
- `All teamspaces` 提供全局发现，已加入列表提供个人日常视图。
- 锁图标、成员资格和可见性设置表达访问边界。
- Favorites 脱离组织层级，提供个人高频入口。
- teamspace filter 将全局 Search 收窄到组织上下文。

### 权衡

- 用户定制侧边栏降低噪声，但两位同事看到的结构不再相同，口头指路更困难。
- teamspace 增强组织边界，却可能被误当成必须与组织架构一一对应；临时项目和兴趣组需要同等支持。
- 公共内容可搜索但不常驻侧边栏，有利于聚焦，却降低偶然发现。
- 私有 teamspace 保护敏感信息，也要求跨页同步内容严格遵守来源权限，避免从公开页泄露内容或标题。

### 可迁移原则

1. **规模化不是把同一导航树做得更深，而是把全局组织与个人视图分离。**
2. **组织边界不应阻断引用、搜索和分享；边界负责权限，检索负责连通。**
3. **“加入”“收藏”“权限”是三种不同关系：归属、高频和访问不可混为一谈。**
4. **跨页面共享内容要能跨组织区出现，但必须保留来源 ACL 和归属信号。**

### 来源 URL

- New sidebar design will help Notion scale to fit even the biggest companies（2022-03-02）：https://www.notion.com/blog/new-sidebar-design
- Notion 2.18, now with teamspaces（2022-08-29）：https://www.notion.com/releases/2022-08-29
- Structure your sidebar for more focused work with teamspaces：https://www.notion.com/help/guides/structure-sidebar-focused-work-teamspaces
- Notion 2.40: Home with calendar, navigate content, and organize your sidebar（2024-06-11）：https://www.notion.com/releases/2024-06-11

---

## 5. Search 与 Command Search：从页面导航到跨应用召回

### 问题

当内容层级增多、teamspace 扩张、用户不记得页面路径时，侧边栏无法承担全部导航。传统搜索若只按标题或字面匹配，会让热门、最近和真正相关的结果混在一起；而用户身处其他应用时，切回 Notion 本身也是一次上下文切换。

### 关键设计动作

- 2022 年 Notion 2.18 将 `Quick find` 改名为更直白的 `Search`，并改进排序与过滤。
- 当前 workspace Search 位于侧边栏顶部，可用 `cmd/ctrl + P` 打开；光标不在 block 中时也可用 `cmd/ctrl + K`。
- 空查询先显示最近访问页面；结果可带 `Most viewed`、`Popular this week` 等社会/行为信号。
- `Best Matches` 综合相关性与最近编辑时间，并提高标题匹配权重；用户也可按编辑/创建时间排序。
- 过滤器包括 `Title only`、创建者、Teamspace、父级范围和日期。
- Desktop 的 Command Search 可从 Notion 应用外通过自定义快捷键、Mac 菜单栏或 Windows 任务栏唤起，不必先把 Notion 窗口切到前台。
- Command Search 同时承载传统检索与 Ask Notion AI；结果悬停后可快速复制链接。
- 当前官方说明仍明确列出边界：workspace Search 不索引 comments/discussions，部分 @mention 和数据库属性值也有限制；数据库内搜索与 workspace Search 的索引范围不同。

### 给用户的感觉

用户不必记住“内容放在哪个树枝上”，只要记得一个词、一个人或最近使用情境就能回到目标；在其他应用里也能像调用系统命令一样找 Notion 内容。

### 信号 / 机制

- 最近访问解决“我刚才在哪”的回返任务。
- 标题权重、最近编辑、浏览热度共同提供相关性排序。
- filter chips 把模糊召回变成逐步收窄。
- Command Search 的全局快捷键把能力从一个页面入口提升为操作系统级入口。
- AI 作为同一检索入口的另一种模式，处理“我不知道关键词，只知道问题”的情形。

### 权衡

- 行为排序提升个人相关性，也可能把旧但权威的文档压到后面。
- `Most viewed` 与 `Popular this week` 帮助判断，却可能制造从众偏差。
- Search、数据库搜索、页面内 `cmd/ctrl + F`、Command Search 和 AI Search 的边界需要清晰命名，否则用户只会觉得“同一个词有时能搜到、有时搜不到”。
- 全局快捷键降低切换成本，但默认开机启动、菜单栏常驻和快捷键冲突必须允许关闭或自定义。

### 可迁移原则

1. **检索是信息架构的安全网，但不能代替清晰结构。**
2. **默认排序应利用情境信号，同时给用户可见的收窄和排序控制。**
3. **不同索引范围必须说明边界；“零结果”应告诉用户没搜什么，而不只是说没找到。**
4. **高频能力可以突破应用窗口，但要尊重系统资源、快捷键和用户关闭权。**
5. **关键词搜索与语义问答应共用入口、区分结果形态。**

### 来源 URL

- Notion 2.18, now with teamspaces（含 Quick find → Search）：https://www.notion.com/releases/2022-08-29
- Search in your workspace（当前 Search、Command Search、过滤和限制）：https://www.notion.com/help/search
- Notion for desktop（Command Search 系统级入口）：https://www.notion.com/help/notion-for-desktop

---

## 6. Android 启动性能：把“速度感”定义成可测量的用户旅程

### 问题

Notion 原移动端曾是打开 WebView 的简单包装。复杂 block 数据、会话、实验配置、日志和依赖初始化挤在启动路径上，导致用户想记一条想法、创建任务或回复评论时先等待。平均值还会掩盖低端设备和慢会话的真实体验。

### 关键设计动作

- 2020 年起，团队将高可见、高交互的早期体验从 WebView 迁往更多 native code。
- 2021 年加强原生基础设施，支持 block 数据查询、缓存和实时更新。
- 2022 年推出 native Home Tab，使感知启动性能改善 3 倍；2023 年初 native Search Tab 的加载时间改善超过 80%。
- 团队把用户首次看到 Home 内容的旅程定义为 `Initial Home Render`，主要关注生产会话的 P95，而不是只看实验室平均值。
- 通过细分 span、Android Studio CPU Profiling、Perfetto trace 和 flamegraph 找到三类瓶颈：等待依赖、串行/阻塞操作、主线程占用。
- 具体动作包括缓存实验配置、缓冲 analytics/logging、缓存 session、只在真正需要时解析 migration JSON、把 JSON 序列化移到 worker thread。
- 为 Jetpack Compose 加入 Baseline Profiles，用 UIAutomator/JUnit 定义真实启动与滚动旅程；生成的 profile 随 release 包进入 APK/AAB，带来约 12% 的 P95 `Initial Home Render` 改善。
- PR 合并后用 Macrobenchmark、Trace markers 和生产周报持续防回归。到 2024 年文章发布时，Android 启动速度相较 2023 年初提高超过一倍。

### 给用户的感觉

不是看到一张更早出现的启动图，而是更快进入可阅读、可滚动、可操作的 Home；用户的想法不必等应用准备好。

### 信号 / 机制

- `initial_home_render` 把“快”绑定到用户看到内容，而非进程启动完成。
- P95 代表绝大多数用户的较差体验，防止平均值美化结果。
- trace span 把一次启动拆成可定位的责任单元。
- 缓存、buffer、后台线程把非关键工作移出 critical path。
- release 级 Baseline Profiles 与 Macrobenchmark 让优化可重复并持续生效。

### 权衡

- 缓存实验配置和 session 提升速度，但要接受短时间内不是最新值，并设计一致性与 exposure 规则。
- 延后日志处理降低启动成本，却要求磁盘持久化和补发机制，避免崩溃时丢失关键诊断信息。
- 原生化提升性能，也增加 Web 与 native 双层状态、IPC 和跨平台维护成本。
- P95 更接近弱设备用户，但单一指标仍不能代替首屏正确性、滚动流畅度和可交互时间。

### 可迁移原则

1. **性能指标应命名为用户完成的可感知里程碑，而不是内部生命周期事件。**
2. **优先优化尾部体验；平均快不代表大多数人不被卡住。**
3. **启动关键路径只保留首屏必需工作，其他工作缓存、延后、并行或缓冲。**
4. **性能改进必须进入每次 release 的自动验证，否则会被后续功能逐步吃掉。**
5. **“感觉更快”可以来自正确的呈现顺序，但不能用骨架屏掩盖不可交互。**

### 来源 URL

- Notion on Android is now more than twice as fast to launch（Karn Saheb，2024-05-01）：https://www.notion.com/blog/notion-on-android-is-now-more-than-twice-as-fast-to-launch

---

## 7. Contextual AI：让 AI 成为横向能力，而非孤立聊天框

### 问题

一个空白聊天框要求用户自己说明正在写什么、选中了什么、页面属于谁以及希望完成什么。它虽然通用，却把产品已有上下文全部丢给用户重复输入；固定 AI 工具栏则会在所有状态展示同一组动作，造成噪声和低相关建议。

### 关键设计动作

- 2023 年，Notion 产品设计师 Ryo Lu 将 Notion AI 定义为横向 layer，而不是独立的垂直功能；AI 要进入 block、页面、数据库以及常见 Wiki、Docs、Projects 工作流。
- 系统读取当前产品状态来排序工具：新空白页按 `Space` 时优先 drafting/brainstorm；已有内容页的新 block 中按 `Space` 时优先 `Continue writing`、`Summarize`、`Find action items`。
- 选中文本时，优先显示改写、调整语气等作用于当前 selection 的能力。
- 预置 skill 降低普通用户的 prompt 成本，同时保留自由提示词，服务希望自定义工作流的 toolmaker。
- AI block 可进入模板；Summary、Action items 或 Custom AI block 能带着特定页面上下文重复运行。
- AI favorites 允许用户保存、命名并一击复用自定义 prompt。
- 后续产品继续扩大上下文范围：workspace、connected apps 与 web search；2025 年 Notion 3.0 又把 Agent 从回答问题扩展为可创建页面、构建数据库和执行多步工作的行动者。

### 给用户的感觉

AI 不是让用户离开当前工作去“问机器人”，而是在恰当位置先理解眼前材料，再给出最可能需要的动作；简单需求一击完成，复杂需求仍能继续对话和定制。

### 信号 / 机制

- 页面是否为空、当前 block 类型、是否有 selection、所在 workflow 决定推荐动作顺序。
- 预置 skill 命名具体结果，如 `Summarize`、`Find action items`，而非抽象地写“使用 AI”。
- AI 输出仍落回 block 和数据库等既有对象，保持可编辑、可组合。
- follow-up 提供渐进式控制：先给快速默认结果，不满意再加约束。
- workspace / connector 权限决定 AI 能使用哪些来源；回答与动作需要来源和权限边界。

### 权衡

- 情境推荐降低 prompt 成本，也可能过早假定用户意图；自由输入和其他动作入口必须仍可达。
- 使用更广上下文提升答案质量，同时扩大隐私、权限、时效性和来源解释责任。
- AI 作为横向 layer 能保持工作流连续，却可能让每个表面都出现 AI 入口；需要以高相关性而不是高曝光率衡量设计。
- Agent 从建议走向写入和批量操作后，错误成本显著上升，需要计划、预览、审批、版本历史和可撤销机制。

### 可迁移原则

1. **先利用产品已经知道的上下文，再要求用户补充 prompt。**
2. **按当前状态排序能力，不要在每个状态展示同一张 AI 菜单。**
3. **AI 输出应回到原生对象模型中，不能成为不可组合的聊天孤岛。**
4. **预置路径服务多数人，自由提示与模板化服务高级用户。**
5. **上下文越广、行动能力越强，权限说明、来源、预览和撤销就必须越强。**
6. **成功指标应是减少完成任务的步骤和切换，而不是 AI 按钮点击率。**

### 来源 URL

- The design thinking behind Notion AI（Ryo Lu，2023-09-21）：https://www.notion.com/blog/the-design-thinking-behind-notion-ai
- The crucial moments and decisions leading up to the launch of Notion AI（Ivan Zhao，2023-11-30）：https://www.notion.com/blog/behind-the-scenes-notion-ai
- Introducing Notion 3.0（2025-09-18）：https://www.notion.com/blog/introducing-notion-3-0
- Search in your workspace（当前 workspace、connected apps 与 web AI search）：https://www.notion.com/help/search

---

## 8. 跨案例结论

这些案例反复出现同一组 Notion 式产品判断：

1. **先找到稳定原语。** 评论附着于内容，Synced Blocks 共享 block 身份，AI 回到 block，Teamspaces 仍组织页面，而不是为每个新需求造一个孤岛。
2. **把默认路径做简单，把高级能力渐进披露。** reaction、Search、contextual AI 和自动邻接规则都先解决最常见动作，过滤、自定义 prompt、跨页同步再按需出现。
3. **在副作用发生处给信号。** 同步编辑边框、评论未读、teamspace 锁、AI 预览和性能里程碑都让系统状态可感知。
4. **全局身份与局部视图分开。** 一个评论线程、一个共享 block、一个 workspace 可以在多个位置出现；每个人仍可拥有自己的侧边栏、过滤或呈现密度。
5. **规模会放大模糊语义。** 两个同步副本时不清楚的删除动作，扩展到 10 个以上就成为数据损失风险；几十页时可用的侧边栏，扩到数千员工就必须重构。
6. **真正的“安静”来自可靠机制。** 页面看似自然，是因为邻接规则在工作；Android 看似立即可用，是因为关键路径被严格测量；AI 看似懂用户，是因为产品上下文被结构化利用。

用于“跨页面共享内容并同步编辑”时，最重要的落点是：**保留普通复制的独立语义；只有显式动作才创建共享身份；共享关系建立后，在编辑、权限和删除处持续显示其影响范围。**
