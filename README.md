# rich-report for Kimi Code

**把图直接画进对话** —— 给 Kimi Code for VS Code 加一套内联图能力：
流程图用手写 SVG，多图对比用图墙，全部渲染成**可折叠、可放大**的图块。

`kimi-code` · `mcp` · `inline-svg` · `figure-block` · `image-wall` · `markdown-rendering` · `vscode-extension-patch`

| 工具 | 作用 |
|---|---|
| `render_diagram` | 手写 SVG → 图块（默认宽 520，`max_width` 可调） |
| `build_image_wall` | 多张本地图片 → 图墙：每格独立、可单击放大；`split=false` 拼成一张 |
| `check_diagram` | 校验 SVG 是否符合宿主渲染约定 |

| 渲染补丁 | 作用 |
|---|---|
| `apply_rb1/2/3` | `svg` 代码块 → 内联 SVG；工具卡片里的裸 SVG；撑满容器并居中 |
| `apply_img_full` | markdown 图片：大图按容器全宽，小图保持缩略图 |
| `apply_img_grid(2)` | 同段多图排网格（被图块取代，保留兼容） |
| `apply_wall_block` … `6` | `wall` 代码块 → 可折叠图块（CSS Grid、标题栏、折叠） |
| `apply_wall_step` | 图块所在 step 也画时间线轨道 |
| `apply_wall_icon(2)` | 图块图标（xy 轴 + 三根实心柱） |
| `apply_wall_svg` | 图块内支持手写 SVG（文字可选中、不抢点击） |
| `apply_wall_card` | 工具卡片遇到图块规格时整卡替换 |
| `apply_qd6` | 图片绝对路径 → 工作区相对 |

## 安装

```bash
# 1) 打渲染补丁（按文件名顺序），脚本自动定位扩展目录，可用 KIMI_DIST 覆盖
python3 patches/apply_rb1.py && python3 patches/apply_rb2.py && python3 patches/apply_rb3.py
python3 patches/apply_img_full.py
python3 patches/apply_wall_block.py   # 然后 block2 … block6
python3 patches/apply_wall_step.py && python3 patches/apply_wall_icon.py && python3 patches/apply_wall_icon2.py
python3 patches/apply_wall_svg.py && python3 patches/apply_wall_card.py
python3 patches/apply_qd6.py

# 2) 装插件
mkdir -p ~/.kimi-code/plugins/managed/rich-report
cp rich-report/{mcp_server.py,wall.py,kimi.plugin.json} ~/.kimi-code/plugins/managed/rich-report/
# 在 ~/.kimi-code/plugins/installed.json 里登记（见 rich-report/README.md）

# 3) Reload Window
```

## 目标产物

`…/extensions/moonshot-ai.kimi-code-0.8.2-linux-x64/dist/{webview.js,extension.js}`。
锚点不匹配会直接退出，不会写坏文件；每个脚本在改动前自动备份，多数支持 `--revert`。

## 约定

- 图块宽度：流程图默认 **520px**（`max_width` 可调，建议 ≤900）；图墙按格子等分。
- 图墙单块 ≤4 格；PNG 走文件（`~/tmp/chat_figures/walls/`），不受单条工具结果 5 万字符限制。
- 颜色一律 `currentColor`，跟随主题；图块里文字可选中，图片格可单击放大。

## License

MIT（见 `LICENSE`）。补丁作用对象是第三方扩展，使用前请自行确认其许可条款；本项目不提供担保、不包含任何上游代码。
