# ubuntu

在新建置的 ubuntu host 上, 執行公司習慣的環境配置, 適用於 16.04 (xenial) 版本

## Before started

取得 [playbook source](https://github.com/softleader/playbooks#get-the-latest-playbooks)

## Prepare ansible environment

```sh
# 切換成 root
sudo su

# 設定 root 密碼 
passwd

# 安裝 ansible 及必要 package
# ubuntu-20以上請刪除 apt-add-repository -y ppa:ansible/ansible && \
apt-get update && \
apt-get install -y software-properties-common && \
apt-add-repository -y ppa:ansible/ansible && \
apt-get update && \
apt-get install -y ansible && \
apt-get install -y make
```

## Play

```sh
# 修改此次腳本執行的 config
vi group_vars/all.yml

# 執行所有腳本
make all

# 執行單一腳本
make role=${role}
```

### Role

- `common` - 安裝會用到的 apt packages, 並配製基本的環境設定(如加入 google dns-nameservers)
- `sshd` - 安裝 open-ssh 並允許 root 可以直接連線
- `ssh` - 將 ssh 的 key 等資料複製到 root (僅 ec2 適用)
- `nfs` - 安裝 nfs
- `docker` - 安裝 docker

## Add a worker node to SoftLeader Docker Swarm Ecosystem

1. *group_vars/all.yml* 只需要修改 hostname, 如 `swarm-worker-99`
1. `make role=common`
1. `make role=sshd`
1. `make role=docker`
1. `reboot` - 重啟後再 ssh 進入
1. `make role=nfs`
1. `ll /nfs` - 確認 nfs 已同步所有資料夾, 也許需要等待一小段同步時間
1. `ssh root@192.168.1.60 -- cat /root/docker-swarm-join-token | bash` - 加入 swarm

#### 讓 Swarm master trush 這台新的電腦

1. `ssh root@192.168.1.60`
1. `ssh-copy-id -i ~/.ssh/id_rsa.pub root@WORKER_NODE_IP`

## For Developers

以下方式可以簡單的模擬出一個環境去執行所有 playbooks 加以測試:

```sh
# 打包 image 後執行 container, 並連線進去
docker build -t playbooks . && docker run --rm -it playbooks bash

# 修改此次腳本執行的 config
vi group_vars/all.yml

# 執行想要測試的腳本 
make role=${role}
```
