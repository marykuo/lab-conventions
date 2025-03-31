# Ansible Git Repository 版本控制

1. [Target](#target)
1. [分支管理策略](#branching_strategy)
1. [Commit 風格](#commit)

## Target<a name="target"></a>

本指南旨在提供 Ansible Git Repository 的版本管理。

## 分支管理策略<a name="branching_strategy"></a>

使用 Mainline Development，所有變更直接提交到 main 分支，避免分支混亂。

## Commit 風格<a name="commit"></a>

使用清晰的 commit 訊息，例如：

- 軟體版本發佈
  - 只有上版至 production 時才需 commit，若僅上版至 uat，則不需 commit，確保 commit history 的清晰度。
  - commit message 需能對應到 Jira 上的 Release Version。
  ```shell
  release: 20250324
  hotfix: 20250325
  rollback: 20250324
  ```
- 修改應用程式設定（須註明環境）
  ```shell
  feat: setup timezone of rabbitmq container on prod, uat
  feat: change database account on uat
  ```
- 修改 Ansible 腳本
  ```shell
  feat: add gw-bpsp-api playbook
  chore: rename gw-bpsp-backend to gw-bpsp-auth
  ```
