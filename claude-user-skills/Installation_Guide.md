# Claude Code用户级STM32开发Skill

<br>

**作者**：Huang Xiaofeng [ai4mcu@gmail.com](mailto:ai4mcu@gmail.com), Huang Shan [youcans@qq.com](mailto:youcans@qq.com)

---

<br>

本目录用于保存 AI4MCU 项目的 Claude Code 用户级 STM32 开发 Skill 源码。

这些 Skill 面向基于 STM32 开发板的实验项目开发，按照 AI 协同开发流程组织，用于多个 STM32 项目的重复使用。

本目录用于 GitHub 发布、版本管理和共享，不是 Claude Code 实际加载 Skill 的目录。

---

<br>


## 1. 使用指南

### 1.1 快速安装

#### 步骤1：拉取项目

1. 打开 PowerShell。
2. 首次安装时，将项目克隆到本地：

```powershell
git clone https://github.com/youcans/AI-for-STM32-Skills.git C:\AI-for-STM32-Skills
```

3. 后续更新项目时，执行：

```powershell
cd C:\AI-for-STM32-Skills
git pull
```

#### 步骤2：安装Skill

1. 清理可能存在的旧版STM32 Skill：

```powershell
@("stm32-init","stm32-scan","stm32-plan","stm32-code","stm32-log") | ForEach-Object {
    Remove-Item "$HOME\.claude\skills\$_" -Recurse -Force -ErrorAction SilentlyContinue
}
```

2. 将项目中的STM32 Skill复制到Claude Code用户级Skill目录：

```powershell
Copy-Item -Path "C:\AI-for-STM32-Skills\claude-user-skills\*" -Destination "$HOME\.claude\skills\" -Recurse -Force
```

### 1.2 安装确认

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

---

<br>

## 2. Skill目录结构

每个 Skill 使用独立目录保存，核心文件为：

    SKILL.md

部分 Skill 包含额外资源文件，例如模板文件。

目录结构：

    claude-user-skills/
    ├── README.md
    ├── stm32-init/
    │   ├── SKILL.md
    │   └── assets/
    │       └── proj_requirements.tpl.md
    ├── stm32-scan/
    │   └── SKILL.md
    ├── stm32-plan/
    │   └── SKILL.md
    ├── stm32-code/
    │   └── SKILL.md
    └── stm32-log/
        └── SKILL.md

目录名称用于标识具体 Skill，应保持稳定。

---

<br>

## 3. STM32开发Skill体系

本项目采用简化版 STM32 Claude Code Skill 体系，面向基于 STM32 开发板的实验项目开发。

Skill 按照 AI 协同开发流程组织：

    项目初始化
        ↓
    工程扫描
        ↓
    任务规划
        ↓
    代码实现
        ↓
    开发记录

当前 Skill 包括：

| Skill名称 | 目录名 |
| --- | --- |
| STM32初始化Skill | `stm32-init` |
| STM32工程扫描Skill | `stm32-scan` |
| STM32任务规划Skill | `stm32-plan` |
| STM32代码实现Skill | `stm32-code` |
| STM32开发记录Skill | `stm32-log` |

各 Skill 根据开发任务分别调用，并通过项目任务需求文件、工程分析文件、任务规划文件和开发日志等文档进行衔接。

---

<br>

## 4. AI4MCU运行环境配置

### 4.1 硬件资料库

STM32开发项目使用 AI4MCU 硬件资料库：

    C:\AI4MCU\hardware\

硬件资料库目录示例：

    hardware/
    ├── nucleo-g431rb/
    │   └── hardware_overview_g431rb.md
    └── nucleo-c542rc/
        └── hardware_overview_c542rc.md

`stm32-init` 根据项目开发板型号，从硬件资料库读取对应硬件概述文件。

---

### 4.2 项目CLAUDE.md配置

每个 STM32 项目需要在项目根目录创建：

    <project-name>/CLAUDE.md

例如：

    C:\AI4MCU\projects\DemoF01\CLAUDE.md

文件内容：

```text
    # AI4MCU Project Configuration
    
    ## Hardware Path
    
    AI4MCU_HARDWARE_PATH:
    
    C:\AI4MCU\hardware
```

其中：

`AI4MCU_HARDWARE_PATH` 用于指定 AI4MCU 硬件资料库路径。

---

### 4.3 环境检查

检查 Skill 安装目录：

    Get-ChildItem $HOME\.claude\skills

检查 `stm32-init`：

    Get-ChildItem $HOME\.claude\skills\stm32-init -Recurse

检查硬件资料库：

    Test-Path C:\AI4MCU\hardware

检查硬件资料：

    Get-ChildItem C:\AI4MCU\hardware

检查项目配置：

    Get-Content C:\AI4MCU\projects\<project-name>\CLAUDE.md

---

<br>

## 5. Skill测试运行

完成 Skill 安装和环境配置后，可以通过实际项目测试 Skill 工作流程。

例如：

进入项目目录：

    cd C:\AI4MCU\projects

启动 Claude Code：

    claude

执行：

    /stm32-init DemoF01

`stm32-init` 将完成：

1. 创建项目目录结构；
2. 创建项目开发日志文件；
3. 获取用户需求描述；
4. 读取硬件资料；
5. 生成项目任务需求文件；
6. 在出口调用 `stm32-log` 追加开发记录。

---

<br>

## 6. Skill维护

本目录中的文件作为 Skill 源码进行维护，并通过 Git 进行版本管理。

修改 Skill 时，应首先更新：

    C:\AI4MCU\claude-user-skills\<skill-name>\

然后将更新后的 Skill 同步到 Claude Code 用户级目录：

    C:\Users\<用户名>\.claude\skills\<skill-name>\

通过区分源码目录和安装目录，可以避免将 `claude-user-skills` 误认为 Claude Code 实际加载目录。