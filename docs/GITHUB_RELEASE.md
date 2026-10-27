# GitHub 发布准备

项目标题：**Wuwa Tappo Android版**。仓库：`Averionx/WuwaTappo-Android`。基于 Xime，不是原 Fcitx5 方案。

已创建公开 Android 仓库，本阶段仅发布项目说明、使用文档、桌面版原图标和设置首页教程截图；尚未上传完整源码或 APK，也未创建 Release。

本版作者、维护与发行者为 **Averionx**。应用内关于页、设置、启用向导、候选栏与无线导入均使用本版标识，Xime 原作者信息保留在开源来源与致谢中。

发行信息统一定义在 `app/src/main/java/com/kingzcheung/xime/branding/WuwaEdition.kt`。目前 `PUBLISHED=false`，本版发布、源码和反馈入口先打开 Averionx 的 GitHub 主页；完整源码与 Release 正式公开后，再更新仓库地址并将标记改为 `true` 并重新构建，使入口指向本版仓库和 Releases，避免引导用户安装 Xime 官方包。

## 仓库内容

保留 GPLv3、上游商标说明和来源记录；附带 `LICENSES/LGPL-2.1-or-later.txt`。根 README 使用本项目说明，上游原 README 另存为 `README.upstream.md`。

README 顶部使用桌面版原始 Logo。发布项目说明时，要一并保留 `docs/images/wuwa-tappo-desktop.png` 及其相对路径，避免只复制 Markdown 导致图片不显示；图标来源与权利说明保留在 `docs/WUWA_ASSETS.md`。APK 图标也使用同一张原图。

提交源码、资源转换和审核工具及文档。不要提交私有素材包、签名密钥、账号令牌、构建缓存或本机 SDK。APK 已内置素材，不要求安装者手动导入。素材版权与代码许可证分开说明。

含原版素材的 APK 尚缺明确的公开再分发授权证据，公开上传前需解决这一项。代码开源不自动解除素材限制。

## 源码构建

需要 JDK 21、Gradle 9.8.2、Android SDK 37.2、NDK 29.0.14206865 和 CMake 3.22.1。保留上游 wrapper，可按下列方式构建：

```powershell
git submodule update --init --recursive
.\gradlew.bat :app:assembleDebug -PbuildABI=arm64-v8a -PwuwaBundlePack="D:/private/WuwaTappo-1.0.1.wuwapet.zip"
```

构建主机需能下载 Maven、Google Maven 和 GitHub 上的原生依赖。Windows 不具备符号链接权限时，构建脚本复制相同插件源码，不需要管理员许可。

定向验证：

```powershell
.\gradlew.bat :app:testDebugUnitTest --tests "com.kingzcheung.xime.pet.*" --tests "com.kingzcheung.xime.ui.*" --tests "com.kingzcheung.xime.viewmodel.*" --tests "com.kingzcheung.xime.rime.*" --tests "*KeyboardT9ConfigTest" --tests "*SchemaListBlockTest" --tests "*EditorInfoClassifierTest" -PwuwaBundlePack="D:/private/WuwaTappo-1.0.1.wuwapet.zip"
python tools/audit_apk.py app/build/outputs/apk/debug/Wuwa-Tappo-Android-1.0-arm64-v8a.apk
```

运行全部上游插件测试前，仍需按 AGENTS.md 编译 `xipm` 插件和测试夹具。这里不将定向测试描述为全部插件或真机验证。

## Release 文件

使用本目录准备的 APK、SHA256SUMS、RELEASE_NOTES.md，并给出对应源码。桌宠版编号为 `1.0`，Android 宿主 versionName 保留 `3.4.1` 以兼容插件、商店和配置约束，使用 Android Debug 签名；正式发布前需决定并保存后续升级使用的签名密钥，不要把密钥上传仓库。

从旧 Fcitx5 试验版切换，用户需重新启用输入法。Xime 官方更新不会自动带入桌宠整合；新增图鉴需重新生成资源包、校验清单并构建本项目新版。
