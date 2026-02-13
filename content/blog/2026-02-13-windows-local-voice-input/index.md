---
title: Windows 本地语音输入方案配置记录 (2026.02)
author: Ji Huang
date: '2026-02-13'
categories:
    - misc
tags:
    - windows
    - speech-to-text
    - dictation
    - sensevoice
    - lazytyper
    - whisper
description: 在无独显 Windows 笔记本上，用 LazyTyper + SenseVoice 做本地、免费、低延迟的中英混合语音输入。
---

> **简介**：这篇是我在 Windows 笔记本（无独显、只用 CPU）上折腾本地语音输入的配置记录：主力用 **LazyTyper + SenseVoice Small**，同时把几个我觉得值得试的备选方案也放一起方便对照。

## ✅ 背景与需求

* **硬件环境**：Windows 笔记本，无独立显卡，只能 CPU 推理。
* **语言场景**：中英文混合输入为主，偶尔纯英文。
* **核心诉求**：必须本地离线（隐私 + 无网络延迟），并且尽量免费。

---

## 🧩 当前主力方案：LazyTyper + SenseVoice Small

我目前用下来，在“无显卡 + 中英混合”这个场景里，这套组合最省心。

### 1) 客户端：LazyTyper

* **定位**：本地语音输入的 GUI 封装工具。
* **优点**：免费、开箱即用；界面直观；支持一键下载/切换本地模型；资源占用相对克制。
* **获取**：[LazyTyper 官网](https://lazytyper.com/) | [GitHub Releases](https://github.com/oldcai/LazyTyper-releases/releases) ![GitHub stars](https://img.shields.io/github/stars/oldcai/LazyTyper-releases?style=social)

### 2) 模型：SenseVoice Small

* **发布方**：FunAudioLLM（阿里巴巴通义实验室）。
* **体积**：Int8 量化后约 894 MB。
* **我选它的原因**：
    - **中英混合识别强**：对中英夹杂（“Chinglish”）的识别很稳，我的体验是明显优于 OpenAI Whisper。
    - **CPU 也够快**：非自回归架构，延迟很低；在无独显笔记本上也能做到很快的响应。
* **项目地址**：[GitHub - FunAudioLLM/SenseVoice](https://github.com/FunAudioLLM/SenseVoice) ![GitHub stars](https://img.shields.io/github/stars/FunAudioLLM/SenseVoice?style=social)

---

## 🧰 备选方案（都是开源免费）

如果你想更极客、更极致性能、或者需要长音频转写，可以从下面三个里挑：

### A) 轻量/极客：CapsWriter-Offline

* **适用场景**：极低内存占用；或偏好“按下 CapsLock 说话”的交互。
* **特点**：
    - 完全开源、无广告，隐私风险低。
    - 同样支持 **SenseVoice** / **Paraformer**，并针对 CPU 做了极致优化。
    - 无 GUI 干扰，后台静默运行。
* **项目地址**：[GitHub - HaujetZhao/CapsWriter-Offline](https://github.com/HaujetZhao/CapsWriter-Offline) ![GitHub stars](https://img.shields.io/github/stars/HaujetZhao/CapsWriter-Offline?style=social)

### B) Whisper 极致速度：WhisperDesktop（Const-me）

* **适用场景**：追求纯英文识别的速度；或者在老硬件上跑 Whisper。
* **特点**：
    - C++ 实现，主要走 DirectX 做 GPU 加速，但也能 CPU 跑。
    - 是我见过的 Windows 上跑 OpenAI Whisper **速度非常快**的实现之一。
    - Win32 风格界面，但胜在稳定。
* **局限**：只支持 Whisper；中英混合能力一般不如 SenseVoice。
* **项目地址**：[GitHub - Const-me/Whisper](https://github.com/Const-me/Whisper) ![GitHub stars](https://img.shields.io/github/stars/Const-me/Whisper?style=social)

### C) 跨平台/长音频：Buzz

* **适用场景**：会议录音转写、视频字幕（SRT）。
* **特点**：
    - 基于 OpenAI Whisper，英文识别率高。
    - 支持导入音频/视频文件离线处理。
* **项目地址**：[GitHub - chidiwilliams/buzz](https://github.com/chidiwilliams/buzz) ![GitHub stars](https://img.shields.io/github/stars/chidiwilliams/buzz?style=social)

---

### D) 另一个我准备试试的：闪电说

这个是我在 X 上看到有人推荐的（我自己还没深度用过），先放在备选里占个位。

* **项目主页**：<https://shandianshuo.cn/#>

---


## ✅ 总结

现在对我来说，**LazyTyper + SenseVoice Small** 是“易用性”和“性能”之间最平衡的一套本地语音输入方案。

* 想要更轻量、更开源、更偏快捷键流：试 **CapsWriter-Offline**。
* 纯英文优先、想把 Whisper 跑到更快：试 **WhisperDesktop**。
