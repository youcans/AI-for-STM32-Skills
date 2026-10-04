# AI-for-STM32-Skills

**基于Claude Code的STM32基础实验AI协同开发Skill**

- 作者：Huang Xiaofeng & Huang Shan
- GitHub：[@youcans](https://github.com/youcans)

---

## 📖 项目简介

AI-for-STM32-Skills是一套面向STM32基础实验开发的Claude Code用户级Skill，主要用于需求整理、初始工程分析、编程任务规划、代码实现和开发日志记录。

本项目重点面向GPIO、ADC、DAC、UART、定时器、PWM、中断、DMA以及主循环、事件驱动、状态机等STM32基础外设和程序运行机制实验。

项目不追求复杂的软件工程流程和高度自动化，而是根据STM32基础实验功能明确、工程规模较小、开发任务相对简单的特点，将AI协同开发过程简化为少量职责明确、能够实际运行的Skill。

基本原则是：

> **准确、清晰、简洁。**

开发思路是：

> **最小实现，先跑通；实践验证，再迭代。**

---

## 🎯 项目背景与定位

Claude Code能够读取工程文件、分析源码并直接修改程序，但STM32开发并不只是编写代码，还涉及STM32CubeMX配置、代码生成、工程构建、程序烧录和实际硬件运行验证。

因此，本项目不把AI编程简单理解为“让AI生成STM32代码”，而是把Claude Code嵌入真实STM32开发过程，由AI与开发者分别承担适合自己的工作。

整个开发过程主要由三部分组成：

- **Claude Code Skill**：负责需求整理、工程分析、任务规划、代码修改和开发记录；
- **开发者**：负责关键内容确认、任务控制、Build、烧录、调试和实际硬件运行验证；
- **STM32开发工具**：STM32CubeMX负责底层硬件配置和初始工程生成，VS Code及相关工具负责构建、烧录和调试。

本项目采用开发者主动调用Skill的方式推进项目，不设置统一流程调度Skill。开发者根据当前开发阶段决定调用哪个Skill，并在需要人工操作时完成对应工作。

---

## 🧩 STM32 Skill体系

当前项目包含5个STM32开发Skill：

| Skill | 主要功能 |
|---|---|
| `stm32-init` | 项目初始化、需求整理和硬件信息准备 |
| `stm32-scan` | STM32CubeMX初始工程分析 |
| `stm32-plan` | 编程任务拆分和任务规划 |
| `stm32-code` | 指定编程任务的代码实施 |
| `stm32-log` | 项目开发日志追加记录 |

各Skill保持职责单一，通过项目正式文档和当前STM32工程传递开发信息。

`stm32-log`作为内部服务Skill，由其它业务Skill在出口自动调用，不作为独立开发阶段使用。

---

## 🔄 AI协同开发流程

典型开发流程如下：

```text
用户提出实验需求
        │
        ▼
stm32-init
        │
        ├── 建立项目目录
        └── 生成项目任务需求文件
        │
        ▼
开发者使用STM32CubeMX完成配置
并生成初始工程
        │
        ▼
开发者完成初始Build
确认初始工程能够正常构建
        │
        ▼
stm32-scan
        │
        └── 生成初始工程分析文件
        │
        ▼
stm32-plan
        │
        ├── 第一阶段：确定任务拆分
        │        ↓
        │     开发者确认
        │
        └── 第二阶段：细化各编程任务
        │
        ▼
stm32-code <project-name> <task-id>
        │
        └── 实施当前T0x代码修改
        │
        ▼
开发者Build、烧录和运行验证
        │
        ▼
当前任务完成
        │
        ▼
继续下一个T0x
```

基础实验中的编程任务按照相对完整的阶段性目标划分，不按照单个文件、单个函数、单条语句或单个修改点机械拆分。

任务统一采用：

`T01、T02、T03……`

连续编号，编号同时表示建议实施顺序。

---

## 🛠️ 各Skill主要职责

### `stm32-init`

调用方式：

`/stm32-init <project-name> [user-description]`

主要完成：

- 建立项目目录；
- 获取实验任务和功能要求；
- 读取开发板硬件资料；
- 整理硬件定义；
- 生成项目任务需求文件；
- 初始化项目开发日志。

主要输出：

`docs/requirements/proj_requirements.md`

正常完成后，由开发者使用STM32CubeMX完成配置并生成初始工程。

---

### `stm32-scan`

调用方式：

`/stm32-scan <project-name>`

主要读取：

- `<project-name>.ioc`
- `CMakeLists.txt`
- `Core/Inc/*.h`
- `Core/Src/*.c`

主要分析：

- 工程基本信息；
- 工程目录与构建结构；
- 程序初始化与运行结构；
- 外设配置、初始化函数和句柄；
- 中断与HAL回调；
- 后续代码接入位置。

主要输出：

`docs/analysis/proj_init_scan.md`

分析仅限规定输入文件和模板要求，不扩展到无关文件或额外深入分析。

---

### `stm32-plan`

调用方式：

`/stm32-plan <project-name>`

主要输入：

- `docs/requirements/proj_requirements.md`
- `docs/analysis/proj_init_scan.md`

任务规划分为两个阶段。

第一阶段确定：

- 任务编号；
- 任务名称；
- 任务概要。

形成完整的初步任务清单后，由开发者集中审阅确认。

第二阶段对确认后的每项任务进行细化，明确：

- 任务目标；
- 涉及文件；
- 实施要点；
- 验证要求。

主要输出：

`docs/tasks/proj_tasks.md`

---

### `stm32-code`

调用方式：

`/stm32-code <project-name> <task-id>`

例如：

`/stm32-code DemoF02 T01`

每次只实施一个指定任务。

Skill读取：

- `docs/tasks/proj_tasks.md`
- `docs/analysis/proj_init_scan.md`
- 当前任务规定涉及的`.c`和`.h`源码文件。

代码修改仅允许发生在当前任务规定的文件和代码范围内。

`stm32-code`不自动执行：

- Build；
- 编译；
- STM32CubeMX Generate Code；
- 程序烧录；
- 实际硬件运行验证。

代码修改完成后，Skill显示任务完成摘要和验证要求，由开发者自行完成后续验证。

---

### `stm32-log`

`stm32-log`由其它业务Skill在退出前自动调用。

日志文件：

`docs/<project-name>_log.md`

每次调用追加一条开发记录：

```markdown
### YYYY-MM-DD HH:MM Skill名称

- 开发事项：
- 处理结果：
- 当前状态：
```

已有日志内容必须保留，不覆盖、不修改、不删除，只在文件末尾追加新的记录。

---

## 🚀 快速开始

### 1. 准备开发环境

需要准备：

- Claude Code；
- STM32CubeMX；
- Visual Studio Code；
- STM32 VS Code Extension及相应STM32工具链；
- NUCLEO开发板或其它已支持硬件。

当前支持开发板：

- `nucleo-g431rb`
- `nucleo-c542rc`

---

### 2. 安装Skill

Skill源码目录：

`C:\AI4MCU\claude-user-skills\`

Claude Code用户级Skill目录：

`~/.claude/skills/`

Windows PowerShell中可执行：

```powershell
@("stm32-init","stm32-scan","stm32-plan","stm32-code","stm32-log") | ForEach-Object {
    Remove-Item "$HOME\.claude\skills\$_" -Recurse -Force -ErrorAction SilentlyContinue
}

Copy-Item "C:\AI4MCU\claude-user-skills\*" "$HOME\.claude\skills\" -Recurse
```

---

### 3. 启动项目

进入STM32工作目录：

```powershell
cd C:\STM32
```

启动Claude Code：

```powershell
claude
```

初始化项目：

```text
/stm32-init DemoF01
```

---

### 4. 使用STM32CubeMX生成初始工程

根据`stm32-init`形成的项目任务需求完成STM32CubeMX配置并Generate Code。

在VS Code中完成初始Build，确认初始工程可以正常构建。

---

### 5. 分析初始工程

```text
/stm32-scan DemoF01
```

---

### 6. 规划编程任务

```text
/stm32-plan DemoF01
```

确认初步任务清单后，Skill继续生成完整编程任务说明。

---

### 7. 实施代码任务

例如：

```text
/stm32-code DemoF01 T01
```

代码修改完成后，由开发者进行Build、烧录和实际开发板运行验证。

当前任务验证完成后，再执行下一任务：

```text
/stm32-code DemoF01 T02
```

---

## 📁 项目文档结构

STM32项目统一建立在：

`C:\STM32\<project-name>`

主要开发文档结构：

```text
<project-name>/
└── docs/
    ├── tmp/
    ├── requirements/
    │   └── proj_requirements.md
    ├── analysis/
    │   └── proj_init_scan.md
    ├── tasks/
    │   └── proj_tasks.md
    └── <project-name>_log.md
```

各文件主要作用：

- `proj_requirements.md`：项目任务需求和硬件定义；
- `proj_init_scan.md`：STM32CubeMX初始工程分析结果；
- `proj_tasks.md`：编程任务规划；
- `<project-name>_log.md`：项目开发过程记录。

---

## 🤝 AI与开发者的职责边界

### Claude Code负责

- 整理项目需求；
- 分析STM32初始工程；
- 拆分和细化编程任务；
- 按指定任务修改代码；
- 记录开发过程。

### 开发者负责

- 确认项目需求；
- 完成STM32CubeMX配置；
- 确认任务拆分；
- Build；
- 烧录；
- 调试；
- 实际硬件运行验证；
- 控制任务实施顺序。

本项目不试图让AI替代成熟的STM32开发工具，也不试图取消开发者对真实硬件开发过程的控制。

---

## ✅ 当前验证情况

当前5个Skill已经完成第一版设计，并使用NUCLEO-G431RB基础实验进行了实际运行验证。

已验证的基本链路包括：

```text
stm32-init
    ↓
STM32CubeMX生成初始工程
    ↓
初始Build
    ↓
stm32-scan
    ↓
stm32-plan
    ↓
stm32-code
    ↓
开发者Build
    ↓
烧录
    ↓
实际硬件运行验证
```

在LED闪烁实验中，`stm32-code`按照任务规划仅修改指定的`main.c` USER CODE区域，没有自动执行构建或烧录。

代码修改后由开发者完成Build和程序烧录，NUCLEO-G431RB板载LD2按照预期运行，验证了当前简化AI协同开发链路能够实际工作。

---

## 🔭 后续计划

当前阶段不以增加Skill数量为主要目标，而是继续使用真实STM32基础实验验证现有体系。

重点包括：

- GPIO实验；
- ADC和DAC实验；
- UART通信实验；
- 定时器和PWM实验；
- 中断和DMA实验；
- 主循环多任务实验；
- 事件驱动实验；
- 状态机实验；
- 多个`T0x`连续实施过程。

根据实际运行结果，对Skill、模板和开发流程进行必要的局部调整。

---

## 📄 许可证

本项目主要用于嵌入式软件AI编程方法研究、教学和技术交流。

代码、Skill、文档以及第三方资料的具体许可方式，以仓库中的许可证文件以及各目录中的相关声明为准。

---

## 📬 联系作者

如有问题、建议或项目交流，欢迎联系：

- 作者：Huang Xiaofeng & Huang Shan
- 邮箱：ai4mcu@qq.com
- GitHub：[@youcans](https://github.com/youcans)

---

> **让AI不只是生成STM32代码，而是在明确职责和开发者控制下参与需求整理、工程理解、任务规划和代码实现，并最终回到真实硬件完成验证。**