# Ansible 自動化與 CI/CD

1. [Target](#target)
2. [檢查 Ansible Playbook](#auto_check)
   - [使用 GitHub Actions 自動化檢查](#auto_check_by_github_actions)
3. [自動化部署](#auto_deploy)
   - [使用 GitHub Actions 自動化部署](#auto_deploy_by_github_actions)

## Target<a name="target"></a>

本指南旨在提供 Ansible 自動化流程之最佳實踐。

## 檢查 Ansible Playbook<a name="auto_check"></a>

確保 Playbook 遵循最佳實踐，可使用 Ansible Lint 進行檢查：

```sh
ansible-lint playbooks/
```

### 使用 GitHub Actions 自動化檢查<a name="auto_check_by_github_actions"></a>
可在 `.github/workflows/ansible-ci.yml` 配置自動測試：
```yaml
name: Ansible CI

on: [push, pull_request]

jobs:
  ansible-lint:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Install Ansible
        run: sudo apt-get update && sudo apt-get install -y ansible

      - name: Run Ansible Lint
        run: ansible-lint playbooks/
```

## 自動化部署<a name="auto_deploy"></a>

可以撰寫 Playbook，並在 CI/CD 流程中觸發：

```sh
ansible-playbook -i inventories/prod deploy.yml
```

### 使用 GitHub Actions 自動化部署<a name="auto_deploy_by_github_actions"></a>

```hcl
name: Deploy with Ansible

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install Ansible
        run: |
          python -m pip install --upgrade pip
          pip install ansible

      - name: Configure SSH
        run: |
          mkdir -p ~/.ssh
          echo "${{ secrets.SSH_PRIVATE_KEY }}" > ~/.ssh/id_rsa
          chmod 600 ~/.ssh/id_rsa
          ssh-keyscan -H "${{ secrets.REMOTE_HOST }}" >> ~/.ssh/known_hosts

      - name: Run Ansible Playbook
        run: |
          ansible-playbook playbooks/deploy.yml -i inventories/prod -e env=prod
        env:
          ANSIBLE_HOST_KEY_CHECKING: "False"
```
