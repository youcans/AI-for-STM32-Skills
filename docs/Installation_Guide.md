# AI4STM32 Installation and Usage Guide

English | [简体中文](Installation_Guide_zh-CN.md)

This document explains how to install, update, and use AI4STM32. For an overview of the project, the Skill system, and the basic experiments, see the `README.md` file in the repository root.

---

## 1. Installation

### 1.1 Prerequisites

Before using AI4STM32, prepare the following development environment:

- Windows;
- Windows PowerShell;
- Git;
- Claude Code;
- STM32CubeMX;
- Visual Studio Code;
- STM32 VS Code Extension and the corresponding STM32 toolchain.

All installation commands in this document are intended to be executed in Windows PowerShell. A PowerShell prompt usually begins with `PS`, for example:

```text
PS C:\Users\Administrator>
```

AI4STM32 currently uses the following fixed hardware documentation path:

```text
C:\AI-for-STM32-Skills\hardware\
```

Therefore, the AI4STM32 repository should be installed at:

```text
C:\AI-for-STM32-Skills
```

### 1.2 Get AI4STM32

1. Open Windows PowerShell.

2. For the first installation, run:

```powershell
git clone https://github.com/youcans/AI-for-STM32-Skills.git C:\AI-for-STM32-Skills
```

3. Check the repository directory:

```powershell
Get-ChildItem C:\AI-for-STM32-Skills
```

The repository should contain entries such as:

```text
claude-user-skills
demo_basics
docs
hardware
LICENSE
README.md
README_zh-CN.md
THIRD_PARTY_NOTICES.md
```

### 1.3 Install the Claude Code User-Level Skills

1. Create the Claude Code user-level Skill directory:

```powershell
New-Item -ItemType Directory -Path "$HOME\.claude\skills" -Force | Out-Null
```

2. Remove any previously installed AI4STM32 Skills:

```powershell
Remove-Item "$HOME\.claude\skills\stm32-init" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-scan" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-plan" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-code" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-log" -Recurse -Force -ErrorAction SilentlyContinue
```

3. Copy the AI4STM32 Skills into the Claude Code user-level Skill directory:

```powershell
Copy-Item -Path "C:\AI-for-STM32-Skills\claude-user-skills\*" -Destination "$HOME\.claude\skills\" -Recurse -Force
```

### 1.4 Verify the Installation

Run:

```powershell
Get-ChildItem "$HOME\.claude\skills" -Directory | Where-Object Name -Like "stm32-*"
```

The following Skill directories should be present:

```text
stm32-init
stm32-scan
stm32-plan
stm32-code
stm32-log
```

You can also inspect an individual Skill, for example:

```powershell
Get-ChildItem "$HOME\.claude\skills\stm32-init" -Recurse
```

The directory should contain `SKILL.md` and its related resource files.

### 1.5 Check the Hardware Documentation

Run:

```powershell
Get-ChildItem C:\AI-for-STM32-Skills\hardware
```

The current repository should contain:

```text
nucleo-g431rb
nucleo-c542rc
```

For example, check the NUCLEO-G431RB hardware documentation:

```powershell
Get-ChildItem C:\AI-for-STM32-Skills\hardware\nucleo-g431rb
```

The directory should contain:

```text
hardware_overview_g431rb.md
```

together with related datasheets, schematics, user manuals, BOM files, and other hardware documentation.

### 1.6 Update AI4STM32

1. Enter the AI4STM32 repository:

```powershell
cd C:\AI-for-STM32-Skills
```

2. Pull the latest version:

```powershell
git pull
```

3. Remove the previously installed Skills:

```powershell
Remove-Item "$HOME\.claude\skills\stm32-init" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-scan" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-plan" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-code" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-log" -Recurse -Force -ErrorAction SilentlyContinue
```

4. Copy the latest Skills into the Claude Code user-level Skill directory:

```powershell
Copy-Item -Path "C:\AI-for-STM32-Skills\claude-user-skills\*" -Destination "$HOME\.claude\skills\" -Recurse -Force
```

---

## 2. Usage

AI4STM32 provides STM32CubeMX configuration files for basic STM32 experiments in the `demo_basics/` directory. These files can be used directly to learn and test the AI4STM32 development workflow.

The following example uses the `GPIO_EXTI` experiment.

### 2.1 Select a Basic Experiment

Use the following experiment directory:

```text
C:\AI-for-STM32-Skills\demo_basics\GPIO_EXTI
```

The directory provides a preconfigured STM32CubeMX project file:

```text
GPIO_EXTI.ioc
```

Other basic experiments can be used in the same way.

### 2.2 Start Claude Code

1. Open Windows PowerShell.

2. Enter the experiment directory:

```powershell
cd C:\AI-for-STM32-Skills\demo_basics\GPIO_EXTI
```

3. Start Claude Code:

```powershell
claude
```

The directory in which Claude Code is started is treated as the current STM32 project root directory. The project name is taken from the current directory name.

In this example:

```text
<project-name> = GPIO_EXTI
```

### 2.3 Initialize the Project

In Claude Code, run:

```text
/stm32-init
```

If a user requirement description file is available, run:

```text
/stm32-init [user-description]
```

`stm32-init` collects the project goals and functional requirements and combines them with the AI4STM32 hardware documentation to generate the project requirements document.

After developer confirmation, the formal document is saved to:

```text
docs/requirements/proj_requirements.md
```

### 2.4 Generate the Initial Project with STM32CubeMX

The basic experiments in `demo_basics/` already provide configured `.ioc` files, so there is no need to configure STM32CubeMX from scratch when testing the AI4STM32 workflow.

1. Open the following file in STM32CubeMX:

```text
GPIO_EXTI.ioc
```

2. Review the existing configuration as needed.

3. Generate the initial STM32 project with STM32CubeMX.

4. Confirm that the generated project is located in the current experiment directory.

5. Perform the initial Build in VS Code and confirm that the STM32CubeMX-generated initial project builds successfully.

When developing your own STM32 project, use the project requirements generated by `stm32-init` as the basis for configuring STM32CubeMX and generating the initial project.

### 2.5 Analyze the Initial Project

After generating the STM32CubeMX project and completing the initial Build, run:

```text
/stm32-scan
```

`stm32-scan` analyzes the current STM32CubeMX initial project together with the project requirements and generates:

```text
docs/analysis/init_scan.md
```

After the analysis is complete, continue to programming task planning.

### 2.6 Plan the Programming Tasks

Run:

```text
/stm32-plan
```

Programming task planning is performed in two stages:

1. Stage 1 generates the programming task list, including the task ID, task name, and task summary, and submits it to the developer for review.
2. The developer confirms the programming task list or provides modification requests through dialogue.
3. After the programming task list is confirmed, the Skill enters Stage 2 and refines each task with its task objective, involved files, main modification locations, implementation points, and validation requirements.
4. The formal programming task list is then saved to:

```text
docs/tasks/task_list.md
```

Programming tasks use consecutive IDs:

```text
T01, T02, T03, ...
```

### 2.7 Implement a Programming Task

Start with the first task in the programming task list.

For example:

```text
/stm32-code T01
```

`stm32-code` reads the detailed description of `T01` from the programming task list and modifies the code strictly according to the task objective, involved files, main modification locations, and implementation points.

`stm32-code` only performs code modification. It does not automatically perform:

- Build;
- compilation;
- STM32CubeMX project generation;
- firmware flashing;
- debugging;
- hardware runtime validation.

After the code modification is completed, the Skill outputs a summary of the current programming task and prompts the developer to perform validation according to the validation requirements in the programming task list.

### 2.8 Validate and Continue with Subsequent Tasks

1. Perform the validation required by the current programming task:

```text
Build
  ↓
Firmware flashing
  ↓
Debugging or hardware runtime validation
```

2. After the current programming task has been validated, continue with the next task, for example:

```text
/stm32-code T02
```

3. Continue implementing and validating subsequent tasks in task ID order until all programming tasks are completed.

### 2.9 Development Log

`stm32-log` is automatically called by `stm32-init`, `stm32-scan`, `stm32-plan`, and `stm32-code` before they exit. The developer does not need to invoke it separately.

The project development log is saved to:

```text
docs/<project-name>_log.md
```

For example:

```text
docs/GPIO_EXTI_log.md
```

---

## 3. Using AI4STM32 in Your Own STM32 Project

AI4STM32 can also be used in STM32 projects outside the `demo_basics/` directory.

1. Create or enter an STM32 project directory, for example:

```powershell
mkdir D:\workspace\MySTM32Project
cd D:\workspace\MySTM32Project
```

2. Start Claude Code in that directory:

```powershell
claude
```

3. Run:

```text
/stm32-init
```

4. Based on the generated project requirements document, configure the MCU, pins, clocks, peripherals, interrupts, DMA, and other required resources in STM32CubeMX, and generate the initial project in the current project directory.

5. Perform the initial Build.

6. Continue with:

```text
/stm32-scan
```

```text
/stm32-plan
```

```text
/stm32-code T01
```

7. After each programming task is completed, the developer performs Build, flashing, debugging, and hardware runtime validation before continuing with the next task.

---

## 4. Troubleshooting

### 4.1 PowerShell Reports an Unrecognized Command

`Remove-Item`, `Copy-Item`, and `Get-ChildItem` used in this document are PowerShell commands.

If you see an error similar to:

```text
'Remove-Item' is not recognized as an internal or external command
```

you are usually running the command in Windows Command Prompt (`cmd.exe`) instead of PowerShell.

Open Windows PowerShell and confirm that the prompt looks similar to:

```text
PS C:\Users\Administrator>
```

### 4.2 Claude Code Cannot Find the AI4STM32 Skills

Run:

```powershell
Get-ChildItem "$HOME\.claude\skills" -Directory | Where-Object Name -Like "stm32-*"
```

Confirm that the following directories exist:

```text
stm32-init
stm32-scan
stm32-plan
stm32-code
stm32-log
```

If they are missing, reinstall the Skills using the installation commands above.

### 4.3 `stm32-init` Cannot Read the Hardware Documentation

Confirm that AI4STM32 is installed at:

```text
C:\AI-for-STM32-Skills
```

Then check whether the hardware documentation directory exists:

```powershell
Test-Path C:\AI-for-STM32-Skills\hardware
```

The expected result is:

```text
True
```

### 4.4 A Skill Uses the Wrong Project Name or Project Files

AI4STM32 treats the directory in which Claude Code is started as the STM32 project root directory and uses the current directory name as the project name.

Before invoking a Skill, enter the correct STM32 project directory and then start Claude Code.

For example:

```powershell
cd C:\AI-for-STM32-Skills\demo_basics\GPIO_EXTI
claude
```

The project name is then:

```text
GPIO_EXTI
```

### 4.5 The Installed Skills Do Not Change After Updating the Repository

Running:

```powershell
git pull
```

only updates:

```text
C:\AI-for-STM32-Skills
```

It does not automatically update the Skills already installed under:

```text
$HOME\.claude\skills\
```

After updating the repository, remove the previously installed Skills and copy the latest versions into the Claude Code user-level Skill directory again.