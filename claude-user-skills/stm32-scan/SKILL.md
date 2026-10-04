---
name: stm32-scan
description: 分析STM32CubeMX生成的初始工程，生成初始工程分析文件
argument-hint: "<project-name>"
disable-model-invocation: true
metadata:
  author: "Huang Shan <ai4mcu@qq.com>"
---

# STM32 初始工程扫描 Skill

## 1. 输入规范

### 1.1 需求描述

`<project-name>` 为 Skill 调用时指定的项目名称参数。

项目目录固定为 `C:\STM32\<project-name>`。

### 1.2 输入文件

读取以下初始工程文件：
- `<project-name>.ioc`
- `CMakeLists.txt`
- `Core/Inc/*.h`
- `Core/Src/*.c`

## 2. 输出规范

### 2.1 初始工程分析文件

初始工程分析文件用于描述 STM32CubeMX 生成的初始工程状态，为后续任务规划和代码开发提供工程基础信息。

文件内容包括：
- 工程基本信息；
- 工程目录与构建结构；
- 程序初始化与运行结构；
- 外设配置、初始化函数和句柄；
- 中断与 HAL 回调；
- 后续代码接入位置；
- 分析结论与待确认事项。

文件格式采用当前 Skill 目录下的模板 `assets/proj_init_scan.tpl.md`。

生成文件保存至 `docs/analysis/proj_init_scan.md`。

## 3. 执行步骤

### 3.1 读取输入文件

1. 根据 `<project-name>` 定位项目目录 `C:\STM32\<project-name>`。
2. 如果已存在 `docs/analysis/proj_init_scan.md`，询问用户是否继续分析并覆盖原有文件；如果用户选择退出，则进入异常出口。
3. 读取 `<project-name>.ioc` 和 `CMakeLists.txt`。
4. 读取 `Core/Inc/*.h` 和 `Core/Src/*.c`。

### 3.2 分析工程结构

1. 分析当前工程的主要目录和文件组织。
2. 根据 `CMakeLists.txt` 分析源码、头文件和构建系统之间的关系。
3. 确定后续新增源码和头文件的工程接入位置。

### 3.3 分析程序与外设配置

1. 根据 `.ioc` 和源码分析程序初始化顺序、系统时钟配置和主循环结构。
2. 分析当前工程实际配置的 GPIO 和其它外设。
3. 分析外设初始化函数、句柄、通道、引脚及 DMA 等关联关系。
4. 分析当前工程中的中断配置、IRQHandler 和 HAL 回调。
5. 确定初始化阶段、主循环、中断回调和自定义源文件的代码接入位置。

### 3.4 生成初始工程分析文件

1. 根据分析结果，按照 `assets/proj_init_scan.tpl.md` 生成初始工程分析文件。
2. 对无法从当前输入文件确认的内容，在对应位置标记为未知或待确认。
3. 将生成文件保存至 `docs/analysis/proj_init_scan.md`。

## 4. 边界约束

1. 仅读取当前 `.ioc`、`CMakeLists.txt` 和源码中的实际内容，不读取上述文件引用或关联的其它文件。
2. 仅以当前 `.ioc`、`CMakeLists.txt` 和源码中的实际内容为依据进行分析，不得使用其它文件。
3. 仅按照输出模板要求提取和整理工程信息，不得扩展分析模板未要求的内容。
4. 不得自动补充或猜测无法确认的信息。
5. 不得修改 CubeMX 配置、工程文件和源代码。


## 5. 出口规范

### 5.1 正常出口

满足以下条件进入正常出口：
1. 初始工程分析完成。
2. 初始工程分析文件保存至 `docs/analysis/proj_init_scan.md`。

如果进入正常出口，输出执行情况和分析摘要，并提示：
【下一步建议：

- 调用 `stm32-plan` Skill 进行开发任务规划。】

调用 `stm32-log` Skill。

### 5.2 异常出口

不满足正常出口条件时进入异常出口。

如果进入异常出口，输出未完成原因和当前执行状态，并调用 `stm32-log` Skill。