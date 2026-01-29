## 带插件的自定义 Caddy 构建

本仓库构建了一个自定义的 [Caddy](https://caddyserver.com/) 二进制文件，其中捆绑了在腾讯云 DNS 和阿里 DNS 中管理证书所需的 DNS 插件。

### Docker 镜像

多阶段 `Dockerfile` 使用以下插件构建 Caddy 二进制文件：

- `github.com/caddy-dns/tencentcloud`
- `github.com/caddy-dns/alidns`

您可以通过 `CADDY_VERSION` 构建参数自定义上游 Caddy 基础版本（默认值为 `2`）。

```bash
# 本地构建
docker build -t my-custom-caddy --build-arg CADDY_VERSION=2.10.2 .
```

### GitHub Actions 工作流

`.github/workflows/docker.yml` 中的工作流：

1. 使用 `xcaddy` 构建 Docker 镜像。
2. 将镜像标记为 `latest` 和 `<CADDY_VERSION>`。
3. 将镜像推送到 GitHub 容器注册表 (GHCR)，地址为 `ghcr.io/<owner>/<repo>/custom-caddy`。

#### 前置条件

1. 创建 GitHub 仓库并将此项目推送到其中。
2. 为您的账户/组织启用 GitHub 容器注册表。
3. （可选）如果需要针对其他注册表进行身份验证，请定义仓库密钥。

推送到 `main` 分支后，工作流将自动运行。您也可以通过 GitHub Actions 中的*运行工作流*按钮手动触发它。

