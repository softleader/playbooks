# ec2 ubuntu

在 ec2 上新開一個 ubuntu VM 後, 自動配置所有系統參數

## Before started (在 local 端執行)

```sh
# 在 ec2 上建立腳本資料夾, 如: /home/ubuntu/playbook
ssh -i /path/to/key.pem ubuntu@ec2.ip "mkdir -p /home/ubuntu/playbook"

# ec2-ubuntu/ 資料夾下所有檔案 copy 到 ec2 的 vm 上
scp -i /path/to/key.pem -r ec2-ubuntu/* ubuntu@ec2.ip:/home/ubuntu/playbook
```

## Run ansible （在 ec2 上執行)

```sh
# 安裝 ansible
apt-get update && \
apt-get install -y software-properties-common && \
apt-add-repository ppa:ansible/ansible && \
apt-get update && \
apt-get install -y ansible

# 修改此次腳本執行的 config
vim /source/group_vars/all.yml

# 執行所有腳本
ansible-playbook /home/ubuntu/playbook/all.yml -vvv

# 執行單一腳本
ansible-playbook /home/ubuntu/playbook/play.yml -e 'role=${pick-a-role}' -vvv
```

### Role

- `common` - 安裝會用到的 apt packages
- `sshd` - 允許 root 可以直接連線
- `ssh` - 將 ssh 的 key 等資料複製到 root
- `nfs` - 安裝 nfs
- `docker` - 安裝最新版 docker
