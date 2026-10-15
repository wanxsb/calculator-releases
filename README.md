# 计算器 · 发布仓

这个仓库**只放发布产物**，不放源码。

## 下载

到 [Releases](../../releases/latest) 页面选对应的安装包：

| 机型 | 文件 |
|---|---|
| 绝大多数现代手机 | `calculator-<版本>-arm64-v8a.apk` |
| 较老的 32 位手机 | `calculator-<版本>-armeabi-v7a.apk` |
| 模拟器 / x86 平板 | `calculator-<版本>-x86_64.apk` |

拿不准就选 `arm64-v8a` 那个。安装时系统会提示「未知来源」，允许即可。

## update-manifest.json 是什么

应用内的「检查更新」会在启动时读这个文件，判断有没有新版本。
**普通用户不需要下载它。**

它被应用读取的地址是：

```
https://github.com/wanxsb/calculator-releases/releases/latest/download/update-manifest.json
```

⚠️ 这个地址**编译在每个安装包里**，所以它必须永远指向「最新一次发布」里的
同名附件 —— 也就是这里用的是 `releases/latest/download/` 这个固定别名，
而不是带版本号的 `releases/download/v1.0.0/...`。

一旦写成带版本号的地址，装着旧版的用户读到的就永远是那份旧清单，
**再也发现不了新版本**。

## 发新版本时的步骤

1. 在应用仓库里改 `pubspec.yaml` 的版本号，出包
2. 把 `kUpdateManifestUrl` 保持指向上面那个 `latest` 地址不动（**不要改它**）
3. 改 `update-manifest.json` 里的 `version` / `buildNumber` / `downloadUrl`
   （`downloadUrl` 指向本次带版本号的安装包）
4. 发一次新的 GitHub Release，把**安装包和 update-manifest.json 一起**传上去

第 4 步里 `update-manifest.json` 必须每次都传 —— 老用户读的就是最新那次发布里的它。
