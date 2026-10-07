# AI4STM32 Skill安装与使用指南

[English](Installation_Guide.md) | 简体中文

本文介绍AI4STM32 Skill的安装、更新和基本使用方法。项目介绍、Skill体系和基础实验说明参见项目根目录下的`README_zh-CN.md`。

---

## 1. 安装

### 1.1 安装前准备

使用AI4STM32 Skill前，需要准备以下开发环境：

- Windows；
- Windows PowerShell；
- Git；
- Claude Code；
- STM32CubeMX；
- Visual Studio Code；
- STM32 VS Code Extension及相应STM32工具链。

本文中的安装命令均在Windows PowerShell中执行。PowerShell命令提示符通常以`PS`开头，例如：

```text
PS C:\Users\Administrator>
```

AI4STM32 Skill当前的硬件资料路径固定为：

```text
C:\AI-for-STM32-Skills\hardware\
```

因此，应将AI4STM32 Skill仓库安装到：

```text
C:\AI-for-STM32-Skills
```

### 1.2 获取AI4STM32 Skill

1. 打开Windows PowerShell。

2. 首次安装时，执行：

```powershell
git clone https://github.com/youcans/AI-for-STM32-Skills.git C:\AI-for-STM32-Skills
```

3. 检查项目目录：

```powershell
Get-ChildItem C:\AI-for-STM32-Skills
```

正常情况下应能够看到：

```text
claude-user-skills
demo_basics
demo_basics_ref
docs
hardware
LICENSE
README.md
README_zh-CN.md
THIRD_PARTY_NOTICES.md
```

### 1.3 安装Claude Code用户级Skill

1. 创建Claude Code用户级Skill目录：

```powershell
New-Item -ItemType Directory -Path "$HOME\.claude\skills" -Force | Out-Null
```

2. 清理可能存在的旧版STM32 Skill：

```powershell
Remove-Item "$HOME\.claude\skills\stm32-init" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-scan" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-plan" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-code" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-log" -Recurse -Force -ErrorAction SilentlyContinue
```

3. 将AI4STM32 Skill复制到Claude Code用户级Skill目录：

```powershell
Copy-Item -Path "C:\AI-for-STM32-Skills\claude-user-skills\*" -Destination "$HOME\.claude\skills\" -Recurse -Force
```

### 1.4 检查安装结果

执行：

```powershell
Get-ChildItem "$HOME\.claude\skills" -Directory | Where-Object Name -Like "stm32-*"
```

正常情况下应包含：

```text
stm32-init
stm32-scan
stm32-plan
stm32-code
stm32-log
```

也可以检查某个Skill，例如：

```powershell
Get-ChildItem "$HOME\.claude\skills\stm32-init" -Recurse
```

应能够看到`SKILL.md`及其相关资源文件。

### 1.5 检查硬件资料

执行：

```powershell
Get-ChildItem C:\AI-for-STM32-Skills\hardware
```

当前应包含：

```text
nucleo-g431rb
nucleo-c542rc
```

例如检查NUCLEO-G431RB硬件资料：

```powershell
Get-ChildItem C:\AI-for-STM32-Skills\hardware\nucleo-g431rb
```

其中应包含：

```text
hardware_overview_g431rb.md
```

以及数据手册、原理图、用户手册、BOM等相关资料。

### 1.6 更新AI4STM32 Skill

1. 进入AI4STM32 Skill仓库：

```powershell
cd C:\AI-for-STM32-Skills
```

2. 拉取最新版本：

```powershell
git pull
```

3. 清理已经安装的旧版Skill：

```powershell
Remove-Item "$HOME\.claude\skills\stm32-init" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-scan" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-plan" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-code" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-log" -Recurse -Force -ErrorAction SilentlyContinue
```

4. 重新复制最新版Skill：

```powershell
Copy-Item -Path "C:\AI-for-STM32-Skills\claude-user-skills\*" -Destination "$HOME\.claude\skills\" -Recurse -Force
```

---

## 2. 使用基础实验

AI4STM32 Skill在`demo_basics/`中提供15个STM32基础实验起始项目。每个实验目录只保留已经配置好的STM32CubeMX`.ioc`文件和示例用户需求文件`requirements.txt`，用于学习和测试AI4STM32 Skill开发流程。

以下以`GPIO_EXTI`实验为例说明基本使用方法。

### 2.1 选择基础实验

进入：

```text
C:\AI-for-STM32-Skills\demo_basics\GPIO_EXTI
```

该实验目录提供以下两个起始文件：

```text
GPIO_EXTI/
├── GPIO_EXTI.ioc        # 已完成基础配置的STM32CubeMX工程文件
└── requirements.txt     # 示例用户需求描述文件
```

其中：

- `GPIO_EXTI.ioc`用于生成STM32初始工程；
- `requirements.txt`用于向`stm32-init`提供实验目标、功能要求及其它必要的用户需求。

其它基础实验按照相同方式使用。

### 2.2 启动Claude Code

1. 打开Windows PowerShell。

2. 进入实验目录：

```powershell
cd C:\AI-for-STM32-Skills\demo_basics\GPIO_EXTI
```

3. 启动Claude Code：

```powershell
claude
```

Claude Code启动时所在目录作为当前STM32项目根目录，项目名称取当前目录名称。

本例中：

```text
<project-name> = GPIO_EXTI
```

### 2.3 初始化项目

当前实验目录中已经提供：

```text
requirements.txt
```

因此在Claude Code中直接执行：

```text
/stm32-init
```

调用时未提供`<User_Req>`参数，`stm32-init`会自动读取当前项目根目录下的`requirements.txt`。

如果需要使用其它用户需求文件，可以执行：

```text
/stm32-init <User_Req>
```

其中，`<User_Req>`为开发者指定的用户需求文件路径和名称，例如：

```text
/stm32-init D:\requirements\gpio_exti.txt
```

如果未找到用户需求文件、文件读取失败，或者需求内容不足以确定项目目标和功能要求，`stm32-init`会通过对话交互获取和完善相关需求。

`stm32-init`结合用户需求和AI4STM32 Skill硬件资料生成项目任务需求文件。

开发者确认后，正式项目任务需求文件保存至：

```text
docs/requirements/proj_requirements.md
```

### 2.4 使用STM32CubeMX生成初始工程

`demo_basics/`中的基础实验已经提供配置完成的`.ioc`文件，因此在测试AI4STM32 Skill开发流程时，不需要从头完成STM32CubeMX配置。

1. 使用STM32CubeMX打开：

```text
GPIO_EXTI.ioc
```

2. 根据需要检查已有配置。

3. 使用STM32CubeMX生成初始工程。

4. 确认生成的工程位于当前实验项目目录。

5. 使用VS Code完成初始Build，确认STM32CubeMX生成的初始工程能够正常构建。

### 2.5 分析初始工程

完成STM32CubeMX工程生成和初始Build后，在Claude Code中执行：

```text
/stm32-scan
```

`stm32-scan`结合项目任务需求，对当前STM32CubeMX初始工程进行分析，并生成：

```text
docs/analysis/init_scan.md
```

完成后，根据提示进入编程任务规划。

### 2.6 规划编程任务

执行：

```text
/stm32-plan
```

编程任务规划分为两个阶段：

1. 第一阶段生成编程任务列表，包括编程任务编号、任务名称和任务概要，并提交开发者审阅。
2. 开发者确认编程任务列表，或通过对话提出修改意见。
3. 编程任务列表确认后，Skill进入第二阶段，逐项细化任务目标、涉及文件、主要修改位置、实施要点和验证要求。
4. 完成后形成正式编程任务清单：

```text
docs/tasks/task_list.md
```

编程任务采用：

```text
T01、T02、T03……
```

连续编号。

### 2.7 实施编程任务

按照编程任务清单，从第一个编程任务开始实施。

例如：

```text
/stm32-code T01
```

`stm32-code`读取编程任务清单中`T01`对应的详细说明，并严格按照任务目标、涉及文件、主要修改位置和实施要点修改代码。

`stm32-code`只负责代码修改，不自动执行：

- Build；
- 编译；
- STM32CubeMX工程生成；
- 程序烧录；
- 调试；
- 实际硬件运行验证。

代码修改完成后，Skill输出当前编程任务摘要，并提示开发者按照编程任务清单中的验证要求进行验证。

### 2.8 验证并继续后续任务

1. 根据当前编程任务的验证要求完成：

```text
Build
  ↓
程序烧录
  ↓
调试或实际硬件运行验证
```

2. 当前编程任务验证完成后，实施下一项任务，例如：

```text
/stm32-code T02
```

3. 按照编程任务编号继续实施和验证后续任务，直至完成全部编程任务。

### 2.9 开发日志

`stm32-log`由`stm32-init`、`stm32-scan`、`stm32-plan`和`stm32-code`在出口自动调用，开发者不需要单独执行。

项目开发日志保存至：

```text
docs/<project-name>_log.md
```

例如：

```text
docs/GPIO_EXTI_log.md
```

### 2.10 查看完整参考项目

`demo_basics_ref/`保存已经按照AI4STM32 Skill完整开发流程完成开发和验证的参考项目。

当前提供7个完整参考项目，可用于查看：

- STM32CubeMX生成后的完整工程；
- Skill生成的项目开发文档；
- 编程任务规划结果；
- 完成代码修改后的源码；
- 完整开发流程的最终项目状态。

`demo_basics/`用于用户自行运行AI4STM32 Skill开发流程，`demo_basics_ref/`用于查看已经完成开发后的参考结果。

---

## 3. 在自己的STM32项目中使用

AI4STM32 Skill不仅可以用于`demo_basics/`中的基础实验，也可以用于开发者自己的STM32项目。

1. 创建或进入STM32项目目录，例如：

```powershell
mkdir D:\workspace\MySTM32Project
cd D:\workspace\MySTM32Project
```

2. 准备用户需求描述文件，并保存为当前项目根目录下的：

```text
requirements.txt
```

`requirements.txt`用于描述项目目标、功能要求以及其它必要的开发需求。

例如：

```text
D:\workspace\MySTM32Project\
├── requirements.txt
└── ...
```

3. 在当前项目目录启动Claude Code：

```powershell
claude
```

4. 执行：

```text
/stm32-init
```

`stm32-init`会自动读取当前项目根目录下的`requirements.txt`。

如果需要使用其它需求文件，也可以执行：

```text
/stm32-init <User_Req>
```

如果未找到需求文件、文件读取失败或需求内容不充分，`stm32-init`会通过对话交互获取和完善项目需求。

5. 根据生成的项目任务需求文件，使用STM32CubeMX完成MCU、引脚、时钟、外设、中断和DMA等配置，并在当前项目目录生成初始工程。

6. 使用VS Code完成初始Build，确认初始工程能够正常构建。

7. 继续依次执行：

```text
/stm32-scan
```

```text
/stm32-plan
```

```text
/stm32-code T01
```

8. 每个编程任务完成后，由开发者进行Build、烧录、调试和实际硬件运行验证，再继续实施下一项任务。

---

## 4. 常见问题

### 4.1 PowerShell提示命令无法识别

本文中的`Remove-Item`、`Copy-Item`、`Get-ChildItem`等均为PowerShell命令。

如果出现类似：

```text
'Remove-Item' 不是内部或外部命令
```

通常说明当前使用的是Windows命令提示符`cmd.exe`，而不是PowerShell。

请打开Windows PowerShell，并确认命令提示符类似：

```text
PS C:\Users\Administrator>
```

### 4.2 Claude Code无法找到STM32 Skill

执行：

```powershell
Get-ChildItem "$HOME\.claude\skills" -Directory | Where-Object Name -Like "stm32-*"
```

确认以下5个目录存在：

```text
stm32-init
stm32-scan
stm32-plan
stm32-code
stm32-log
```

如果不存在，重新执行Skill安装命令。

### 4.3 `stm32-init`无法读取硬件资料

检查AI4STM32 Skill是否安装在：

```text
C:\AI-for-STM32-Skills
```

并确认硬件资料目录存在：

```powershell
Test-Path C:\AI-for-STM32-Skills\hardware
```

正常情况下应返回：

```text
True
```

### 4.4 `stm32-init`没有读取预期的需求文件

如果执行：

```text
/stm32-init
```

`stm32-init`默认读取当前项目根目录下的：

```text
requirements.txt
```

请确认：

1. Claude Code在正确的STM32项目根目录中启动；
2. `requirements.txt`位于该项目根目录；
3. 文件名称准确为`requirements.txt`。

如果需要读取其它用户需求文件，应显式指定：

```text
/stm32-init <User_Req>
```

例如：

```text
/stm32-init D:\requirements\my_project.txt
```

### 4.5 Skill使用了错误的项目名称或项目文件

AI4STM32 Skill将Claude Code启动时所在目录作为STM32项目根目录，并以当前目录名称作为项目名称。

调用Skill前，应先进入正确的STM32项目目录，再启动Claude Code。

例如：

```powershell
cd C:\AI-for-STM32-Skills\demo_basics\GPIO_EXTI
claude
```

此时项目名称为：

```text
GPIO_EXTI
```

### 4.6 更新仓库后Skill没有变化

`git pull`只更新：

```text
C:\AI-for-STM32-Skills
```

不会自动更新已经安装到 `$HOME\.claude\skills\`中的Skill。

更新仓库后，需要重新清理旧版Skill并执行复制命令，将最新版Skill同步到Claude Code用户级Skill目录。