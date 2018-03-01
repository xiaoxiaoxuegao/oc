# 使用CDK上的Ansible Playbook Bundles开始
[原文地址：https://blog.openshift.com/getting-started-ansible-playbook-bundles-cdk/](https://blog.openshift.com/getting-started-ansible-playbook-bundles-cdk/).

这篇文章最初由Sebastian Faulhaber在[OpenSourcers.com](http://www.opensourcerers.org/getting-started-with-ansible-playbook-bundles-on-cdk/)上发布。

##将Ansible Service Broker插件安装到您的CDK安装中

在注册时启动您的[红帽容器开发工具包（CDK）](https://developers.redhat.com/products/cdk/overview/)环境（这很重要，因为在Addon安装期间yum被使用）。
<pre>
export MINISHIFT_ENABLE_EXPERIMENTAL=y
minishift start --service-catalog
</pre>
克隆插件存储库并安装Ansible Service Broker Addon：
<pre>
git clone https://github.com/minishift/minishift-addons.git
cd minishift-addons/add-ons/
minishift addon install ansible-service-broker
minishift addon apply ansible-service-broker
</pre>
登录到CDK控制台时，应该已经看到预先安装的Ansible APB：
![alt img](https://blog.openshift.com/wp-content/uploads/ansible1.png)
##在客户机上安装ABP命令行
配置您的shell以使用Minishift Docker守护进程：
<pre>
eval $(minishift docker-env)
</pre>
获取APB命令行脚本并使其在PATH中可用：
<pre>
wget https://raw.githubusercontent.com/ansibleplaybookbundle/ansible-playbook-bundle/master/scripts/apb-docker-run.sh && mv apb-docker-run.sh apb && chmod +x apb
</pre>
验证您的安装是否有效：
<pre>
apb --help
</pre>
如果一切顺利，你应该看到如下情况：
![alt img](https://blog.openshift.com/wp-content/uploads/ansible2.png)
##测试ABP CLI和CDK之间的连接

您将需要特殊权限才能与CDK安装中的代理一起使用。 因此，我们需要执行以下操作：
<pre>
oc adm policy add-cluster-role-to-user cluster-admin developer
oc login -u developer
</pre>
现在让我们看看我们是否可以列出预先安装的APB：
<pre>
apb list
</pre>
![alt img](https://blog.openshift.com/wp-content/uploads/ansible3.png)
https://blog.openshift.com/wp-content/uploads/ansible3.png
##配置Ansible Service Broker从本地注册表中提取图像

在我们的Ansible Service Broker的默认配置中，APB从`https://registry.hub.docker.com/`中提取。 我们需要将其更改为在CDK内运行的本地注册表。
![alt img](https://blog.openshift.com/wp-content/uploads/ansible4.png)
<pre>
registry:
- type: local_openshift
name: lo
namespaces:
- openshift
white_list:
- ".*-apb$"
</pre>
Ansible Service Broker窗格现在需要重新启动才能引入新配置。
##创建你的第一个APB

首先，我们使用CLI搭建我们的新服务：
<pre>apb init sample-service-apb
cd sample-service-apb</pre>
现在我们在本地建立我们的APB。 该过程完成后，新建的APB泊坞窗镜像应该出现在您的本地Docker注册表中：
<pre>apb build</pre>
![alt img](https://blog.openshift.com/wp-content/uploads/ansible5.png)
最后，我们需要将Docker镜像推送到CDK内部的Service Broker中：
<pre>apb push
</pre>
现在您应该能够在CDK的服务目录中看到您的第一个APB：
![alt img](https://blog.openshift.com/wp-content/uploads/ansible6.png)
##参考

从这些资源中了解更多关于Ansible Playbook开发和Minishift插件的信息：

* [Ansible Playbook文档](https://github.com/ansibleplaybookbundle/ansible-playbook-bundle/blob/master/docs/apb_cli.md#installing-the-apb-tool)
* [开始使用APB开发](https://docs.openshift.com/container-platform/3.7/apb_devel/writing/getting_started.html)
* [Minishift Addon - Ansible Service Broker](https://github.com/minishift/minishift-addons/tree/master/add-ons/ansible-service-broker)
