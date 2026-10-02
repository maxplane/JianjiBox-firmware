# JianjiBox 固件发布

这个仓库**只放编译好的固件**，方便 JianjiBox App 检查更新并下载：

* `PhysicalSmartBox-{版本}.bin` —— 固件镜像（ESP32-C3 SuperMini）
* `firmware.json` —— 更新清单（版本 / 下载地址 / sha256 / 大小 / 更新说明）

App 读取的固定地址（永远指向最新一版）：

```
https://github.com/maxplane/JianjiBox-firmware/releases/latest/download/firmware.json
```

## 为什么单独开一个仓库

简记药盒的**源码仓库是私有的**，而 GitHub 对私有仓库的 Release 附件**不给匿名下载**
（不带登录态取会 404）。App 里不能放访问令牌，所以把"只读的编译产物"放在这个公开仓库里 ——
**源码不在这里**，也不用把源码公开。

## 发版

在源码仓库里跑（会同时更新这里的 Release）：

```bash
Firmware/tests/publish_firmware.sh --release --notes "这一版改了什么"
```

## 升级是安全的

* 升级走**局域网 OTA**（App 把镜像推给药盒，写进另一个 OTA 槽，成功才重启切过去）；
* App 下载后会**校验 sha256**，对不上直接丢弃；
* 只升不降；写失败/镜像不合法 → 旧固件继续跑，不会变砖。
