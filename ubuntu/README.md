# ubuntu

在新建置的 ubuntu host 上, 自動配置所有系統參數 

## Before started

取得 playbook source

## Prepare ansible environment

```sh
# 切換成 root
sudo su

# 設定 root 密碼 
passwd

# 安裝 ansible
apt-get update && \
apt-get install -y software-properties-common && \
apt-add-repository -y ppa:ansible/ansible && \
apt-get update && \
apt-get install -y ansible
```

## Play

```sh
# 修改此次腳本執行的 config
vi group_vars/all.yml

# 執行所有腳本
ansible-playbook all.yml -vvv

# 執行單一腳本
ansible-playbook play.yml -e 'role=${pick-a-role}' -vvv
```

### Role

- `common` - 安裝會用到的 apt packages, 並配製基本的環境設定(如加入 google dns-nameservers)
- `sshd` - 安裝 open-ssh 並允許 root 可以直接連線
- `ssh` - 將 ssh 的 key 等資料複製到 root (僅 ec2 適用)
- `nfs` - 安裝 nfs
- `docker` - 安裝 docker
