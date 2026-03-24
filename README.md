# Craft Agent Preferences Example

这是一个 **Craft Agent 偏好配置示例仓库**。

## 文件位置
- `.craft-agent/preferences.json`

## 这个文件是什么
该文件用于保存 Craft Agent 的用户偏好，例如：
- 回复语言
- 执行稳定性要求
- 工具调用纪律
- 多步任务中的止损、验证与收敛规则

## 当前示例的特点
当前仓库中的 `preferences.json` 是一个 **脱敏版本**：
- 保留了可复用的执行偏好与工作习惯
- 去除了明显个人化或不适合公开仓库的内容

## 如何使用
你可以把这个文件作为模板：

1. 复制 `.craft-agent/preferences.json`
2. 放到你自己的 Craft Agent 配置目录中
3. 根据自己的工作习惯做修改

## 适用场景
适合希望让 Craft Agent 更稳定地执行多步任务的用户，尤其是当你希望它：
- 先检查前置条件
- 避免重复低级错误
- 优先走最短完成路径
- 明确区分“准备做 / 尝试中 / 已完成”
- 完成后先验证再汇报

## 说明
这是一个示例配置仓库，便于公开分享和复用。你可以在此基础上扩展更多 Craft Agent 配置文件。

## Included project skill
仓库中还包含一个可复用的 project-level skill：

- `.agents/skills/publish-craft-config/SKILL.md`
- `.agents/skills/publish-craft-config/icon.svg`

这个 skill 适合用于：
- 将 Craft Agent 配置发布到 Git/GitHub
- 在公开分享前先做脱敏处理
- 按固定流程完成校验、commit、push 与远程验证
