# 《吸血莱恩：终极剪辑版》简体中文汉化

BloodRayne: Terminal Cut 的简体中文汉化补丁与重建工具，适用于 **Steam 终极剪辑版**。提供英文基准与日文基准两套中文译文，并附带文本、字体和语言组合切换工具。

## 当前状态

已发布 **v1.0**，发布记录确认已使用英文基准文本完整通关测试；该结论不扩大到所有日文基准与音视频组合。

目标名单底部的人名和部分德语军衔仍不在现有 TSV / POD 导入范围，目标完成提示、任务说明和常规文本已汉化。具体引擎结论见[技术与历史记录](docs/chinese_note.md)。

## 下载 / 使用

[v1.0 发布页](https://github.com/jyh9521/BloodRayne-CN/releases/tag/v1.0)，附件为 `BloodRayne_CN_v1.0.7z`。

退出游戏并备份将被覆盖的同名文件，将发布包内容按包内说明放入游戏目录。默认中文采用英文基准；使用 `zh_cn_tools\BloodRayneCNTool.exe` 可切换文本基准、语音、视频字幕与字体，并编辑译文。玩家工具无需另装 Python 或 .NET。

恢复时还原安装前备份，移除原先不存在的汉化文件；不要把含汉化资源的目录当作干净构建输入。

## 从源码重建

需要 Windows 10/11、干净的 Steam 游戏、Python 3 与 Pillow。可选 Visual Studio 2022 C++ Build Tools；缺少 C++ 环境时使用仓库的预编译玩家工具。

```powershell
python -m pip install pillow
powershell -ExecutionPolicy Bypass -File .\build.ps1 -GameDir "D:\Games\BloodRayne Terminal Cut"
```

也可省略 `-GameDir` 让脚本自动查找。构建前确认 `JAPANESE.POD` 为原版；必要时先用 Steam 验证文件完整性。

输出目录为 `dist\BloodRayne_CN_Build`，将其中内容复制到游戏根目录即可测试。主要产物：

- `JAPANESE.POD`：中文文本与字库，默认采用英文基准。
- `WORLD.POD`：包含 `DE_C1CASTLE` 原游戏缺字幕修复。
- `zh_cn_tools\`：玩家工具、可编辑 TSV、默认资源和切换用 POD。
- `SHA256SUMS.txt`：构建产物校验。

## 视频字幕

视频字幕译文保存在 `translations\video_subtitles.tsv`。重新生成 ASS：

```powershell
python .\tools\generate_hardsub_ass_from_tsv.py .\translations\video_subtitles.tsv --out-dir .\dist\_work\ass
```

硬字幕 BIK 需另行安装 RAD Video Tools，配置 `radvideo64.exe` 或 `RADVIDEO` 后使用 `tools\build_hardsub_bik.py`。一键核心构建不等于已完成视频重压，仓库不提交原始或重压后的完整游戏视频。

## 仓库结构与文档

| 路径 | 内容 |
| --- | --- |
| `translations/` | 英文 / 日文基准译文与视频字幕 TSV |
| `tools/` | POD、文本导入、字库与字幕工具 |
| `src/tool/` | Win32 玩家工具源码 |
| `prebuilt/` | 无 C++ 环境时使用的玩家工具 |
| `subtitles/video/` | 视频字幕源稿与整理稿 |
| `docs/` | 格式、验证、重建过程与历史记录 |

- [工程说明](docs/README.md)
- [技术记录](docs/chinese_note.md)
- [第三方声明](THIRD_PARTY_NOTICES.md)
- [可选兼容组件说明](third_party/README.md)

## 来源与许可

原始游戏与商业资源归原权利人所有。本仓库当前未提供项目级 `LICENSE`，源码公开不等同于已授予统一开源许可。

工具界面的 Noto Sans SC 子集字体遵循 SIL OFL。`third_party\winmm.dll` 不默认包含；自行加入前应核实来源与再分发条款。构建时存在该文件才会复制到输出目录。
