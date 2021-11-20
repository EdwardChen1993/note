[TOC]

# Jenkins

## 系统要求

最低推荐配置:

- 256MB可用内存
- 1GB可用磁盘空间(作为一个Docker容器运行jenkins的话推荐10GB)

为小团队推荐的硬件配置:

- 1GB+可用内存
- 50 GB+ 可用磁盘空间



## 安装

参考：[文档](https://www.jenkins.io/zh/doc/book/installing/)



## 运行

**第一步、使用docker拉取jenkins镜像并启动容器。**

[参考](https://github.com/jenkinsci/docker)

```bash
docker run -d --restart=always -u root -v /data/jenkins/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker -v /usr/local/go:/usr/local/go -p 11005:8080 -p 50000:50000 --name jenkins_test jenkins/jenkins:lts
# 启动后放行端口
firewall-cmd --add-port=11005/tcp --permanent
firewall-cmd --reload
```



**第二步、打印容器日志，复制输出的admin密码。**

```bash
docker logs jenkins_test -f
```

![image-20210421140209638](Jenkins.assets/image-20210421140209638.png)



**第三步、浏览器输入ip+端口查看jenkins页面，如：http://192.168.11.79:11005/。将第二步复制的密码输入，然后点继续。**

![image-20210421140048346](Jenkins.assets/image-20210421140048346.png)



**第四步、安装插件，插件下载比较缓慢，不同网络环境耗时不一样（部分插件需要连接谷歌服务，所以不使用VPN可能安装失败）。**

![img](https://pic1.zhimg.com/80/v2-0b7d1622804b2db0d9af9774ac65a0c0_720w.jpg)



**第五步、创建管理员账号。**

![image-20210421141628916](Jenkins.assets/image-20210421141628916.png)



**第六步、保存Jenkins URL，后续与gitlab进行连接时需要使用到。**

![image-20210421141910908](Jenkins.assets/image-20210421141910908.png)



**第七步、安装完成后点击重启，稍等片刻即可进入jenkins操作面板。**

![image-20210421142223217](Jenkins.assets/image-20210421142223217.png)





## 插件

### 使用清华镜像加速

**第一步、前往[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/)，搜索jenkins进入jenkins/updates目录。找到[update-center.json](https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json)文件，复制链接地址。**



**第二步、点击系统管理-插件管理-高级。将第一步复制的链接地址替换掉升级站点的URL。**

![image-20210421142704609](Jenkins.assets/image-20210421142704609.png)

![image-20210421142809639](Jenkins.assets/image-20210421142809639.png)



### 安装插件

**第一步、进入[官网](https://www.jenkins.io/zh/)，点击插件。**

![image-20210421144108053](Jenkins.assets/image-20210421144108053.png)



**第二步、搜索需要的插件名，点击插件进入详情。**

![image-20210421144310668](Jenkins.assets/image-20210421144310668.png)



**第三步、进入插件详情，点击右上角的Archives。**

![image-20210421144222952](Jenkins.assets/image-20210421144222952.png)



**第四步、点击插件版本号，即可开始下载插件。**

![image-20210421144405750](Jenkins.assets/image-20210421144405750.png)



**第五步、进入jenkins操作面板，点击系统管理-插件管理-高级。将第四步下载的hpi插件上传，即可开始安装插件。勾选“安装完成后重启Jenkins(空闲时)”，jenkins重启后即可完成安装。**

![image-20210421144536116](Jenkins.assets/image-20210421144536116.png)

![image-20210421144723850](Jenkins.assets/image-20210421144723850.png)



## gitlab

### 授权登录

**第一步、jenkins中安装gitlab相关插件。**

![image-20210422095354550](Jenkins.assets/image-20210422095354550.png)



**第二步、jenkins中系统管理-全局安全配置-授权策略，勾选Gitlab Commiter Authorization Strategy。然后Admin User Names输入gitlab用户名，勾选Use Gitlab repository permissions、Grant READ permissions to all Authenticated Users、Grant READ permissions for Anonymous Users、Grant ViewStatus permissions for Anonymous Users，最后点保存。**

![image-20210422095517218](Jenkins.assets/image-20210422095517218.png)



**第三步、gitlab中点管理中心-应用，点击New application。输入名称、Redirect URI、并在范围里勾选api。Redirect URI填写jenkins的ip地址 + /securityRealm/finishLogin即可。最后点保存。**

![image-20210422095721941](Jenkins.assets/image-20210422095721941.png)

![image-20210422095922367](Jenkins.assets/image-20210422095922367.png)



**第四步、在jenkins中系统管理-全局安全配置-安全域勾选Gitlab Authentication Plugin，将第三步保存后生成的应用id和密码，分别填写到Client ID和Client Secret。GitLab Web URI和GitLab API URI填写gitlab的ip地址即可。最后点保存。**

![image-20210422100112257](Jenkins.assets/image-20210422100112257.png)

![image-20210422100201746](Jenkins.assets/image-20210422100201746.png)



**第五步、以上步骤设置成功后，在jenkins中点右上角注销然后重新登录，就会出现gitlab授权登录页面，点击授权即可使用gitlab账号成功登录jenkins。**

![image-20210422095219545](Jenkins.assets/image-20210422095219545.png)





### 自动化构建

**第一步、jenkins中，操作面板界面，点击新建任务。输入任务名称，选择构建一个自由风格的软件项目，点击确定。**

![image-20210422103952136](Jenkins.assets/image-20210422103952136.png)

![image-20210422104044001](Jenkins.assets/image-20210422104044001.png)



**第二步、在gitlab中，创建一个示例项目。**

![image-20210422104211725](Jenkins.assets/image-20210422104211725.png)



**第三步、在jenkins中，点击右上角用户名，然后点击左侧凭据，点击全局凭据-添加凭据。**

![image-20210422104313818](Jenkins.assets/image-20210422104313818.png)

![image-20210422104535349](Jenkins.assets/image-20210422104535349.png)



**第四步、凭据类型选择SSH Username with private key，然后填写Username和Private Key。然后在虚拟机中输入家目录下新建一个临时目录，执行ssh-keygen生成公私钥对。将私钥填入Private Key中，最后点确定。**

![image-20210422104950638](Jenkins.assets/image-20210422104950638.png)



**第五步、在gitlab中，进入管理中心-部署密钥，点击新建部署密钥。然后将第四步生成的公钥填入键中，点击Create。**

![image-20210422105207169](Jenkins.assets/image-20210422105207169.png)

![image-20210422105251422](Jenkins.assets/image-20210422105251422.png)



**第六步、在gitlab中回到项目，点击左边设置-仓库，然后点击Deploy keys的展开。找到公开访问的部署密钥点击启用。**

![image-20210422105513951](Jenkins.assets/image-20210422105513951.png)

![image-20210422105610552](Jenkins.assets/image-20210422105610552.png)



**第七步、在gitlab中克隆项目的仓库地址，回到jenkins，点击任务列表中的任务名称，选择配置。在源码管理中选择Git，将克隆的仓库地址复制到Repository URL，然后点击Credentials下拉选项，选择第四步生成的凭据deploy。**

![image-20210422105900576](Jenkins.assets/image-20210422105900576.png)

![image-20210422110053444](Jenkins.assets/image-20210422110053444.png)



**第八步、在构建触发器中勾选Build when a change is pushed to GitLab. GitLab webhook URL: http://192.168.11.79:11005/project/front-dev，然后点高级，生成Secret token。**

![image-20210422110236763](Jenkins.assets/image-20210422110236763.png)

![image-20210422110418869](Jenkins.assets/image-20210422110418869.png)

![image-20210422110456454](Jenkins.assets/image-20210422110456454.png)



**第九步、在gitlab中，点击左侧设置-集成，点击转到Webhooks。将第八步的webhook URL和Secret token输入，然后取消勾选启用SSL验证。最后点击Add webhook。**

![image-20210422110803842](Jenkins.assets/image-20210422110803842.png)

![image-20210422110838552](Jenkins.assets/image-20210422110838552.png)![image-20210422110927311](Jenkins.assets/image-20210422110927311.png)



**第十步、在jenkins中，在构建中选择执行 shell，输入你要执行的shell命令，最后点击保存。**

备注：当在jenkins中安装Build with Parameters插件，可以使用”General“的”参数化构建过程“来添加自定义参数，然后在shell脚本中直接使用 `${参数名}` 即可。点击保存后，点击左侧Build with Parameters，然后点击开始构建即可。

![image-20210422111217674](Jenkins.assets/image-20210422111217674.png)

![image-20210423155042117](Jenkins.assets/image-20210423155042117.png)

![image-20210423155148212](Jenkins.assets/image-20210423155148212.png)

![image-20210423155449441](Jenkins.assets/image-20210423155449441.png)

备注：shell脚本代码如下：

```shell
#!/bin/bash

# 设置运行容器的名称
CONTAINER=${container_name}
PORT=${port}

# 使用docker build进行构建
# 使用--no-cache参数，保证每次不使用缓存
docker build --no-cache -t ${image_name}:${tag} .

# RUUNING变量去记录docker容器的运行状态
# 用于后面的判断
RUNNING=$(docker inspect --format="{{ .State.Running }}" $CONTAINER 2>/dev/null)

if [ ! -n $RUNNING ]; then
  echo "$CONTAINER does not exist."
  return 1
fi

# 这里是指容器已经创建，但是没有运行，可能是人工手动停止了。
# 或者是docker daemon重启后，容器停止了
if [ "$RUNNING" == "false" ]; then
  echo "$CONTAINER is not running."
  return 2
else
  echo "$CONTAINER is running"
  # 删除同名容器
  matchingStarted=$(docker ps --filter="name=$CONTAINER" -q | xargs)
  if [ -n $matchingStarted ]; then
    # 删除同名容器，先停止
    docker stop $matchingStarted
  fi

  matching=$(docker ps -a --filter="name=$CONTAINER" -q | xargs)
  if [ -n $matching ]; then
    # 删除同名容器，再删除
    docker rm $matching
  fi
fi

# 创建容器
# 注意这里的80端口，与自己的EXPOSE指定的端口要一致
docker run -itd --name $CONTAINER -p $PORT:80 ${image_name}:${tag}
```



**第十一步、在本地克隆gitlab对应的项目仓库，修改代码，然后提交并推送到gitlab。稍等片刻在jenkins中的构建队列即可看到对应的任务正在构建。点击任务名进入任务详情，在Build History上点击序号上显示的控制台输出，即可查看到构建过程中的控制台输出。构建成功后，即可在浏览器使用 ip+端口 访问服务。**

![image-20210422103705650](Jenkins.assets/image-20210422103705650.png)

![image-20210422103847102](Jenkins.assets/image-20210422103847102.png)

![image-20210425143102948](Jenkins.assets/image-20210425143102948.png)

