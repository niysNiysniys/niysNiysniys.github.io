---
layout: archive
title: "LLM Section 3"
permalink: /llm/s3/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

# 词向量
Embeddings将非结构化数据，转化为实数向量。

优势：
- 更适合检索：如果是文字，则匹配程度取决于关键词的数量或者是否完全匹配查询句。
如果是词向量，则可以通过计算问题与数据库中数据的点积、余弦距离、欧几里得距离等指标，直接获取问题与数据在语义层面上的相似度。
- 比其他媒介的综合信息能力更强：传统数据库存储文字、声音、图像、视频等多种媒介时，很难去将上述这些媒介构建关联与跨模态的查询方式，词向量可以通过多种向量模型将多种数据映射成统一的向量形式。


可以使用不同嵌入模型来构建词向量。
- 公司api
- 本地模型。

# 向量数据库
专门存储和检索向量数据的数据库系统。主要关注向量数据的特性和相似性。
每个向量代表一个数据项。
向量数据库通过计算与目标向量的余弦距离、点积等获取与目标向量的相似度。

主流向量数据库：
- chroma：轻量，不支持gpu加速。
- weaviate：开源，支持多种搜索方法的混合搜索。
- qdrant：使用rust开发，检索效率高。可以通过为页面内容和元数据指制定不同的键来复用数据。

# 使用Embedding API
## 智谱
![zhipu_api_test](https://niysniysniys.github.io/_pages/llm/assets/zhipu_api_test.png)
