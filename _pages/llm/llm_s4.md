---
layout: archive
title: "LLM Section 4"
permalink: /llm/s4/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

# 将LLM接入LangChain

# 构建检索问答链
使用搭建好的向量数据库，对query查询问题进行召回，并将召回结果和query结合起来构建prompt，输入到大模型中进行问答。
![c4_1](https://niysniysniys.github.io/_pages/llm/assets/c4.png)

# 部署知识库助手
简单地构建streamlit应用。

首先需使用st.sidebar.text_input('api key', type='password')创建文本输入框，用户将llm模型的api秘钥输入进去。

使用st.form()构建文本框st.text_area()供用户输入。当用户点击Submit按钮时，将用户输入作为prompt，然后用llm生成回答。
![c4_2](https://niysniysniys.github.io/_pages/llm/assets/streamlit_app1.png)

这里使用`st.session_state.messages = []`创建跟踪对话。
- 将对话添加到对话历史中，`st.session_state.messages.append({"role": "assistant", "text": answer})`
- 显示对话历史：
  1. 判断对话用户：`message['role'] == 'assistant'`
  2. 显示对话：`messages.chat_message("assistant").write(message["text"]) `
![c4_3](https://niysniysniys.github.io/_pages/llm/assets/streamlit_app2.png)


这里遇到了个bug，原因是在创建llm时，使用的temperature参数设置为0了。
![c4_4](https://niysniysniys.github.io/_pages/llm/assets/streamlit_app3.png)

将temperature设置为0.1后，bug解决。
![c4_5](https://niysniysniys.github.io/_pages/llm/assets/cp1.png)
相同问题在带有历史记录中检索。
![c4_6](https://niysniysniys.github.io/_pages/llm/assets/cp2.png)
