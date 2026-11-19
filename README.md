# vcmi-mods-repo

个人 VCMI mod 源，两个 mod：

- **中文地图补充包**（`sod-cn-maps-extra`），111 张中文地图，来自网上流传的 `SOD地图.rar`，
  是官方 `chinese-maps-collection`（地图收集包）里没有的部分。
  包含 `回归罗马之路` —— Darklod 的 *The Empire of The World II* 的完整中文译本。
  地图收集包里那份 `一条小路通罗马1/2016` 是 pacl 版，正文事件文字根本没翻译，只机翻了简介。
- **Tonic**（`tonic`），随机图模板：Tonic、Tonic Mosaic、Muzgob Azure、Tonic Nostalgia、Tonic Mini Nostalgia。
  都是资源偏富的玩法，两个 Nostalgia 是原版布局加 Tonic Mosaic 的宝物和怪物强度。

## 怎么用

### 方式一：添加自定义源（推荐，能收到更新）

VCMI 启动器 → **设置** → 找到第二个仓库地址那一栏，勾选启用并填入：

```
https://raw.githubusercontent.com/tonicmuroq/vcmi-mods-repo/main/repo.json
```

回到 **模组** 页，点右上角「重载源」，列表里会出现「中文地图补充包」和「Tonic」，点安装。

### 方式二：本地安装（不自动更新）

中文地图补充包下载 [main.zip](https://github.com/tonicmuroq/vcmi-mods-repo/archive/refs/heads/main.zip)，
Tonic 下载 [tonic.zip](https://github.com/tonicmuroq/vcmi-mods-repo/releases/download/tonic/tonic.zip)，
直接拖进 VCMI 启动器窗口即可。

启动器接受拖放的后缀：`.zip .vsgm1 .h3m .h3c .vmap .vcmp .json .exe`——单张地图也能直接拖。

### 方式三：手动放目录

把 `sod-cn-maps-extra/` 或 `tonic/` 整个目录复制到 VCMI 的 `Mods/` 下：

| 系统 | 路径 |
| --- | --- |
| macOS | `~/Library/Application Support/vcmi/Mods/` |
| Linux | `~/.local/share/vcmi/Mods/` |
| Windows | `%USERPROFILE%\Documents\My Games\vcmi\Mods\` |

## 装完在哪

地图列表里会多出一个跟「TE对战地图」「非官方地图」并列的 **中文地图补充** 文件夹。
VCMI 的场景列表是按目录分层显示的，不会跟官方地图混在一起。

Tonic 的模板出现在新建随机地图的模板下拉框里。每个模板有尺寸范围，
比如 Tonic Nostalgia 要 XL 带地下或更大，尺寸不对就不显示。

## 结构

```
vcmi-mods-repo/
├── repo.json                     # 源索引,方式一填的就是它的 raw 地址
├── sod-cn-maps-extra/
│   ├── mod.json                  # mod 元数据,modType: Maps
│   └── Content/Maps/中文地图补充/  # 111 张 .h3m
└── tonic/
    ├── mod.json                  # modType: Templates,templates 列出 content/ 下的模板文件
    └── content/                  # 每个 .json 一个随机图模板
```

中文地图补充包的下载包是整个仓库的 GitHub archive。zip 顶层是 `vcmi-mods-repo-main/`，
启动器安装时取它找到的**第一个**带 `mod.json` 的目录，只装那部分，`repo.json`、`README.md` 这些会被忽略。
archive 里按字母序 `sod-cn-maps-extra/` 排在 `tonic/` 前面，所以它还能用；
Tonic 要是也用这个 archive，装到的会是地图包，所以 Tonic 单独打包，放在 tag 为 `tonic` 的 release 里。

## 加地图

1. 把 `.h3m` 放进 `sod-cn-maps-extra/Content/Maps/中文地图补充/`
2. `sod-cn-maps-extra/mod.json` 里的 `version` 往上加
3. 推上去之后更新 `repo.json` 的 `downloadSize`（archive 的实际字节数 ÷ 1000000）：

```sh
curl -sL -o /tmp/m.zip https://github.com/tonicmuroq/vcmi-mods-repo/archive/refs/heads/main.zip
stat -f%z /tmp/m.zip
```

`downloadSize` 只用于启动器的进度条，填得不准不影响安装。

## 改模板

1. 改 `tonic/content/` 下的模板；新模板还要加进 `tonic/mod.json` 的 `templates`
2. `tonic/mod.json` 里的 `version` 往上加，提交推上去
3. 重新打包并替换 release 里的文件，`downloadSize` 填 zip 字节数 ÷ 1000000：

```sh
rm -f /tmp/tonic.zip && zip -qr -X /tmp/tonic.zip tonic && stat -f%z /tmp/tonic.zip
gh release upload tonic /tmp/tonic.zip --clobber
```

## 授权

这些地图出自几百位不同作者，我没有取得任何人的授权，只是把一个流传已久的合集里
官方地图包没收的部分整理出来。原作者如果介意，开个 issue 我立刻撤下。
