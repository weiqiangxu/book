# 如何安装gitlab

### Linux上安装gitlab

[gitlab.cn/install中文安装手册](https://gitlab.cn/install/)

1. install in CentOS [其他安装看相关文章的“官方install手册”]

```
gitlab/gitlab-ee：完整的 GitLab 软件包，包含所有社区版功能以及 企业版功能
gitlab/gitlab-ce：一个精简的包，仅包含社区版功能
gitlab/unstable: 发布候选版本和其他不稳定版本
```
```
# RHEL/CentOS 6 and 7

$ curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash

$ The repository is setup! You can now install packages.

$ sudo EXTERNAL_URL="http://127.0.0.1:8899" yum install -y gitlab-jh
```

```
# 更新配置
gitlab-ctl reconfigure

# 重启GitLab
gitlab-ctl restart
```

2. 查看并修改密码

```
cat /etc/gitlab/initial_root_password
```

3. 访问gitlab


### 内容配置

```
# https://docs.gitlab.com/ee/administration/operations/puma.html
puma['per_worker_max_memory_mb'] = 1024
```

### 常用的一些命令

```
# 启动服务
gitlab-ctl start

# 更新配置
gitlab-ctl reconfigure

# 重启GitLab
gitlab-ctl restart

# 查看gitlab运行状态
sudo gitlab-ctl status

#停止gitlab服务
sudo gitlab-ctl stop

# 查看gitlab运行所有日志
sudo gitlab-ctl tail

#查看 nginx 访问日志
sudo gitlab-ctl tail nginx/gitlab_acces.log	

#查看 postgresql 日志
sudo gitlab-ctl tail postgresql	

# 停止相关数据连接服务
gitlab-ctl stop unicorn
gitlab-ctl stop sidekiq

# 系统信息监测
gitlab-rake gitlab:env:info
```

### 相关文章

[gitlab中文手册](https://docs.gitlab.cn/jh/index.html)

[官方install手册](https://docs.gitlab.com/ce/install/)

[知乎-如何搭建gitlab服务器——使用离线安装包部署](https://zhuanlan.zhihu.com/p/338882906)

[https://hub.docker.com/r/gitlab/gitlab-ce](https://hub.docker.com/r/gitlab/gitlab-ce)

[https://docs.gitlab.com/ee/install/docker.html](https://docs.gitlab.com/ee/install/docker.html)

[https://docs.gitlab.com/](https://docs.gitlab.com/)

[gitlab CI/CD](https://docs.gitlab.com/ee/ci/)

