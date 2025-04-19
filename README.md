# DockerHub to GHCR Sync

自动同步多个 DockerHub 镜像到 GitHub Container Registry（GHCR）。

## 使用说明

1. 编辑根目录的 `images.yaml` 文件，添加你需要同步的镜像。

2. 在仓库设置中添加以下 Secrets：

    | Secret 名称           | 用途 |
    |------------------------|------|
    | `DOCKERHUB_USERNAME`   | DockerHub 用户名 |
    | `DOCKERHUB_TOKEN`      | DockerHub 密码或 Token |
    | `GHCR_TOKEN`           | GitHub PAT，需有 write:packages 权限 |

3. 自动每 6 小时同步一次，也可以手动触发 Actions Workflow。
