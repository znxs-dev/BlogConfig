---
title: hexo部署报错，找不到仓库
date: 2024-09-11 20:02:04
description: hexo部署报错，找不到仓库  具体解决方案
tags: 
  - 解决方案
categories:
password:
message:
cover: https://img.znxs.vip/study/202409112007583.png
top_img:
---

今天推送博客的时候发现这个报错

![hexo d 报错](https://img.znxs.vip/study/202409112007583.png)

网上搜索了一番解决方法，找到了解决办法  [解决使用Hexo搭建个人博客遇到的一些问题](https://blog.csdn.net/qq_59039063/article/details/132459418)

一般这种情况大多是网络原因导致的，科学上网工具不稳定，DNS解析被污染等因素。我们可以详细看看建立 ssh 连接的过程中发生了什么，可以使用 ssh -v命令，-v表示 verbose，会打出详细日志。

```powershell
PS W:\project\github-znxs\blog> ssh -vT git@github.com
OpenSSH_for_Windows_8.6p1, LibreSSL 3.4.3
debug1: Authenticator provider $SSH_SK_PROVIDER did not resolve; disabling
debug1: Connecting to github.com [20.205.243.166] port 22.
debug1: connect to address 20.205.243.166 port 22: Connection timed out
ssh: connect to host github.com port 22: Connection timed out
```

以看出，虽然访问的IP地址目测也没什么毛病，但是不管重复几次它还是连接超时导致无法部署，然后它显示SSH也连接失败，说明这个 22 端口目前是有问题的。

### 解决办法

1. 在 C:\Users\Administrator.ssh 中找到.ssh文件夹（此前配置SSH时会生成该文件夹）

   在 .ssh 文件夹中新建文本文件 config ,不带后缀（可以新建文本文档，去掉 .txt 后缀）

   打开 config 文件，输入以下内容，保存后即可，其中xxx@qq.com 为你自己的邮箱

   ```config
   Host github.com
   User xxx@qq.com
   Hostname ssh.github.com
   PreferredAuthentications publickey
   IdentityFile ~/.ssh/id_rsa
   Port 443
   ```

2. 修改hosts文件

   操作系统中 hosts 文件的权限优先级高于DNS服务器，在 C:\Windows\System32\drivers\etc 目录下找到并修改 hosts 文件，增加一条 github.com 的域名映射可以解决。

   ```config
   # github
   140.82.113.4 github.com
   ```

3. 再次运行`$ ssh -vT git@github.com`检查，发现连接成功了！

   ```powershell
   PS W:\project\github-znxs\blog> ssh -vT git@github.com
   OpenSSH_for_Windows_8.6p1, LibreSSL 3.4.3
   debug1: Reading configuration data C:\\Users\\zn/.ssh/config
   debug1: C:\\Users\\zn/.ssh/config line 1: Applying options for github.com
   debug1: Authenticator provider $SSH_SK_PROVIDER did not resolve; disabling
   debug1: Connecting to ssh.github.com [20.205.243.160] port 443.
   debug1: Connection established.
   debug1: identity file C:\\Users\\zn/.ssh/id_rsa type 0
   debug1: identity file C:\\Users\\zn/.ssh/id_rsa-cert type -1
   debug1: Local version string SSH-2.0-OpenSSH_for_Windows_8.6
   debug1: Remote protocol version 2.0, remote software version babeld-fdcea1d49
   debug1: compat_banner: no match: babeld-fdcea1d49
   debug1: Authenticating to ssh.github.com:443 as 'git'
   debug1: load_hostkeys: fopen C:\\Users\\zn/.ssh/known_hosts2: No such file or directory
   debug1: load_hostkeys: fopen __PROGRAMDATA__\\ssh/ssh_known_hosts: No such file or directory
   debug1: load_hostkeys: fopen __PROGRAMDATA__\\ssh/ssh_known_hosts2: No such file or directory
   debug1: SSH2_MSG_KEXINIT sent
   debug1: SSH2_MSG_KEXINIT received
   debug1: kex: algorithm: curve25519-sha256
   debug1: kex: host key algorithm: ssh-ed25519
   debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
   debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
   debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
   debug1: SSH2_MSG_KEX_ECDH_REPLY received
   debug1: Server host key: ssh-ed25519 SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU
   debug1: load_hostkeys: fopen C:\\Users\\zn/.ssh/known_hosts2: No such file or directory
   debug1: load_hostkeys: fopen __PROGRAMDATA__\\ssh/ssh_known_hosts: No such file or directory
   debug1: load_hostkeys: fopen __PROGRAMDATA__\\ssh/ssh_known_hosts2: No such file or directory
   debug1: checking without port identifier
   debug1: load_hostkeys: fopen C:\\Users\\zn/.ssh/known_hosts2: No such file or directory
   debug1: load_hostkeys: fopen __PROGRAMDATA__\\ssh/ssh_known_hosts: No such file or directory
   debug1: load_hostkeys: fopen __PROGRAMDATA__\\ssh/ssh_known_hosts2: No such file or directory
   debug1: hostkeys_find_by_key_cb: found matching key in C:\\Users\\zn/.ssh/known_hosts:1
   debug1: hostkeys_find_by_key_hostfile: hostkeys file C:\\Users\\zn/.ssh/known_hosts2 does not exist
   debug1: hostkeys_find_by_key_hostfile: hostkeys file __PROGRAMDATA__\\ssh/ssh_known_hosts does not exist
   debug1: hostkeys_find_by_key_hostfile: hostkeys file __PROGRAMDATA__\\ssh/ssh_known_hosts2 does not exist
   The authenticity of host '[ssh.github.com]:443 ([20.205.243.160]:443)' can't be established.
   ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
   This host key is known by the following other names/addresses:
       C:\Users\zn/.ssh/known_hosts:1: github.com
   Are you sure you want to continue connecting (yes/no/[fingerprint])?:
   【这里输入yes】
   Warning: Permanently added '[ssh.github.com]:443' (ED25519) to the list of known hosts.
   debug1: rekey out after 134217728 blocks
   debug1: SSH2_MSG_NEWKEYS sent
   debug1: expecting SSH2_MSG_NEWKEYS
   debug1: SSH2_MSG_NEWKEYS received
   debug1: rekey in after 134217728 blocks
   debug1: pubkey_prepare: ssh_get_authentication_socket: No such file or directory
   debug1: Will attempt key: C:\\Users\\zn/.ssh/id_rsa RSA SHA256:+TMCwpzDWA2iaDqVQh6UI9qKkIPySnAA+pISnbWl1gQ explicit
   debug1: SSH2_MSG_EXT_INFO received
   debug1: kex_input_ext_info: server-sig-algs=<ssh-ed25519-cert-v01@openssh.com,ecdsa-sha2-nistp521-cert-v01@openssh.com,ecdsa-sha2-nistp384-cert-v01@openssh.com,ecdsa-sha2-nistp256-cert-v01@openssh.com,sk-ssh-ed25519-cert-v01@openssh.com,sk-ecdsa-sha2-nistp256-cert-v01@openssh.com,rsa-sha2-512-cert-v01@openssh.com,rsa-sha2-256-cert-v01@openssh.com,ssh-rsa-cert-v01@openssh.com,sk-ssh-ed25519@openssh.com,sk-ecdsa-sha2-nistp256@openssh.com,ssh-ed25519,ecdsa-sha2-nistp521,ecdsa-sha2-nistp384,ecdsa-sha2-nistp256,rsa-sha2-512,rsa-sha2-256,ssh-rsa>
   debug1: SSH2_MSG_SERVICE_ACCEPT received
   debug1: Authentications that can continue: publickey
   debug1: Next authentication method: publickey
   debug1: Offering public key: C:\\Users\\zn/.ssh/id_rsa RSA SHA256:+TMCwpzDWA2iaDqVQh6UI9qKkIPySnAA+pISnbWl1gQ explicit
   debug1: Server accepts key: C:\\Users\\zn/.ssh/id_rsa RSA SHA256:+TMCwpzDWA2iaDqVQh6UI9qKkIPySnAA+pISnbWl1gQ explicit
   debug1: Authentication succeeded (publickey).
   Authenticated to ssh.github.com ([20.205.243.160]:443).
   debug1: channel 0: new [client-session]
   debug1: Entering interactive session.
   debug1: pledge: filesystem full
   debug1: client_input_global_request: rtype hostkeys-00@openssh.com want_reply 0
   debug1: client_input_hostkeys: searching C:\\Users\\zn/.ssh/known_hosts for [ssh.github.com]:443 / (none)
   debug1: client_input_hostkeys: searching C:\\Users\\zn/.ssh/known_hosts2 for [ssh.github.com]:443 / (none)
   debug1: client_input_hostkeys: hostkeys file C:\\Users\\zn/.ssh/known_hosts2 does not exist
   debug1: client_input_hostkeys: host key found matching a different name/address, skipping UserKnownHostsFile update
   debug1: client_input_channel_req: channel 0 rtype exit-status reply 0
   Hi znxs! You've successfully authenticated, but GitHub does not provide shell access.
   debug1: channel 0: free: client-session, nchannels 1
   Transferred: sent 3136, received 2960 bytes, in 0.6 seconds
   Bytes per second: sent 5069.3, received 4784.8
   debug1: Exit status 1
   ```

   冒号处输入yes，就连接成功了！

最后，输入 hexo d 就能够上传部署成功了！