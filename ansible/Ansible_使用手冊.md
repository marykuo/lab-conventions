# ansible 使用手冊

## Assumption

- Ansible is installed.

## Setup

1. Modify `ansible.cfg` to specify your SSH private key file.
1. Update the `ansible_user` in the inventory to match the appropriate server user.

## Usage

Initialize Virtual Machine

```
ansible-playbook 00_provision.yml -i uat
ansible-playbook 00_provision_users.yml -i uat -e env=uat
```

deploy infra
```
ansible-playbook 01_deploy_rabbitmq.yml -i uat -e env=uat
ansible-playbook 02_deploy_redis.yml -i uat -e env=uat
```

deploy frontend

```
ansible-playbook 10_deploy_web.yml -i uat -e env=uat
```

deploy backend

```
ansible-playbook 20_deploy_auth.yml -i uat -e env=uat
```

## Test Ansible Playbooks

```
ansible-playbook --syntax-check 10_deploy_web.yml -i uat -e env=uat
```
