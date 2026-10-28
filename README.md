# LibreTV 安卓 APK（WebView 壳）

把 LibreTV 网站（已部署在国内主机，带分享码门禁）封装成一个真正的安卓 APK：
点开即全屏独立 App，无浏览器地址栏，启动地址已写死分享码，冷启动直接进。

## 工程说明
- `app/src/main/java/com/libretv/webview/MainActivity.kt`：WebView 加载 `app_url`（含分享码）；
  启用 JS / DOM 存储 / 媒体自动播放 / 混合内容；支持视频全屏与返回键回退。
- 启动地址在 `app/src/main/res/values/strings.xml` 的 `app_url`，改这里即可换链接。
- 不依赖 PWA manifest / Digital Asset Links，完全绕开门禁，最稳。

## 如何拿到 APK（GitHub Actions 自动构建，纯网页操作）

### 第 1 步：新建仓库
- 登录 GitHub → 右上角 **+ → New repository**。
- Repository name 随便起（如 `libretv-android`），**保持 Public 或 Private 均可**，勾不勾 README 都行。
- 点 **Create repository**。

### 第 2 步：把工程文件传上去（不用装任何软件）
- 进到刚建的仓库，点 **Add file → Upload files**。
- 把本目录里 **除了 `.git` 之外所有文件和文件夹** 一起拖进上传框（即 `.github/`、`app/`、`gradle.properties`、`settings.gradle`、`README.md` 这 11 个文件），GitHub 会自动保留子目录结构。
- 页面底部填一句提交说明（如 `init libretv android`），点 **Commit changes**。

### 第 3 步：设一个签名密码（只需一次）
- 仓库里点 **Settings → Secrets and variables → Actions → New repository secret**：
  - Name：`KS_PWD`
  - Secret：你自己想一段密码（8 位以上，用于给 APK 签名；**请牢记**，以后升级安装必须一致）
- 点 **Add secret**。

### 第 4 步：触发构建
- 仓库顶部点 **Actions** 标签 → 左侧选 **Build LibreTV APK** → 右侧 **Run workflow → Run workflow**。
- 等 3～6 分钟，出现绿色 ✓ 即成功。

### 第 5 步：下载 APK
- 点进那条成功的运行记录，拉到底部 **Artifacts** 区，下载 `libretv-apk`（里面是 `app-release.apk`）。
- 把 APK 传到手机，点开安装（需开启「允许未知来源」安装权限）。

## 安装后
- 桌面出现 LibreTV 图标，点开即全屏 App。
- 首次打开输入站点密码 `LibreTV@2026` 即可（与网页端一致）。
- 视频播放、搜索、长按倍速与网页端一致。

## 注意
- 分享码（`I7GLYKL3VS`）若被平台重置，需改 `app_url` 重新构建一次。
- APK 无自动更新，每次迭代重新下载安装（同签名可覆盖升级）。
