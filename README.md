<h1 align="center">You Tao</h1>

<p align="center">
  <b>音频 · 视频多模态大模型研究者 · Audio-Visual Foundation Models</b><br/>
  <sub>从声音表示学习出发，一路走到把音视频编码器接入大语言模型</sub>
</p>

<p align="center">
  <img alt="Research" src="https://img.shields.io/badge/Research-Audio--Visual%20Foundation%20Models-6f42c1" />
  <img alt="Focus" src="https://img.shields.io/badge/Focus-Multimodal%20LLM-1f6feb" />
  <img alt="Approach" src="https://img.shields.io/badge/Approach-Reproducible%20%26%20Offline--first-2ea043" />
</p>

---

## 关于我

我是一名长期从事**音频与视频多模态大模型**（audio-visual foundation models）研究的工程研究者。
硕士到博士一直深耕视听（audio-visual）方向——从**声音表示学习**起步，逐步走向
**音视频跨模态对齐**与**视听语音理解**，直至把**音频 / 视频编码器接入大语言模型**。

我偏好**可复现、离线优先**的开源研究。下面每个项目都遵循同一套取舍：参考算法以
**纯 NumPy 核心**实现，`import` 时不拉入任何深度学习框架，无 GPU、甚至无网络也能
完整跑通与单测；需要真正训练神经网络时，再按需启用**可选的 PyTorch 后端**。这样既方便
在普通机器上做原型与教学，也保留了通往真实训练的通道。

## 一条研究主线

```
 声音表示学习   ──▶   音视频跨模态对齐   ──▶   视听语音理解   ──▶   接入大语言模型
  sonobase             avalign               avhark              avlingua
  自监督音频基座        对齐 / 同步 / 检索      唇音融合 · AV-ASR     编码器 → 投影器 → LLM
```

四个仓库不是彼此孤立的 demo，而是沿着上面这条主线逐级搭起来的：先给「音频侧」做一个
干净的自监督基座，再把音频与视觉对齐到同一个嵌入空间，接着在噪声场景下把两种模态融合
去理解语音，最后把这些编码器接进 LLM 完成生成式的视听理解。

## 精选项目

以下均为本账号自己维护的原创仓库（非 fork），每个都配有 CI、类型检查、文档与可运行示例。

| 项目 | 简介 | 关键词 |
| --- | --- | --- |
| [**sonobase**](https://github.com/HuggingNobody/sonobase) | 自监督音频表示学习工具包：掩码谱图重建（MAE）与对比学习（NT-Xent）预训练，作为视听研究的音频基座；MAE 与对比两条路线共用同一个 `SpectrogramTransformer` 编码器，预训练后可直接做线性探针 | `自监督` · `MAE` · `对比学习` |
| [**avalign**](https://github.com/HuggingNobody/avalign) | 音视频跨模态对齐与检索框架：用 InfoNCE / CLIP 对称损失把音频与视觉编码器对齐到同一嵌入空间，配套基于归一化互相关的同步偏移估计，以及双向 Top-K 跨模态检索（Recall@K / mAP） | `对比对齐` · `音视频同步` · `跨模态检索` |
| [**avhark**](https://github.com/HuggingNobody/avhark) | 面向噪声场景的视听语音理解框架：**SNR 自适应门控**融合唇动与音频——安静时多信任音频、嘈杂时多信任唇动——做鲁棒 AV-ASR（CTC 贪心 / beam）、说话人分离与声学事件理解（WER / CER / DER 评测） | `AV-ASR` · `说话人分离` · `噪声鲁棒` |
| [**avlingua**](https://github.com/HuggingNobody/avlingua) | 把预训练音视频编码器（Whisper / BEATs · CLIP-ViT / VideoMAE）经**可插拔投影器**（线性 / MLP / Q-Former）接入 LLM 词嵌入空间，统一视听问答、视频描述与多模态指令跟随；内置确定性 mock 后端，零权重即可跑通全链路 | `多模态 LLM` · `投影器` · `视听问答` |

## 研究兴趣

- **音频表示学习** —— 自监督预训练（掩码重建、对比学习）、谱图特征、可迁移的音频骨干
- **跨模态对齐** —— 音频 ↔ 视觉的对比对齐、音视频同步、跨模态检索
- **视听语音理解** —— 唇音融合、噪声鲁棒的 AV-ASR、说话人分离与声学事件理解
- **多模态大模型** —— 编码器到 LLM 的投影 / 适配、模态 token 的提示词组装、生成式视听理解

## 技术取向

<p>
  <img alt="Python" src="https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white" />
  <img alt="NumPy" src="https://img.shields.io/badge/NumPy-first-013243?logo=numpy&logoColor=white" />
  <img alt="PyTorch" src="https://img.shields.io/badge/PyTorch-optional-EE4C2C?logo=pytorch&logoColor=white" />
  <img alt="Reproducible" src="https://img.shields.io/badge/Reproducible-CPU%20%2F%20offline-2ea043" />
  <img alt="License" src="https://img.shields.io/badge/License-MIT-yellow" />
</p>

- **NumPy 优先的核心**：算法参考实现零重依赖，便于阅读、替换与逐模块单测。
- **PyTorch 只在需要时出现**：深度编码器与训练循环放在可选 `[torch]` extra，惰性加载。
- **诚实基线**：手写通路让每个深度组件的收益都能与可复现的基线对照，而非一味刷分。
- **完整工程化**：每个仓库都配有 CI（部分含 CodeQL）、类型检查、文档（架构 / 用法 / 设计笔记 / API）与可运行示例。

## 正在做的事

把上面四条线索继续往「统一的视听 + 语言」方向收拢——让同一套编码器 → 投影器 → LLM 的链路
既能听懂噪声中的语音，也能看懂画面并用自然语言作答，同时始终守住可复现、离线可跑的底线。

<sub>Built for reproducible, offline-first audio-visual research.</sub>
