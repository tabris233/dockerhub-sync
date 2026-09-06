# DockerHub to GHCR Sync

自动同步多个 Docker Hub 镜像到 GitHub Container Registry（GHCR）。工作流会保留多架构
manifest（例如 `amd64`、`arm64`）。

## 使用说明

1. 编辑根目录的 `images.yaml` 文件，添加你需要同步的镜像。

2. 在仓库设置中添加以下 Secrets：

    | Secret 名称 | 用途 |
    |---|---|
    | `DOCKERHUB_USERNAME` | Docker Hub 用户名 |
    | `DOCKERHUB_TOKEN` | Docker Hub access token |

   GHCR 使用 Actions 自带的 `GITHUB_TOKEN`，不需要创建或保存 GitHub PAT。工作流已按任务
   声明最小权限；若已有 GHCR 包并非由本仓库工作流创建，请在该包的 Actions access 设置中授权本仓库。

3. `images.yaml` 中每项必须包含 `source` 和 `target`：

    ```yaml
    - source: upstream/image:tag
      target: mirrored-image:tag
    ```

   请显式写出源和目标 tag。目标不应包含 `ghcr.io/<owner>/` 前缀，工作流会自动添加。
   目标名称在配置中必须唯一。

4. 自动任务每 6 小时的第 17 分钟运行，也可以手动触发 Actions Workflow。同步以 manifest digest
   判断是否更新；发生更新时通过 `skopeo copy --all --preserve-digests` 复制完整镜像。

## 安全说明

- 不要将令牌写入脚本或提交到仓库。用于本地调试的 `ghcr.sh` 已被 Git 忽略，并只从环境变量读取令牌。
- 包可见性由 GHCR 的 Package settings 管理。已有镜像会保持现有可见性；新包通过
  `GITHUB_TOKEN` 创建时会继承仓库的可见性设置。
