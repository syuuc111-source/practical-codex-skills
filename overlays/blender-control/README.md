# Blender 操控工作流

当用户要用 Blender、BlenderMCP 或 Blender Python 操控 3D 场景、产品建模、材质、modifier、相机动画、导出与渲染时使用。此 skill 会把自然语言需求转成可验证的 Blender 操控计划、必要参数、代码或 MCP 工具调用；不适用于一般 3D 概念教学、纯图片生成、法律/商业报价或没有 Blender 执行环境的幻想式承诺。

## 基本数据

- Skill ID：`blender-control`
- 分类：`07-presentations-visual`
- 版本：`未標示`
- 主要指令：[SKILL.md](SKILL.md)

## 快速使用

在支持 Skills 的环境中，可以直接描述任务；若要明确指定，可使用：

```text
$blender-control 請依我的目標與現有資料完成任務，並列出需要我確認的事項。
```

建议一并提供目标、使用情境、已知数据、限制条件，以及希望取得的成果格式。若数据不足，Skill 会依核心指令只询问真正影响结果的问题。

## 内容结构

- `SKILL.md`：AI 执行此能力时采用的核心指令
- `agents/`：界面显示与触发设置（1 个文件）
- `references/`：按需求加载的参考数据（4 个文件）
- `assets/`：产出时可使用的素材或模板（2 个文件）

## 维护说明

本 README 供人员浏览、搜寻与版本管理；真正影响 AI 行为的是 `SKILL.md` 及其引用的资源。修改功能时，应优先更新核心指令，再重新检查本说明是否仍一致。
