# ec2 ubuntu

在 ec2 上新開一個 ubuntu VM 後, 自動配置所有系統參數

## Before started

參考 [curl指令範例](https://github.com/softleader/playbooks#get-the-latest-playbooks) 在 ec2 上取得最新的 playbook source

## Run ansible

```sh
# 切換成 root
sudo su

# 安裝 ansible
apt-get update && \
apt-get install -y software-properties-common && \
apt-add-repository ppa:ansible/ansible && \
apt-get update && \
apt-get install -y ansible

# 修改此次腳本執行的 config
vim group_vars/all.yml

# 執行所有腳本
ansible-playbook all.yml -vvv

# 執行單一腳本
ansible-playbook play.yml -e 'role=${pick-a-role}' -vvv
```

### Role

- `common` - 安裝會用到的 apt packages
- `sshd` - 允許 root 可以直接連線
- `ssh` - 將 ssh 的 key 等資料複製到 root
- `nfs` - 安裝 nfs
- `docker` - 安裝最新版 docker
