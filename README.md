# Codex Pocket

从 Android、网页或 Mac/Windows 桌面客户端使用自己的 Codex 电脑。任务由被控制电脑执行，沿用本机 Codex 登录或中转配置；手机和服务器不保存模型密钥。所有客户端自行填写服务器地址，没有发布者的默认 IP 或域名。

本仓库提供公开安装制品，不需要下载源码。普通使用者只需安装电脑 Agent 和控制端；已有团队服务器时，无需自己购买服务器。

## 下载

打开 [最新正式发行](https://github.com/yuetongdai0-glitch/codex-pocket-releases/releases/latest)，在 Assets 选择对应文件。需要固定旧版本时使用 [全部发行记录](https://github.com/yuetongdai0-glitch/codex-pocket-releases/releases)。

| 使用位置             | 下载或安装方式                                                                                                                            |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Android 手机         | [Codex-Pocket-Android.apk](https://github.com/yuetongdai0-glitch/codex-pocket-releases/releases/latest/download/Codex-Pocket-Android.apk) |
| Mac 控制端           | 最新发行中的 `Codex-Pocket-macOS-arm64-版本.zip`（Apple Silicon）或 `Codex-Pocket-macOS-x64-版本.zip`（Intel）                            |
| Windows 控制端       | 最新发行中的 `Codex-Pocket-Windows-x64-版本.zip`                                                                                          |
| 被控制的 Mac/Windows | [npm 官方包 codex-pocket-agent](https://www.npmjs.com/package/codex-pocket-agent)                                                         |
| 浏览器               | 打开管理员提供的 HTTPS 服务器地址，无需额外安装                                                                                           |
| Linux 服务器         | 最新发行的四个部署文件；归档升级或网络无法拉取镜像时额外下载对应架构 Relay 镜像归档和 `SHA256SUMS`                                        |

详细文档：[完整使用流程与命令](https://github.com/yuetongdai0-glitch/codex-pocket-releases/releases/latest/download/PUBLIC-USAGE.md)、[Word 使用说明](https://github.com/yuetongdai0-glitch/codex-pocket-releases/releases/latest/download/Codex-Pocket-User-Guide.docx)、[服务器部署说明](https://github.com/yuetongdai0-glitch/codex-pocket-releases/releases/latest/download/DEPLOYMENT.md)。

Electron 仅作为控制端时，不需要安装模型或 Agent；该电脑也要被控制时，再安装 Agent。当前公开手机发行是 Android，未提供 iOS 真机安装包。

## 被控制电脑：安装一次

1. 安装 [Node.js](https://nodejs.org/en/download) 22.12 或更高版本，推荐稳定安装的 Node.js 24；安装后重开终端。
2. 安装并完成本机 Codex 认证。已有桌面 Codex 可由 Agent 自动查找；没有可用程序时可执行 `npm install -g @openai/codex`，再按自己的登录或中转配置认证。
3. 向管理员取得服务器 HTTPS 根地址和一次性电脑注册邀请。在 Mac 终端或 Windows PowerShell 执行：

```sh
npm install -g codex-pocket-agent@latest
codex-pocket-agent setup
codex-pocket-agent doctor
codex-pocket-agent service install
codex-pocket-agent status
codex-pocket-agent pair
```

`setup` 询问服务器、允许访问的实际项目目录和管理员邀请。电脑名默认主机名，Codex 自动查找；`--name`、`--codex-bin` 可自定义，具体参数用 `codex-pocket-agent --help` 查看。目录必须真实存在，仅添加需要访问的工作区。

`doctor` 检查依赖、服务器和认证；`service install` 登记并启动当前用户守护。Mac/Windows 在用户登录后启动，异常退出由系统恢复。`status` 查看进程和服务器连接；`pair` 输出一次性手机配对码与终端二维码，当前默认有效 5 分钟。邀请用于注册电脑，配对码用于手机连接，两者不要混用。

地址和电脑身份保存在当前用户独立状态目录 `.codex-pocket-agent`，不在 npm 包安装目录。升级、重启和换网络不会主动清除身份；不要删除状态或复制给另一台电脑。睡眠、关机、未登录时不能保证执行，电脑保持联网和运行。

## 控制端：扫码或手动输入

Android 安装正式 APK，Mac/Windows 解压对应桌面 ZIP；浏览器也可直接打开服务器 HTTPS 地址。首次填写管理员提供的 HTTPS 根地址，进入“电脑与服务器”，扫描电脑二维码或手动输入 `pair` 的码，核对电脑名称后配对。

首次扫码需相机权限；相机不可用时保留手动输入。后续直接载入上次服务器，授权与已读历史保留。连接失败时重试或切换服务器，不要把清除应用数据作为常规修复。

新建任务或打开聊天后，可以发送任务、选择模型和推理强度、查看实时输出，在执行中纠偏或停止，回答命令/文件审批及输入问题。工作区文件可查看和编辑，Git 面板可看当前分支、提交记录和未提交差异，包含人工与 AI 修改。只批准已核对的请求。

## 一个手机控制多电脑，多手机访问同电脑

第二台电脑使用新的管理员邀请，重复安装向导，执行 `pair`。手机在同一服务器选择“添加电脑”，配对第二台，原电脑不会被替换。之后从“电脑与服务器”切换，确认名称、工作区和聊天再发任务。

第二部手机需要电脑生成另一份配对码，再独立配对。一个码仅一次使用。多部手机访问同一台电脑上的真实文件和聊天，不是独立文件副本；需要约定操作者，避免同时修改同一聊天或文件。

电脑查看和撤销手机权限：

```sh
codex-pocket-agent devices
codex-pocket-agent revoke DEVICE_UUID
```

把 `DEVICE_UUID` 换为 `devices` 列出的实际编号，只撤销对应设备对这台电脑的授权。多服务器时加 `--server` 明确选择，不批量撤销不明设备。

## 手机与电脑连接多服务器

控制端在“管理服务器”保存多个地址，并在各服务器分别配对电脑。电脑可同时加入最多 20 台服务器；每台服务器需自己的管理员邀请：

```sh
codex-pocket-agent servers add --server https://second.example.com
codex-pocket-agent service restart
codex-pocket-agent servers list
codex-pocket-agent pair --server https://second.example.com
```

示例地址需换为团队实际地址。电脑、手机授权、草稿与缓存按连接身份隔离；同主机不同端口的服务器凭证也不互相覆盖。电脑 IP 变化后主动重连，不需要配置电脑公网 IP、开放电脑入站端口或安装 SSH 隧道。

所有服务器共享该电脑同一份允许工作区名单，仅加入可信服务器。移除本地连接可用 `codex-pocket-agent servers remove --server https://second.example.com` 后重启守护；离开不可信服务器还应让管理员撤销远端电脑注册。

## 管理员：部署团队服务器

Linux 安装 Docker Engine 和 Compose，开启 Docker 开机启动，开放安全组及防火墙 TCP 80/443。域名解析到自己的服务器；直接使用公网 IPv4 也可。大陆域名访问需要满足接入商备案要求。

0.2.2 已验证 GHCR 的 amd64/arm64 双架构匿名拉取。首次部署只需从同一正式 Release 下载以下四个文件并放入同一目录：

- [compose.yaml](https://github.com/yuetongdai0-glitch/codex-pocket-releases/releases/latest/download/compose.yaml)
- [Caddyfile](https://github.com/yuetongdai0-glitch/codex-pocket-releases/releases/latest/download/Caddyfile)
- [setup.sh](https://github.com/yuetongdai0-glitch/codex-pocket-releases/releases/latest/download/setup.sh)
- [default.env.example](https://github.com/yuetongdai0-glitch/codex-pocket-releases/releases/latest/download/default.env.example)

执行：

```sh
sh setup.sh
docker compose ps
docker compose logs --tail 100 gateway
docker compose exec relay node dist/relay/index.mjs create-invite --hours 24
```

在镜像提示直接回车，使用 `ghcr.io/yuetongdai0-glitch/codex-pocket:0.2.2` 自动拉取，不需要注册表账号；也可先执行 `docker pull ghcr.io/yuetongdai0-glitch/codex-pocket:0.2.2`。网关 Caddy 镜像仍需能从 Docker Hub 下载。服务器地址输入自己的公网 IPv4 或域名，不带协议、路径和端口，再填写证书联系邮箱。公开文件使用 `default.env.example`，脚本同时兼容已有本地 `.env.example`。

网络无法拉取 GHCR 时，额外下载 [SHA256SUMS](https://github.com/yuetongdai0-glitch/codex-pocket-releases/releases/latest/download/SHA256SUMS) 和对应架构归档：Intel/AMD 选 `Codex-Pocket-Relay-linux-amd64-版本.tar.gz`，ARM 选 `Codex-Pocket-Relay-linux-arm64-版本.tar.gz`。脚本在镜像提示回车时校验摘要后导入归档，无需注册表账号。

网关和 HTTPS 配置已随部署包提供，无需另写 Nginx。真实生产证书必须受设备信任；保持 TCP 80 可达供签发续期，不关闭 TLS 校验。Relay 后端通过认证 WSS 转发已授权手机操作，8080 仅容器内部可见。

电脑注册邀请默认 1 小时一次使用；`--hours 24` 可供次日安装，`--hours` 只接受 1-168 的整数小时。每台新电脑分别生成，通过可信渠道私下交付；过期重新生成，不能把邀请或配对码放入公共 README。

0.2.2 的 [REGISTRY-STATUS.txt](https://github.com/yuetongdai0-glitch/codex-pocket-releases/releases/latest/download/REGISTRY-STATUS.txt) 已记录 GHCR 双架构匿名拉取成功。未来版本仍先核对对应报告，再使用其中已验证的镜像名称与固定版本；没有这个证明，使用归档，注册表权限不足不能靠跳过鉴权解决。

详细服务器配置见 [DEPLOYMENT.md](https://github.com/yuetongdai0-glitch/codex-pocket-releases/releases/latest/download/DEPLOYMENT.md)。

## 更新与数据保留

网页 HTML 和更新清单不缓存，JS/CSS 可缓存。服务器部署最新网页后，顶部刷新加载新版本，保留登录、IndexedDB 历史和草稿。普通网页功能升级通常不用重装客户端；原生插件、键盘或桥接能力变化仍需更新 APK/Electron。

Android 应用内检查更新，验证摘要、包名与签名后交由系统安装。来源安装权限和系统确认仍由用户操作，不能静默替代。正式版本使用同一签名覆盖保留配置；旧调试包签名不同，使用发行说明提供的迁移流程，不随意卸载清数据。

Mac/Windows 桌面 ZIP 当前未做平台代码签名，首次启动可能被系统询问或拦截。确认来自本仓库后使用系统允许的打开流程，不全局关闭安全设置。Electron 提供下载更新入口，下载对应 ZIP、退出后替换应用，保留用户数据；不承诺已签名静默更新。

电脑更新 npm 后重新登记守护，刷新当前 Node 与程序路径：

```sh
npm install -g codex-pocket-agent@latest
codex-pocket-agent doctor
codex-pocket-agent service install
codex-pocket-agent status
```

服务器升级前备份数据卷，在原目录下载新版对应架构归档、摘要与部署文件，执行 `sh setup.sh --upgrade`。当前升级脚本仍必须使用归档，不能只下载首次安装的四个文件。保留 `.env`、`relay-data`、`gateway-data` 和 `gateway-config`；不要执行 `docker compose down -v`。电脑和服务器恢复网络后重新同步，不重新配对。

已打开工作台临时断网可看已读缓存，不能离线执行或审批；完全断网冷启动不能保证加载在线网页。手机掉线不会自动停止电脑正在执行的任务。

## 排障与兼容范围

电脑异常先执行 `codex-pocket-agent status`、`codex-pocket-agent logs`、`codex-pocket-agent doctor`，检查用户已登录、电脑醒着、服务器在线、Codex 认证和程序路径仍可用。`service stop` 主动停后台；`service restart` 恢复并重启，执行任务时谨慎使用。升级 Node 或全局安装路径后再执行 `service install`。

配对失败检查电脑和控制端是否选同一服务器、码是否已过期或使用；重新 `pair` 即可。HTTPS 失败检查证书、DNS、安全组和 `gateway` 日志，不能忽略证书错误。分享日志前检查项目名、目录或其他私人信息。

新电脑上的 Codex 桌面本机通信兼容性仍需实际检测；部分插件额外权限或特殊交互可能需要电脑处理。工作台不强占桌面会话，不自动批准未知请求。当前功能和限制以 [完整说明](https://github.com/yuetongdai0-glitch/codex-pocket-releases/releases/latest/download/PUBLIC-USAGE.md) 与对应正式发行说明为准。
