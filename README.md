### Auto_Symlink
**如果你觉得这个项目对你有帮助，可以考虑赞助我。你的支持将有助于这个项目的持续发展和改进。谢谢你的考虑！🙏**

微信:

<img alt="Snipaste_2023-12-19_14-15-09" src="https://github.com/shenxianmq/Auto_Symlink/assets/76782947/77a14e36-c6dd-4ee8-ae86-f031fb50ffc0" width="10%">

支付宝:

<img src="https://github.com/shenxianmq/Auto_Symlink/assets/76782947/ec8a3d7d-41ba-42d9-bec0-73bad0147ce2" alt="image" width="10%">

---
**小白牙整理**

### 项目简介
`Auto_Symlink` 是一个自动化工具，专门设计用于管理通过 CloudDrive2/Alist 挂载到本地的网盘。它能够创建软链接，使得像 Emby/Jellyfin 这样的媒体服务器能够更容易地刮削和读取内容，同时减少对网盘的频繁访问。

#### 主要特性
- **实时监控**: 监控指定目录，自动进行必要的更新和管理。
- **自动化处理**: 创建与更新软链接/strm文件，自动复制与更新元数据。
- **清理功能**: 清空无效文件夹和软链接，保持本地云端一致性。
- **Web 界面操作**: 提供一个简洁易用的Web界面，用于查看日志、编辑配置和监控系统状态。这使得用户能够更方便地管理和调整 Auto_Symlink 的运行。

---

### 更新记录

#### 版本 2.0 (GUI版)
- **新功能**: 引入了图形用户界面（GUI），提供了一个更直观、易用的操作界面。
- **Web 界面**: 新增了Web界面，允许用户通过浏览器访问来管理配置、查看日志和监控系统状态。
- **改进**: 对用户交互进行了优化，现在用户可以更方便地进行配置和管理。
- **插件**: 新增了手动同步、手动备份、恢复备份、通知工具、Emby通知、刷新媒体库、清除空文件夹、115cookie等常用工具。

#### 版本 1.0 (非GUI版)
- **核心功能**: 实现了 `Auto_Symlink` 的基础功能，包括创建软链接、监控指定目录、自动更新和管理软链接/strm文件。
- **实时监控**: 加入了实时监控功能，能够自动检测文件夹变化并进行必要的同步操作。
- **自动化处理**: 自动化创建和更新软链接及元数据，减少了对网盘的频繁访问。
- **配置文件管理**: 提供了 `config.yaml` 配置文件，允许用户根据需要进行详细配置。
- **兼容性**: 支持 Docker 容器运行，方便在不同系统环境中部署和使用。

---

### 安装和使用
1. **直接运行 Python 文件**:
   - 在首次运行后，`config` 文件夹中会生成 `config.yaml` 文件。根据文件中的注释进行配置。
   - 配置完成后，使用命令 `python auto_symlink.py` 运行。
   - 在 Windows 系统中，需要以管理员模式运行。

2. **Docker 运行**:
   使用以下命令运行 Docker 容器：
   ```bash
   docker run -d \
     --name auto_symlink \
     -e TZ=Asia/Shanghai \
     -v /volume1/CloudNAS:/volume1/CloudNAS:rslave \
     -v /volume2/Media:/Media \
     -v /volume1/docker/auto_symlink/config:/app/config \
     -p 8095:8095 \
     --restart unless-stopped \
     --log-opt max-size=10m \
     --log-opt max-file=3 \
     shenxianmq/auto_symlink:latest
   ```
   注意：映射网盘路径时必须使用绝对路径。

---

### Docker 运行指令详解
- `-v /your/cloud/path:/cloudpath:rslave`: 将你的云盘路径（`/your/cloud/path`）映射到容器内的路径（`/your/cloud/path`）。`rslave` 表示使用相对于宿主机的从属挂载模式。确保左右路径保持一致,否则生成的软链接不是指向真实路径,导入emby中的时候会导致无法观看
- `-v /your/media/path:/media`: 将你的媒体文件路径映射到容器内的 `/media` 目录。
- `-p 8095:8095`: 映射8095端口，可方便的查看日志以及管理服务。
- `-v /path/to/auto_symlink/config:/app/config`: 将 `auto_symlink` 的配置目录映射到容器内的 `/app/config`。这样可以使容器中的 `auto_symlink` 使用外部的配置文件。
- `--restart unless-stopped`: 设置容器在退出时自动重启。
- `--log-opt max-size=10m`: 设置容器日志文件的最大大小为 10MB。
- `--log-opt max-file=3`: 设置容器日志文件的最大文件数为 3。

#### 注意：
- 映射路径时必须使用绝对路径（虽然此处是本工具的docker运行说明，但EMBY也应使用同样的绝对路径，否则软连接将指向错误的位置，从而导致无法播放），以确保软连接可以正确指向原始文件或目录。
- 根据你的实际路径和需求调整 `-v` 选项中的路径。
---
### Web 界面访问和账户信息

#### 账号密码
- 默认账号：`admin`
- 默认密码：`password`

在首次登录时，你可以使用这些凭据进行登录。为了安全起见，建议登录后立即更改密码。

#### Web界面说明
通过映射端口8095，用户可以方便地访问 `Auto_Symlink` 的Web界面。在任何支持的浏览器中输入 `http://[你的服务器地址]:8095` 即可访问。

---

## 常见问题解答 (FAQ)

#### Q: `auto_symlink` 在什么情况下特别有用？
**答**: 当你正在使用CloudDrive2/Alist等工具管理媒体，并使用EMBY/Jellyfin等工具来管理这些媒体时，本工具将大大降低媒体刮削时访问网盘的频率。

#### Q: EMBY显示当前没有兼容的流
**答**: 请确保你EMBY映射的也是绝对路径，需要与 `auto_symlink`设置的路径保持一致。

#### Q: 虽然我有元数据，但EMBY扫库还是很慢？
**答**: 因为我们映射了所有影片的软连接，所以可以尝试先禁用EMBY的FFmpeg进程，CloudDrive2可以在设置黑名单添加`/bin/ffprobe`，扫库完成后，再删除该黑名单即可。

#### Q: 我映射后为什么不能在windows下播放？
**答**: 映射的软连接仅支持绝对路径，windows下的绝对路径肯定与linux不一致，所以请在EMBY内验证。

#### Q: 为什么运行完毕后，只同步了文件夹？
**答**: 群晖`File Station`或部分工具不支持显示软连接，可以尝试用windows或者命令行查看。

---

## 旧版(无GUI界面)解答 (FAQ)
#### Q:配置文件只给出了一个目录映射，我想映射多个怎么办？
**答**: 仅需将`sync_list:`后面的所有内容再复制一遍即可,注意空格,yaml文件格式很注意缩进。
tips：注意不要将`sync_list:`一起复制进去了，否则会只有一个目录生效

#### Q: 配置文件修改每次都要重启吗？
**答**: 不需要，本工具支持热重载。

#### Q: 我创建了docker，为什么启动软件后没有生效？
**答**: 第一次启动仅仅会自动创建`config`配置文件，还需要进入`config`目录下编辑`config.yaml`文件，文件内有详细使用说明。

#### Q:我配置了多个目录，但只有一个目录生效了是怎么回事？
**答**: 请检查目录配置中是否将`sync_list:`也一起复制进去了

---

#### 配置文件说明
- `config.yaml`: YAML 格式的主配置文件，详细配置可以参考文件中的注释。
- `last_sync.yaml`: 保存已同步的目标目录，用于避免重复全同步。
---

#### 开源许可
本项目遵循 [LICENSE](LICENSE) 中所述的开源许可。
