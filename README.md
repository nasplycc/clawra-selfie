# Clawra Selfie

<p align="center">
  <img src="assets/demo.jpg" alt="Clawra Demo" width="320" />
</p>

<p align="center">
  <strong>一个受 Clawra 启发、运行在 OpenClaw 里的自拍/日常状态图技能</strong><br/>
  当前默认使用 Hugging Face 免费路线，兼顾可用性、成本与后续可升级性。
</p>

<p align="center">
  <img alt="状态" src="https://img.shields.io/badge/状态-可用-3fb950">
  <img alt="后端" src="https://img.shields.io/badge/后端-HuggingFace-fcc72b">
  <img alt="Gemini" src="https://img.shields.io/badge/Gemini-默认禁用-6f42c1">
  <img alt="许可证" src="https://img.shields.io/badge/许可证-MIT-blue">
</p>

---

## 项目简介

这个项目的灵感来自 [SumeLabs/clawra](https://github.com/SumeLabs/clawra)，但我把它改造成了一个更适合当前实际使用场景的版本：

- 保留 **Clawra / Raya 式的人设出图体验**
- 直接融入 **OpenClaw 原生工作流**
- 不把 **付费模型 / 付费后端** 作为默认前提
- 先用一条**当前可跑通的 Hugging Face 免费路线**
- 同时为后续升级到更强的人脸一致性方案预留空间

一句话概括就是：

> **Clawra 的感觉，OpenClaw 的工作流，free-first 的实现路线。**

---

## 为什么要做这个

原版 Clawra 的思路很有吸引力，但其中一些更强的模型调用链路是要花钱的。

这个版本的目标不是一开始就追求最强，而是先做到：

1. **先能稳定用起来**
2. **日常对话里能自然出图**
3. **以后要升级时不用推倒重来**

所以当前版本更看重：

- 可用性
- 简洁性
- 成本控制
- 后续升级空间

---

## 目前能做什么

- **根据提示词生成自拍 / 日常状态图 / 场景图**
- **Direct 模式**：适合近景自拍、当前状态、表情与氛围感
- **Mirror 模式**：适合半身、全身、健身房、穿搭、镜前区域构图
- **官方脸软锚点机制**：通过工作区参考图尽量稳住脸
- **固定 FACE_ANCHOR / NEGATIVE_ANCHOR**：尽量降低脸和身材漂移
- **可接 OpenClaw 消息发送链路**：能直接发回 Telegram 等渠道
- **Gemini 探测链路已接入，但默认禁用**
- **当前稳定默认后端仍是 Hugging Face**

---

## 效果展示

<table>
  <tr>
    <td align="center">
      <img src="assets/showcase-face.jpg" alt="原参考图" width="280" /><br/>
      <sub>原参考图 / 用作官方脸锚点</sub>
    </td>
    <td align="center">
      <img src="assets/showcase-beach.jpg" alt="海边散步全景图" width="280" /><br/>
      <sub>海边散步全景图 / 基于参考图的人设场景生成结果</sub>
    </td>
  </tr>
</table>

这两张图用于说明当前 README 里的完整工作流：

- **左图**：用户提供的人脸参考图，作为官方脸锚点来源
- **右图**：基于该参考图继续生成的海边散步场景图，用来展示人物状态图与场景延展能力

---

## 当前后端策略

### 默认启用
- **Hugging Face**
- 当前默认模型：
  - `black-forest-labs/FLUX.1-schnell`

### 已接入但默认禁用
- **Gemini image probe**

之所以默认禁用 Gemini，是因为实际测试下来，Gemini 生图这条免费链路在当前环境下并不稳定可用，甚至会出现 **free tier = 0** 的情况。

如果以后要重新打开 Gemini，可以再启用：

```bash
ENABLE_GEMINI=1
GEMINI_API_KEY=your_google_gemini_api_key
```

---

## 人脸一致性策略

这个项目当前走的是 **软一致性（soft consistency）**，还不是严格意义上的硬锁脸。

目前主要通过下面几层一起工作：

- 固定的 **FACE_ANCHOR**
- 固定的 **NEGATIVE_ANCHOR**
- 工作区里的 **官方脸参考图路径**
- 针对具体场景做 prompt 塑形

### 官方脸参考图

脚本会优先检查这些路径：

- `references/raya-official-face-current.png`
- `references/raya-official-face-current.jpg`
- `references/raya-official-face-current.jpeg`
- `references/raya-official-face-v1.png`
- `references/raya-official-face-v1.jpg`
- `references/raya-official-face-v1.jpeg`

如果存在，就把它当作当前“官方脸”的软锚点。

### 重要限制

由于当前 Hugging Face 免费路线本质上仍然更偏向 **text-to-image**，所以它目前能做到的是：

- 人设气质尽量稳定
- 五官方向尽量相近
- 连续多次出图时角色感觉不至于完全跑掉

但它**还做不到每次都生成完全同一张脸**。

如果以后真的要更强的人脸稳定性，核心升级方向不会是继续堆 prompt，而是：

- 更强的 reference-image editing 后端
- 本地 ComfyUI
- LoRA / 数据集训练路线

---

## 模式说明

### Direct 模式
适合：
- 近景自拍
- 当前状态照
- 日常随手感
- 表情 / 气氛感 / 近景人像

### Mirror 模式
适合：
- 镜前区域构图
- 穿搭图
- 半身 / 全身照
- 健身房 / 校园 / 室内生活场景

这个项目里的 Mirror 模式 **默认不要求一定拿手机**。

---

## 项目结构

```text
clawra-selfie/
├─ assets/
│  ├─ clawra.png
│  ├─ demo.jpg
│  ├─ showcase-face.jpg
│  ├─ showcase-gym.jpg
│  └─ showcase-beach.jpg
├─ examples/
│  └─ README.md
├─ scripts/
│  ├─ clawra-selfie.sh
│  └─ clawra-selfie.ts
├─ .gitignore
├─ CHANGELOG.md
├─ LICENSE
├─ README.md
└─ SKILL.md
```

### 关键文件

- `scripts/clawra-selfie.sh`
  - 主生成脚本
  - 负责 prompt 组装、后端调用、fallback 和输出文件写入

- `scripts/clawra-selfie.ts`
  - 一个轻量的 Node 包装层

- `SKILL.md`
  - 面向 OpenClaw skill 使用的说明

- `README.md`
  - 面向 GitHub 仓库展示的项目说明页

---

## 运行要求

### 必需
- `bash`
- `curl`
- `jq`
- OpenClaw 运行环境
- Hugging Face token

### 环境变量

```bash
HF_TOKEN=your_huggingface_token
HF_IMAGE_MODEL=black-forest-labs/FLUX.1-schnell
HF_API_BASE=https://router.huggingface.co/hf-inference/models
```

### 可选的 Gemini 变量

```bash
ENABLE_GEMINI=1
GEMINI_API_KEY=your_google_gemini_api_key
GEMINI_IMAGE_MODEL=gemini-2.5-flash-image
```

---

## 用法示例

### Shell

```bash
HF_TOKEN=your_token \
./scripts/clawra-selfie.sh \
  "at the gym, post-workout mirror-area fitness snapshot" \
  "telegram" \
  "mirror" \
  "健身房状态 ✨"
```

### Node 包装脚本

```bash
node ./scripts/clawra-selfie.ts \
  "in class, side profile from deskmate perspective" \
  "telegram" \
  "mirror" \
  "上课时的侧颜 ✨"
```

参数说明：

1. `user_context` —— 她在做什么 / 穿什么 / 身处哪里
2. `channel` —— 目标渠道名
3. `mode` —— `direct`、`mirror` 或 `auto`
4. `caption` —— 输出时附带的文案

---

## 场景示例

- 早安自拍
- 早餐店全身照
- 教室上课状态
- 下课后操场跑步
- 健身房状态图
- 咖啡店侧颜
- 城市街头随手感

更多示例可以看：[`examples/README.md`](examples/README.md)

---

## 已知限制

- Hugging Face 免费生图 **不是真正的 reference-image editing**
- 人脸一致性弱于付费后端或本地高级流程
- 不能保证每次都完全同一张脸
- 免费后端的可用性、限额和效果可能随时间变化

---

## Roadmap

- [x] 跑通 Hugging Face 生图主链路
- [x] 拆分 direct / mirror 两种模式
- [x] 建立官方脸软锚点机制
- [x] 整理成 GitHub 可展示项目
- [ ] 接入更强的 reference-image editing 后端
- [ ] 抽象掉更通用的 workspace / path 配置
- [ ] 增加更完整的示例图 / 截图展示
- [ ] 加入数据集整理流程说明
- [ ] 为未来 LoRA 工作流预留更清晰接口

---

## 灵感来源

- 原始灵感项目： [SumeLabs/clawra](https://github.com/SumeLabs/clawra)
- 这个版本的目标：做一个更贴近 OpenClaw 日常使用、成本更低、但保留后续升级空间的实现

---

## 当前状态

当前这个仓库已经是一个**可实际使用的私有项目版本**，包含：

- Hugging Face 默认生图链路
- Gemini 默认禁用
- 官方脸软锚点支持
- 围绕 Raya 的 prompt 固化工作流

它现在已经能用；而更强的第二阶段，主要取决于后端升级，而不是只靠继续修 prompt。
