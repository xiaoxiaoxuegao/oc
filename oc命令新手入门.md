
## oc命令新手入门

[原文地址：https://blog.openshift.com/oc-command-newbies/]

有一天我遇到了关于[bash](https://zwischenzugs.com/2018/01/06/ten-things-i-wish-id-known-about-bash/)的这篇文章。 如果你是专业bash用户，你可能已经知道所有这些技巧，但如果你是一个新手或不是这样的专业用户，这很有可能是你的一天。

我认为为oc命令创建类似的东西会很有用。 oc命令是dope，每个人都应该知道。它的设计精良，一致，灵活，并且如您所见，还有许多值得尝试的隐藏功能。

如果你是OpenShift专业人士，你可能已经知道我将在这里展示的大部分内容; 否则，如果你刚开始使用OpenShift，或者你不是一个有经验的用户，这将为你节省一些宝贵的时间和痛苦。</code></pre>

##1.首先要做的是调试
* 当我不知道发生了什么，或者收到错误消息时，我总是使用标志`--loglevel`。 它将日志级别信息写入stderr。 根据日志级别的不同，您将获得卷曲API Rest调用，API Rest正文答案或甚至更详细的信息。
![mahua](https://blog.openshift.com/wp-content/uploads/image2-8.png)
<pre>$ oc --loglevel 7 get pod
...
I0216 21:24:12.027793     973 cached_discovery.go:72] returning cached discovery info from /home/jtudelag/.kube/192.168.42.77_8443/v1/serverresources.json
I0216 21:24:12.028046     973 round_trippers.go:383] GET https://192.168.42.77:8443/api/v1/namespaces/myproject/pods
I0216 21:24:12.028052     973 round_trippers.go:390] Request Headers:
I0216 21:24:12.028057     973 round_trippers.go:393]     Accept: application/json
I0216 21:24:12.028061     973 round_trippers.go:393]     User-Agent: oc/v1.7.6+a08f5eeb62 (linux/amd64) kubernetes/c84beff
I0216 21:24:12.053230     973 round_trippers.go:408] Response Status: 200 OK in 25 milliseconds
I0216 21:24:12.055143     973 cached_discovery.go:119] returning cached discovery info from /home/jtudelag/.kube/192.168.42.77_8443/servergroups.json
I0216 21:24:12.055228     973 cached_discovery.go:72] returning cached discovery info from /home/jtudelag/.kube/192.168.42.77_8443/authentication.k8s.io/v1/serverresources.json
I0216 21:24:12.055288     973 cached_discovery.go:72]
...</pre>
* 如果您想修补OCP对象，则loglevel 9非常方便，因为它显示了您需要应用的修补程序（API请求主体）。
* 假设您想要更改服务对象的标签，在这种情况下，标签“app：hello-jorge”。
<pre>
$ oc --loglevel 9 edit svc hello-openshift
...
I0216 21:33:15.786463    1389 request.go:994] Request Body: {"metadata":{"labels":{"app":"hello-jorge"}}}
I0216 21:33:15.786590    1389 round_trippers.go:386] curl -k -v -XPATCH  -H "Accept: application/json" -H "Content-Type: application/strategic-merge-patch+json" -H "User-Agent: oc/v1.7.6+a08f5eeb62 (linux/amd64) kubernetes/c84beff" https://192.168.42.77:8443/api/v1/namespaces/myproject/services/hello-openshift
I0216 21:33:15.797185    1389 round_trippers.go:405] PATCH https://192.168.42.77:8443/api/v1/namespaces/myproject/services/hello-openshift 200 OK in 10 milliseconds
...
</pre>

* 注意：在绝望的时刻，你可以随心所欲的添加9个，结果会和9个是一样的，但会让你感到放心。
<pre>$ oc --loglevel 9999 get pod</pre>
##2.su - 
* 是的，你正在阅读正确的。 您可以替换正在运行oc命令的用户，或以OCP行话替换，可以[模拟](https://docs.openshift.com/container-platform/3.7/architecture/additional_concepts/authentication.html#authentication-impersonation)用户。 如果你有足够的权限来模仿，显然。 你只需要使用标识`--as`。
例如：
<pre># run the command as jorge user
$ oc --as=jorge get pods
</pre>
* 此外，可以完成组模拟，而不是用户模拟：
<pre># run the command as the developers group
$ oc --as-group=developers get pods
</pre>
在许多情况下，它非常方便快捷，例如，检查用户是否可以执行特定操作或检查用户在运行oc时会收到的输出。 这在搞乱角色和权限时也很有用。
##3.Whoami？
* `oc whoami`是众所周知的，特别是用于获取当前用户/会话的不记名令牌的`-t`标志。 但是当你有一个令牌时，会发生什么，而你不是谁的主人？
* 你可以做的一件事是用令牌登录OpenShift，然后做一个`oc whoami` ...但等一下。 `oc whoami`为您提供这些信息！ 只需在命令行中传递令牌作为第三个参数，不需要标志。
* 试一试：
<pre>
 # save the token
$ token=$(oc whoami -t)
 # get the owner of the token
$ oc whoami $token
jorge</pre>
##4.oc调试
* 你可以运行一个pod并获得一个shell。 有时，获取正在运行的pod配置的精确副本并使用shell进行故障排除是非常有用的。 这是默认行为。
* 查看`oc debug`选项，可以以root身份或任何其他用户标识运行容器，强制它在特定节点中运行或运行与shell不同的命令。
您必须针对有效的dc运行该命令，
* 例如：
<pre># get a shell inside a pod for dc/jorge
$ oc debug dc/jorge
 # same but as root user
$ oc debug --as-root=true dc/jorge
</pre>
##5.oc解释
* OpenShift / k8s对象有时很复杂，有许多字段。 很多时候，我最终都会在OCP文档或其他来源中查找对象定义示例。 当涉及到OCP / k8s对象定义时，您可以考虑`oc explain`真相的来源。
* `oc explain`为您提供资源及其字段的文档。 当声明新的OCP对象时，或者当您只是无法访问官方的OCP文档时，这非常有用。
* 例如，您可以获取pod文档和pod规范关联字段说明：
<pre># get pod explanation
$ oc explain pod
DESCRIPTION:
Pod is a collection of containers that can run on a host. This resource is created by clients and scheduled onto hosts.

FIELDS:
  metadata     <Object>
    Standard object's metadata. More info:
    http://releases.k8s.io/HEAD/docs/devel/api-conventions.md#metadata

  spec <Object>
    Specification of the desired behavior of the pod. More info:
    http://releases.k8s.io/HEAD/docs/devel/api-conventions.md#spec-and-status

  status       <Object>
    Most recently observed status of the pod. This data may not be up to date.
    Populated by the system. Read-only. More info:
    http://releases.k8s.io/HEAD/docs/devel/api-conventions.md#spec-and-status

  apiVersion   <string>
    APIVersion defines the versioned schema of this representation of an
    object. Servers should convert recognized schemas to the latest internal
    value, and may reject unrecognized values. More info:
    http://releases.k8s.io/HEAD/docs/devel/api-conventions.md#resources

  kind <string>
    Kind is a string value representing the REST resource this object
    represents. Servers may infer this from the endpoint the client submits
    requests to. Cannot be updated. In CamelCase. More info:
    http://releases.k8s.io/HEAD/docs/devel/api-conventions.md#types-kinds

  # get pod spec affinity field
  $ oc explain pod.spec.affinity
RESOURCE: affinity <Object>

DESCRIPTION:
    If specified, the pod's scheduling constraints

   Affinity is a group of affinity scheduling rules.

FIELDS:
  nodeAffinity <Object>
    Describes node affinity scheduling rules for the pod.

  podAffinity  <Object>
    Describes pod affinity scheduling rules (e.g. co-locate this pod in the
    same node, zone, etc. as some other pod(s)).

  podAntiAffinity      <Object>
    Describes pod anti-affinity scheduling rules (e.g. avoid putting this pod
    in the same node, zone, etc. as some other pod(s)).</pre>
    
##6.忘记grep，awk，cut等
<pre>* 关于oc命令的一个非常酷的事情是它具有格式化输出的内置功能。 我们都知道`-o json`或`-o yaml`，但`-o`标志为您提供了许多其他可能性。
* 从所有这些输出选项中，我发现`go-template`和`jsonpath`是最强大的：
<pre>json|yaml|wide|name|custom-columns=…|custom-columns-file=...|go-template=...|go-template-file=...|jsonpath=...|jsonpath-file=...</pre>
* 例如，假设您想获取特定路由（docker registry route）公开的服务：
<pre># get the service being exposed by a route, only if the hostname matches my-docker-registry.example.com
$ oc get routes -o=go-template='{{range .items}}{{if eq .spec.host "my-docker-registry.example.com"}}{{.metadata.name}}{{end}}{{end}}'
docker-registry</pre>
* 或者您想知道路由器dc的部署策略：
<pre># get router deployment strategy
$ oc get dc router -o=go-template='{{ .spec.strategy.type }}'
Rolling</pre>
* 正如你所看到的，oc命令很棒。 我鼓励你继续玩，因为OpenShift是最酷的事情之一。
* 作者Jorge Tudela Gonzalez de Riancho在红帽西班牙担任云端顾问，专门从事OpenShift和与容器相关的技术。
