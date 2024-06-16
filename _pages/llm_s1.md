---
layout: archive
title: "LLM Section 1"
permalink: /llm/s1/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

# 项目参考
- datawhale 动手学大模型应用开发

# 项目简介
## 项目目的
1. 学习如何使用llm api开发llm应用。


# 第一章 大模型简介
“”“
2024.6.16
”“”
国外知名LLM：GPT-3.5、GPT-4、PaLM、Claude 和 LLaMA 等。
国内知名LLM：文心一言、讯飞星火、通义千问、ChatGLM、百川等。

大语言模型LLM相比于小型语言模型，在在解决复杂任务时表现出了惊人的潜力，这被称为“**涌现能力**”。以 GPT-3 和 GPT-2 为例，GPT-3 可以通过学习上下文来解决少样本任务。


## GPT
GPT 模型的基本原则是通过语言建模将世界知识压缩到仅解码器 **(decoder-only)** 的 Transformer 模型中，这样它就可以恢复(或记忆)世界知识的语义，并充当通用任务求解器。它能够成功的两个关键点：
- 训练能够准确预测下一个单词的 decoder-only 的 Transformer 语言模型
- 扩展语言模型的大小

## 开源LLM——LLaMa
LLaMA 系列模型是 Meta 开源的一组参数规模 从 7B 到 70B 的基础语言模型。
它们都是在数万亿个字符上训练的，展示了如何仅使用公开可用的数据集来训练最先进的模型，而不需要依赖专有或不可访问的数据集。
LLaMA 模型使用了大规模的数据过滤和清洗技术，以提高数据质量和多样性，减少噪声和偏见。
LLaMA 模型还使用了高效的数据并行和流水线并行技术，以加速模型的训练和扩展。

与 GPT 系列相同，LLaMA 模型也采用了 decoder-only 架构，同时结合了一些前人工作的改进：

- Pre-normalization 正则化：为了提高训练稳定性，LLaMA 对每个 Transformer 子层的输入进行了 RMSNorm 归一化，这种归一化方法可以避免梯度爆炸和消失的问题，提高模型的收敛速度和性能；
- SwiGLU 激活函数：将 ReLU 非线性替换为 SwiGLU 激活函数，增加网络的表达能力和非线性，同时减少参数量和计算量；
- 旋转位置编码（RoPE，Rotary Position Embedding）：模型的输入不再使用位置编码，而是在网络的每一层添加了位置编码，RoPE 位置编码可以有效地捕捉输入序列中的相对位置信息，并且具有更好的泛化能力。

## 其他开源LLM
1. 通义千问由阿里巴巴基于“通义”大模型研发，于 2023 年 4 月正式发布。2023 年 9 月，阿里云开源了 Qwen（通义千问）系列工作。2024 年 2 月 5 日，开源了 Qwen1.5（Qwen2 的测试版）。并于 2024 年 6 月 6 日正式开源了 Qwen2。 Qwen2 是一个 decoder-Only 的模型，采用 SwiGLU 激活、RoPE、GQA的架构。中文能力相对来说非常不错的开源模型。
2. GLM 系列模型是清华大学和智谱 AI 等合作研发的语言大模型。2023 年 3 月 发布了 ChatGLM。
3. Baichuan 是由百川智能开发的开源可商用的语言大模型。其基于Transformer 解码器架构（decoder-only）。

## LLM能力与特点
### 涌现能力（emergent abilities）
涌现能力：通俗来讲就是量变引起质变，模型性能随着规模增大而迅速提升，超过了随机水平。

LLM三个典型涌现能力：
- 
