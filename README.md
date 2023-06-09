# Docker Pack

Docker Pack 是一个自动化的工具，它可以将 Github 上的开源项目打包成 Docker 镜像，并推送到 Docker Hub 中。这样其他人就可以通过 Docker 方式部署这些项目，从而简化了部署流程并提高了开发效率。

## 使用方法

使用 Docker Pack 非常简单，只需要fork本项目后，在项目根目录下创建一个目录，目录名为需打包 Docker 镜像的名称，再在文件夹内创建一个 Dockerfile 文件即可。当需要打包或更新镜像时，在 GitHub Actions 中启动 Workflow，并输入镜像名称（即目录名）和tag，Docker Pack 会自动读取您的 Dockerfile 文件，并构建、打包和推送 Docker 镜像。

### 注意事项

- 请确保您的 Dockerfile 文件能够成功构建，否则 Docker Pack 将无法构建镜像。
- 请确保您的 Dockerfile 文件能够成功运行，否则您的 Docker 镜像可能无法正常工作。
- 请注意保护您的 Docker Hub 凭证，避免凭证泄漏导致不必要的损失。

## 直接使用作者打包的镜像

您也可以直接使用本项目打包好的镜像，作者只对开源项目进行 docker 镜像打包，不做其他修改，保证安全性。以下是可以使用的镜像列表：

- [surenkid/chatgpt-web-share](https://hub.docker.com/r/surenkid/chatgpt-web-share/tags)
- [surenkid/chatgpt-wework-robot](https://hub.docker.com/r/surenkid/chatgpt-wework-robot/tags)
- [surenkid/openai-api-proxy-key-pool](https://hub.docker.com/r/surenkid/openai-api-proxy-key-pool/tags)
- [surenkid/go-chatgpt-api](https://hub.docker.com/r/surenkid/go-chatgpt-api/tags)
- [surenkid/undetected-chromedriver](https://hub.docker.com/r/surenkid/undetected-chromedriver/tags)
- [surenkid/chatgpt-proxy-v4](https://hub.docker.com/r/surenkid/chatgpt-proxy-v4/tags)
- [surenkid/chatgpt-to-api](https://hub.docker.com/r/surenkid/chatgpt-to-api/tags)
- [surenkid/dujiaoka](https://hub.docker.com/r/surenkid/dujiaoka/tags)
- [surenkid/vmq](https://hub.docker.com/r/surenkid/vmq/tags)
- [surenkid/warp-cli](https://hub.docker.com/r/surenkid/warp-cli/tags)

### 提交 issue 或者 fork

本项目作者直接打包好的镜像，仅根据作者需求选择，如果您有其他需要打包的镜像，可以提交 issue，或者 fork 本项目自行构建。

## 许可证

Docker Pack 采用 [MIT 许可证](LICENSE)。
