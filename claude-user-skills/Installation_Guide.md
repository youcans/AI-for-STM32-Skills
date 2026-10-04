# Claude Code用户级STM32开发Skill

<br>

**作者**：Huang Xiaofeng [ai4mcu@gmail.com](mailto:ai4mcu@gmail.com), Huang Shan [youcans@qq.com](mailto:youcans@qq.com)

---

<br>

本目录用于保存AI4MCU项目的Claude Code用户级STM32开发Skill源码。

这些Skill面向基于STM32开发板的实验项目，按照AI协同开发流程组织，可用于多个STM32项目的重复使用。

本目录用于GitHub发布、版本管理和共享，不是Claude Code实际加载Skill的目录。

---

<br>

## 1. 使用指南

### 1.1 快速安装

#### 步骤1：拉取项目

1. 打开PowerShell。
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

2. 将STM32 Skill复制到Claude Code用户级Skill目录：

```powershell
@("stm32-init","stm32-scan","stm32-plan","stm32-code","stm32-log") | ForEach-Object {
    Copy-Item -Path "C:\AI-for-STM32-Skills\claude-user-skills\$_" -Destination "$HOME\.claude\skills\" -Recurse -Force
}
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

每个Skill使用独立目录保存，核心文件为`SKILL.md`，部分Skill包含模板文件。

目录结构：

```text
claude-user-skills/
├── Installation_Guide.md
├── stm32-init/
│   ├── SKILL.md
│   └── assets/
│       └── proj_requirements.tpl.md
├── stm32-scan/
│   ├── SKILL.md
│   └── assets/
│       └── init_scan.tpl.md
├── stm32-plan/
│   ├── SKILL.md
│   └── assets/
│       └── task_list.tpl.md
├── stm32-code/
│   └── SKILL.md
└── stm32-log/
    └── SKILL.md
```

目录名称用于标识具体Skill，应保持稳定。

---

<br>

## 3. STM32开发Skill体系

本项目采用简化版STM32 Claude Code Skill体系，面向基于STM32开发板的实验项目开发。

Skill按照AI协同开发流程组织：

```text
项目初始化
    ↓
初始工程扫描
    ↓
编程任务规划
    ↓
代码实现
    ↓
开发记录
```

当前Skill包括：

| Skill名称 | 目录名 | 主要职责 |
|---|---|---|
| STM32项目初始化Skill | `stm32-init` | 获取项目目标和功能要求，生成项目任务需求文件 |
| STM32初始工程扫描Skill | `stm32-scan` | 分析CubeMX生成的初始工程，生成初始工程分析文件 |
| STM32编程任务规划Skill | `stm32-plan` | 拆分并细化编程任务，生成编程任务清单 |
| STM32代码实现Skill | `stm32-code` | 按指定编程任务修改代码 |
| STM32开发日志维护Skill | `stm32-log` | 由其它Skill调用并追加开发记录 |

各Skill通过项目任务需求文件、初始工程分析文件、编程任务清单和开发日志进行衔接。

---

<br>

## 4. 运行环境

### 4.1 项目目录

Claude Code当前工作目录作为STM32项目根目录，项目名称取当前目录名称。

项目可以位于任意工作路径，例如：

```text
D:\workspace\DemoF01
```

其中项目名称为：

```text
DemoF01
```

使用Skill前，应先进入项目根目录，再启动Claude Code：

```powershell
cd D:\workspace\DemoF01
claude
```

### 4.2 硬件资料库

`stm32-init`使用AI4MCU硬件资料库，硬件资料路径为：

```text
C:\STM32\hardware\<board-name>
```

当前支持：

```text
C:\STM32\hardware\
├── nucleo-g431rb\
│   └── hardware_overview_g431rb.md
└── nucleo-c542rc\
    └── hardware_overview_c542rc.md
```

当前支持的开发板型号为：

- `nucleo-g431rb`
- `nucleo-c542rc`

---

<br>

## 5. Skill开发流程

### 5.1 项目初始化

进入已创建的STM32项目根目录并启动Claude Code。

没有用户需求描述文件时执行：

```text
/stm32-init
```

提供用户需求描述文件时执行：

```text
/stm32-init [user-description]
```

`stm32-init`将获取项目目标、功能要求和硬件信息，并生成项目任务需求文件。

### 5.2 STM32CubeMX配置

根据项目任务需求文件，使用STM32CubeMX完成硬件和外设配置，并在项目根目录生成`.ioc`文件和初始工程。

完成配置后，由开发者根据需要进行初始工程构建确认。

### 5.3 初始工程扫描

执行：

```text
/stm32-scan
```

`stm32-scan`结合项目任务需求，分析CubeMX生成的初始工程，并生成初始工程分析文件。

### 5.4 编程任务规划

执行：

```text
/stm32-plan
```

`stm32-plan`分两个阶段进行编程任务规划：

1. 生成编程任务列表，由开发者审阅确认；
2. 根据确认后的编程任务列表逐项细化编程任务。

完成后生成正式编程任务清单。

### 5.5 代码实现

按照编程任务编号依次执行，例如：

```text
/stm32-code T01
```

完成代码修改后，由开发者按照编程任务清单中的验证要求进行构建、烧录、调试或硬件运行验证。

继续下一项编程任务时执行：

```text
/stm32-code T02
```

依次完成后续编程任务。

`stm32-log`由其它Skill在出口自动调用，不需要开发者单独执行。

---

<br>

## 6. 项目文档

`stm32-init`在项目根目录建立以下项目文档目录结构：

```text
docs/
├── tmp/
├── requirements/
│   └── proj_requirements.md
├── analysis/
│   └── init_scan.md
├── tasks/
│   └── task_list.md
└── <project-name>_log.md
```

主要文档作用如下：

| 文件 | 作用 |
|---|---|
| `docs/requirements/proj_requirements.md` | 项目目标、功能要求和硬件定义 |
| `docs/analysis/init_scan.md` | 初始工程状态及代码接入条件 |
| `docs/tasks/task_list.md` | 编程任务列表及编程任务详细说明 |
| `docs/<project-name>_log.md` | Skill执行开发记录 |

---

<br>

## 7. Skill维护

本目录中的文件作为Skill源码进行维护，并通过Git进行版本管理。

源码目录：

```text
C:\AI-for-STM32-Skills\claude-user-skills\
```

Claude Code用户级Skill安装目录：

```text
C:\Users\<用户名>\.claude\skills\
```

修改Skill源码后，重新执行安装命令即可更新Claude Code用户级Skill。

源码目录和安装目录相互独立，不应直接在Claude Code用户级Skill安装目录中维护源码。