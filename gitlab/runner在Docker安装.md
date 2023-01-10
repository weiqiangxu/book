# runner

[GitLab Runner介绍及安装](https://zhuanlan.zhihu.com/p/441581000)

[docker中安装runner](https://docs.gitlab.cn/runner/install/docker.html)

[注册docker.runner到gitlab](https://docs.gitlab.cn/runner/register/index.html#docker)

[简书gitlab+GitLab Runner注册](https://www.jianshu.com/p/a096ebd62275)

1. docker安装
```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun

sudo systemctl start docker
```

2. 登录gitlab获取token

> http://localhost:8899/admin/runners

3. 安装runner [本地卷]

```
mkdir -p /srv/gitlab-runner/config

docker run -d --name gitlab-runner --restart always \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:latest
```

```
# 注意这里需要填写gitlab地址
# 在docker之中访问宿主机的localhost
# 那么需要执行ifconfig查看网卡docker0的inet地址

docker run --rm -it -v /srv/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner register
```

4. 详细的配置过程
```
docker run --rm -it -v /srv/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner register

Enter the GitLab instance URL (for example, https://gitlab.com/):
<这里输入gitlab私仓的url>

Enter the registration token:
<这里输入runner的token>

Enter a description for the runner:
[007fa02670d1]: <给这个runner起个名字，会显示在gitlab中>

Enter tags for the runner (comma-separated):
<这里输入tag，跑任务的时候可以通过 tags 来指定>

Registering runner... succeeded runner=rANP_dLs
Enter an executor: docker, docker-ssh, parallels, shell, virtualbox, docker+machine, custom, ssh, docker-ssh+machine, kubernetes:
<运行方式，这里写 docker>

Enter the default Docker image (for example, ruby:2.6):
<默认运行容器，如果在job中不指定容器，默认采用的运行容器，这里我添了 tico/docker>

Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
```

5. 注册成功后在gitlab可以看到runner

> http://43.156.75.90:8899/admin/runners

### 进入docker容器验证可以ping到宿主机的localhost

```
ifconf

docker exec -it gitlab-runner /bin/bash

apt-get update && apt install iputils-ping

ping $ip

telnet $ip $port
```

### ifconfig的mtu\inet\inet6是什么

### 在docker容器内如何访问宿主机的localhost
