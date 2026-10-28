# 来源与采用说明

上游审查日期：2026-06-14

## yanlin-cheng/skill-blender-industrial

仓库：https://github.com/yanlin-cheng/skill-blender-industrial

采用的内容：

- 工业/产品建模分类：基础几何体、布尔运算、倒角、阵列、唇口/凹槽、螺纹、加强筋、材质、容器本体、盖子装配以及 OBJ/STL 导出。
- Blender 技能通常生成 Blender Python，再通过 BlenderMCP 发送执行，而非独立程序这一设计思路。
- 处理 MCP 连接失败，以及检查 Blender 安装和插件就绪状态的要求。

未采用的内容：

- 旧式单行文件头和 `triggerKeywords` 结构；上游仓库采用 YAML 文件头与语义块验证。
- 对 `skill-dev-driver` 的依赖；本技能需要作为可移植的 Agent Skill 使用，并采用本地任务交接规则。
- 未经外部工程验证就宣称生成的几何体可以直接制造。

## kevinbadi/blender-skills

仓库：https://github.com/kevinbadi/blender-skills

采用的内容：

- 将模型生成、相机动画、产品细化与自动化/工具包任务分开。
- 使用 Blender 自动化前进行安装和连接检查。
- 转台、缓慢变焦、推拉/旋转、升降揭示、透明背景、帧序列及 ffmpeg 后期编码等相机动画思路。
- 将脚本/封装程序作为工具使用，非必要时不将其完整内容载入上下文的原则。

未采用的内容：

- 强制依赖特定 WebSocket 封装、端口范围、项目配置路径、Meshy API、Mixamo 工作流或 Blender 5.x。
- 该仓库中的默认渲染输出位置；本技能会询问或报告明确路径。

## BlenderMCP

仓库：https://github.com/ahujasid/blender-mcp

采用的内容：

- BlenderMCP 通过插件和 MCP 服务器，在助手与 Blender 之间建立双向通信。
- 相关能力包括场景/对象检查、形状修改、材质控制和执行任意 Blender Python。
- 任意 Python 执行能力很强，进行破坏性操作或写文件前尤其需要谨慎。

兼容性说明：

- 本技能将 BlenderMCP 视为可选的实时执行路径。没有可用的实时 MCP 工具时，提供 Blender Python 和验证步骤，不声称已执行操作。
