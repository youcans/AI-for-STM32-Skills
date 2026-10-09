# AI4MCU-STM32-Skills Installation and Usage Guide

English | [简体中文](Installation_Guide_zh-CN.md)

This document describes how to install, update, and use AI4MCU-STM32-Skills. For the project overview, Skill system, and basic experiment descriptions, see `README.md` in the project root directory.

---

## 1. Installation

### 1.1 Prerequisites

Before using AI4MCU-STM32-Skills, prepare the following development environment:

- Windows;
- Windows PowerShell;
- Git;
- Claude Code;
- STM32CubeMX;
- Visual Studio Code;
- STM32 VS Code Extension and the corresponding STM32 toolchain.

All installation commands in this guide are executed in Windows PowerShell. A PowerShell prompt usually starts with `PS`, for example:

```text
PS C:\Users\Administrator>
```

The hardware documentation path currently used by AI4MCU-STM32-Skills is fixed as:

```text
C:\AI4MCU-STM32-Skills\hardware\
```

Therefore, the AI4MCU-STM32-Skills repository should be installed at:

```text
C:\AI4MCU-STM32-Skills
```

### 1.2 Get AI4MCU-STM32-Skills

1. Open Windows PowerShell.

2. For the initial installation, run:

```powershell
git clone https://github.com/youcans/AI4MCU-STM32-Skills.git C:\AI4MCU-STM32-Skills
```

3. Check the project directory:

```powershell
Get-ChildItem C:\AI4MCU-STM32-Skills
```

Normally, the following items should be present:

```text
claude-user-skills
nucleo-g431rb_basics
nucleo-c542rc_basics
nucleo-g431rb_basics_ref
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

2. Remove any previously installed STM32 Skills:

```powershell
Remove-Item "$HOME\.claude\skills\stm32-init" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-scan" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-plan" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-code" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$HOME\.claude\skills\stm32-log" -Recurse -Force -ErrorAction SilentlyContinue
```

3. Copy the AI4MCU-STM32-Skills files to the Claude Code user-level Skill directory:

```powershell
Copy-Item -Path "C:\AI4MCU-STM32-Skills\claude-user-skills\*" -Destination "$HOME\.claude\skills\" -Recurse -Force
```

### 1.4 Check the Installation

Run:

```powershell
Get-ChildItem "$HOME\.claude\skills" -Directory | Where-Object Name -Like "stm32-*"
```

Normally, the following five directories should be present:

```text
stm32-init
stm32-scan
stm32-plan
stm32-code
stm32-log
```

You can also check a specific Skill, for example:

```powershell
Get-ChildItem "$HOME\.claude\skills\stm32-init" -Recurse
```

You should see `SKILL.md` and its related resource files.

### 1.5 Check the Hardware Documentation

Run:

```powershell
Get-ChildItem C:\AI4MCU-STM32-Skills\hardware
```

The following directories should currently be present:

```text
nucleo-g431rb
nucleo-c542rc
```

For example, to check the NUCLEO-G431RB hardware documentation:

```powershell
Get-ChildItem C:\AI4MCU-STM32-Skills\hardware\nucleo-g431rb
```

The directory should include:

```text
hardware_overview_g431rb.md
```

as well as related datasheets, schematics, user manuals, BOM files, and other hardware documentation.

### 1.6 Update AI4MCU-STM32-Skills

1. Enter the AI4MCU-STM32-Skills repository:

```powershell
cd C:\AI4MCU-STM32-Skills
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

4. Copy the latest Skills again:

```powershell
Copy-Item -Path "C:\AI4MCU-STM32-Skills\claude-user-skills\*" -Destination "$HOME\.claude\skills\" -Recurse -Force
```

---

## 2. Using the Basic Experiments

AI4MCU-STM32-Skills provides STM32 basic experiment starter projects organized by development board for learning and testing the AI4MCU-STM32-Skills development workflow:

- `nucleo-g431rb_basics/` provides 15 basic experiment starter projects for NUCLEO-G431RB;
- `nucleo-c542rc_basics/` currently provides six basic experiment starter projects for NUCLEO-C542RC, with more to be added.

The following sections use the NUCLEO-G431RB `GPIO_EXTI` experiment as an example.

### 2.1 Select a Basic Experiment

Select a basic experiment from `nucleo-g431rb_basics/` or `nucleo-c542rc_basics/` according to your development board. For this example, go to:

```text
C:\AI4MCU-STM32-Skills\nucleo-g431rb_basics\GPIO_EXTI
```

The experiment directory contains the following two starter files:

```text
GPIO_EXTI/
├── GPIO_EXTI.ioc        # Preconfigured STM32CubeMX project file
└── requirements.txt     # Example user requirements file
```

The two files are used as follows:

- `GPIO_EXTI.ioc` is used to generate the initial STM32 project;
- `requirements.txt` provides the experiment goals, functional requirements, and other necessary user requirements to `stm32-init`.

Other basic experiments are used in the same way.

### 2.2 Start Claude Code

1. Open Windows PowerShell.

2. Enter the experiment directory:

```powershell
cd C:\AI4MCU-STM32-Skills\nucleo-g431rb_basics\GPIO_EXTI
```

3. Start Claude Code:

```powershell
claude
```

The directory in which Claude Code is started is treated as the current STM32 project root directory, and the project name is taken from the current directory name.

In this example:

```text
<project-name> = GPIO_EXTI
```

### 2.3 Initialize the Project

The current experiment directory already contains:

```text
requirements.txt
```

Therefore, directly run the following command in Claude Code:

```text
/stm32-init
```

Because the `<User_Req>` parameter is not provided, `stm32-init` automatically reads `requirements.txt` from the current project root directory.

If another user requirements file should be used, run:

```text
/stm32-init <User_Req>
```

Here, `<User_Req>` specifies the path and filename of the user requirements file. For example:

```text
/stm32-init D:\requirements\gpio_exti.txt
```

If the user requirements file cannot be found, cannot be read, or does not contain enough information to determine the project goals and functional requirements, `stm32-init` obtains and refines the required information through dialogue with the developer.

`stm32-init` combines the user requirements with the AI4MCU-STM32-Skills hardware documentation to generate the project requirements document.

After developer confirmation, the formal project requirements document is saved as:

```text
docs/requirements/proj_requirements.md
```

### 2.4 Generate the Initial Project with STM32CubeMX

The basic experiments in `nucleo-g431rb_basics/` already provide configured `.ioc` files. Therefore, when testing the AI4MCU-STM32-Skills development workflow, there is no need to configure STM32CubeMX from scratch.

1. Open the following file in STM32CubeMX:

```text
GPIO_EXTI.ioc
```

2. Review the existing configuration as needed.

3. Generate the initial project with STM32CubeMX.

4. Confirm that the generated project is located in the current experiment directory.

5. Perform the initial Build in VS Code and confirm that the STM32CubeMX-generated initial project builds successfully.

### 2.5 Analyze the Initial Project

After generating the STM32CubeMX project and completing the initial Build, run the following command in Claude Code:

```text
/stm32-scan
```

`stm32-scan` analyzes the current STM32CubeMX initial project together with the project requirements and generates:

```text
docs/analysis/init_scan.md
```

After completion, continue to programming task planning as prompted.

### 2.6 Plan the Programming Tasks

Run:

```text
/stm32-plan
```

Programming task planning is divided into two stages:

1. The first stage generates the programming task list, including task IDs, task names, and task summaries, and submits it to the developer for review.
2. The developer confirms the programming task list or provides modification requests through dialogue.
3. After the programming task list is confirmed, the Skill enters the second stage and refines each task with its task goal, related files, main modification locations, implementation points, and validation requirements.
4. After completion, the formal programming task list is generated at:

```text
docs/tasks/task_list.md
```

Programming tasks use consecutive IDs such as:

```text
T01, T02, T03...
```

### 2.7 Implement a Programming Task

Start with the first programming task in the programming task list.

For example:

```text
/stm32-code T01
```

`stm32-code` reads the detailed description of `T01` from the programming task list and modifies the code strictly according to the task goal, related files, main modification locations, and implementation points.

`stm32-code` only modifies code. It does not automatically perform:

- Build;
- compilation;
- STM32CubeMX project generation;
- firmware flashing;
- debugging;
- actual hardware runtime validation.

After the code modification is completed, the Skill outputs a summary of the current programming task and prompts the developer to perform validation according to the validation requirements in the programming task list.

### 2.8 Validate and Continue with Subsequent Tasks

1. Complete the validation required by the current programming task:

```text
Build
  ↓
Firmware flashing
  ↓
Debugging or actual hardware runtime validation
```

2. After the current programming task has been validated, continue with the next task, for example:

```text
/stm32-code T02
```

3. Continue implementing and validating subsequent tasks in task ID order until all programming tasks are completed.

### 2.9 Development Log

`stm32-log` is automatically called when `stm32-init`, `stm32-scan`, `stm32-plan`, and `stm32-code` exit. Developers do not need to invoke it separately.

The project development log is saved as:

```text
docs/<project-name>_log.md
```

For example:

```text
docs/GPIO_EXTI_log.md
```

### 2.10 View Complete Reference Projects

The `nucleo-g431rb_basics_ref/` directory contains NUCLEO-G431RB reference projects that have already been developed and validated using the complete AI4MCU-STM32-Skills development workflow.

Seven complete reference projects are currently provided. They can be used to inspect:

- the complete STM32CubeMX-generated project;
- project development documents generated by the Skills;
- programming task planning results;
- source code after the programming tasks have been implemented;
- the final project state after completion of the full development workflow.

`nucleo-g431rb_basics/` and `nucleo-c542rc_basics/` provide starter projects for their respective development boards, allowing users to run the AI4MCU-STM32-Skills development workflow themselves. `nucleo-g431rb_basics_ref/` provides completed NUCLEO-G431RB projects for reference.

---

## 3. Using AI4MCU-STM32-Skills in Your Own STM32 Project

AI4MCU-STM32-Skills can be used not only with the basic experiments in `nucleo-g431rb_basics/` and `nucleo-c542rc_basics/`, but also with your own STM32 projects.

1. Create or enter an STM32 project directory, for example:

```powershell
mkdir D:\workspace\MySTM32Project
cd D:\workspace\MySTM32Project
```

2. Prepare a user requirements file and save it in the current project root directory as:

```text
requirements.txt
```

`requirements.txt` is used to describe the project goals, functional requirements, and other necessary development requirements.

For example:

```text
D:\workspace\MySTM32Project\
├── requirements.txt
└── ...
```

3. Start Claude Code in the current project directory:

```powershell
claude
```

4. Run:

```text
/stm32-init
```

`stm32-init` automatically reads `requirements.txt` from the current project root directory.

If another requirements file should be used, run:

```text
/stm32-init <User_Req>
```

If the requirements file cannot be found, cannot be read, or does not contain sufficient information, `stm32-init` obtains and refines the project requirements through dialogue.

5. Based on the generated project requirements document, use STM32CubeMX to configure the MCU, pins, clocks, peripherals, interrupts, DMA, and other required items, and generate the initial project in the current project directory.

6. Perform the initial Build in VS Code and confirm that the initial project builds successfully.

7. Continue by running:

```text
/stm32-scan
```

```text
/stm32-plan
```

```text
/stm32-code T01
```

8. After each programming task is completed, the developer performs Build, flashing, debugging, and actual hardware runtime validation before continuing with the next task.

---

## 4. Troubleshooting

### 4.1 PowerShell Commands Are Not Recognized

The commands `Remove-Item`, `Copy-Item`, and `Get-ChildItem` used in this guide are PowerShell commands.

If you see a message similar to:

```text
'Remove-Item' is not recognized as an internal or external command
```

you are probably using Windows Command Prompt (`cmd.exe`) instead of PowerShell.

Open Windows PowerShell and confirm that the prompt looks similar to:

```text
PS C:\Users\Administrator>
```

### 4.2 Claude Code Cannot Find the STM32 Skills

Run:

```powershell
Get-ChildItem "$HOME\.claude\skills" -Directory | Where-Object Name -Like "stm32-*"
```

Confirm that the following five directories exist:

```text
stm32-init
stm32-scan
stm32-plan
stm32-code
stm32-log
```

If they do not exist, reinstall the Skills using the installation commands above.

### 4.3 `stm32-init` Cannot Read the Hardware Documentation

Check whether AI4MCU-STM32-Skills is installed at:

```text
C:\AI4MCU-STM32-Skills
```

Then confirm that the hardware documentation directory exists:

```powershell
Test-Path C:\AI4MCU-STM32-Skills\hardware
```

Normally, the command should return:

```text
True
```

### 4.4 `stm32-init` Does Not Read the Expected Requirements File

When you run:

```text
/stm32-init
```

`stm32-init` reads the following file from the current project root directory by default:

```text
requirements.txt
```

Confirm that:

1. Claude Code was started in the correct STM32 project root directory;
2. `requirements.txt` is located in that project root directory;
3. the filename is exactly `requirements.txt`.

If another user requirements file should be used, specify it explicitly:

```text
/stm32-init <User_Req>
```

For example:

```text
/stm32-init D:\requirements\my_project.txt
```

### 4.5 A Skill Uses the Wrong Project Name or Project Files

AI4MCU-STM32-Skills treats the directory in which Claude Code is started as the STM32 project root directory and uses the current directory name as the project name.

Before invoking a Skill, first enter the correct STM32 project directory and then start Claude Code.

For example:

```powershell
cd C:\AI4MCU-STM32-Skills\nucleo-g431rb_basics\GPIO_EXTI
claude
```

The project name is then:

```text
GPIO_EXTI
```

### 4.6 The Installed Skills Do Not Change After Updating the Repository

`git pull` only updates:

```text
C:\AI4MCU-STM32-Skills
```

It does not automatically update the Skills already installed in:

```text
$HOME\.claude\skills\
```

After updating the repository, remove the previously installed Skills and run the copy command again to synchronize the latest versions to the Claude Code user-level Skill directory.
