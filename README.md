# Left 4 Dead 2 战役整合包服务器

这是一个用于 Left 4 Dead 2 专用服务器的容器项目，使用 GHCR 镜像启动 Left 4 Dead 2 Dedicated Server，适合临时开战役整合包、插件服或三方图服务器。

## 主要功能

### 镜像构建

GitHub Actions 会把镜像发布到 GHCR。可选在仓库中设置 Steam 账号密码。

### 插件管理

通过 Go 入口程序在容器启动时处理插件目录

- 支持把插件内的 `addons` 和 `cfg` 文件映射到游戏目录。
- 支持把插件放入 `plugins` 目录，由入口程序在启动时建立软链接。
- 支持把地图放入 `maps` 目录，并挂载到容器内的插件地图路径。
- 仓库中包含 GeoLite2-City.mmdb，用于 SourceMod 插件在本地根据 IP 查询玩家所在国家或城市。
- 插件目录最多读取到第二层文件夹。在 `plugins` 下的某个目录建立 `disable` 目录（如 `plugins/_娱乐/disable`）可临时停用或启用某些插件。

### 启动服务器

请执行 `docker compose up -d` 以启动容器

- 默认开放 `27015` TCP/UDP 端口。
- 默认使用 `96 tick`，避免高 tickrate (如 `100`)下的异常。
- 容器启动后不会自动选图，进入控制台后执行 `map c1m2_streets` 或其他地图名来开图。

## 目录说明

- `docker-compose.yml`：本地启动配置，默认使用镜像 `ghcr.io/Rogerma12345/l4d2_server:latest`。
- `entrypoint.go`：启动前处理插件目录，建立软链接，然后运行 `srcds_run`。
- `build.yml`：GitHub Actions 镜像构建流程。
- `Dockerfile`：Docker 镜像构建文件。
- `data`：服务器和游戏配置文件。
- `plugins`：插件目录。
- `maps`：地图目录。
