---
author: "syui"
date: 2017-01-21
title: ArchのDockerイメージ
weight: 10
---

Arch Linux JPでArch LinuxのDockerイメージを配布しはじめました。

理由としては、DockerのArch Linuxイメージでどれを使えば良いのか迷ってしまう問題やイメージが古くなってしまう問題に対応するためです。

当該Dockerイメージは透明性のある方式でビルドされ、毎日アップデートした上でPushされます。ただし、この方式は予告なく変更される可能性があります(Travis CIの一部機能に依存するため)。

## 配布されているDockerイメージを使用する

### 直接実行する

```bash
$ sudo docker pull archlinuxjp/archlinux
$ sudo docker run -it archlinuxjp/archlinux /bin/bash
```

### ビルドして実行する

`Dockerfile`に記述し、ビルドして使用する場合はこちら

> Dockerfile

```bash
FROM archlinuxjp/archlinux
RUN pacman -S $PACK --noconfirm
CMD /bin/bash
```

```bash
$ sudo docker build -t tmp .
$ sudo docker run -it tmp /bin/bash
```

### Arch LinuxのDockerイメージを作成する

```bash
$ sudo docker pull archlinuxjp/docker-arch
$ sudo docker run -v /var/run/docker.sock:/var/run/docker.sock --privileged -d -it archlinuxjp/docker-arch /bin/bash
$ export id=`sudo docker ps -q | peco`
$ sudo docker exec $id docker-arch base
$ sudo docker exec $id /bin/bash /mkimage-arch-jp.sh
```

使用する`docker-arch`コマンドはオプションによって使用するスクリプトが異なります。詳しくはGitHubの[リポジトリ](https://github.com/ArchLinuxJP/docker-mkimage-arch/tree/master/mkimage-arch)をご確認ください。

## イメージの内容

基本的には以下のフローでイメージがアップデートされ続けています。

```
+--------+  Travis CI (Cron Jobs)   +------------+
| GitHub | -----------------------> | Docker Hub |
+--------+                          +------------+
```

具体的なアップデート方法は、Docker in Docker内でArch Linuxを起動し、その中で[docker/docker](https://github.com/docker/docker/blob/master/contrib/mkimage-arch.sh)のスクリプトを一部変更して実行するというものになっています。

Arch LinuxのDockerイメージを作成するために使用されるスクリプトはGitHubの各リポジトリで公開されています。

何かありましたら、[Slack](https://archlinuxjp.slack.com)もしくは、[GitHub](https://github.com/ArchLinuxJP)のIssueやPull Reqにてお願いします。

