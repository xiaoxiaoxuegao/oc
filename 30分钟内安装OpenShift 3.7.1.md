##30分钟内安装OpenShift 3.7.1
  [原文地址：https://blog.openshift.com/installing-openshift-3-7-1-30-minutes/]
  
 [此视频是针对开发人员的OpenShift更新：在30分钟内设置完整群集。](https://blog.openshift.com/openshift-developers-set-full-cluster-30-minutes/)

 我被开发者最常问道的一个问题是如何在本地使用OpenShift进行自己的开发。幸运的是，我们有几个选项，选择正确的选项取决于您喜欢使用的特定开发环境。

 例如，如果你喜欢在虚拟机上运行，​​而担心安装太多，那么[minishift](https://github.com/minishift/minishift)或官方的[CDK](https://blog.openshift.com/how-to-install-red-hat-container-development-kit-cdk-in-minutes/)可能就是你的首选。这两个选项利用您的操作系统管理程序或VirtualBox，主要区别在于minishift使用开源Origin项目，而CDK使用名为OpenShift Container Platform的企业版。

 在本地使用OpenShift的一种我最喜欢的方式是使用[oc群集](https://github.com/openshift/origin/blob/master/docs/cluster_up_down.md)。这是我每天使用的一个很棒的工具，但我建议你也看看我的团队编码和支持的[oc集群包装](https://github.com/openshift-evangelists/oc-cluster-wrapper)项目。 oc集群包装器项目的创建旨在帮助开发人员进一步实现诸如配置文件管理和持久卷等许多任务的自动化。

 在本地玩OpenShift之后，您会意识到您可以全天候安装OpenShift，以便公开承载您的项目。这是很多开发人员因为不是系统管理员而绊倒的地方。出于这个原因，我花了一些时间来创建一个视频，演示如何从头到尾安装OpenShift Origin 3.7.1。这意味着我创建一个裸虚拟机，安装操作系统，安装依赖项（如Docker），然后使用Ansible通过GitHub上的[installcentos存储库安装OpenShift](https://github.com/openshift-evangelists/oc-cluster-wrapper)。安装完成后，我将演示如何为公用主机名设置通配DNS。全部过程包括在在30分钟内。

 我希望你喜欢这个视频并使用OpenShift 3.7.1！

