# AI4STM32

**Claude Code Skills for AI-Collaborative STM32 Basic Experiment Development**

English | [简体中文](README_zh-CN.md)

- Project: AI4STM32
- GitHub Repository: `AI-for-STM32-Skills`
- Authors: Huang Xiaofeng & Huang Shan
- Email: ai4mcu@qq.com
- GitHub: [@youcans](https://github.com/youcans)

---

## Introduction

AI4STM32 is a set of Claude Code user-level Skills designed for STM32 basic experiment development. It organizes project requirement preparation, initial project analysis, programming task planning, code implementation, and development logging into a simplified and practical AI-collaborative STM32 development workflow.

The project mainly targets basic STM32 experiments involving GPIO, timers, PWM, ADC, DAC, UART, I2C, SPI, interrupts, DMA, RTC, watchdogs, Flash, and related MCU programming mechanisms. Claude Code works together with STM32CubeMX, VS Code, and real development boards throughout the development process.

AI4STM32 currently consists of five Skills: `stm32-init`, `stm32-scan`, `stm32-plan`, `stm32-code`, and `stm32-log`.

The project follows two basic principles:

> **Accurate, clear, and simple.**

> **Start with the minimum implementation, make it work first, validate in practice, and iterate.**

---

## Project Components

AI4STM32 consists mainly of Claude Code Skills, hardware documentation, and real STM32 basic experiment projects:

```text
AI4STM32
│
├── claude-user-skills/    # Provides the AI-collaborative development method
│
├── hardware/              # Provides hardware information required for development
│
└── demo_basics/           # Provides real STM32 basic experiment projects
                           # for running and validating the method
```

The `docs/` directory contains project-level documentation for AI4STM32.

The main repository structure is:

```text
AI-for-STM32-Skills/
├── claude-user-skills/
├── demo_basics/
├── docs/
├── hardware/
├── .gitignore
├── LICENSE
├── README.md
├── README_zh-CN.md
└── THIRD_PARTY_NOTICES.md
```

---

## Skill System and Development Workflow

### Skill Overview

| Skill | Main Function | Invocation |
|---|---|---|
| `stm32-init` | Collects project goals, functional requirements, and hardware information, and generates the project requirements document | `/stm32-init [user-description]` |
| `stm32-scan` | Analyzes the STM32CubeMX-generated initial project together with the project requirements | `/stm32-scan` |
| `stm32-plan` | Splits and refines programming tasks and generates the programming task list | `/stm32-plan` |
| `stm32-code` | Implements code changes for one specified programming task | `/stm32-code <task-id>` |
| `stm32-log` | Appends development logs | Automatically called by other Skills |

### Typical Development Workflow

```text
Developer creates an STM32 project directory
        ↓
Enter the project directory and start Claude Code
        ↓
stm32-init
        │
        └── Generate the project requirements document
        ↓
Developer configures the project with STM32CubeMX
and generates the .ioc file and initial project
        ↓
Developer performs the initial Build
        ↓
stm32-scan
        │
        └── Generate the initial project analysis document
        ↓
stm32-plan
        │
        ├── Stage 1: Generate the programming task list
        │        ↓
        │     Developer review and confirmation
        │
        └── Stage 2: Refine each programming task
        ↓
stm32-code T01
        │
        └── Implement code changes for the current task
        ↓
Developer performs Build, flashing, debugging,
and hardware validation
        ↓
stm32-code T02
        ↓
Continue with subsequent programming tasks
```

`stm32-log` is not used as an independent development stage. It is automatically called before the other business Skills exit and appends a development record.

---

## Quick Start

### 1. Get the Project

AI4STM32 uses a fixed default path for its hardware documentation, so cloning the repository to the following location is recommended:

```text
C:\AI-for-STM32-Skills
```

Run the following command in Windows PowerShell:

```powershell
git clone https://github.com/youcans/AI-for-STM32-Skills.git C:\AI-for-STM32-Skills
```

To update the repository later:

```powershell
cd C:\AI-for-STM32-Skills
git pull
```

### 2. Install the Skills

Create the Claude Code user-level Skill directory if it does not already exist:

```powershell
New-Item -ItemType Directory -Path "$HOME\.claude\skills" -Force | Out-Null
```

Copy all AI4STM32 Skills into the Claude Code user-level Skill directory:

```powershell
Copy-Item -Path "C:\AI-for-STM32-Skills\claude-user-skills\*" -Destination "$HOME\.claude\skills\" -Recurse -Force
```

### 3. Create an STM32 Project and Start Claude Code

An STM32 project can be located in any working directory. For example:

```powershell
mkdir D:\workspace\Demo01
cd D:\workspace\Demo01
claude
```

The directory in which Claude Code is started is treated as the STM32 project root directory, and the project name is taken from the current directory name.

In this example:

```text
<project-name> = Demo01
```

### 4. Initialize the Project

If no user requirement description file is provided:

```text
/stm32-init
```

If a user requirement description file is available:

```text
/stm32-init [user-description]
```

After the project requirements have been prepared, configure the MCU and peripherals in STM32CubeMX and generate the initial project.

### 5. Continue Development

After the initial project has been generated and the initial Build has been completed, run:

```text
/stm32-scan
```

Then plan the programming tasks:

```text
/stm32-plan
```

Implement the first programming task:

```text
/stm32-code T01
```

After the code modification is completed, the developer performs Build, flashing, debugging, and hardware validation according to the validation requirements in the programming task list.

Continue with the next task:

```text
/stm32-code T02
```

For complete installation, update, environment checking, and usage instructions, see:

[Installation and Usage Guide](docs/Installation_Guide.md)

---

## Basic Experiments

The `demo_basics/` directory contains STM32 basic experiment projects developed using the AI4STM32 Skill workflow. These projects are used to run and validate the AI-collaborative development method in real STM32 projects.

The repository currently contains 15 basic experiments. They are listed below in the intended experiment sequence rather than alphabetical order:

```text
demo_basics/
├── GPIO_IOToggle/      # GPIO output toggle and LED blinking experiment
├── GPIO_BUTTON/        # GPIO button input experiment
├── GPIO_EXTI/          # GPIO external interrupt experiment
├── TIM_IT_500ms/       # Timer 500 ms periodic interrupt experiment
├── TIM_PWM_1kHz/       # Timer 1 kHz PWM output experiment
├── TIM1_PWM_Comp/      # TIM1 complementary PWM output experiment
├── ADC_IT/             # ADC analog data acquisition experiment
├── DAC_Output/         # DAC analog signal output experiment
├── USART_Echo/         # USART data receive/transmit echo experiment
├── LPUART_DMA/         # LPUART DMA communication experiment
├── I2C_Master_Slave/   # I2C master-slave communication experiment
├── SPI_Master_Slave/   # SPI master-slave communication experiment
├── IWDG_Reset/         # Independent watchdog reset experiment
├── RTC_Alarm/          # RTC calendar and alarm experiment
└── Flash_Parameter/    # Flash parameter storage experiment
```

Each directory is an independent STM32 experiment project containing the STM32CubeMX project, source code, and development documents generated during the AI4STM32 Skill workflow.

Together, these experiments cover typical STM32 development topics including GPIO, timers, PWM, ADC/DAC, USART/LPUART, I2C, SPI, interrupts, DMA, RTC, independent watchdogs, and Flash.

---

## Supported Hardware

The AI4STM32 hardware documentation is located at:

```text
C:\AI-for-STM32-Skills\hardware\
```

The repository currently contains:

```text
hardware/
├── nucleo-g431rb/
└── nucleo-c542rc/
```

Currently supported development boards:

- NUCLEO-G431RB
- NUCLEO-C542RC

Each development board directory contains an AI4STM32 hardware overview document together with related datasheets, board schematics, user manuals, BOM files, and other hardware references.

`stm32-init` reads the corresponding hardware information according to the selected development board and extracts the hardware platform, MCU pin definitions, and peripheral connections required for the project requirements.

---

## Documentation

AI4STM32 project documentation is stored in the `docs/` directory.

Public usage-related documents include:

- [Installation and Usage Guide](docs/Installation_Guide.md)
- [AI4MCU Skill Writing Specification](docs/AI4MCU_Skill编写规范.md)

Each STM32 experiment project also contains its own `docs/` directory generated and maintained during the AI4STM32 workflow. These project-specific documents contain project requirements, initial project analysis, programming task lists, and development logs.

---

## License

Skills, code, and documentation originally developed for AI4STM32 are released under the MIT License.

See:

[LICENSE](LICENSE)

Third-party materials and code included or referenced in this repository remain subject to their original copyright and license terms.

See:

[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)

---

## Contact

- Project: AI4STM32
- GitHub Repository: `AI-for-STM32-Skills`
- Authors: Huang Xiaofeng & Huang Shan
- Email: ai4mcu@qq.com
- GitHub: [@youcans](https://github.com/youcans)