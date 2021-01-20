---
title: gitlab部署及常见问题的处理
date: 2019-11-19 16:17:57
tags:
---
# 以 docker 容器部署的 gitlab 常见的问题

## 如何配置 smtp 邮件服务器

- ***首先得明白什么是 smtp ，为什么配置了 smtp 就可以正常接收发送邮件？***

- ***gitlab 中那些地方需要用到邮件？***

- ***如何配置gitlab***

1. SMTP 称为简单邮件传输协议 (Simple Mail Transfer Protocal)，是向用户提供高效、可靠的邮件传输。而 gitlab 配置了 smtp 实际上就是建立了 gitlab 与邮件服务器的连接，所以可以登录邮件服务器来发送和接收邮件。详细的解释可以参考邮件系统。

2. 在 gitlab 中新注册一个用户时，需要通过邮件的连接来初始化密码，或者是提交一个 issue 得到反馈，或者是 push 代码，等等。

3. 配置 gitlab 的过程也和简单

```sh
vim /etc/gitlab/gitlab.rb 

# 修改相应的配置

### Email Settings
 gitlab_rails['gitlab_email_enabled'] = true
 gitlab_rails['gitlab_email_from'] = '*********@163.com'
 gitlab_rails['gitlab_email_display_name'] = 'custom'
 gitlab_rails['gitlab_email_reply_to'] = '*********@163.com'
 gitlab_rails['gitlab_email_subject_suffix'] = '[GITLAB]'

### GitLab user privileges
# gitlab_rails['gitlab_default_can_create_group'] = true
# gitlab_rails['gitlab_username_changing_enabled'] = true

### GitLab email server settings
###! Docs: https://docs.gitlab.com/omnibus/settings/smtp.html
###! ******Use smtp instead of sendmail/postfix.******

 gitlab_rails['smtp_enable'] = true
 gitlab_rails['smtp_address'] = "smtp.163.com"
 gitlab_rails['smtp_port'] = 25
 gitlab_rails['smtp_user_name'] = "*********@163.com"
 gitlab_rails['smtp_password'] = "163授权码"
 gitlab_rails['smtp_domain'] = "163.com"
 gitlab_rails['smtp_authentication'] = "login"
 gitlab_rails['smtp_enable_starttls_auto'] = true
 gitlab_rails['smtp_tls'] = false

# 使配置重新生效
gitlab-ctl reconfigure

# 测试发送邮件
gitlab-rails console

Notify.test_email('yourEmail', 'Hello World', 'This is a test message').deliver_now
```
Notify.test_email('xxx@qq.com', 'Hello World', 'This is a test message').deliver_now
以上就是关于邮件的配置项，示例选用的163邮件服务器，也可以选用其它的。

***注意的点***

- 以上*********@163.com填写自己的邮箱。

- 密码指的是163授权码，该授权码是为了保护密码的安全性，当第三方的客户端登录163邮件服务器的时候需要输入的。

- 以上所有项生效需要将注释`#`打开。

***配置完以上的内容后访问初始化密码邮件中的连接会发现URL如图所示***

![waiting](01.PNG)

gitlab 是 ruby 写的，是运行在 rails 上的，所以我们可以查看 gitlab-rails 的配置文件，会发现 host 对应的是该容器的 id，而不是该容器所在虚拟机的IP，所以这里需要修改为虚拟机的 IP。

![waiting](02.PNG) 

```sh
# file's path 
# /opt/gitlab/embedded/service/gitlab-rails/config/gitlab.yml

host: 192.168.203.130

gitlab-ctl restart
```

这里有两个疑问？

- docker 是如何为容器分配 IP的，docker 容器的 IP 与 docker 宿主机的 IP 之间有什么关系？

// TODO

- gitlab.rb 与 gitlab.yml 这两个配置文件各自的作用是什么？ 

  通过查看这两个配置就能很快的发现他们的关系，gitlab.rb 是 ruby 的配置文件，而 gitlab.yml 是由 gitlab-ctl 管理的文件，这里说 gitlab.yml 中的手动修改将会被删除，如果要修改它得通过修改 gitlab.rb 然后 gitlab-ctl reconfigure 来重新生效配置。

gitlab.yml
> This file is managed by gitlab-ctl. Manual changes will be
> erased! To change the contents below, edit /etc/gitlab/gitlab.rb
> and run `sudo gitlab-ctl reconfigure`.

gitlab.rb
> This file is generated during initial installation and **is not** modified
> ! during upgrades.

## docker 部署的 gitlab 如何创建定时任务来备份数据

- ***备份恢复***

以下两个文件需要手动备份，因为这两个文件包含诸如密码的敏感信息，所以备份文件比较好的方案是写一个定时任务通过 shell 脚本备份。

以下为两个命令的使用

```bash
# bakcup
gitlab-rake gitlab:backup:create

Warning: Your gitlab.rb and gitlab-secrets.json files contain sensitive data 
and are not included in this backup. You will need to restore these files manually.

/etc/gitlab/gitlab.rb   # gitlab configure
/etc/gitlab/gitlab-secrets.json   # db secret

# recovery
cd /var/opt/gitlab/backups
gitlab-rake gitlab:backup:restore BACKUP=fileNo
```

备份的文件路径（该路径可以在 gitlab.rb 中修改）

```
gitlab_rails['backup_path'] = "/var/opt/gitlab/backups"
```

完整的备份 shell 脚本

```bash
#!/bin/bash
gitlab-rake gitlab:backup:create
cd /var/opt/gitlab/backups
newDir=$(date '+%Y%m%d-%H%M' )
mkdir $newDir

filename=$(ls -lt ./ | grep gitlab_backup | head -n 1 |awk '{print $9}')

echo '####################################'
echo 'Latest backup file is => $filename'
echo '####################################'

mv ./$filename ./$newDir

cp /etc/gitlab/gitlab.rb ./$newDir
cp /etc/gitlab/gitlab-secrets.json ./$newDir

echo 'Also you can find backups in /opt/docker_bak/gitlab/data/backups'
echo 'Complete!'
```
因为不知道两个 gitlab 的宿主机是怎样传输数据，所以恢复的脚本照着上面的逆着写就行了。还要注意一点**恢复数据的 gitlab 版本需要与原 gitlab 一致**。

**gitlab升级**

gitlab 升级主要的一点就是主版本的升级需要更新到该主版本的最新版本。

- ***cron 表达式***

|顺序|秒|分钟|小时|日期|月份|星期|
|:---:|---:|---:|---:|---:|---:|---:|
|取值|0-59|0-59|0-23|1-30(31)|1-12|1-7|
|特殊字符|, - * /|, - * /|, - * /|, - * / ? L W C|, - * /|, - * / L # C|

启动服务
```sh
service cron start
```

创建任务
```sh
crontab -e

# m h  dom mon dow   command
*/2 * * * * sh /backup.sh
```

查看任务
```sh
crontab -l
```