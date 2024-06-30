---
title: 'spark kmeans'
date: 2024-6-17
permalink: /posts/2024/06/chatinfoextract/
tags:
  - cool posts
  - category1
  - category2
---

# 基于星火大模型的群聊对话分角色要素提取挑战
1. 注册科大讯飞新用户。
2. 实名制领取1亿tokens。
3. 在百度飞桨 AI Studio运行baseline。

在训练测试中发生以下error:
- index: 18 , error: Key '下一步跟进计划-具体事项' is not of type str
- index: 30 , error: Key '竞品信息' is not of type str
- SparkPythonSDK - ERROR - [/opt/conda/envs/python35-paddle120-env/lib/python3.10/site-packages/sparkai/llm/llm.py:687] - SparkLLMClient wait LLM api response timeout 30 seconds

运行时长：1099.989秒
消耗token数：360184

结果示例：
```
[
    {
        "infos": [
            {
                "基本信息-姓名": "",
                "基本信息-手机号码": "",
                "基本信息-邮箱": "",
                "基本信息-地区": "",
                "基本信息-详细地址": "浙江省吉林市沪太路4858号",
                "基本信息-性别": "",
                "基本信息-年龄": "",
                "基本信息-生日": "",
                "咨询类型": [],
                "意向产品": [
                    "巨石蓝海"
                ],
                "购买异议点": [],
                "客户预算-预算是否充足": "",
                "客户预算-总体预算金额": "",
                "客户预算-预算明细": "",
                "竞品信息": "",
                "客户是否有意向": "有意向",
                "客户是否有卡点": "无卡点",
                "客户购买阶段": "方案交流",
                "下一步跟进计划-参与人": [
                    "周杰5"
                ],
                "下一步跟进计划-时间点": "",
                "下一步跟进计划-具体事项": ""
            }
        ],
        "index": 1
    },
]
```
结果评分：18.59848
