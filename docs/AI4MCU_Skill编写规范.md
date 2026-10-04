# Claude Code STM32 Skill编写规范

<br>

**作者**：Huang Xiaofeng [ai4mcu@gmail.com](mailto:ai4mcu@gmail.com), Huang Shan [youcans@qq.com](mailto:youcans@qq.com)

---

<br>

## 1. 总体原则

本规范用于AI-for-STM32-Skills项目中Claude Code用户级STM32 Skill的设计与编写。

当前Skill体系主要面向STM32基础外设实验和程序运行机制实验，通过需求整理、初始工程分析、任务规划、代码实现和开发日志记录，形成简化的AI协同STM32开发流程。

Skill编写统一遵循“准确、清晰、简洁”的原则：

- 准确：职责、输入、输出、文件路径和执行边界定义明确，不使用含义不清或前后不一致的表达。
- 清晰：统一采用输入、输出、执行、边界、出口的结构描述Skill运行规则。
- 简洁：只规定当前Skill实际需要完成的工作，不增加没有明确必要性的检查、判断和异常处理。

Skill文件是Claude Code的执行规范，不是教程、设计说明或用户使用手册。

除非存在明确且很大的风险，不主动提出增加检查步骤。即使需要增加检查，也应先提出风险和修改建议，由开发者确认后再加入Skill，不得擅自增加流程复杂度。

---

<br>

## 2. Skill目录结构

每个Skill使用独立目录保存。

基本结构：

```text
<skill-name>/
├── SKILL.md
└── assets/
```

其中：

- `SKILL.md`：Skill核心执行规范，必须存在。
- `assets/`：保存输出模板等静态资源，根据Skill需要设置。

当前项目目录结构为：

```text
claude-user-skills/
├── stm32-init/
│   ├── SKILL.md
│   └── assets/
│       └── proj_requirements.tpl.md
├── stm32-scan/
│   ├── SKILL.md
│   └── assets/
│       └── proj_init_scan.tpl.md
├── stm32-plan/
│   ├── SKILL.md
│   └── assets/
│       └── proj_tasks.tpl.md
├── stm32-code/
│   └── SKILL.md
└── stm32-log/
    └── SKILL.md
```

模板文件统一保存在当前Skill的`assets/`目录中，由Skill按照模板生成正式输出文件。

---

<br>

## 3. SKILL.md文件格式

### 3.1 Frontmatter

用户主动调用的业务Skill统一采用以下基本格式：

```yaml
---
name: <skill-name>
description: <一句话说明Skill职责>
argument-hint: "<参数格式>"
disable-model-invocation: true
metadata:
  author: "Huang Xiaofeng & Huang Shan <ai4mcu@qq.com>"
---
```

例如：

```yaml
---
name: stm32-code
description: 根据编程任务清单实施指定STM32编程任务，对规定范围内的源代码进行修改
argument-hint: "<project-name> <task-id>"
disable-model-invocation: true
metadata:
  author: "Huang Xiaofeng & Huang Shan <ai4mcu@qq.com>"
---
```

`stm32-log`属于由其它Skill自动调用的内部Skill，不设置：

```yaml
disable-model-invocation: true
```

Frontmatter字段根据Skill实际需要使用，不机械增加无实际作用的字段。

### 3.2 正文结构

业务Skill正文统一采用：

```markdown
# STM32 <名称> Skill

## 1. 输入规范

## 2. 输出规范

## 3. 执行步骤

## 4. 边界约束

## 5. 出口规范
```

如果Skill自身存在明确的专业规则，可以根据需要增加独立章节。

例如`stm32-plan`在执行步骤和边界约束之间设置：

```markdown
## 4. 任务拆分要求
```

此时后续章节顺延。

---

<br>

## 4. 输入规范编写要求

输入规范只描述Skill运行所需的输入，不描述执行动作。

### 4.1 任务描述

业务Skill一般首先定义调用参数和项目位置。

例如：

```markdown
### 1.1 任务描述

`<project-name>`为Skill调用时指定的项目名称参数。

项目目录固定为`C:\STM32\<project-name>`。
```

如果存在第二个调用参数，应继续明确说明。

例如：

```markdown
`<task-id>`为Skill调用时指定的编程任务编号，例如`T01`、`T02`。
```

调用命令本身不属于Skill进入后的执行内容，一般不在输入规范中重复编写。

### 4.2 输入文件

需要读取项目文件时，应明确规定输入文件。

例如：

```markdown
### 1.2 输入文件

读取以下项目文件：

- `docs/tasks/proj_tasks.md`
- `docs/analysis/proj_init_scan.md`
```

如果输入范围需要严格限制，应直接规定允许读取的文件。

例如`stm32-scan`：

```text
<project-name>.ioc
CMakeLists.txt
Core/Inc/*.h
Core/Src/*.c
```

Skill不得自行扩展到这些文件引用的其它文件。

### 4.3 输入名称一致

同一对象全文统一使用同一名称。

例如：

- 项目名称：`<project-name>`
- 开发板型号：`<board-name>`
- 编程任务编号：`<task-id>`

不得在同一Skill中随意改写为其它参数名称。

---

<br>


## 5. 路径与文件命名规范

当前项目运行路径已经固定，不设计路径参数化机制。

主要路径如下：

- STM32工作目录：`C:\STM32`
- 项目目录：`C:\STM32\<project-name>`
- 硬件资料目录：`C:\STM32\hardware`
- Claude Code用户级Skill目录：`~/.claude/skills/`

不得重新引入`CLAUDE.md`或其它配置文件作为路径来源。

正式项目文档当前统一为：

```text
docs/requirements/proj_requirements.md
docs/analysis/proj_init_scan.md
docs/tasks/proj_tasks.md
docs/<project-name>_log.md
```

模板文件当前统一为：

```text
assets/proj_requirements.tpl.md
assets/proj_init_scan.tpl.md
assets/proj_tasks.tpl.md
```

文件名称确定后，应在输入规范、输出规范、执行步骤和出口规范中保持一致。

---

<br>

## 6. 输出规范编写要求

输出规范描述Skill形成什么结果以及结果格式，不描述执行过程。

推荐：

```markdown
## 2. 输出规范

### 2.1 编程任务清单

编程任务清单用于……

文件格式采用当前Skill目录下的模板`assets/proj_tasks.tpl.md`。

生成文件保存至`docs/tasks/proj_tasks.md`。
```

涉及路径和文件名时，尽量保持完整句子，不将一句话拆分为多行。

例如推荐：

> 文件格式采用当前Skill目录下的模板`assets/proj_tasks.tpl.md`。

不推荐：

```markdown
文件格式采用当前Skill目录下的模板：

`assets/proj_tasks.tpl.md`
```

模板中使用`<...>`标记的说明文字仅用于规定生成要求，不应原样写入正式输出文件。

---

<br>

## 7. 执行步骤编写要求

### 7.1 按实际执行顺序编写

存在先后关系时必须使用连续编号。

例如：

```markdown
1. 读取项目任务需求文件和初始工程分析文件。
2. 根据功能目标和当前工程状态确定编程任务。
3. 生成初步任务清单。
4. 提交开发者确认。
```

不得使用没有顺序关系的无序列表代替实际流程。

### 7.2 只描述当前Skill工作

执行步骤应围绕当前Skill职责展开，不重复其它Skill的工作。

例如：

- `stm32-scan`负责分析初始工程；
- `stm32-plan`负责任务拆分和细化；
- `stm32-code`负责指定任务的代码修改。

不得在一个Skill中顺便完成其它阶段工作。

### 7.3 不增加无必要检查

执行步骤中只设置当前流程明确需要的检查。

例如：

- 已存在`proj_init_scan.md`时，询问是否覆盖；
- 已存在`proj_tasks.md`时，询问是否覆盖。

不得因为“保险起见”“提高鲁棒性”等原因增加大量额外检查。

### 7.4 人工确认只设置在必要节点

需要开发者决定开发方向或保护正式成果时，可以设置人工确认。

例如：

- `stm32-init`确认项目任务需求；
- `stm32-plan`确认初步任务拆分；
- 覆盖已有正式输出文件前确认。

已经由前一阶段确认的内容，不在后一阶段重复确认。

---

<br>


## 8. 专业规则编写要求

如果某个Skill存在重要专业规则，可以独立设置章节，不必全部塞入“边界约束”。

例如`stm32-plan`的任务拆分要求：

```markdown
## 4. 任务拆分要求
```

任务规划应围绕相对完整的阶段性目标进行划分。

如果若干文件创建、接口建立和工程接入共同构成一个完整的程序基础，应作为一个整体任务，不按照单个文件、单个函数或单个修改点机械拆分。

业务功能任务应围绕相对完整的实现目标进行划分。

每项任务应目标明确，并能够通过编译、调试器或实际硬件运行等方式独立验证。

编译、烧录和运行验证属于代码任务的验证方式，不单独形成任务。

---

<br>


## 9. 边界约束编写要求

边界约束用于明确Skill不能超出的职责范围。

原则：

- 只写真正重要的边界；
- 表达直接；
- 不罗列大量假设性禁止事项；
- 不重复执行步骤已经明确的内容。

例如`stm32-scan`：

```markdown
## 4. 边界约束

1. 不得修改CubeMX配置、工程文件和源代码。
2. 分析结果仅以当前`.ioc`、`CMakeLists.txt`和源码中的实际内容为依据，不使用其它文件。
3. 不得自动补充或猜测无法确认的信息。
4. 仅按照输出模板要求提取和整理工程信息，不得分析模板未要求的内容。
```

例如`stm32-code`：

```markdown
## 4. 边界约束

1. 仅允许修改`proj_tasks.md`中当前`<task-id>`任务明确规定的文件和代码范围，不得修改当前任务未涉及的文件、函数或代码区域。
2. 不得修改CubeMX配置，不得实施其它编程任务，不得修改`proj_tasks.md`和`proj_init_scan.md`。
3. 本Skill不进行代码验证，不得自动调用工具执行构建、编译、工程生成或烧录。
```

边界条数以实际职责需要为准，不为了形式机械限制数量。

---

<br>


## 10. 输出内容范围要求

Skill只输出当前职责范围要求的内容。

不得：

- 因为发现更多工程信息而自行增加分析章节；
- 因为能够推断而加入输入资料没有支持的结论；
- 因为熟悉STM32而增加模板没有要求的技术分析；
- 因为代码实现方便而顺便修改其它区域；
- 因为任务简单而增加额外流程。

例如`stm32-scan`：

> 仅按照输出模板要求提取和整理工程信息，不得分析模板未要求的内容。

例如`stm32-code`：

> 不得增加当前任务未涉及的程序功能，不得同时实施其它编程任务。

---

<br>


## 11. 代码修改类Skill要求

`stm32-code`一次只实施一个`T0x`任务。

调用形式：

```text
stm32-code <project-name> <task-id>
```

代码修改必须以`proj_tasks.md`中当前任务为实施依据。

允许且仅允许修改：

- 当前任务规定的文件；
- 当前任务规定的函数；
- 当前任务规定的用户代码区域；
- 当前任务明确允许的其它代码范围。

代码实现细节可以结合当前源码自主完成，但不得自行改变任务范围。

修改STM32CubeMX生成源码时，应遵守当前工程已有的`USER CODE BEGIN/END`保护方式和任务规定的代码接入位置。

`stm32-code`完成代码修改后不自动执行：

- Build；
- 编译；
- STM32CubeMX Generate Code；
- 烧录；
- 实际硬件运行验证。

这些验证由开发者根据`proj_tasks.md`中的验证要求完成。

---

<br>


## 12. 出口规范编写要求

业务Skill原则上定义：

```markdown
## 5. 出口规范

### 5.1 正常出口

### 5.2 异常出口
```

如果增加了其它章节，编号相应调整。

### 12.1 正常出口

正常出口应明确完成条件。

例如：

```markdown
满足以下条件进入正常出口：

1. 当前`<task-id>`规定的代码修改完成。
2. 修改内容未超出当前任务规定的文件和代码范围。
```

正常出口可以：

- 输出执行摘要；
- 显示下一步建议；
- 显示开发者需要完成的验证要求。

提示内容如果容易被模型误认为当前Skill执行步骤，应使用明确符号整体分隔。

例如：

```markdown
【下一步建议：
- 调用`stm32-plan` Skill进行开发任务规划。】
```

### 12.2 异常出口

统一采用简洁规则：

> 不满足正常出口条件时进入异常出口。

异常出口输出：

- 未完成原因；
- 当前执行状态。

不建立大量异常类型和失败分支。

### 12.3 `stm32-log`调用

所有业务Skill在出口最后调用`stm32-log`。

正常出口：

1. 完成当前Skill需要输出的摘要、提示或验证要求；
2. 最后调用`stm32-log`；
3. 结束当前Skill。

异常出口：

1. 输出未完成原因和当前执行状态；
2. 最后调用`stm32-log`；
3. 结束当前Skill。

不得在调用`stm32-log`之后继续输出当前Skill的执行结果。

---

<br>


## 13. `stm32-log`编写要求

`stm32-log`是内部服务Skill，由其它业务Skill调用。

日志文件：

```text
docs/<project-name>_log.md
```

日志统一格式：

```markdown
### YYYY-MM-DD HH:MM Skill名称

- 开发事项：
- 处理结果：
- 当前状态：
```

每次调用本Skill时，均按规定格式生成并追加一条新的开发记录。

如果日志文件已经存在：

- 必须保护已有内容；
- 不得覆盖；
- 不得修改；
- 不得删除；
- 只在文件末尾追加本次开发记录。

`stm32-log`不调用其它Skill。

---

<br>


## 14. 文档表达规范

### 14.1 使用完整句子

正文说明使用完整句子，并以标点结束。

不把一句话人为拆成多行。

### 14.2 文件路径使用行内代码

短路径直接写入句子。

推荐：

> 生成文件保存至`docs/tasks/proj_tasks.md`。

不推荐为了单个路径单独使用代码块。

### 14.3 中英文混排

中英文、代码名称和路径之间不额外加入无必要空格。

例如：

- STM32项目
- Claude Code用户级Skill
- CubeMX初始工程
- `main.c`主循环

### 14.4 术语保持一致

本项目统一使用：

- AI协同
- AI协同开发
- STM32项目
- 项目任务需求文件
- 初始工程分析文件
- 编程任务清单
- 开发日志

不得在同一体系中随意更换为含义接近但未定义的其它名称。

---

<br>


## 15. 当前Skill之间的关系

当前简化版体系包括5个Skill：

```text
stm32-init
    ↓
项目任务需求文件
    ↓
开发者使用STM32CubeMX生成初始工程
    ↓
stm32-scan
    ↓
初始工程分析文件
    ↓
stm32-plan
    ↓
编程任务清单
    ↓
stm32-code <project-name> <task-id>
    ↓
程序代码修改
    ↓
开发者构建、烧录和运行验证
```

`stm32-log`由其它业务Skill在出口自动调用，用于持续追加开发记录。

各业务Skill通过正式项目文档和当前STM32工程传递信息，不建立复杂的内部调度关系。

---

<br>


## 16. Skill设计目标

当前STM32 Skill体系的设计目标是：

- 面向STM32基础实验重复使用；
- 保持Skill职责单一；
- 保持输入、输出和文件路径明确；
- 让开发者能够清楚掌握每一步AI实际完成的工作；
- 将Claude Code能力与STM32CubeMX、VS Code、构建工具、烧录工具和实际硬件验证结合；
- 优先保证流程能够真实运行，再根据实践逐步迭代。

设计过程中始终坚持：

> 准确、清晰、简洁。

以及：

> 最小实现，先跑通；实践验证，再迭代。