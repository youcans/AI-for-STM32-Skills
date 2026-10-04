---
name: stm32-log
description: 为其它STM32开发Skill追加执行日志，由业务Skill在出口调用
metadata:
  author: "Huang Xiaofeng & Huang Shan <ai4mcu@qq.com>"
---

# STM32开发日志维护

## 1. 输入规范

本Skill由其它业务Skill在出口调用，从当前执行上下文获取项目名称、调用Skill名称、开发事项、处理结果和当前状态。

## 2. 输出规范

日志记录追加至`<project-name>/docs/<project-name>_log.md`。

日志记录格式如下：

```markdown
### YYYY-MM-DD HH:MM Skill名称

- 开发事项：
- 处理结果：
- 当前状态：
```

## 3. 执行步骤

1. 根据当前执行上下文，按照规定格式生成本次开发记录。
2. 将开发记录追加至日志文件末尾，不得覆盖、修改或删除原有内容。

## 4. 边界约束

1. 本Skill不读取或修改项目其它文件。
2. 本Skill不调用其它Skill。

## 5. 出口规范

完成日志追加后结束本Skill。