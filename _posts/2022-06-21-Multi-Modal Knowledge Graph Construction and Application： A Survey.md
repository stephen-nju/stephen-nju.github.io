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

# 1. Definition

传统图谱可以看成一个图 $$\mathcal{G} =\{ \mathcal{E},\mathcal{R},\mathcal{A},\mathcal{V},\mathcal{T_{\mathcal{R}}}, \mathcal{T_{\mathcal{A}}} \} $$ 其中 $$\mathcal{E}$$ 表示实体， $$\mathcal{R}$$表示关系， $$\mathcal{A}$$表示属性， $$ \mathcal{V}$$ 表示属性值， $$ \mathcal{T_{\mathcal{R}}} , \mathcal{T_{\mathcal{A}}} $$ 分别表示关系三元组和属性三元组。多模态的知识图谱作为传统知识图谱的一部分，其实就是图谱 $$ \{ \mathcal{E},\mathcal{R},\mathcal{A},\mathcal{V},\mathcal{T_{\mathcal{R}}}, \mathcal{T_{\mathcal{A}}} \} $$中知识的多模态化。按照这种思路，多模态图谱的表示大致分为两种:


- 1.A-MMKG<br>
    将多模态数据当作实体和概念的特定属性数据，图谱可表示为 $$\mathcal{G} =\{ \mathcal{E},\mathcal{R},\mathcal{A},\mathcal{V},\mathcal{T_{\mathcal{R}}}, \mathcal{T_{\mathcal{A}}} \} $$,其中
    $$\mathcal{T_{\mathcal{A}}}= \mathcal{E}\times\mathcal{A}\times (\mathcal{V_{\mathcal{KG}}}\cup \mathcal{V_{\mathcal{MM}}})$$


- 2.N-MMKG<br>
    将多模态数据表示为图谱中的实体，图谱可表示为 $$\mathcal{G} =\{ \mathcal{E},\mathcal{R},\mathcal{A},\mathcal{V},\mathcal{T_{\mathcal{R}}}, \mathcal{T_{\mathcal{A}}} \} $$,其中
    $$\mathcal{T_{\mathcal{R}}}= (\mathcal{E_{\mathcal{KG}}}\cup \mathcal{E_{\mathcal{MM}}})\times\mathcal{R}\times (\mathcal{E_{\mathcal{KG}}}\cup \mathcal{E_{\mathcal{MM}}})$$,显然该表示方案会扩充多模态知识图谱中的光系类型

*注*：理解属性和关系的区别
关系刻画的是实体与实体之间的联系，属性刻画的是实体本身固有特性

*注*：图谱如何表示，取决于图谱的本体如何设计和图谱如何应用

具体差异可以参考下图：
<figure>
<a><img src="{{site.url}}/pictures/mutlimodal_image_1.png"></a>
</figure>

## 2.Construction

多模态图谱构建的本质是构建传统KG中符号化的知识(包括实体、概念和关系)与它们相对应的图像之间的联系。从这个角度出发，图谱构建的方式的出发点就有两个，一个是图像，一个是传统的符号。
### 从图像到符号-图像打标(Labeling Image)
    
    我们从已有的图像当中抽取出KG中的符号表示。

### 从符号到图像-符号落地(Symbol Grounding)

