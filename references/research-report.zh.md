# Notion 产品设计深度研究

> 版本：2026-07-10
> 研究对象：Notion 从早期「可组装的软件工具」到 AI Workspace、Agents 与 Developer Platform 的设计旅程
> 核心材料：Andrew Lee 在 Design Matters 2022 的演讲《Notion’s design process and principles》及其字幕；Notion 官方设计、工程、产品资料；创始人访谈；少量可信二手史料。

## 一句话结论

Notion 最值得研究的不是它的极简视觉，而是它持续十余年不变的一种产品架构思想：**先设计一套足够小、足够一致、可以组合的原语，再用熟悉的交互、模板、预制工作流和 AI，把原语的复杂性逐层包装起来，让普通用户获得接近“编程”的工具塑造能力。**

它的产品演进并不是从笔记软件不断“加功能”，而是用户能动性的三次升级：

1. **直接操纵**：用户拖动、嵌套、转换 block，亲手搭建工具。
2. **声明式配置**：用户通过数据库、属性、视图、模板、公式和自动化描述工作系统。
3. **委托式行动**：用户用自然语言把目标交给 Agent；Agent 使用同一批 block、页面、数据库和权限完成工作。

这三代能力叠加在一起，才构成 Notion，而不是彼此替代。

---

## 1. 研究方法与判断边界

本文把内容分为三类：

- **事实**：可由官方资料、演讲或直接访谈确认。
- **分析**：把多个事实连接起来得到的产品设计判断，不代表 Notion 官方表述。
- **启示**：可以迁移到其他产品的方法。

需要注意：Andrew Lee 的演讲录制于 2022 年，反映的是当时约 6 人设计团队的工作方式；Notion 在 2025—2026 年已经进入 Agent 和开发者平台阶段，因此本文把演讲当作“设计 DNA 的剖面”，而不是当前组织流程的完整描述。

本地原始材料：

- [英文字幕（SRT）](./design-matters-2022.en.srt)
- [Design Matters 官方演讲页](https://recordings.designmatters.io/talks/notions-design-process-and-principles/)

---

## 2. Notion 真正要解决的，并不是“记笔记”

### 2.1 起点：计算机不应只是电子化的打字机与文件柜

Andrew Lee 的演讲从 19 世纪办公室中的打字机与文件柜讲起，再连接到 Douglas Engelbart、Ted Nelson、Alan Kay 等计算先驱。其论点是：很多现代软件只是把旧工具搬进屏幕——文字处理器是像素化打字机，文件系统和传统数据库是数字化文件柜；软件仍然按厂商预设的边界分裂成一座座孤岛。

Notion 的创始动机不是把其中某一种工具做得更好，而是恢复早期个人计算的一个承诺：**计算机应增强人的思考与创造能力，普通人也应能塑造自己的工具。** Ivan Zhao 在访谈中把问题概括得很直接：有审美、有想法的人往往不是缺少品味，而是缺少能让他们表达和构造的工具。[Sequoia 对 Ivan Zhao 的访谈与公司史](https://sequoiacap.com/article/notion-spotlight/)把 Engelbart 的《Augmenting Human Intellect》视为其创始思想的重要来源。

演讲中，Notion 的使命被表述为“让软件工具制造普及到每个人”（约 00:04:23）。这解释了一个长期存在的误读：Notion 看上去像文档编辑器，但它把“文档的熟悉感”当作入口，而不是终点。

### 2.2 2015 年危机：宏大愿景不能替代真实需求

2015 年，早期 Notion 一度走到开发停滞的边缘。Ivan 后来回顾：团队做的是自己觉得“很酷”的产品，却不是用户真正需要的产品。创始团队回到原点，在京都集中重做产品。京都的建筑、工艺与服务文化也强化了他们对“简单、耐久、功能性、手工感和待客体验”的偏好。[京都市对 Ivan Zhao 的采访](https://kyo-working.city.kyoto.lg.jp/article/report/2022-04-04/)是这一阶段最直接的第一手材料。

这段经历带来一个重要的产品教训：

> 愿景负责确定方向，真实任务负责确定入口。

Notion 没有放弃“人人都能造软件”的愿景，但把最初的入口收敛成每个人都认识的页面、文字、清单和 Wiki。

### 2.3 设计气质不是“黑白极简”，而是克制的工具性

Notion 的黑白界面、衬线 Logo、插画和 emoji 很容易被当成品牌风格。更深层的逻辑是：**内容和用户结构应成为视觉主角，工具本身退后。**

这种克制并不等于功能贫乏。它要求产品表面保持安静，而复杂能力通过上下文逐步出现：空白页首先像文字编辑器；输入 `/` 才看到 block 类型；悬停才出现六点手柄；把内容放进数据库后才出现属性、筛选、排序、关系、公式和自动化。

---

## 3. 产品原语：为什么 “Everything is a block” 如此关键

### 3.1 Block 同时是数据单位、交互单位和组合单位

在演讲约 00:05:43，Andrew 把 Notion 的灵活性归因于 block。官方工程文章进一步说明：文字、图片、列表、数据库中的一行、甚至页面本身，底层都被建模为 block。每个 block 具有 ID、properties、type，以及指向内容和父级的关系。[Notion 的 block 数据模型](https://www.notion.com/blog/data-model-behind-notion)

这一决定同时统一了三层：

- **数据层**：不同内容具有一致的基本身份与关系模型。
- **交互层**：选择、拖动、复制、转换、嵌套等操作可跨内容类型复用。
- **产品层**：新增能力经常可以成为新 block，接入已有组合系统，而不是另造一套产品。

官方帮助中心对用户使用的解释同样保持一致：页面是 block 的堆叠；block 可以转换，也可以通过六点手柄拖放重排。[What is a block?](https://www.notion.com/help/what-is-a-block)

### 3.2 “Turn into” 的价值是保护用户意图，而非炫技

Notion 允许待办事项变成标题、引用或 callout。工程上，改变 type 不一定删除原 properties；例如待办的 checked 状态在变成其他类型时可以暂时被忽略，转回待办时仍保留。官方把它解释为尽可能保护用户意图。

产品含义是：**低成本、可逆的试错会提高用户探索原语的意愿。** 如果“换一种表达”意味着重建内容，用户就会在创建之前被迫做结构决策；如果转换是安全的，用户可以先捕捉想法，再逐渐赋予结构。

### 3.3 缩进不是样式，而是结构

传统文字处理器中，缩进通常只是视觉格式；Notion 中，缩进会改变 block 在 render tree 里的父子关系。页面看上去仍像文档，但用户其实在编辑一棵结构树。

这是 Notion 常见设计策略的缩影：

> 用熟悉动作承载更深的计算结构。

复制粘贴、拖拽、缩进、斜杠命令都很熟悉，但它们操作的不是一张“纸”，而是一组可寻址、可重组、可关联的数据对象。

### 3.4 Page 的递归身份：容器也是内容

页面是 block，数据库也是页面，数据库中的每一行又是页面。这种递归关系消除了传统工具中的一些硬边界：

- 一条任务不仅是表格里的一行，还能拥有正文、子页面、图片和讨论。
- 一个数据库既能独立存在，也能嵌在普通页面中。
- 一个页面既是内容，也可以成为另一页面中的结构节点。

Notion 官方对数据库的描述非常明确：数据库是页面集合；数据库本身也是页面；每一行都是可以继续承载内容的页面。[What is a database?](https://www.notion.com/help/what-is-a-database)

这种递归性是 Notion 能同时覆盖文档、Wiki、任务和轻量业务系统的关键。

---

## 4. 数据库旅程：把文档变成可运行的工作系统

### 4.1 Notion 2.0 的关键不是“增加表格”，而是分离数据与呈现

2016 年发布的 Notion 1.0 主要提供页面、清单、Wiki 与模板；2018 年的 2.0 引入关系数据库与更强的项目管理能力。[Sequoia 公司史](https://sequoiacap.com/article/notion-spotlight/)记录了这一转折。

数据库最有力量的设计不是 table，而是**同一批页面可以有 table、list、board、calendar、gallery、timeline、chart 等不同视图**。数据的身份保持不变，呈现方式根据任务变化。

这带来三个重要能力：

1. 不同角色可以观察同一事实源，而不必复制数据。
2. 工作流可以从“写内容”平滑过渡到“管理状态”。
3. 结构与内容不再互斥：属性负责可计算结构，页面正文负责开放表达。

### 4.2 数据库其实是一种温和的终端用户编程

当用户定义属性、关系、rollup、公式、筛选、视图与自动化时，实际上正在定义：

- 数据模式；
- 对象关系；
- 查询条件；
- 呈现逻辑；
- 事件触发与动作。

Notion 没有要求用户先理解“schema”“query”或“event handler”，而是把这些概念包装成可见的表格、标签和菜单。这是“让软件工具制造普及化”最具体的实现。

### 4.3 结构化的代价：复杂性只是被移动了

统一的 block/graph 模型赋予用户自由，但也把复杂性转移到系统内部：

- 权限必须沿层级继承，并处理同一内容在多处引用的歧义；
- 页面加载需要递归寻找 block 及依赖记录；
- 离线模式必须保证一个页面依赖的所有记录完整可用，并解决同步冲突；
- 任意嵌套和多种 block 会产生大量边界状态。

Notion 在 2021 年就公开承认 block 架构在性能、离线和编辑器细节上仍有大量工作；离线模式直到 2025 年才正式推出，官方称其长期是最高频需求，并详细说明了引用追踪、后台同步与富文本冲突解决的难点。[数据模型文章](https://www.notion.com/blog/data-model-behind-notion)；[离线模式工程复盘](https://www.notion.com/blog/how-we-made-notion-available-offline)

因此，原语化架构不是“免费获得无限灵活性”，而是一个清晰的交换：**前台把组合权交给用户，后台承担组合爆炸。**

---

## 5. Notion 的核心体验策略：熟悉入口，逐层显露能力

### 5.1 第一层：像文字编辑器一样直接

空白页、光标、回车、Markdown 快捷方式让用户不必先理解 Notion 就能输入。Andrew 在演讲中强调，初次打开它应简单、可接近、熟悉。

### 5.2 第二层：用 `/` 把“写作模式”切换成“构建模式”

斜杠命令是一座桥：用户不离开键盘，不切换到复杂工具栏，只需在当前意图位置召唤结构。它既保持写作流，也把大量 block 隐藏到需要时出现。[Notion 键盘与 slash commands](https://www.notion.com/help/keyboard-shortcuts)

### 5.3 第三层：六点手柄把隐形结构变成可操纵对象

六点手柄同时承担选择、拖动、转换、复制、删除和配置入口。它让页面保持干净，又在悬停时揭示“这一段其实是一个对象”。

### 5.4 第四层：模板把原语包装成任务

空白画布的优点是自由，缺点是用户不知道“可以做什么”。模板把一套 block、属性、视图和示例内容冻结成可复制的起点，完成三件事：

- 展示可能性；
- 缩短首次价值时间；
- 把高手搭建的系统变成可分发产品。

当前 Notion 会根据 onboarding 信息选择 starter templates；Marketplace 中有大量用户创建的模板。[Start with a template](https://www.notion.com/help/start-with-a-template)

数据库模板进一步把团队流程固化为重复结构，例如 bug report、研究访谈、周会记录。这使模板不只是 onboarding 内容，也成为组织流程的“轻量代码”。[Database templates](https://www.notion.com/help/database-templates)

### 5.5 第五层：把高频组合“产品化”

Notion 后来不再只提供原语，而是把常见组合打包为 Docs、Wikis、Projects，以及 Calendar、Mail、Forms、Sites 等更有主见的产品。

这不是背离通用工具愿景，而是对一个现实的承认：少数 toolmakers 愿意从零搭建，大多数人希望工具开箱即用。Notion 在其 AI 设计文章中直接表述了这一点：通用系统与单一用途工具之间并非二选一，而应同时存在。[The design thinking behind Notion AI](https://www.notion.com/blog/the-design-thinking-behind-notion-ai)

---

## 6. Notion 设计团队的思考尺度：Chair、Car 与 City

Andrew 在演讲约 00:07:53 提出一个尺度隐喻：设计一把椅子与设计一座城市是不同类型的问题。

### 6.1 Chair-shaped problems

边界明确、局部、相对可逆，主要依靠工艺与执行。例如：给图片增加左、中、右对齐。

### 6.2 City-shaped problems

由多个互相影响的系统组成，答案不明确，往往不可逆，需要跨团队协调。例如：新用户不理解 Notion 能做什么，究竟应修改 onboarding、信息架构、产品定位、模板，还是基础产品本身？

### 6.3 Car-shaped problems

评论改版被 Andrew 称为介于二者之间的 car-shaped problem：概念看似清楚，但包含大量交互和状态细节。

### 6.4 从概念到原子的四层设计栈

演讲中可以抽象出四层：

1. **产品概念**：产品是什么、服务什么心智模型。
2. **信息架构与导航**：内容和动作如何组织、如何到达。
3. **功能与交互**：具体任务如何完成。
4. **系统组件**：modal、input、popover、菜单和状态模式。

Notion 设计师要在四层之间往返，而不是只交付 UI。越靠上，决策越难逆转、越需要探索；越靠下，越依赖系统一致性与细节工艺。

### 6.5 “Fit” 比孤立的好方案更重要

Andrew 引用 Christopher Alexander：好设计不是孤立的 form，而是 form 与 context 之间的 fitness。对产品团队而言，一个看上去漂亮的功能，如果破坏 block 心智、页面节奏、权限结构或组合能力，就不是真正适合 Notion 的方案。

---

## 7. 评论系统案例：从问题定义到 10 像素

演讲约 00:13:16 开始完整复盘评论改版。这是观察 Notion 方法最好的材料。

### 7.1 先把“评论不好用”拆成三个可设计的问题

团队没有停留在模糊反馈，而是拆成：

1. **不可发现**：页边的小图标太弱，用户甚至找不到评论。
2. **上下文割裂**：评论以浮层覆盖编辑器；切换 thread 要反复打开与关闭。
3. **缺少更新与待处理信号**：文档负责人难以判断何处出现新反馈、什么需要回复。

这里值得学习的是：问题不是按功能模块拆，而是按用户认知与行为断点拆。

### 7.2 最终方案不是一个界面，而是两个互补尺度

约 00:16:20，团队落到双入口方案：

- **页边评论预览**：贴近对应内容，支持浏览时快速感知局部讨论。
- **评论侧栏流**：集中展示整页所有 thread，支持连续检查与处理。

局部入口回答“这里发生了什么”，全局入口回答“这页还有哪些事要处理”。二者点击后都定位到对应 block，使内容空间与讨论空间保持同步。

这是通用的设计规律：当对象既有空间上下文，又有队列处理需求时，常需要“就地视图 + 汇总视图”，而不是强迫一个界面承担两种任务。

### 7.3 Wilderness：探索的目标是理解问题结构

团队尝试过：

- 把 updates 与 comments 混成按时间排序的单一流——信息太吵；
- 分成 comments/updates tabs，但仍使用旧浮层——根本摩擦没有消失；
- 点击一个 thread 后用整个侧栏展示——干净，但失去整页概览；
- 最终把 comments 与 updates 分开，同时让 comment stream 与正文联动。

这些“失败方案”并非浪费，它们逐步明确了几组不能同时被一个表面解决的需求：焦点与概览、内容上下文与处理效率、静态浏览与新消息提醒。

### 7.4 Fit & finish：通用规则必须容纳局部例外

演讲约 00:20:41 进入细节阶段：

- comment thread 有 collapsed、expanded、focused 等状态；
- 一条、两条、二十条回复的密度不同；
- 默认预览前三行，但单条评论时打破规则，多显示内容；
- 图片和数据库等 full-width block 会侵入边栏，评论需要随 block resize “躲避”；
- toggle 收起正文时，评论仍需暗示隐藏内容里有讨论，并能成为展开入口；
- 多列布局不再有简单线性阅读顺序，需要重新定义评论排序；
- V1 发布后又把每个 thread 压缩 10 像素，在密度与可读性之间继续打磨（约 00:24:41）。

这里体现 Notion 的系统观：**规则提供可预测性，例外保护真实任务。** 好的设计系统不是消灭例外，而是让例外有明确理由。

---

## 8. Synced Blocks：如何把陌生概念藏进熟悉动作

Synced Blocks 的理论来源是 Ted Nelson 的 transclusion：同一内容可在多处出现并双向保持一致。但 transclusion 对大众用户很陌生。

Notion 没有从术语教育开始，而是借用复制粘贴这个人人都会的动作作为发现入口。设计过程又经历了重容器、编辑态内边距、窄列冲突等问题，最终收敛成一个很轻的 halo：平时融入页面，光标靠近时醒来，编辑将影响其他位置时变得更明显；权限不一致等高风险状态则使用更重的提示。[Designing Synced Blocks](https://www.notion.com/blog/designing-synced-blocks)

这个案例展示了四条原则：

1. 新能力优先嫁接在已有行为上，而不是发明全新手势。
2. 阅读状态尽量安静，编辑状态才揭示结构。
3. 视觉重量应与行动风险成正比。
4. 先让人“用懂”，再让人“学懂”。

---

## 9. 页面排版：为什么一致间距反而不一致

2026 年 Notion 重新处理页面节奏。问题看似只是 padding：段落需要更大呼吸感，但列表需要紧凑；如果所有 block 使用同一间距，至少有一种内容会显得不对。

团队没有为每种组合逐一写死，而是引入 adjacency rules：根据当前 block 的上下邻居动态决定间距；连续列表项靠拢，段落保持更清晰的分隔。[Updating the design of Notion pages](https://www.notion.com/blog/updating-the-design-of-notion-pages)

这背后是一个成熟的系统设计转变：

> 从“组件自身决定外观”，走向“组件与邻居关系共同决定外观”。

对于高度可组合的产品，单个组件规范永远不够；真正的设计对象是组合关系。

---

## 10. 从单一画布到产品家族：Calendar、Forms、Mail

### 10.1 Calendar：补上“时间层”

Notion 2022 年收购 Cron，2024 年推出 Notion Calendar。官方把时间描述为同步与异步工作的桥：页面、项目和任务解决“做什么”，日历解决“何时发生”。Calendar 通过菜单栏、键盘命令、多个日历合并与 Notion 数据库连接减少上下文切换。[Cron 收购公告](https://www.notion.com/blog/notion-acquires-cron)；[Introducing Notion Calendar](https://www.notion.com/blog/introducing-notion-calendar)

分析：Notion 没有把日历简单做成一个新 database view，而是保留独立、速度优先的专用产品，再与通用 workspace 连接。这说明通用原语存在边界：某些高频、强时间性的交互需要专门表面。

### 10.2 Forms 与 Layouts：扩展信息的流入与呈现

Forms 让外部信息直接进入数据库，省去第三方表单与复制；Layouts 让同一个数据库页面根据菜谱、社交内容或产品反馈等用途呈现不同结构。[2024 产品发布汇总](https://www.notion.com/blog/conference-product-releases)

它们分别扩展了系统的两端：

- Forms：从“用户在 Notion 内创建数据”扩展为“任何人向系统提交数据”。
- Layouts：从“数据库有多种集合视图”扩展为“单条记录也能有用途化视图”。

### 10.3 Mail：把数据库的 view 思维迁移到收件箱

Notion Mail 没有只做极简 Gmail，而是把 Notion 的两个既有原语带入邮件：

- 自定义 views 用于按个人任务和优先级切分收件箱；
- `/` 与 block 用于编写邮件；
- AI 根据自然语言自动打标签、排序和生成内容；
- Calendar 处理排期。

[Introducing Notion Mail](https://www.notion.com/blog/introducing-notion-mail)

分析：产品家族的一致性并不来自外观相同，而来自同一种“用户可以塑造视图和工作流”的哲学。

---

## 11. AI 旅程：从一个 block 到会行动的协作者

### 11.1 2023：AI 是横向能力，不是独立聊天框

Notion 早期 AI 设计明确反对“空白输入框包一层 API”的做法。AI 被放进已有上下文：

- 空白页优先显示起草能力；
- 已有内容后的新 block 优先显示续写、总结和提取行动项；
- 选中文字后优先显示改写、语气和校对；
- 数据库中用 Autofill 抽取结构化信息。

同一批 AI skills 根据页面状态重新排序。这是一种上下文式 progressive disclosure：不是把所有 AI 功能展示出来，而是在当前对象、状态和任务中给出最可能的动作。[The design thinking behind Notion AI](https://www.notion.com/blog/the-design-thinking-behind-notion-ai)

### 11.2 Block 架构后来变成 AI 的结构化上下文

在普通文档里，“4 月 30 日”可能只是一段文字；在 Notion 中，它可以是某个 task block 的 due date，并与负责人、项目和状态关联。官方称这种结构化 graph 让 AI 不只是做关键词搜索，而能理解关系、生成 tracker、汇总状态和推理 roadmap。[Speed, Structure, and Smarts: The Notion AI Way](https://www.notion.com/blog/speed-structure-and-smarts-the-notion-ai-way)

这是架构复利：为人类可组合性设计的数据结构，也成为机器可理解性的基础。

### 11.3 2025：Notion 3.0 从“生成内容”转向“执行工作”

Notion 官方把版本演进概括为：

- 1.0：协作页面与知识画布；
- 2.0：数据库与集成；
- 3.0：Agent 使用这些 building blocks 完成工作。

Agent 不只是回答，而能创建页面、搭建数据库、跨来源搜索和执行多步任务。Agent 的个性与长期指令仍然放在一个普通 Notion 页面里，用户可以随时编辑；specialized skills 也可以模板化。[Introducing Notion 3.0](https://www.notion.com/blog/introducing-notion-3-0)

这个设计非常“Notion”：新的 AI 记忆机制没有另造复杂设置系统，而是复用 page 与 template 原语。

### 11.4 2026：Custom Agents 把工作流变成可分享对象

Custom Agents 增加 schedule/trigger、跨工具行动、共享、版本与运行记录。它们延续了模板经济的逻辑：一个人把重复工作封装成 Agent，其他团队可以复用和改造。

更重要的是，Agent 的风险控制被设计为产品能力：详细权限、运行日志、预算提示、自动暂停、可逆更改，以及对 prompt injection 的显式警告。[Introducing Custom Agents](https://www.notion.com/blog/introducing-custom-agents)

这里出现新的设计原则：**自动化程度越高，控制、可见性和可逆性就越应该成为一等界面。**

### 11.5 Developer Platform：从 no-code 到 code 与 agent 共存

2026 年 Developer Platform 引入 Workers、外部数据同步、agent tools、External Agents API 与 CLI。官方把它们继续称为新的 building blocks，并提出 progressive trust：Agent 初期行动可要求人工复核，可靠后再扩大自治范围；认证、权限和 sandbox 从第一天进入平台设计。[Introducing Notion’s Developer Platform](https://www.notion.com/blog/introducing-developer-platform)

到 2026 年 7 月，Notion 又开始把外部 Agent 放进团队共享界面，并加入交互式 HTML blocks。[Notion 3.6 发布记录](https://www.notion.com/releases/2026-07-01)

分析：Notion 的边界已从“一个可组装的工作空间”扩展为“人、数据、代码与 Agent 共同工作的运行环境”。但其基础逻辑仍是相同的：可组合原语、共享上下文、明确权限、可见行动与社区复用。

---

## 12. Notion 设计中反复出现的七组张力

### 12.1 灵活 vs. 好上手

- 灵活性创造差异化，也创造空白画布焦虑。
- 对策不是削掉能力，而是熟悉入口、模板、starter content、预制产品和 AI 辅助。
- 风险是包装越来越多，产品重新变成一组固定工具。

### 12.2 通用原语 vs. 专用体验

- block 和 database 提供普适能力。
- Calendar、Mail 等高频场景需要更有主见、速度更高的专用表面。
- Notion 的选择是“共享哲学与数据，允许表面分化”。

### 12.3 一致性 vs. 上下文例外

- 统一交互降低学习成本。
- 评论单条预览、full-width block、toggle、列布局、列表间距证明机械一致会破坏任务。
- 成熟系统需要可解释的例外机制，而非绝对统一。

### 12.4 创作自由 vs. 阅读秩序

- 作者希望自由组合 block。
- 阅读者需要节奏、层级、密度和清晰导航。
- 2026 页面 spacing 重构说明，创作工具最终也必须认真设计消费体验。

### 12.5 组合能力 vs. 系统可靠性

- 任意嵌套、引用和多视图带来巨大表达能力。
- 同时放大权限、加载、同步、离线和边界状态的复杂度。
- 设计愿景必须与基础设施投资匹配，否则自由度会以延迟和不确定性伤害体验。

### 12.6 安静界面 vs. 能力发现

- 控件隐藏得太深，用户不知道产品能做什么。
- 控件全部常驻，页面又会失去专注。
- Notion 常用悬停、选择态、slash、模板和上下文菜单解决，但评论改版证明 affordance 过弱时必须提高信号。

### 12.7 用户控制 vs. Agent 自治

- AI 价值来自替用户行动。
- 可信度来自权限、来源、日志、预算、撤销和逐步授权。
- Agent 时代的“极简”不能是隐藏系统行为，而应是用清晰控制管理复杂自动化。

---

## 13. Notion 的增长其实也是产品设计

Notion 早期团队很小，创始人直接做客服、在 Twitter 回复用户；Product Hunt 和一群高度投入的用户帮助产品进入科技与创业社区。2.0 后，数据库与模板让用户不仅使用 Notion，还能展示“我是如何使用 Notion 的”。[Sequoia 公司史](https://sequoiacap.com/article/notion-spotlight/)

可以把增长飞轮写成：

```text
少量一致原语
  → 用户搭出高度个性化系统
  → 系统可被展示、复制、售卖
  → 模板降低新用户门槛
  → 更多场景与高手用法被发现
  → 反向影响官方 block、模板与预制产品
```

模板在这里同时扮演：示例、onboarding、分发物、社区内容、商业商品和产品研究样本。Custom Agents 又把这一飞轮从“分享结构”扩展到“分享自动化行为”。

这说明社区并不是营销外挂，而是通用产品发现 use case 的分布式研发系统。

---

## 14. 可以复用的 Notion 式产品设计方法

### 14.1 先找最小原语，再决定功能列表

问：不同 use case 是否可以由同一种对象、关系与操作表达？如果可以，优先统一模型；如果不可以，不要为了“平台感”强行统一。

### 14.2 为原语设计一套通用动词

Notion 的关键动词是 create、move、nest、turn into、link、duplicate、filter、view、share。用户掌握动词后，可以迁移到新 block 和新场景。

### 14.3 用熟悉动作承载新概念

Synced Blocks 借复制粘贴，结构树借缩进，block menu 借 `/`，数据库借表格。不要先要求用户学习产品理论。

### 14.4 把复杂性按意图逐层展开

默认界面只支持最常见动作；在对象被选择、悬停、放入特定上下文或用户表达更深意图时，再显示高级能力。

### 14.5 把模板视为“已编译的经验”

模板不只是空白样板，应包含经过验证的结构、默认值、提示、示例数据与流程约束。每次团队学到新做法，都可以更新模板，让经验进入下一次执行。

### 14.6 同时设计局部视图与全局处理视图

评论案例表明，协作信息往往既需要贴近对象，也需要集中 triage。对批注、错误、审核、任务和通知系统都可使用这一框架。

### 14.7 用风险决定界面重量

- 低风险、可逆动作：轻、快、就地完成。
- 影响多处的编辑：增加状态提示。
- 删除、权限冲突、Agent 跨系统行动：明确警告、日志、审批和撤销。

### 14.8 为组合设计，而不只为组件设计

检查：相邻、嵌套、窄宽、超长、空状态、单项、大量、多列、隐藏、只读、权限不一致、离线、多人同时编辑。组合型产品的主要 bug 和体验债都出现在关系处。

### 14.9 按可逆性决定设计流程

- Chair：快速实现、上线验证。
- Car：先拆状态与交互，再做多方案原型。
- City：先统一问题模型与系统边界，明确长期不可逆代价，再进入界面。

### 14.10 AI 行动必须伴随可见、可控、可逆

不要只设计“Agent 能做什么”，还要设计：它读了什么、为什么行动、将改动什么、花费多少、谁授权、如何停止、如何撤销、如何审计。

---

## 15. 对 Notion 的批判性判断

### 15.1 最大优势与最大缺点来自同一个地方

Notion 的灵活性使它能适配大量任务，也使新用户难以形成清晰心智；它能让团队建立自己的流程，也可能让团队建立出难以维护的“内部软件”。模板和 AI 能降低起步成本，但也可能掩盖结构设计不足。

### 15.2 “一个工作空间”不会自动消灭信息孤岛

把内容放进同一产品不等于内容就有结构、归属、时效和可发现性。数据库、teamspace、权限、模板和 AI search 只是条件；组织仍需要信息架构与维护责任。

### 15.3 组合自由容易制造局部最优

个人搭出的美观 dashboard 不一定适合团队；过多 relations、rollups 和 views 会把可塑性变成认知负担。Notion 缺少传统业务软件的强流程约束，这既是吸引力，也是治理成本。

### 15.4 AI 正在改变“用户是工具制造者”的含义

早期 Notion 要求用户亲自搭 block；Agent 时代，用户更多描述目标并监督结果。工具制造能力从直接操纵转向意图表达、规则设定与治理。若 Agent 自动化过强，用户可能失去对系统结构的理解；若控制过多，Agent 又无法带来足够价值。

Notion 下一阶段真正的设计难题，不是 Agent 会不会创建页面，而是：**如何让非技术用户理解、验证和治理一个由 Agent 持续修改的工作系统。**

---

## 16. 最终框架：Notion 的设计 DNA

可以把 Notion 的设计 DNA 压缩为八条：

1. **工具塑造权属于用户。**
2. **少量原语胜过大量孤立功能。**
3. **熟悉感是进入新计算模型的桥。**
4. **先允许直接行动，再逐步显露结构。**
5. **模板把高手经验转化为大众入口。**
6. **系统规则保证一致，情境例外保证适用。**
7. **细节工艺不是装饰，而是复杂系统可用性的最后一公里。**
8. **AI 不应漂浮在产品上方，而应理解并操作产品原语，同时接受权限、日志与撤销约束。**

Notion 最长期的竞争力不是某个单点功能，而是这套原语、交互语言、社区作品和结构化数据共同形成的复利。页面、数据库、模板、Calendar、Mail、Agent 与 Developer Platform 看起来跨越了多个品类，但它们都在回答同一个问题：

> 人怎样才能用更低的技术门槛，创造、调整并委托属于自己的软件工具？

---

## 17. 关键时间线

| 时间 | 产品/组织节点 | 设计意义 |
|---|---|---|
| 2013 前后 | 创始想法形成 | 从 Engelbart 等人的“增强人类智力”出发，目标是普及软件工具制造 |
| 2015 | 早期产品危机、京都重构 | 从“团队觉得酷”回到“用户需要什么”；强化克制、工艺与待客意识 |
| 2016-03 | Notion 1.0 | 页面、清单、Wiki、拖拽与模板；以熟悉文档作为通用系统入口 |
| 2018 | Notion 2.0 | 关系数据库与多视图；从内容工具进入工作系统 |
| 2021 | Block 数据模型公开、API、Synced Blocks | 原语开始成为外部集成、跨页复用与平台化的基础 |
| 2022 | 评论系统设计复盘；收购 Cron | 协作细节成熟；开始补同步工作的时间层 |
| 2023 | Notion AI | AI 作为跨 block 和上下文的横向能力，而非独立聊天产品 |
| 2024 | Notion Calendar；Forms、Layouts；Mail 预览 | 从通用画布扩展到专用产品家族，并继续复用 views/blocks 哲学 |
| 2025 | Mail；AI Meeting Notes；Offline；Notion 3.0 Agent | 从生成与搜索进入跨页面行动；补齐长期基础体验债 |
| 2026-02 | Custom Agents | 自动化工作流成为可共享、可运行、可治理的对象 |
| 2026-03 | 页面排版系统更新 | 从组件一致性升级到基于邻接关系的组合一致性 |
| 2026-05 | Developer Platform | Workers、数据同步、外部 Agent 与 CLI 扩大可编程边界 |
| 2026-07 | External Agents、HTML blocks | Notion 朝人、Agent、代码与数据共享运行环境继续演进 |

---

## 18. 精选来源

### 第一手设计与工程资料

- [Notion’s design process and principles — Design Matters](https://recordings.designmatters.io/talks/notions-design-process-and-principles/)
- [The data model behind Notion’s flexibility](https://www.notion.com/blog/data-model-behind-notion)
- [Designing Synced Blocks](https://www.notion.com/blog/designing-synced-blocks)
- [The design thinking behind Notion AI](https://www.notion.com/blog/the-design-thinking-behind-notion-ai)
- [Updating the design of Notion pages](https://www.notion.com/blog/updating-the-design-of-notion-pages)
- [How we made Notion available offline](https://www.notion.com/blog/how-we-made-notion-available-offline)
- [Speed, Structure, and Smarts: The Notion AI Way](https://www.notion.com/blog/speed-structure-and-smarts-the-notion-ai-way)
- [Creating the Notion API](https://www.notion.com/blog/creating-the-notion-api)

### 产品演进资料

- [Introducing Notion Calendar](https://www.notion.com/blog/introducing-notion-calendar)
- [Everything we launched at Make with Notion 2024](https://www.notion.com/blog/conference-product-releases)
- [Introducing Notion Mail](https://www.notion.com/blog/introducing-notion-mail)
- [Introducing Notion 3.0](https://www.notion.com/blog/introducing-notion-3-0)
- [Introducing Custom Agents](https://www.notion.com/blog/introducing-custom-agents)
- [Introducing Notion’s Developer Platform](https://www.notion.com/blog/introducing-developer-platform)
- [Notion 3.6: External Agents, HTML blocks, and more](https://www.notion.com/releases/2026-07-01)

### 产品机制资料

- [What is a block?](https://www.notion.com/help/what-is-a-block)
- [What is a database?](https://www.notion.com/help/what-is-a-database)
- [Start with a template](https://www.notion.com/help/start-with-a-template)
- [Database templates](https://www.notion.com/help/database-templates)
- [Comments & mentions](https://www.notion.com/help/comments-mentions-and-reminders)

### 创始与历史材料

- [Notion CEO Ivan Zhao: Augmenting Human Intellect — Sequoia](https://sequoiacap.com/article/notion-spotlight/)
- [Ivan Zhao 京都采访](https://kyo-working.city.kyoto.lg.jp/article/report/2022-04-04/)
- [First Block with Ivan Zhao and Simon Last](https://www.notion.com/blog/first-block-with-ivan-zhao-simon-last)
