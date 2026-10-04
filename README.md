# AI4MCU

**基于 Claude Code 的 STM32 AI 协同开发流程**

> **作者**：Huang Xiaofeng [ai4mcu@gmail.com](mailto:ai4mcu@gmail.com), Huang Shan [youcans@qq.com](mailto:youcans@qq.com)

---

## 📖 项目简介

本项目面向 STM32 嵌入式软件开发，探索基于 Claude Code 的 AI 协同开发方法，并建立一套覆盖需求分析、系统设计、STM32CubeMX 配置、程序设计、代码实施、工程构建、调试测试和项目维护的完整开发流程。

项目以 `stm32-project-flow` 作为流程控制入口，以一组面向 STM32 开发任务的 Claude Code Skill 作为专业工作单元，通过“流程控制 + 专业 Skill + 开发者操作”的方式组织实际工程开发，使 Claude Code 能够在明确的工程上下文、正式开发文档和受控开发流程中参与 STM32 软件开发。

本仓库同时作为《基于 Claude Code 的 STM32 开发实战》的配套项目，用于保存项目流程、Skill、示例工程、开发文档和相关资源。

---

## 🎯 项目背景与定位

Claude Code 等 AI 编程工具已经能够读取工程文件、分析代码、修改程序并调用开发工具，但 STM32 开发不仅包括代码编写，还涉及需求分析、硬件资源规划、STM32CubeMX 配置、代码生成、工程构建、烧录、调试、测试以及大量需要开发者参与的实际操作。

因此，本项目不把 AI 编程简单理解为“让 AI 自动生成代码”，而是将 STM32 开发过程拆分为一系列具有明确输入、输出和职责边界的开发活动。

整个开发过程主要由三部分组成：

* **项目流程控制**：由 `stm32-project-flow` 判断当前开发状态、检查前置条件、调用 Skill、衔接人工操作并控制流程推进；
* **专业 Skill**：分别负责需求分析、架构设计、代码规划、代码实施、错误分析、测试设计等专业工作；
* **开发者操作**：完成 STM32CubeMX 配置、Generate Code、Build、烧录、调试、实际测试以及必要的人工确认。

通过这种方式，将 AI 的代码分析和生成能力嵌入实际 STM32 工程开发流程，同时保留必要的开发者控制和硬件验证环节。

---

## 🧩 AI4MCU Skill 体系

项目目前规划了 19 个 STM32 开发 Skill：

| Skill                    | 主要功能                |
| :----------------------- | :------------------ |
| `stm32-tech-docs`        | STM32 技术资料整理与技术文档分析 |
| `stm32-project-reqs`     | 项目需求分析              |
| `stm32-software-reqs`    | 软件需求分析              |
| `stm32-resource-review`  | MCU资源与外设需求审查        |
| `stm32-architecture`     | 软件架构设计              |
| `stm32-cubemx-plan`      | STM32CubeMX 配置规划    |
| `stm32-cubemx-review`    | CubeMX 配置结果审查       |
| `stm32-project-analysis` | STM32 初始工程分析        |
| `stm32-detail-design`    | 软件详细设计              |
| `stm32-task-plan`        | 编程任务规划              |
| `stm32-code-plan`        | 单项编程方案制定            |
| `stm32-code-write`       | 程序代码实施与修改           |
| `stm32-code-review`      | 代码变更审查              |
| `stm32-build-debug`      | 构建错误分析              |
| `stm32-module-debug`     | 模块运行错误分析            |
| `stm32-test-plan`        | 系统测试方案设计            |
| `stm32-system-debug`     | 系统测试错误分析            |
| `stm32-dev-log`          | 开发过程与流程状态记录         |
| `stm32-change-analysis`  | 需求变更与二次开发分析         |

各 Skill 只负责自身专业工作，不自行控制完整项目流程。Skill 之间的调用顺序、人工操作衔接和开发状态恢复由 `stm32-project-flow` 统一组织。

---

## 🔄 项目开发流程

AI4MCU 使用：

`/stm32-project-flow <项目名称> [F0x]`

作为项目流程控制入口。

正常开发过程包括：

**需求分析 → 软件设计 → CubeMX 配置 → 初始工程生成与分析 → 详细设计 → 编程任务规划 → 单项代码实施 → 构建与模块验证 → 系统测试 → 项目维护。**

进入编程阶段后，以单个编程任务 `T0x` 为基本闭环进行推进。

典型的单项编程任务流程为：

```text
制定当前T0x编程方案
        ↓
代码实施 / 修改
        ↓
代码变更审查
        ↓
写入正式工程
        ↓
Build
        ↓
模块验证
        ↓
任务验证完成
        ↓
建立新的Git代码基线
        ↓
进入下一编程任务
```

如果 Build、模块验证或后续测试发现问题，则先完成对应错误分析，再重新进入代码修改和代码审查流程。

未经代码审查通过和开发者批准的代码，不直接写入正式工程。

---


## 🚀 快速开始

1. **准备 Claude Code 和 STM32 开发环境。**

   安装 Claude Code、STM32CubeMX、Visual Studio Code、STM32 VS Code Extension 以及对应 STM32 工具链，并确认能够正常创建和构建 STM32 工程。

2. **安装 AI4MCU Skill。**

   将项目提供的 STM32 Skill 安装到 Claude Code 用户级 Skill 目录，例如：

   `~/.claude/skills/stm32-code-write/`

   每个 Skill 的核心文件为 `SKILL.md`。

3. **建立项目目录。**

   新建项目后准备 `docs/`、`software/` 等项目目录，并将需求资料、芯片资料及相关参考文件放入对应位置。

4. **启动项目流程。**

   在项目根目录启动 Claude Code，并调用：

   `/stm32-project-flow <项目名称>`

   项目流程将根据当前开发状态逐步执行需求分析、设计、工程配置、代码实施和测试等工作。

5. **完成需要开发者参与的操作。**

   在流程提示下使用 STM32CubeMX 完成配置和 Generate Code，并在 VS Code 中完成 Configure、Build、烧录、调试和实际硬件测试。

6. **按照开发记录继续项目。**

   项目流程状态统一记录在：

   `docs/logs/development_log.md`

   当一次开发无法立即完成时，可以结束当前会话。后续重新进入项目流程后，系统根据开发记录恢复当前工作位置。

---

## 🔐 Git 与代码变更管理

AI4MCU 将 Git 作为正式代码实施阶段的重要基线机制。

初始 STM32 工程完成 Generate Code、VS Code Configure 和初始 Build，并完成初始工程分析后，建立初始 Git 代码基线。

代码实施过程中，`stm32-code-write`不直接修改正式工程，而是首先形成：

`.claude/tmp/code_change_<task-id>.diff`

代码差异经过 `stm32-code-review` 审查并获得开发者批准后，才写入正式 STM32 工程。

当前编程任务完成构建、模块验证和后续验证后，再建立新的 Git 代码基线，作为下一编程任务的起点。

---

## 📝 开发记录与流程恢复

项目使用：

`docs/logs/development_log.md`

统一记录项目流程状态。

开发记录主要用于回答三个问题：

* 项目已经完成了哪些工作；
* 当前正在进行什么工作；
* 下一步应该从哪里继续。

Skill 内部的临时分析过程不作为项目流程状态记录。项目流程恢复以开发记录为主要依据，并结合正式成果和必要的中间文件进行状态一致性检查。

---



## 📄 许可证

本项目主要用于嵌入式软件 AI 编程方法研究、教学和技术交流。

代码、Skill、文档及第三方资料的具体许可方式，以仓库中的许可证文件以及各目录或原始资料中的声明为准。

---

## 📬 联系作者

如有问题、建议或项目交流，欢迎通过以下方式联系：

* 邮箱：[ai4mcu@gmail.com](mailto:ai4mcu@gmail.com)
* GitHub：[@youcans](https://github.com/youcans)

---

> **让 AI 不只是生成代码，而是参与需求、设计、实现、调试和验证全过程，在真实 STM32 工程中形成可执行、可检查、可恢复的 AI 协同开发流程。**


---

## 📄 许可证

本项目仅供学习与参考使用，具体许可证以各目录下工程文件的声明为准。

---

## 📬 联系作者

如有问题或建议，欢迎通过以下方式联系：

- 邮箱：ai4mcu@gmail.com
- GitHub：[@youcans](https://github.com/youcans)

---

> **“在实践中遇到问题、借助 AI 理解问题、回到硬件验证结论”——让 AI 成为嵌入式开发的得力协作者。**