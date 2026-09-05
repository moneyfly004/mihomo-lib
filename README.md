# mihomo-lib

MoneyFly Android 内核发布仓库:把官方 [MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo)(**无 fork,原版源码**)用 gomobile bind 编译为 `libmihomo.aar`,发布到 GitHub Releases。

MoneyFly 主仓库构建 APK 时直接下载本仓库对应版本的 aar,避免每次现编内核(省 10~20 分钟/次)。

## Releases

- Tag 与 mihomo 内核版本一致,如 `v1.19.30`
- 资产:`libmihomo.aar`(arm64-v8a / armeabi-v7a / x86 / x86_64,`with_gvisor`)+ sha256
- 自动跟随:每天 UTC 03:00 检测官方新稳定版并自动编译发布;也可手动 `workflow_dispatch` 指定版本

## 生成的 Java API(包 `top.moneyfly.mihomelib`)

```kotlin
Mihomelib.start(homeDir: String, configYaml: ByteArray, tunFd: Int)  // 启动内核;tunFd>0 注入 VpnService fd
Mihomelib.stop()
Mihomelib.reload(configYaml: ByteArray)
Mihomelib.running(): Boolean
Mihomelib.version(): String
```

## 本地编译(调试用)

需要 Go 1.26+ 与 Android NDK:

```bash
export ANDROID_NDK_HOME=$ANDROID_HOME/ndk/27.2.12479018
go mod download
go install golang.org/x/mobile/cmd/gomobile@latest
gomobile bind -target=android -androidapi 21 -tags with_gvisor \
  -javapkg top.moneyfly -o libmihomo.aar .
```
