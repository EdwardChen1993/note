[TOC]

# ssh 远程连接

ssh（Secure Shell）安全外壳协议，是一种加密的网络传输协议，可在不安全的网络中为网络服务提供安全的传输环境。默认端口是22。



## 连接远程服务器命令

```bash
ssh 用户名@服务器地址
```

**参数：**

* -p：指定端口，使用默认端口22不需要携带。
* -i：指定私钥文件路径



## 密钥登录

**第一步、服务器上制作密钥对：**

```bash
ssh-keygen
```

公钥：`id_rsa.pub`

私钥：`id_rsa`



**第二步、服务器上 `authorized_keys` 添加新的公钥：**

```bash
cd ~/.ssh
cat id_rsa.pub >> authorized_keys
```

为了确保连接成功，请保证以下文件权限正确：

```bash
chmod 600 authorized_keys
chmod 700 ~/.ssh
```



**第三步、服务器上设置 SSH，打开密钥登录功能：**

```bash
vi /etc/ssh/sshd_config
```

进行如下设置：

```bash
RSAAuthentication yes
PubkeyAuthentication yes
PermitRootLogin yes
```



**第四步、重启 SSH 服务：**

```bash
service sshd restart
```



**第五步、服务器上复制私钥内容：**

```bash
cat ~/.ssh/id_rsa
```



**第六步、客户机上在 `~/.ssh` 上新建一个私钥文件，将第五步的内容粘贴到里面：**

```bash
cd ~/.ssh
vi my_id_rsa # 粘贴第五步的内容
```

然后防止本地电脑上的私钥文件权限太大，修改私钥文件的权限：

```bash
chmod 600 my_id_rsa
```



**第七步、手动指定私钥文件登录：**

```ruby
ssh -i ~/.ssh/my_id_rsa 用户名@服务器地址
```

**备注：**

如果不希望每次登录都需要指定私钥文件，可以在客户机上的 `~/.ssh/config` 中配置主机，config文件用于做多密钥管理：

```bash
vi ~/.ssh/config
```

进行如下设置：

```bash
Host edward
Port 22
HostName 192.168.11.79
User root
IdentityFile ~/.ssh/my_id_rsa
```

* Host：ssh登录的主机名
* Port：端口
* Hostname：主机地址
* User：用户名
* IdentityFile：私钥地址



配置好即可在客户机上使用“ssh+主机名”直接登录远程服务器：

```sh
ssh edward
```

