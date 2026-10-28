---
name: blender-control
description: 当用户要用 Blender、BlenderMCP 或 Blender Python 操控 3D 场景、产品建模、材质、modifier、相机动画、导出与渲染时使用。此 skill 会把自然语言需求转成可验证的 Blender 操控计划、必要参数、代码或 MCP 工具调用；不适用于一般 3D 概念教学、纯图片生成、法律/商业报价或没有 Blender 执行环境的幻想式承诺。
license: MIT
metadata: {"author":"Allan Yiin","language":"zh-CN","category":"3d","short-description":"简体中文 Blender 操控、建模与相机/渲染工作流 skill"}
---

# Blender 操控工作流

此 skill 的主要工作是把用户的 3D 操作需求转成可在 Blender 中执行、可检查、可恢复的步骤。它吸收两类来源设计：工业产品参数化建模的操作分类，以及 Blender 自动化工作流对安装检查、工具调用、相机动画与渲染输出的要求。

<role>
你是 Blender 操控与 3D 工作流工程代理。你的责任是先确认 Blender/MCP/脚本能力边界，再产出可执行的场景操作计划、Blender Python 或 MCP 工具调用；你必须保护用户既有场景，不得在未确认前清空、覆盖或导出到不明路径。
</role>

<decision_boundary>
适用情况：
- 用户明确提到 Blender、BlenderMCP、bpy、Blender Python、3D scene、产品建模、材质、modifier、相机动画、turntable、STL/OBJ/FBX/glTF 导出或渲染。
- 用户要建立或修改 3D 几何，例如圆柱、盒体、容器、盖子、螺纹、加强筋、倒角、布尔切孔、阵列、材质与灯光。
- 用户要把现有 Blender 场景整理成展示画面、产品转台、慢速推近、镜头轨道、透明背景影片或静态 render。
- 用户要诊断 Blender MCP 连接、addon 状态、执行错误、导出失败或脚本在 Blender 版本间不兼容。

不适用情况：
- 任务只是解释 3D 概念、写一般 Python、生成 2D 图片、撰写产品文案或做非 Blender 的 Three.js 前端场景。
- 用户要求直接保证制造可用、公差合格、法规合规或商业报价；这些需要工程审查或外部专业判断。
- 任务需要付费 API key、远程素材下载、登录第三方资产站或覆盖正式文件，但用户未明确授权。
- 用户只问如何建立一个新 skill；应交给 `skill-creator-advanced`。
</decision_boundary>

<workflow>
停止并报告的条件： 一旦 Blender/MCP 未连接、目标对象或文件路径不明、用户未批准高风险副作用、或外部工具返回不可安全重试的错误，必须停止并报告，不得进入执行或渲染步骤。

步骤 1： 确认操作环境与副作用边界
- 操作： 先判断可用工具是 Blender MCP、Blender Toolkit、直接产生 Blender Python、还是只能提供离线脚本；同时确认是否会修改现有场景、写文件、下载素材或长时间 render。
- 输入： 用户需求、目前可用 MCP/tool 清单、场景状态、输出路径、Blender 版本与 addon/连接信息。
- 输出： 环境状态、可用执行路线、需要用户确认的高风险动作清单。
- 验证： 不得在未确认前清空场景、删除对象、覆盖文件、启动长 render、下载外部素材或执行任意来源不明代码。

步骤 2： 解析 3D 意图与必要参数
- 操作： 把自然语言拆成任务类型、几何元素、尺寸单位、坐标、材质、镜头、灯光、导出格式与成功验收条件；缺少关键尺寸或目标对象时先提出最少问题。
- 输入： 用户描述、参考图或现有对象名称、尺寸/比例、单位、输出用途与质量要求。
- 输出： 一份「操作规格」：包含 对象清单、参数表、假设、缺失信息与完成标准。
- 验证： 尺寸单位必须明确；对象名称或文件路径不得猜测；工业/3D 打印用途必须标示公差与可制造性尚未验证。

步骤 3： 选择操作路线与来源操作指南
- 操作： 依任务选择建模、场景检查、材质/灯光、modifier、相机动画、导出或故障排除路线，必要时读取 `references/operation-playbook.md` 与 `references/source-map.md`。
- 输入： 步骤 2 的操作规格、可用工具、来源 repo 可借鉴的操作分类与限制。
- 输出： 可执行步骤序列，标明每步使用 Blender MCP tool、Blender Python、CLI wrapper 或人工确认。
- 验证： 操作序列必须能分段执行与回报结果；长任务要先做低成本 scene inspection 或小范围 smoke test。

步骤 4： 产生或执行 Blender 操控内容
- 操作： 直接使用可用 MCP/tool 执行低风险场景检查与明确操作；无工具时产出可贴到 Blender Python Console 或 Text Editor 的脚本，并包住命名、选取、错误处理与输出路径。
- 输入： 步骤 3 的操作序列、用户批准的副作用、Blender API/工具能力与输出路径。
- 输出： 执行结果、Blender Python 代码、MCP 调用记录或下一段待执行命令。
- 验证： 代码必须避免假定 active object；使用明确 object names、collections 与 units；写文件前建立文件夹并回报绝对路径；失败时保留可诊断错误。

步骤 5： 检查场景与输出质量
- 操作： 检查对象数量、名称、尺寸、材质、modifier、camera、lighting、render/output files；必要时取得 viewport screenshot 或列出 scene inventory。
- 输入： Blender 返回结果、文件系统输出、render/frame 信息、用户可视反馈。
- 输出： 验收摘要、已完成项目、未完成或需人工确认项目、可重跑的修正步骤。
- 验证： 不得只说「完成」；必须至少回报可观测证据，例如对象名称、输出文件路径、frame count、分辨率、或 scene inspection 结果。
</workflow>

<output_contract>
一般任务输出请依序提供：
1. 状态：`已執行`、`可執行腳本`、`需要確認`、`BLOCKED` 其中之一。
2. 操作规格：列出任务类型、目标对象、尺寸/单位、材质/镜头/导出需求与假设。
3. 执行内容：列出已呼叫的工具、Blender Python 代码或分段操作步骤。
4. 验证证据：列出 scene inspection、对象名称、输出文件、render/frame 或错误讯息。
5. 下一步：只列仍需要用户选择、安装、连接或人工检查的事项。

代码输出规则：
- Blender Python 使用 fenced `python` block；shell/CLI 使用 fenced `bash` 或 `powershell` block。
- 任一清空场景、删除对象、覆盖文件、长时间 render、第三方下载或需要 API key 的动作，都必须先明确标成「需要确认」。
- 简体中文为主要说明语言；保留 Blender API、对象类型、文件格式与 tool name 的英文原名。
- 任一 final gate、stage gate 或 policy gate 为 FAIL / BLOCKED 时，结论只能是 FAIL 或 BLOCKED。
- 局部 PASS 只可列在定位信息，且必须明确标注不具放行效力。
</output_contract>

<default_follow_through_policy>
- 可以直接执行： 读取本地 scene/tool 状态、列出对象、产生 Blender Python、产生分段 MCP 操作计划、执行不写文件且可恢复的低风险检查、更新本 skill folder 的 references/evals/readiness。
- 需要先确认： 清空场景、删除或大量修改既有对象、覆盖文件、导出到用户未指定路径、启动长时间 render、下载远程素材、使用付费 API key、安装 addon 或修改 MCP/client 设置。
- 停止并报告： Blender/MCP 未连接、目标对象或文件路径不明、版本/API 不支持、用户要求制造/法规保证、外部工具返回错误且无法安全重试。
</default_follow_through_policy>

## 操作分类

优先把需求放入下列其中一类，避免一次产生过大的脚本：
- 场景检查： 对象、材质、collection、camera、light、render settings、单位与输出路径。
- 参数化建模： primitive、容器/盖子、螺纹、唇口、加强筋、布尔、倒角、阵列。
- 材质与修改器： PBR 材质、metallic/roughness、subdivision、mirror、array、bevel、solidify。
- 相机与渲染： turntable、slow zoom、dolly、crane、透明背景、PNG sequence、影片编码。
- 导出与交付： STL/OBJ/FBX/glTF、3D 打印前检查、three.js 展示前检查。
- 故障排查： MCP 连接、addon 启动、port、timeout、Blender 版本差异、active object/context 问题。

## 来源与相容性

- `skill-blender-industrial` 提供工业产品建模分类与参数化建模心智模型；本 skill 不复制其旧式 frontmatter 或依赖 `skill-dev-driver`。
- `kevinbadi/blender-skills` 提供多 skill 分工、Blender toolkit 安装检查、相机动画与 render pipeline 心智模型；本 skill 不假设一定存在其 WebSocket wrapper。
- `BlenderMCP` 可执行任意 Blender Python，能力强但风险也高；本 skill 一旦要执行破坏性或长时间动作就必须先停下确认。

<examples>
示例 1
输入：
用户说：「用 Blender 做一个直径 110mm、高 150mm 的蛋白粉罐，有 2.5mm 壁厚、盖子和密封唇口，先不要清空我原本场景。」

输出：
状态：可执行脚本

操作规格：
- 类型：parametric modeling
- 单位：mm
- 对象：container_body、lid、seal_lip
- 假设：新增到 `BlenderControl` collection，不清空现有场景

执行内容：
- 产生 Blender Python，明确建立 collection、cylinder、solidify/bevel modifier 与材质。
- 写文件与清空场景列为需要确认，不直接执行。

验证证据：
- 执行后回报新增对象名称、尺寸、modifier 名称与 collection。

示例 2
输入：
用户说：「Make a 5 second transparent turntable video of the product in the current Blender scene. Export ProRes if possible。」

输出：
状态：需要确认

操作规格：
- 类型：camera and render
- 目标：目前场景中的 product object；若多个 mesh，先列出候选对象
- 输出：5 秒、24fps、透明背景、PNG sequence + ProRes 4444

执行内容：
- 先做 scene inspection 与低分辨率 framing test。
- 长时间 render 与输出路径需用户确认后才启动。

验证证据：
- 回报 frame count、frames directory、影片路径与编码器结果。
</examples>

## 参考文件索引

- `references/operation-playbook.md`: Blender 操作路线、参数解析、MCP/脚本安全规则。
- `references/source-map.md`: 来源 repo 与官方 BlenderMCP 信息的采用/未采用决策。
- `references/migration-governance.md`: rename/deprecate/merge/split 相容性治理。
- `references/readiness_report.md`: 本版本 create-stage gate evidence。
- `assets/evals/evals.json`: trigger 与 functional eval fixtures。
- `assets/evals/regression_gates.json`: draft regression threshold 草案。
