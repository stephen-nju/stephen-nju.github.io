---
title: Multi-Modal Knowledge Graph Construction and Application:A Survey
date: 2022-06-21
categories:
- Paper Reading

tags:
- 多模态
- 知识图谱

description: 多模态知识图谱综述
---

# Introduction
这是一篇关于多模态知识图谱的综述，作者从下面几个方面介绍了目前为止多模态知识图谱的相关工作，多模态知识图谱的构建，多模态任务的相关技术和多模态知识图谱的应用

# Construction

## 1. Definition

传统图谱可以看成一个图 $$\mathcal{G} =\{ \mathcal{E},\mathcal{R},\mathcal{A},\mathcal{V},\mathcal{T_{\mathcal{R}}}, \mathcal{T_{\mathcal{A}}} \} $$ 其中 $$\mathcal{E}$$ 表示实体， $$\mathcal{R}$$表示关系， $$\mathcal{A}$$表示属性， $$ \mathcal{V}$$ 表示属性值， $$ \mathcal{T_{\mathcal{R}}} , \mathcal{T_{\mathcal{A}}} $$ 分别表示关系三元组和属性三元组。多模态的知识图谱作为传统知识图谱的一部分，其实就是图谱 $$ \{ \mathcal{E},\mathcal{R},\mathcal{A},\mathcal{V},\mathcal{T_{\mathcal{R}}}, \mathcal{T_{\mathcal{A}}} \} $$中知识的多模态化。按照这种思路，可以将多模态知识图谱的构建大致分为两个方案
- 1.A-MMKG  
    将多模态数据当作实体和概念的特定属性数据，图谱 $$\mathcal{G} =\{ \mathcal{E},\mathcal{R},\mathcal{A},\mathcal{V},\mathcal{T_{\mathcal{R}}}, \mathcal{T_{\mathcal{A}}} \} $$,其中
    $$\mathcal{T_{\mathcal{A}}}= \mathcal{E}\times\mathcal{A}\times (\mathcal{V_{\mathcal{KG}}}\cup \mathcal{V_{\mathcal{MM}}})$$

- 2.N-MMKG
    将多模态数据表示为图谱中的实体

具体差异可以参考下图：
<figure>
<a><img src="{{site.url}}/pictures/mutlimodal_image_1.png"></a>
</figure>


