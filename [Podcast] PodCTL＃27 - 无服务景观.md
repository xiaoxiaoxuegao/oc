## [Podcast] PodCTL＃27 - 无服务景观
[原文地址：https://blog.openshift.com/podcast-podctl-27-serverless-landscape/](https://blog.openshift.com/podcast-podctl-27-serverless-landscape/).

技术趋势和技术辩论经常出现，特别是随着新技术的出现。 虽然我们许多人都专注于集装箱和Kubernetes，但人们一直在寻找“无服务器”和/或“功能即服务”（FaaS）概念的平行轨迹。 除了公有云上可用的一些无服务器服务外，还有超过6种无服务器实现（开源项目）运行在Kubernetes之上。 这引发了最新的技术争论 - 无服务器/ FaaS是否基于容器？

上周，CNCF的无服务器工作组发布了无服务器方面的一些声明，因此我们认为现在是时候了解他们的指导以及围绕整个行业的无服务器的更广泛的趋势。

**PodCTL - 容器| Kubernetes |OpenShift**
##PodCTL #27 - The Serverless Landscape
**概述**：Brian和Tyler谈论了CNCF新的无服务器工作组和白皮书，无服务器的四个要素，无服务器和FaaS的区别，以及Ops团队在无服务器世界中的持续作用。

**显示注释：**

* [CNCF无服务器白皮书](https://github.com/cncf/wg-serverless/tree/master/whitepaper#cncf-serverless-whitepaper-v10)    
* [CNCF中的无服务器工作组](https://www.slideshare.net/DanielKrook/event-specifications-state-of-the-serverless-landscape-and-other-news-from-the-cncf-serverless-working-group)  
* [事件规范](https://cloudevents.io/)
* [创新峰会 - 无服务器的状态](http://calcotestudios.com/talks/slides-innovate-summit-2017-state-of-serverless-the-cncf.html#/)
* [无服务景观（通过Redpoint VC）](https://medium.com/memory-leak/serverless-cloud-native-landscape-new-from-redpoint-ventures-and-the-cloud-native-computing-181711d885f7)
*  Kubernetes /无服务器框架 -  [Fission](http://fission.io/)，[Fn](https://fnproject.io/)，[Kubeless](http://kubeless.io/)，[Nuclio](https://nuclio.io/)，[OpenWhisk](https://openwhisk.apache.org/)，[OpenFaaS](https://www.openfaas.com/)，[Riff](https://github.com/projectriff/riff)（以及其他几个新兴）
* [无服务并不简单](https://medium.com/m/global-identity?redirectUrl=https://medium.freecodecamp.org/serverless-is-cheaper-not-simpler-a10c4fc30e49)
* [关于无服务器和操作系统的Twitter Rant](https://twitter.com/ibuildthecloud/status/965781691078868992)
* [适用于无服务器](https://opensource.com/article/17/8/ansible-serverless-applications)
**主题1** - 让我们来谈谈CNCF中的无服务器的历史，也许在PaaS和Kubernetes的背景下。

**主题2** - 在讨论无服务器时，似乎有4个方面需要解剖：

* 执行该函数的东西（这是一个容器管弦乐器）
* 数据源可以位于函数执行的任一侧
* 开发者的经验（或缺乏经验）
* 计费/用途/测光

**主题3**- 阅读CNCF无服务器白皮书的关键内容是什么？

**主题4** - 运营情况如何？ 这些工作是否会消失？ 是否有Ops用于无服务器？


