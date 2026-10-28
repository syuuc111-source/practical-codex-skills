# Practical Codex Skills · 实用工作流技能集

把本地使用的 BIM 建模、科研和开发工作流整理成可移植的 Agent Skills，保留中文适配和上游来源。

本仓库包含 **5 个技能入口 + 2 组中文覆盖包**。第三方完整工具、模型、付费软件和账号凭据不随仓库分发；42 项本地技能及 3 项 LibTV 插件工作流的整理结果见 [CATALOG.md](CATALOG.md)。

## 独立 Skill 仓库

每项已拆分为独立公开仓库，可以单独获取、维护和安装。当前仓库保留为合集；两个中文覆盖包仍需对应上游原版。

- [图片转 BIMBase · image-to-bimbase](https://github.com/syuuc111-source/image-to-bimbase)
- [论文写作 · research-paper-writing](https://github.com/syuuc111-source/research-paper-writing)
- [论文配图 · academic-figure-generator](https://github.com/syuuc111-source/academic-figure-generator)
- [科学研究 · scientific-agen](https://github.com/syuuc111-source/scientific-agen)
- [DeepFlow 中文入口 · deepflow](https://github.com/syuuc111-source/deepflow)
- [Blender 中文覆盖包 · blender-control](https://github.com/syuuc111-source/blender-control)
- [Codex Dream Skin 中文覆盖包 · codex-dream-skin](https://github.com/syuuc111-source/codex-dream-skin)

## 包含什么

| 入口 | 用途 | 运行前需要 |
|---|---|---|
| [image-to-bimbase](skills/image-to-bimbase/SKILL.md) | 根据参考图片复原道路/铁路，生成和保存 BIMBase 工程 | Windows、BIMBase 2024/SDK、VS Code、可用组件库及桌面操作工具 |
| [research-paper-writing](skills/research-paper-writing/SKILL.md) | 论文结构、段落和证据审阅 | 对应上游资料 |
| [academic-figure-generator](skills/academic-figure-generator/SKILL.md) | 论文架构图、方法图和英文绘图提示词 | 对应上游资料；真正生成图像需另有生成工具 |
| [scientific-agen](skills/scientific-agen/SKILL.md) | 按任务查找 K-Dense 科研技能 | 对应上游资料；具体工具按选中技能另行配置 |
| [deepflow](skills/deepflow/SKILL.md) | 需求探索、规格、执行与验收的 Codex 中文入口 | 对应上游资料；不会注册 Claude hooks |
| [Blender 中文覆盖包](overlays/blender-control/SKILL.md) | 保留本地翻译的技能、参考文档和显示信息 | 固定版本上游完整技能、Blender/执行工具 |
| [Dream Skin 中文覆盖包](overlays/codex-dream-skin/SKILL.md) | Windows Codex 皮肤使用和恢复说明 | 固定版本上游 Windows 技能及其软件依赖 |

## 安装技能入口

需要 Git、Python 3.9+（检查包结构时使用）和支持 Skills 的客户端。以下为 PowerShell 命令，从本仓库根目录执行。安装仅复制说明文件，不会启动 BIMBase、Blender 或修改 Codex 皮肤。

```powershell
$codexRoot = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $env:USERPROFILE '.codex' }
$skillRoot = Join-Path $codexRoot 'skills'
New-Item -ItemType Directory -Force -Path $skillRoot | Out-Null
# 可只保留需要的名字。
$selected = @('image-to-bimbase', 'research-paper-writing', 'academic-figure-generator', 'scientific-agen', 'deepflow')
# 先检查所有目标，避免覆盖已有安装。
foreach ($name in $selected) {
    if (Test-Path -LiteralPath (Join-Path $skillRoot $name)) { throw "技能已存在，请先备份并移走旧目录：$name" }
}
foreach ($name in $selected) {
    Copy-Item -LiteralPath (Join-Path '.\skills' $name) -Destination (Join-Path $skillRoot $name) -Recurse
}
```

论文写作入口统一为小写 `research-paper-writing`。如果已安装旧名 `Research-Paper-Writing`，先备份旧目录，避免重复注册（Windows 文件名不区分大小写）。让客户端重新加载技能或新建会话后使用。

## 获取上游依赖

除 BIMBase 入口外，4 个入口需要外部源码。执行下面的 PowerShell 命令，按 [dependencies.json](dependencies.json) 中固定提交安装；可缩小 `$selected`。这里的目录名必须与入口说明一致。

```powershell
$codexRoot = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $env:USERPROFILE '.codex' }
$sourceRoot = Join-Path $codexRoot 'skill-sources'
New-Item -ItemType Directory -Force -Path $sourceRoot | Out-Null
$deps = Get-Content -LiteralPath '.\dependencies.json' -Raw | ConvertFrom-Json
$selected = @('research-paper-writing', 'academic-figure-generator', 'scientific-agen', 'deepflow')
foreach ($name in $selected) {
    if (Test-Path -LiteralPath (Join-Path $sourceRoot $name)) { throw "依赖目录已存在，请检查或备份：$name" }
}
foreach ($name in $selected) {
    $spec = $deps.$name
    $target = Join-Path $sourceRoot $name
    git clone --filter=blob:none --no-checkout $spec.url $target
    if ($LASTEXITCODE -ne 0) { throw "clone 失败：$name" }
    git -C $target checkout --detach $spec.commit
    if ($LASTEXITCODE -ne 0) { throw "checkout 失败：$name" }
}
```

可以使用其他已有检出目录，调用时明确告诉助手位置。下载资料不等于安装论文工具、图像 API、Node 依赖或浏览器。

## 安装中文覆盖包（可选）

两个覆盖包均依赖完整上游文件，不能只复制 `overlays/` 中的目录后直接使用。以下示例选择一个覆盖包，下载固定版本，在临时目录合成后再安装；不会覆盖已安装技能，也不会运行上游安装脚本。

```powershell
$name = 'blender-control' # 或 'codex-dream-skin'
$deps = Get-Content -LiteralPath '.\dependencies.json' -Raw | ConvertFrom-Json
$spec = $deps.$name
$stageRoot = Join-Path ([IO.Path]::GetTempPath()) ('codex-skill-' + [guid]::NewGuid().ToString('N'))
$checkout = Join-Path $stageRoot 'source'
$codexRoot = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $env:USERPROFILE '.codex' }
$skillRoot = Join-Path $codexRoot 'skills'
$destination = Join-Path $skillRoot $name
if (Test-Path -LiteralPath $destination) { throw "请先备份并移走已有技能：$destination" }
New-Item -ItemType Directory -Force -Path $stageRoot,$skillRoot | Out-Null
git clone --filter=blob:none --no-checkout $spec.url $checkout
if ($LASTEXITCODE -ne 0) { throw 'clone 失败' }
git -C $checkout checkout --detach $spec.commit
if ($LASTEXITCODE -ne 0) { throw 'checkout 失败' }
$upstreamSkill = Join-Path $checkout (Split-Path $spec.entry -Parent)
$merged = Join-Path $stageRoot $name
Copy-Item -LiteralPath $upstreamSkill -Destination $merged -Recurse
Get-ChildItem -LiteralPath (Join-Path '.\overlays' $name) -Force | Copy-Item -Destination $merged -Recurse -Force
Copy-Item -LiteralPath (Join-Path '.\licenses' ($name + '.txt')) -Destination (Join-Path $merged 'LICENSE.txt')
Copy-Item -LiteralPath $merged -Destination $destination -Recurse
```

Blender 的历史 PASS 记录来自上游报告，不是本仓库实测。Dream Skin 的安装/启动步骤可能关闭或重新启动 Codex，应在明确请求应用皮肤时再执行；请阅读上游说明。

## BIMBase 环境

先阅读 [环境配置](skills/image-to-bimbase/references/windows-bimbase.md)，设置组件库、软件和 SDK 路径。默认输出到当前项目 `outputs/bimbase`，也可通过 `BIMBASE_OUTPUT_ROOT` 指定目录。原机器路径和历史工程不包含在公开包中。

此技能是操作流程，不是图片直接生成 BIM 的独立程序；运行依赖 SDK、有效组件和实际桌面工具。估算几何不等于工程测量或规范验证。

## 使用示例

```text
$image-to-bimbase 根据这张公铁交叉参考图复原主要线路，先确认关键参数，再生成独立新工程。
$research-paper-writing 审阅这段方法部分，检查段落逻辑和结论的证据支撑。
$academic-figure-generator 为这个方法写一份论文架构图英文提示词，采用 pastel 风格。
$scientific-agen 为这份实验数据寻找合适的分析工作流，先检查工具依赖。
$deepflow 根据这个功能需求编写有可验证验收标准的规格。
$blender-control 根据这张产品参考图生成可编辑模型，并检查导出结果。
$codex-dream-skin 检查 Windows 环境，说明应用与恢复皮肤的步骤。
```

## 验证与贡献

```powershell
python scripts/validate.py
```

检查技能文件头、依赖清单、文档链接、敏感文件及机器专用路径。实际建模、渲染、皮肤应用和第三方 API 需要各自运行环境，结构检查不证明这些流程已经执行。具体记录见 [VALIDATION.md](VALIDATION.md)。

提交改动时保留来源和许可证，附一个可复现的使用场景，并说明实际运行过的检查。不要提交凭据、聊天记录、私人素材或机器专用配置。

## 许可

新增工作流和整理内容使用 [MIT](LICENSE)。上游适配、中文翻译及来源说明见 [THIRD_PARTY.md](THIRD_PARTY.md) 与 `licenses/`；不将第三方技能标为原创。
