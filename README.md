
# 🚀 Kometa 配置 (原 Plex Meta Manager)

这是我的 Kometa 配置，用于为 Plex 自动创建合集和叠层。这些文件最初基于模板创建，自 2021 年以来经过我（原作者 jhn322）彻底重写/编辑并定期更新。在叠层方面，我侧重于提供一目了然的实用信息，同时避免过度堆砌；合集方面则力求全面，但又不至于信息过载。

*（译者注：本仓库是基于 jhn322/kometa-config 的 Fork，旨在进行优化和汉化，使其更符合中国用户的观影习惯。）*

## ⚙️ 安装指南 (Docker Compose)

> [!NOTE]
> 本指南介绍如何使用 Docker Compose 安装 Kometa，并将其配置为包含三个容器的堆栈 (stack)，每日定时运行以处理合集、叠层和操作。

### 前提条件

在安装之前，您需要获取以下服务的 API 密钥，以确保大部分列表和叠层的完整功能：

- [Trakt](https://metamanager.wiki/en/latest/config/trakt/)
- [TMDb](https://metamanager.wiki/en/latest/config/tmdb/)
- [MdbList](https://metamanager.wiki/en/latest/config/mdblist/)
- [OMDb](https://metamanager.wiki/en/latest/config/omdb/)
- [AniDB](https://metamanager.wiki/en/latest/config/anidb/)
- [MyAnimeList](https://metamanager.wiki/en/latest/config/myanimelist/)
- [Tautulli](https://metamanager.wiki/en/latest/config/tautulli/)

### 选项一：Docker Compose (推荐)

1.  克隆或下载本仓库。
2.  在 `config.yml` 文件中，添加**您**本地 Plex 服务器的 IP 地址和[令牌 (token)](https://support.plex.tv/articles/204059436-finding-an-authentication-token-x-plex-token/)，并**务必**将每个库的标题 (`library` 下的键名，如 `Movies`) 修改为您 Plex 库的**确切**名称。
3.  (如果尚未安装) 请安装 [Docker 及 Docker Compose](https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/How-to-install-Docker-and-docker-compose-on-Ubuntu) 或适用于 Windows/Mac 的 [Docker Desktop](https://www.docker.com/products/docker-desktop/)。虽然也可以在本地使用 Python 运行，但长远来看不推荐此方式，详情请参阅 [Wiki](https://metamanager.wiki/en/latest/kometa/install/local/)。
4.  打开终端并导航到您的 Kometa 配置文件夹路径：

    ```powershell
    # Linux
    cd /path/to/Kometa-folder

    # Windows
    cd C:\path\to\Kometa-folder

    # Mac
    cd ~/path/to/Kometa-folder
    ```

5.  现在，在终端中粘贴以下命令来创建容器：

    ```powershell
    # Linux (如果需要 root 权限)
    sudo docker-compose up -d
    
    # Windows/Mac 或 Linux (无需 root)
    docker-compose up -d
    ```

**完成！**

> [!TIP]
> 外部列表 (如 Trakt 或 Letterboxd) 有时会因列表所有者删除列表或更改 URL 而导致错误。为了主动查找并识别 Kometa YAML 文件中的这些失效链接，可以考虑使用原作者的 [Dead Link Checker](https://github.com/jhn322/dead-link-checker) 脚本。它可以扫描您的配置文件并报告所提供 `.yml` 文件中任何无法访问的列表 URL，帮助您维护一个干净且无错误的 Kometa 设置。

### 选项二：合并容器

> [!NOTE]
> 另一种方法是使用单个容器连续运行所有 3 种不同的库操作（合集、叠层、操作）。要使用此方法：
>
> 1. 将原始的 `docker-compose.yml` 重命名为 `docker-compose-original.yml` (或其他名称)。
> 2. 将 `docker-compose COMBINED.yml` 重命名为 `docker-compose.yml`。
> 3. 使用与选项一中步骤 5 相同的方法运行合并后的容器。

> [!WARNING]
> 虽然这种合并方法运行起来更简单、完成更快，但对于拥有大型库的 Plex 服务器，通常**不推荐**使用。根据原作者的经验，连续按 `config.yml` 中的顺序运行所有操作可能会导致 Plex 无响应和/或崩溃。分离容器的方法（选项一）更稳定，是推荐的方式，尽管完成时间较长。

### 选项三：Docker Run 命令

如果您因某种原因不想使用 Docker Compose，只需使用以下 `docker run` 命令即可达到相同效果（请将 `/path/to/Kometa-folder` 替换为您的实际路径）：

```powershell
# Linux
sudo docker run --restart=unless-stopped -d -v "/path/to/Kometa-folder/config:/config:rw" kometateam/kometa -op --time 06:00
sudo docker run --restart=unless-stopped -d -v "/path/to/Kometa-folder/config:/config:rw" kometateam/kometa -ov --time 06:30
sudo docker run --restart=unless-stopped -d -v "/path/to/Kometa-folder/config:/config:rw" kometateam/kometa -co --time 08:00

# Windows
docker run --restart=unless-stopped -d -v "C:\path\to\Kometa-folder/config:/config:rw" kometateam/kometa -op --time 06:00
docker run --restart=unless-stopped -d -v "C:\path\to\Kometa-folder/config:/config:rw" kometateam/kometa -ov --time 06:30
docker run --restart=unless-stopped -d -v "C:\path\to\Kometa-folder/config:/config:rw" kometateam/kometa -co --time 08:00

# Mac
docker run --restart=unless-stopped -d -v "~/path/to/Kometa-folder/config:/config:rw" kometateam/kometa -op --time 06:00
docker run --restart=unless-stopped -d -v "~/path/to/Kometa-folder/config:/config:rw" kometateam/kometa -ov --time 06:30
docker run --restart=unless-stopped -d -v "~/path/to/Kometa-folder/config:/config:rw" kometateam/kometa -co --time 08:00
```

- `-op --time 06:00`: 每天 06:00 运行操作 (Operations)
- `-ov --time 06:30`: 每天 06:30 运行叠层 (Overlays)
- `-co --time 08:00`: 每天 08:00 运行合集 (Collections)

#### 用于测试 (单次运行):

如果您想立即手动触发一次运行以进行测试：

PowerShell

```
# Linux
sudo docker run -it -v "/path/to/Kometa-folder/config:/config:rw" kometateam/kometa --run

# Windows
docker run -it -v "C:\path\to\Kometa-folder/config:/config:rw" kometateam/kometa --run

# Mac
docker run -it -v "~/path/to/Kometa-folder/config:/config:rw" kometateam/kometa --run
```

## 🔄 更新 Kometa 镜像

要更新 Kometa 的 Docker 镜像到最新版本：

PowerShell

```
# 拉取稳定版 (Stable)
docker pull kometateam/kometa

# 拉取开发版 (Develop)
docker pull kometateam/kometa:develop

# 拉取每夜版 (Nightly)
docker pull kometateam/kometa:nightly
```

更新镜像后，如果使用的是 Docker Compose，您需要停止并重新创建容器来应用新镜像：

PowerShell

```
# Linux (如果需要 root)
sudo docker-compose down
sudo docker-compose up -d

# Windows/Mac 或 Linux (无需 root)
docker-compose down
docker-compose up -d
```

如果使用的是 `docker run` 命令，您需要手动停止 (`docker stop <container_id>`)、删除 (`docker rm <container_id>`) 旧容器，然后使用新的镜像重新运行 `docker run` 命令。

## 📋 支持的库类型

下表列出了此配置当前支持（或可以启用）的 Plex 库类型：

| 库类型     | 状态     |
| ---------- | -------- |
| 🎬 电影     | 启用     |
| 📺 电视节目 | 启用     |
| 🏮 动漫     | 启用     |
| 🎵 音乐     | 启用     |
| 🎞️ Remux    | 启用     |
| 📚 有声读物 | 默认禁用 |
| 🎼 原声带   | 默认禁用 |
| 🎥 视频     | 默认禁用 |

导出到 Google 表格

*（注：默认禁用的库类型可以通过修改 `config.yml` 文件来启用。）*

### 💾 合集示例

**电影 (Movies)**

**电视节目 (TV Shows)**

**动漫 (Anime)**

### 🖼️ 叠层示例

**电影 (Movies)**

**电视节目 (TV Shows)**

**Remux**

## 📚 文档

欲了解 Kometa 的更多详细信息和高级配置，请访问 [Kometa 官方 Wiki](https://metamanager.wiki/en/latest/)。
