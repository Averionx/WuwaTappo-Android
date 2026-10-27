# GitHub 发布准备

项目标题：**Wuwa Tappo Android版**。仓库：`Averionx/WuwaTappo-Android`。基于 Xime，不是原 Fcitx5 方案。

1.0 版本发布在 [GitHub Releases](https://github.com/Averionx/WuwaTappo-Android/releases/tag/v1.0)。附件包含已签名的 arm64-v8a APK、对应源码压缩包和 SHA256SUMS.txt；仓库主页保留项目说明和使用教程。

本版作者、维护与发行者为 **Averionx**。应用内关于页、设置、启用向导、候选栏与无线导入均使用本版标识，Xime 原作者信息保留在开源来源与致谢中。

发行信息定义在 `app/src/main/java/com/kingzcheung/xime/branding/WuwaEdition.kt`。`PUBLISHED=true`，项目和发布入口指向 `Averionx/WuwaTappo-Android` 及其 Releases。桌宠版本为 1.0，Android 宿主兼容版本为 3.4.1，versionCode 为 20261028。

## 仓库内容

保留 GPLv3、上游商标说明和来源记录；附带 `LICENSES/LGPL-2.1-or-later.txt`。根 README 使用本项目说明，上游原 README 另存为 `README.upstream.md`。

README 顶部使用桌面版原始 Logo。发布项目说明时，要一并保留 `docs/images/wuwa-tappo-desktop.png` 及其相对路径，避免只复制 Markdown 导致图片不显示；图标来源与权利说明保留在 `docs/WUWA_ASSETS.md`。APK 图标也使用同一张原图。

源码附件提供当前工作树的完整代码、资源转换和审核工具、文档及展开的子模块。不要提交私有素材包、签名密钥、账号令牌、构建缓存或本机 SDK。APK 已内置素材，不要求安装者手动导入。素材版权与代码许可证分开说明。

Logo、角色、头饰和表情素材仍归原权利人所有，未随代码许可证重新授权。二次分发应遵循原作者相关条款。

## 源码构建

源码附件中包含 `SOURCE_BUILD.md`，解压后先阅读它；无需对源码 ZIP 执行 Git 子模块初始化。素材构建输入可从同版本 APK 中提取，不需要额外下载原版 EXE。

需要 JDK 21、Gradle 9.8.2、Android SDK 37.2、NDK 29.0.14206865 和 CMake 3.22.1。保留上游 wrapper，可按下列方式构建：

```powershell
.\gradlew.bat :app:assembleDebug -PbuildABI=arm64-v8a -PwuwaBundlePack="$PWD/build-input/assets/wuwa/default.wuwapet.zip"
```

构建主机需能下载 Maven、Google Maven 和 GitHub 上的原生依赖。Windows 不具备符号链接权限时，构建脚本复制相同插件源码，不需要管理员许可。

定向验证：

```powershell
.\gradlew.bat :app:testDebugUnitTest --tests "com.kingzcheung.xime.pet.*" --tests "com.kingzcheung.xime.ui.*" --tests "com.kingzcheung.xime.viewmodel.*" --tests "com.kingzcheung.xime.rime.*" --tests "*KeyboardT9ConfigTest" --tests "*SchemaListBlockTest" --tests "*EditorInfoClassifierTest" -PwuwaBundlePack="$PWD/build-input/assets/wuwa/default.wuwapet.zip"
python tools/audit_apk.py app/build/outputs/apk/debug/Wuwa-Tappo-Android-1.0-arm64-v8a.apk
```

运行全部上游插件测试前，仍需按 AGENTS.md 编译 `xipm` 插件和测试夹具。这里不将定向测试描述为全部插件或真机验证。

## Release 文件

使用本目录准备的 APK、SHA256SUMS、RELEASE_NOTES.md，并给出对应源码。桌宠版编号为 `1.0`，Android 宿主 versionName 保留 `3.4.1` 以兼容插件、商店和配置约束，使用 Android Debug 签名；正式发布前需决定并保存后续升级使用的签名密钥，不要把密钥上传仓库。

从旧 Fcitx5 试验版切换，用户需重新启用输入法。Xime 官方更新不会自动带入桌宠整合；新增图鉴需重新生成资源包、校验清单并构建本项目新版。
