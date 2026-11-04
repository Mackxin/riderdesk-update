# riderdesk-update

「骑手跑单」Android App 的**在线更新托管仓库**。

这里只放两样东西：最新安装包（`.apk`）和版本清单（`update.json`）。
App 源码在私有仓库，不在这里。

## 清单地址

```
https://mackxin.github.io/riderdesk-update/update.json
```

填在 App 的「我的 → 检查更新 → 清单地址」里，之后每次启动会静默比一次 `versionCode`，
有新版才提示。

## 发新版的流程

1. 改源码仓库的 `versionCode`（加 1）和 `versionName`，构建出 APK；
2. 把 APK 传进本仓库，改好 `update.json` 里的 `versionCode` / `versionName` / `url` / `sizeBytes` / `notes`；
3. push，等 GitHub Pages 刷新（约 1 分钟）；
4. 手机上点一次「检查更新」能看到新版就算成功。

## 说明

- `versionCode` 必须是**整数且严格递增**。判断有没有新版只比这个整数，
  不比版本号字符串（字符串会把 "1.9.0" 判成大于 "1.32.0"）。
- `sizeBytes` 是 APK 的字节数，用来在下载前提示大小、下载后校验。
- 安装包不含源码、不含任何密钥、不含个人记账数据；App 的 AI Key / GitHub Token
  只存在手机本地，不进安装包。

## 安装

下载最新 APK 直接安装即可。升级走系统安装器**覆盖安装**，本地数据（记录、设置）都会保留。
首次安装需要允许「安装未知应用」。
