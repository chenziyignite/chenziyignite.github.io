---
layout: post
title: Weekly papers---0x1
description: K-BERT
image: assets/images/kg.png
---

<div id="main" class="alt">
    <section id="one">
        <div class="inner">
            <header class="major">
                <h1>K-BERT</h1>
            </header>
            <h2 id="intro">Intro</h2>
            <p>
                这篇Paper的题目是K-BERT: Enabling Language Representation with Knowledge Graph，是由北大、腾讯研究院和北师大联合发表在AAAI2020上的一篇long paper。顾名思义，K-BERT的K指的就是Knowledge，这篇paper也代表了研究NLU相关问题的一种思路，就是在语言模型中嵌入独立于task本身的外部知识，通常可以利用知识库和知识图谱等，从而增强语言模型的能力来提高下游任务的表现。
            </p>
            <h2 id="motiv">Motivation</h2>
            <p>
                运用外部知识的原因很直观。通常情况下的预训练语言模型，比如Google BERT，都是在开放域的语料下进行预训练的。开放域意味着语料的主题很广泛，比如Wikipedia dump就是一种开放域的语料。而如果当前的NLU task是一个与某一特定领域很相关的问题，比如仅仅是关于医学知识的一些文本理解，这类语言模型就只能在fine-tune的时候才可以学习该领域有关的语料，这显然是不充分的。<u>那怎么把领域相关的知识“告诉”语言模型呢？</u>有一种想法是把领域相关的语料拿去给模型预训练，但问题是某些医学词汇极为生僻，可能模型需要很多很多语料才能学到某一个专有名词的含义，这显然是低效且不现实的。因此，作者想到把知识图谱中的三元组输入给模型，同时为了避免计算资源的缺乏，作者只在fine-tune阶段利用了知识图谱。
            </p>
            <h2 id="content">Content</h2>
            <div class="row">
                <div class="6u 12u$(small)">
                    <p>
                        右图是K-BERT的一个简单的框架。我们不难发现作者在四个模块上有所创新，分别是knowledge layer, embedding layer, seeing layer和mask-transformer encoder。对于knowledge layer我们应该关注作者是如何把KG的triples注入到input sequence中的；对于embedding layer我们应当关注作者如何给sentence tree做嵌入的，我们还要关注seeing layer和visible matrix，以及它们是如何被encoder层利用的。下面我将就这几方面精简地说明一下。
                    </p>
                    <p>
                        首先看knowledge layer，输入是一个sentence和一个KG，输出是一个sentence-tree。首先我们需要用一定的策略查找KG的triples(K_Query)，我们都知道triple的格式一般是(entity1，relation，entity2)，所以对于sentence中的每个entity，如果有一个triple是和这个entity有关的，那么就直接把triple中剩余的两个元素附加在它后面(K_Inject)，在逻辑结构上我们会认为这是一棵子树。在本文中作者限制树高为1，即不能在附加的entity后面再附加一个与之相关的triple。总而言之，knowledge layer一共就两部分，一个是K_Query，一个是K_Inject。
                    </p>
                </div>
                <div class="6u$ 12u$(small)">
                    <span class="image fit"><img src="{% link assets/images/kbert.jpg %}" alt="" /></span>
                </div>
                <div class="content">

<p>
   embedding layer主要关注如何把sentence-tree转化成embedding。与BERT类似，embedding也是由三部分构成，分别是token embedding，position embedding，segment embedding。token embedding沿用了Google BERT的embedding，在position embedding中，作者使用了soft-position embedding来解决因为简单地把entity和relation嵌入句子而破坏了句法结构和语义关联性等性质的问题。segment embedding也沿用了Google BERT的方式。这一部分的主要创新点就在于position embedding的部分。 
</p>
<p>
   seeing layer是作者认为与BERT相差最大的地方。它主要解决的是一个knowledge noise的问题。<br>
   未完待续
</p>









                </div>
            </div>
        </div>
    </section>
</div>


