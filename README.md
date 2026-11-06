# riderdesk-update

「骑手跑单」Android App 的**在线更新托管仓库**。App 源码在私有仓库，不在这里。

## 现在的主用法：GitHub Releases

App（v1.37.0 起）直接读本仓库的 Releases 接口：

```
https://api.github.com/repos/Mackxin/riderdesk-update/releases/latest
```

取三样东西：`tag_name`（`v1.37.0`）当版本号、`body` 当更新说明、
`assets` 里第一个 `.apk` 当安装包。**没有清单文件需要维护。**

### 发一个新版本

```bash
# 1. 源码仓库改 versionCode / versionName，构建出 APK
# 2. 建 Release 并把 APK 作为附件挂上去（tag 必须写成 vX.Y.Z）
gh release create v1.37.0 riderdesk-v1.37.0.apk \
  --title "v1.37.0" \
  --notes "在线更新改用 GitHub Releases；去掉发版小工具"
```

旧版手机上点「检查更新」就能看到并升级。

## 保留的兼容路径：update.json

v1.33.0–v1.36.0 的旧版只认 JSON 清单，所以这里仍然留一份 `update.json`
（同上 Pages 地址 `https://mackxin.github.io/riderdesk-update/update.json`）。
旧版读到它会跳到本仓库里的 APK 完成升级；升到 v1.37.0 之后，App 会把
这个老地址自动换成上面的 Releases 接口，不再读清单。

**这份 json 已经不需要跟着每次发版更新了** —— 只有当还想让 v1.36.0 及更早的
旧版继续收到提示时才需要动它。

## 说明

- Release 的 tag 必须是 `vX.Y.Z` 点分格式：App 把三段数字拆成整数比大小
  （`major*1000000 + minor*1000 + patch`），不比字符串（否则 "1.9.0" 会被判成大于 "1.36.0"）。
- `update.json` 里的 `versionCode` 仍必须是整数且严格递增。
- 安装包不含源码、不含任何密钥、不含个人记账数据；App 的 AI Key / GitHub Token
  只存在手机本地，不进安装包。

## 安装

下载最新 APK 直接安装。升级走系统安装器**覆盖安装**，本地数据（记录、设置）都会保留；
首次安装需要允许「安装未知应用」。
