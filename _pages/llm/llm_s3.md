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

# 数据处理
## 读取源文档
使用langchain的PyMuPDFLoader读取pdf文件，该api为pdf解析器中速度最快的一种，结果包含pdf及其页面的详细元数据，并且每页返回一个文档。
![langchain_pdf_reader](https://niysniysniys.github.io/_pages/llm/assets/langchain_pdf_reader.png)

读取markdown文档类似读取pdf文档，除了读取api不同，属性都一致。
```
from langchain.document_loaders.markdown import UnstructuredMarkdownLoader

loader = UnstructuredMarkdownLoader("../../data_base/knowledge_db/prompt_engineering/1. 简介 Introduction.md")
md_pages = loader.load()

```

## 数据清洗
可以看到上文中读取的pdf文件不仅将一句话按照原文的分行添加了换行符\n，也在原本两个符号中间插入了\n，我们可以使用正则表达式匹配并删除掉\n。
进一步分析数据，我们发现数据中还有不少的•和空格，我们的简单实用replace方法即可。
上文中读取的md文件每一段中间隔了一个换行符，我们同样可以使用replace方法去除。
```
import re
pattern = re.compile(r'[^\u4e00-\u9fff](\n)[^\u4e00-\u9fff]', re.DOTALL)
pdf_page.page_content = re.sub(pattern, lambda match: match.group(0).replace('\n', ''), pdf_page.page_content)
print(pdf_page.page_content)

pdf_page.page_content = pdf_page.page_content.replace('•', '')
pdf_page.page_content = pdf_page.page_content.replace(' ', '')
print(pdf_page.page_content)

md_page.page_content = md_page.page_content.replace('\n\n', '\n')
print(md_page.page_content)

```

## 文档分割
由于单个文档的长度往往会超过模型支持的上下文，导致检索得到的知识太长超出模型的处理能力，因此，在构建向量知识库的过程中，我们往往需要对文档进行分割，将单个文档按长度或者按固定的规则分割成若干个 chunk，然后将每个 chunk 转化为词向量，存储到向量数据库中。
在检索时，我们会以 chunk 作为检索的元单位，也就是每一次检索到 k 个 chunk 作为模型可以参考来回答用户问题的知识，这个 k 是我们可以自由设定的。

Langchain 中文本分割器都根据 chunk_size (块大小)和 chunk_overlap (块与块之间的重叠大小)进行分割。

chunk_size 指每个块包含的字符或 Token （如单词、句子等）的数量

chunk_overlap 指两个块之间共享的字符数量，用于保持上下文的连贯性，避免分割丢失上下文信息

RecursiveCharacterTextSplitter(): 按字符串分割文本，递归地尝试按不同的分隔符进行分割文本。
CharacterTextSplitter(): 按字符来分割文本。
MarkdownHeaderTextSplitter(): 基于指定的标题来分割markdown 文件。
TokenTextSplitter(): 按token来分割文本。
SentenceTransformersTokenTextSplitter(): 按token来分割文本
Language(): 用于 CPP、Python、Ruby、Markdown 等。
NLTKTextSplitter(): 使用 NLTK（自然语言工具包）按句子分割文本。
SpacyTextSplitter(): 使用 Spacy按句子的切割文本。


```
#导入文本分割器
from langchain.text_splitter import RecursiveCharacterTextSplitter

# 知识库中单段文本长度
CHUNK_SIZE = 500

# 知识库中相邻文本重合长度
OVERLAP_SIZE = 50

# 使用递归字符文本分割器
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=CHUNK_SIZE,
    chunk_overlap=OVERLAP_SIZE
)
text_splitter.split_text(pdf_page.page_content[0:1000])

split_docs = text_splitter.split_documents(pdf_pages)
print(f"切分后的文件数量：{len(split_docs)}")

print(f"切分后的字符数（可以用来大致评估 token 数）：{sum([len(doc.page_content) for doc in split_docs])}")


```

# 搭建并使用向量数据库
在最后一步中碰到一个bug。

error:
<font color=FF0000>'function' object has no attribute 'embed_documents'</font>
原因：
This is likely because the embed_documents method is not a part of the ZhipuAIEmbeddings class.
The ZhipuAIEmbeddings class in LangChain is designed to generate embeddings for individual documents, not for a list of documents. Therefore, it doesn't have an embed_documents method. 
解决：
![langchain_pdf_reader](https://niysniysniys.github.io/_pages/llm/assets/chroma_test.png)
