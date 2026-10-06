# AI4STM32

[English](README.md) | 简体中文

**基于Claude Code的STM32基础实验AI协同开发Skill**

[English](README.md) | 简体中文

- 项目名称：AI4STM32
- GitHub仓库：`AI-for-STM32-Skills`
- 作者：Huang Xiaofeng & Huang Shan
- 邮箱：ai4mcu@qq.com
- GitHub：[@youcans](https://github.com/youcans)

---

## 项目简介

AI4STM32是一套面向STM32基础实验开发的Claude Code用户级Skill体系，用于将项目任务需求整理、初始工程分析、编程任务规划、代码实现和开发记录组织为一套简化、可实际运行的AI协同STM32开发流程。

项目主要面向GPIO、定时器、PWM、ADC、DAC、UART、I2C、SPI、中断、DMA、RTC、看门狗、Flash等STM32基础实验，通过Claude Code与STM32CubeMX、VS Code及实际开发板配合完成开发。

AI4STM32当前由`stm32-init`、`stm32-scan`、`stm32-plan`、`stm32-code`和`stm32-log`5个Skill组成。

项目遵循：

> **准确、清晰、简洁。**

> **最小实现，先跑通；实践验证，再迭代。**

---

## 项目组成

AI4STM32主要由Claude Code Skill、硬件资料和STM32基础实验工程组成：

```text
AI4STM32
│
├── claude-user-skills/    # 提供AI协同开发方法
├── hardware/              # 提供开发所需硬件资料
└── demo_basics/           # 提供实际STM32基础实验工程，用于运行和验证上述方法
```

仓库中的`docs/`目录用于保存AI4STM32项目文档。

主要目录结构如下：

```text
AI-for-STM32-Skills/
├── claude-user-skills/
├── demo_basics/
├── docs/
├── hardware/
├── .gitignore
├── LICENSE
├── README.md
└── THIRD_PARTY_NOTICES.md
```

---

## Skill体系与开发流程

### Skill一览

| Skill | 主要功能 | 调用方式 |
|---|---|---|
| `stm32-init` | 获取项目目标、功能要求和硬件信息，生成项目任务需求文件 | `/stm32-init [user-description]` |
| `stm32-scan` | 结合项目任务需求分析STM32CubeMX生成的初始工程 | `/stm32-scan` |
| `stm32-plan` | 拆分并细化编程任务，生成编程任务清单 | `/stm32-plan` |
| `stm32-code` | 按照指定编程任务实施代码修改 | `/stm32-code <task-id>` |
| `stm32-log` | 追加项目开发日志 | 由其它Skill自动调用 |

### 典型开发流程

```text
开发者创建STM32项目目录
        ↓
进入项目目录并启动Claude Code
        ↓
stm32-init
        │
        └── 生成项目任务需求文件
        ↓
开发者使用STM32CubeMX完成配置
并生成.ioc文件和初始工程
        ↓
开发者完成初始Build
        ↓
stm32-scan
        │
        └── 生成初始工程分析文件
        ↓
stm32-plan
        │
        ├── 第一阶段：生成编程任务列表
        │        ↓
        │     开发者确认
        │
        └── 第二阶段：逐项细化编程任务
        ↓
stm32-code T01
        │
        └── 实施当前编程任务代码修改
        ↓
开发者Build、烧录、调试和硬件运行验证
        ↓
stm32-code T02
        ↓
继续实施后续编程任务
```

`stm32-log`不作为独立开发阶段使用，而是在其它业务Skill结束前自动调用，用于追加开发记录。

---

## 快速开始

### 1. 获取项目

AI4STM32的硬件资料路径按照默认安装位置配置，因此建议将仓库克隆到：

```text
C:\AI-for-STM32-Skills
```

在Windows PowerShell中执行：

```powershell
git clone https://github.com/youcans/AI-for-STM32-Skills.git C:\AI-for-STM32-Skills
```

后续更新项目：

```powershell
cd C:\AI-for-STM32-Skills
git pull
```

### 2. 安装Skill

创建Claude Code用户级Skill目录：

```powershell
New-Item -ItemType Directory -Path "$HOME\.claude\skills" -Force | Out-Null
```

将AI4STM32 Skill复制到用户级Skill目录：

```powershell
Copy-Item -Path "C:\AI-for-STM32-Skills\claude-user-skills\*" -Destination "$HOME\.claude\skills\" -Recurse -Force
```

### 3. 创建STM32项目并启动Claude Code

STM32项目可以位于任意工作路径。例如：

```powershell
mkdir D:\workspace\Demo01
cd D:\workspace\Demo01
claude
```

Claude Code启动时所在目录即作为STM32项目根目录，项目名称取当前目录名称。本例中的项目名称为`Demo01`。

### 4. 初始化项目

没有用户需求描述文件时：

```text
/stm32-init
```

已有用户需求描述文件时：

```text
/stm32-init [user-description]
```

完成项目任务需求整理后，根据提示使用STM32CubeMX完成硬件配置并生成初始工程。

### 5. 继续开发

初始工程生成并完成初始Build后，依次执行：

```text
/stm32-scan
```

```text
/stm32-plan
```

实施第一个编程任务：

```text
/stm32-code T01
```

代码修改完成后，由开发者按照编程任务清单中的验证要求完成Build、烧录、调试和实际硬件运行验证。

继续实施后续任务：

```text
/stm32-code T02
```

详细安装、更新、环境检查和完整使用方法参见：

[安装与使用指南](docs/Installation_Guide.md)

---

## 基础实验

`demo_basics/`保存按照AI4STM32 Skill体系开展的STM32基础例程测试工程，用于实际运行和验证AI协同开发方法。

当前包含15个基础实验，按照实验实施顺序排列：

```text
demo_basics/
├── GPIO_IOToggle/      # GPIO输出翻转闪灯实验
├── GPIO_BUTTON/        # GPIO按键输入实验
├── GPIO_EXTI/          # GPIO外部中断实验
├── TIM_IT_500ms/       # 定时器500ms周期中断实验
├── TIM_PWM_1kHz/       # 定时器1kHz PWM输出实验
├── TIM1_PWM_Comp/      # TIM1互补PWM输出实验
├── ADC_IT/             # ADC模拟量数据采集实验
├── DAC_Output/         # DAC模拟量信号输出实验
├── USART_Echo/         # USART数据收发回显实验
├── LPUART_DMA/         # LPUART DMA通信实验
├── I2C_Master_Slave/   # I2C主从通信实验
├── SPI_Master_Slave/   # SPI主从通信实验
├── IWDG_Reset/         # 独立看门狗复位实验
├── RTC_Alarm/          # RTC日历与闹钟实验
└── Flash_Parameter/    # Flash参数存储实验
```

每个目录都是一个独立STM32实验项目，包含STM32CubeMX工程、源码以及运行AI4STM32 Skill过程中形成的项目开发文档。

这些实验覆盖GPIO、定时器、PWM、ADC/DAC、USART/LPUART、I2C、SPI、中断、DMA、RTC、独立看门狗和Flash等典型STM32开发内容。

---

## 支持的硬件

当前硬件资料库位于：

```text
C:\AI-for-STM32-Skills\hardware\
```

目前包含：

```text
hardware/
├── nucleo-g431rb/
└── nucleo-c542rc/
```

当前支持的开发板：

- NUCLEO-G431RB
- NUCLEO-C542RC

每个开发板目录包含AI4STM32整理的硬件概述文件，以及数据手册、开发板原理图、用户手册、BOM等相关硬件资料。

`stm32-init`根据开发板型号读取对应硬件资料，为项目任务需求整理提供硬件平台、MCU引脚和外围器件连接信息。

---

## 项目文档

AI4STM32项目文档保存在`docs/`目录。

公开使用相关文档包括：

- [安装与使用指南](docs/Installation_Guide.md)
- [AI4MCU Skill编写规范](docs/AI4MCU_Skill编写规范.md)

具体STM32实验项目运行Skill后，会在各自项目根目录下形成独立的`docs/`开发文档目录，用于保存项目任务需求、初始工程分析、编程任务清单和开发日志。

---

## 许可证

AI4STM32项目自行编写的Skill、代码和文档采用MIT License，具体许可条款见：

[LICENSE](LICENSE)

项目中引用或收录的第三方资料和代码仍遵循其原有版权和许可条件，相关说明见：

[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)

---

## 联系方式

- 项目名称：AI4STM32
- GitHub仓库：`AI-for-STM32-Skills`
- 作者：Huang Xiaofeng & Huang Shan
- 邮箱：ai4mcu@qq.com
- GitHub：[@youcans](https://github.com/youcans)