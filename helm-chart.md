###k8s的配置依赖存在很多的关系。
我的目标是希望用helm chart来把k8s配置自动化打包。
首先一个完备的k8s集群上的服务，如果要实现，非常复杂。
依赖层层叠叠，基本的依赖包括存储（PV）、网络（web service）等等
此上可能有数据库等依赖。

学习结果：
一些常见的命令行语句，在我初期不懂的时候，debug非常有效。
kubectl是最重要的k8s命令行控制语句，其他的，比如集群，可以用minikube or kind or others来生成，其过程比较简单。
kubectl get pod -A显示所有pod信息
kubectl get pod -n namespace显示当前命名空间的pod信息
kubectl describe pods -n namespace描述
kubectl logs pod -n namespace 可以对某个命名空间的pod日志进行追溯。
kubectl get deployment -A/-n namespace
helm install 和uninstall可以对chart进行安装和删除。

运行了chart文件之后，如何检测运行情况：
1.get deployment查看部署是否running，期望的replica是否个数相同。
2.get pods可以查看跑上述服务的几个pods
一种抽象的理解：deployment类似于docker里面的容器，而pod可以类比于镜像。
3.如果出错，最有效的查错手段：
  1.logs，日志永远能够报出问题所在。
  2.describe，查看描述文件，往往能找到异常的点
     由于describe里面不会直接地说明错误在哪，因此可以记录下发现的异常的点：
     1.node，如果node的节点为none，则说明你当前运行的服务是依赖于存储的，而这个存储依赖并不存在，因此可能无法部署该服务。
namespace是一个重要的判别标准，

针对我的目标，首先用最直观的方法，shell脚本语言将helm chart打包化。

设想中的输入可能包括：
最抽象的级别：只给用户的需求，所有参数自己配。
次高级：给定参数，自己生成部署文件。
目前：给定一个部署文件，放到chart里面去。
首先完成一个最简单难度的。
