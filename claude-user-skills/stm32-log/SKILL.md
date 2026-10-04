---
name: stm32-log
description: 为其它STM32开发Skill追加执行日志。由业务Skill在正常出口或异常出口调用。
metadata:
  author: "Huang Shan <ai4mcu@qq.com>"
---

# STM32开发日志维护

## 1. 输入规范

本 Skill 由其它 Skill 在出口调用，用于追加当前 Skill 执行记录。

调用时根据当前项目上下文获取：
- 项目名称；
- 调用 Skill 名称；
- 开发事项；
- 处理结果；
- 当前状态。


## 2. 输出规范

### 2.1 日志文件路径

日志文件路径：
`<project-name>/docs/<project-name>_log.md`

本 Skill 仅向指定日志文件追加记录。


### 2.2 日志记录格式规范

每次调用本 Skill 时，生成并向日志文件追加一条新的开发记录。

日志格式：

```markdown
## 开发记录

### YYYY-MM-DD HH:MM Skill名称

- 开发事项：
- 处理结果：
- 当前状态：
```


## 3. 执行步骤

### 3.1 生成日志记录

1. 根据当前 Skill 执行上下文生成开发记录。
2. 按照日志记录格式生成本次开发记录。


### 3.2 追加日志记录

1. 将生成的日志记录追加至 `<project-name>/docs/<project-name>_log.md`。
2. 如果日志文件已有内容，不得覆盖、修改或删除原有内容，仅在文件末尾追加本次开发记录。


## 4. 边界约束

1. 本 Skill 不读取或修改项目其它文件。
2. 本 Skill 不调用其它 Skill。


## 5. 出口规范

完成日志追加操作后进入正常出口。