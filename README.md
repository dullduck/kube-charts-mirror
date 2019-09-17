# kube-charts-mirror
kubernetes helm 国内镜像，每一天更新一次


> 梯子的确没钱续费了。


## 推荐
微软也提供了`helm`的仓库, 推荐使用微软的仓库：

* stable: http://mirror.azure.cn/kubernetes/charts/
* incubator:	http://mirror.azure.cn/kubernetes/charts-incubator/


## 问题
`Helm`官方`Charts`仓库 https://kubernetes-charts.storage.googleapis.com/ 需要翻墙访问，阿里云提供了一个镜像仓库 https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts 然而从11月开始好像就没有更新过。

然而`Helm`的项目迭代得非常快，阿里镜像里连 rabbitmq-ha 都还没有。`Charts`仓库组织方式其实很简单，只有一个索引文件和对应压缩包，可以直接从官方源爬过来放到自己服务器下。这里使用了官方推荐的使用`GitPages`搭建`Charts`仓库的方式。

## 使用方式：
```shell
$ helm repo add stable https://cnych.github.io/kube-charts-mirror/
"stable" has been added to your repositories
$ helm repo list
NAME   	URL
stable 	https://cnych.github.io/kube-charts-mirror/
local  	http://127.0.0.1:8879/charts
bitnami	https://charts.bitnami.com/bitnami
```

通过`search`命令搜索查看是否有许多相关条目
```shell
$ helm search stable
......
```

或者参照以下步骤搭建您自己的仓库：

* 1.fork 该项目

* 2.clone代码到一台能访问国外地址的服务器
```shell
$ git clone https://github.com/${YourUsername}/kube-charts-mirror.git
```

* 3.启动更新容器(将GIT_REPO，GIT_USER_NAME，GIT_USER_EMAIL替换成您自己的)
```shell
docker build -t kube-charts-updater .
docker run \
-e GIT_REPO=https://cnych:XXX@github.com/cnych/kube-charts-mirror.git \
-e GIT_USER_NAME=cnych \
-e GIT_USER_EMAIL=ych_1024@163.com  \
-v /data/charts:/mnt/charts -d kube-charts-updater
```

以上`docker`镜像完成从原仓库爬取`charts`上传到对应`github`项目中的工作，也可以单独使用(比如爬取其他仓库)，环境变量参数为：
```
CHARTS_URL: 爬取的源仓库
GIT_REPO: 提交的项目地址
GIT_USER_NAME: git config 中的user.name
GIT_USER_EMAIL: git config 中的user.email
UPDATE_INTERVAL: 更新间隔，秒为单位，默认86400(1天)
```

* 4.在`fork`后的项目`settings`里设置开启`gitPages`，定位到`master`分支的`docs`

感谢@burdenbear 的贡献。


