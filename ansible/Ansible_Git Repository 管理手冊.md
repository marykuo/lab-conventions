# Ansible Git Repository 管理手冊

1. [Target](#target)
2. [Git Repository Structure](#git-repo-structure)
3. [權限管理與安全性](#security)
4. [版本管理與回滾策略](#versioning)

## Target<a name="target"></a>

本指南旨在提供 Ansible Git Repository 的最佳實踐，包括倉庫結構、版本管理、協作方式以及自動化流程。

## Git Repository Structure<a name="git-repo-structure"></a>

推薦使用以下的目錄結構，以提高可讀性與可維護性：

```
ansible-repo/
├── group_vars/          # 群組變數
│   ├── all.yml          # 所有主機共用的變數
│   ├── dev.yml          # 開發環境變數
│   ├── uat.yml          # 測試環境變數
│   └── prod.yml         # 正式環境變數
├── host_vars/           # 特定主機變數
│   ├── host1.yml        
│   └── host2.yml        
├── inventories/         # 主機清單
│   ├── dev              # 開發環境 inventory
│   ├── uat              # 測試環境 inventory
│   └── prod             # 正式環境 inventory
├── roles/               # Ansible 角色
│   ├── common/          # 共用角色
│   │   ├── tasks/       # 任務
│   │   ├── templates/   # Jinja2 模板
│   │   ├── files/       # 靜態文件
│   │   ├── handlers/    # 處理程序
│   │   ├── defaults/    # 預設變數
│   │   ├── vars/        # 角色變數
│   │   ├── meta/        # 角色元數據
│   ├── webserver/       # Web 伺服器角色
├── playbooks/           # Playbooks
│   ├── 00_provision.yml
│   ├── 00_provision_users.yml
│   ├── 01_deploy_rabbitmq.yml
│   ├── 02_deploy_redis.yml
│   ├── 10_deploy_web.yml
│   ├── 20_deploy_auth.yml
├── ansible.cfg          # Ansible 配置文件
├── requirements.yml     # 角色依賴
├── README.md            # 倉庫說明文件
```

- `files/`：存放應用程式相關檔案，例如 application.properties、definition.json 等。
- `group_vars/`：存放各環境變數，例如 Docker Image、Container Name、Volumes 等。
- `roles/`：用於組織 Ansible 角色（Roles），實現可重複使用的配置。
- `playbooks/`：存放 Playbook，按照編號劃分步驟，提高可讀性與可控性。
- `inventories/`：管理不同環境的 Inventory 文件，使不同環境的變更清晰可管理。
- `ansible.cfg`：Ansible 配置文件，定義預設行為。

## 權限管理與安全性<a name="security"></a>

1. **Git 倉庫權限**：
   - 設置適當的權限，避免未經授權的變更。
2. **變數加密**：
   - 使用 `ansible-vault` 保護敏感信息：
     ```sh
     ansible-vault encrypt group_vars/prod.yml
     ```
   - 解密查看：
     ```sh
     ansible-vault view group_vars/prod.yml
     ```
3. **SSH Key 安全性**：
   - 使用 CI/CD 時，透過 GitHub Secrets 或 GitLab Variables 設定 SSH Key。

## 版本管理與回滾策略<a name="versioning"></a>

1. 使用 Git Tag 標記版本：
   ```sh
   git tag -a v1.0 -m "Release version 1.0"
   git push origin v1.0
   ```
2. 回滾變更：
   ```sh
   git revert <commit_hash>
   ansible-playbook -i inventories/prod rollback.yml
   ```
