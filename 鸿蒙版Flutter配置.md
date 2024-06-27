# mac m1 配置鸿蒙版Flutter
#### [参考文档](https://gitee.com/openharmony-sig/flutter_samples/blob/master/ohos/docs/03_environment/%E9%B8%BF%E8%92%99%E7%89%88Flutter%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA%E6%8C%87%E5%AF%BC.md#3%E4%B8%8B%E8%BD%BD%E9%B8%BF%E8%92%99%E7%89%88flutter)

## 一、下载
#### 1.官方提供的开发工具下载链接：[https://developer.huawei.com/consumer/cn/download/](https://developer.huawei.com/consumer/cn/download/)，至少要下载DevEco Studio、Command Line Tools，其他的可以按自己需求。
#### 2.下载鸿蒙版flutter，项目地址：[https://gitee.com/openharmony-sig/flutter_flutter](https://gitee.com/openharmony-sig/flutter_flutter)（dev的代码比较新，我使用的是dev分支的）
```shell
git clone https://gitee.com/openharmony-sig/flutter_flutter.git
git checkout dev
```
## 二、安装（mac 安装DevEco Studio挺简单的，就和平常安装其他的软件一样，这边就不多赘诉了。）
### 1、安装SDK 
#### 安装好DevEco Studio后，打开DevEco Studio，进入到设置【Preferences】，点击 【OpenHarmony SDK】, 然后点击OpenHarmony SDK安装位置右边的【编辑】按钮下载最新的SDK，然后一路 【下一步】，等下载完点 【完成】 就可以了。（安装完SDK，回到 OpenHarmony SDK页面，可以看到 OpenHarmony SDK安装位置已经填充上SDK的绝对路径（/Users/xxxx/Library/OpenHarmony/Sdk）了。）OpenHarmony SDK页面，有一个不同版本的API表格，也可以根据自己的需求勾选并下载安装，安装完成，点击 【确认】 即可。
### 2、安装Command Line Tools
#### 把下载好的Command Line Tools压缩包解压后放到和上面SDK安装的同级路径下（应该也可按自己喜好放，我是方便后面找和SDK放一块了）,最终绝对路径：/Users/xxxx/Library/OpenHarmony/command-line-tools
## 三、配置环境变量
#### 注意：HarmonyOS NEXT Developer Beta1（5.0.3.403）因为将HarmonyOS SDK、Node.js、Hvigor、OHPM、模拟器平台等进行合一打包，所以和上面的参考文档有点区别。commandline-tools-mac-arm64-5.0.3.403里面已经没有sdkmanager了，所以关于sdkmanager的配置就不用管了。我们只要在.zshrc文件中配置关于HarmonyOS SDK和Command Line Tools、鸿蒙版flutter的环境变量即可，具体如下：
```shell
# flutter国内镜像
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
#鸿蒙
# 设置鸿蒙版flutter的别名为ohosFlutter，因为我的电脑还保留了官方的flutter，如果只安装鸿蒙版flutter可以不用设置别名
alias ohosFlutter='/Users/momo/Library/Huawei/flutter_flutter/bin/flutter'
# 鸿蒙版flutter的bin目录
export PATH=$PATH:/Users/momo/Library/Huawei/flutter_flutter/bin
# commandline-tools-mac-arm64-5.0.3.403里面包含了HarmonyOS SDK
export HOS_SDK_HOME=/Users/momo/Library/OpenHarmony/command-line-tools/sdk
# commandline-tools-mac-arm64-5.0.3.403的 bin 子目录
export PATH=$PATH:/Users/momo/Library/OpenHarmony/command-line-tools/bin
#鸿蒙 END
```
#### 以上保存成功后，打开终端运行 source ~/.zshrc
```shell
source ~/.zshrc
```
#### 运行 flutter doctor -v 检查环境变量配置是否正确， 输出的内容关于 HarmonyOS 的是有 [✓] 的就没问题了
```shell
# 我的鸿蒙版flutter设置了别名，所以是ohosFlutter，没设置的就用flutter
momo@momodeMac-mini ~ % ohosFlutter doctor -v
[!] Flutter (Channel dev, 3.7.12-ohos, on macOS 13.4.1 22F770820d darwin-arm64
    (Rosetta), locale zh-Hans-CN)
    • Flutter version 3.7.12-ohos on channel dev at
      /Users/momo/Library/Huawei/flutter_flutter
    ! Warning: `flutter` on your path resolves to
      /Users/momo/development/flutter/bin/flutter, which is not inside your
      current Flutter SDK checkout at
      /Users/momo/Library/Huawei/flutter_flutter. Consider adding
      /Users/momo/Library/Huawei/flutter_flutter/bin to the front of your path.
    ! Warning: `dart` on your path resolves to
      /Users/momo/development/flutter/bin/dart, which is not inside your current
      Flutter SDK checkout at /Users/momo/Library/Huawei/flutter_flutter.
      Consider adding /Users/momo/Library/Huawei/flutter_flutter/bin to the
      front of your path.
    ! Upstream repository https://gitee.com/openharmony-sig/flutter_flutter.git
      is not a standard remote.
      Set environment variable "FLUTTER_GIT_URL" to
      https://gitee.com/openharmony-sig/flutter_flutter.git to dismiss this
      error.
    • Framework revision 59ee9c25ad (20 hours ago), 2024-06-26 09:15:23 +0000
    • Engine revision 1a65d409c7
    • Dart version 2.19.6
    • DevTools version 2.20.1
    • Pub download mirror https://pub.flutter-io.cn
    • Flutter download mirror https://storage.flutter-io.cn
    • If those were intentional, you can disregard the above warnings; however
      it is recommended to use "git" directly to perform update checks and
      upgrades.

[✓] HarmonyOS toolchain - develop for HarmonyOS devices
    • OpenHarmony Sdk at /Users/momo/Library/OpenHarmony/command-line-tools/sdk,
      available api versions has [12:HarmonyOS-NEXT-DB1]
    • Ohpm version 5.0.2
    • Node version v20.9.0
    • Hvigorw binary at
      /Users/momo/Library/OpenHarmony/command-line-tools/bin/hvigorw

[✓] Android toolchain - develop for Android devices (Android SDK version 33.0.0)
    • Android SDK at /Users/momo/Library/Android/sdk
    • Platform android-33, build-tools 33.0.0
    • Java binary at: /Applications/Android
      Studio.app/Contents/jre/Contents/Home/bin/java
    • Java version OpenJDK Runtime Environment (build
      17.0.10+0-17.0.10b1087.21-11572160)
    • All Android licenses accepted.

[✓] Xcode - develop for iOS and macOS (Xcode 14.3.1)
    • Xcode at /Applications/Xcode.app/Contents/Developer
    • Build 14E300c
    • CocoaPods version 1.15.2

[✓] Chrome - develop for the web
    • Chrome at /Applications/Google Chrome.app/Contents/MacOS/Google Chrome

[✓] Android Studio (version 2023.3)
    • Android Studio at /Applications/Android Studio.app/Contents
    • Flutter plugin can be installed from:
      🔨 https://plugins.jetbrains.com/plugin/9212-flutter
    • Dart plugin can be installed from:
      🔨 https://plugins.jetbrains.com/plugin/6351-dart
    • Java version OpenJDK Runtime Environment (build
      17.0.10+0-17.0.10b1087.21-11572160)

[✓] VS Code (version 1.90.2)
    • VS Code at /Applications/Visual Studio Code.app/Contents
    • Flutter extension version 3.90.0

[✓] Proxy Configuration
    • HTTP_PROXY is set
    • NO_PROXY is localhost,127.0.0.1,localaddress,.localdomain.com,::1
    • NO_PROXY contains localhost
    • NO_PROXY contains 127.0.0.1
    • NO_PROXY contains ::1

[✓] Connected device (5 available)
    • 127.0.0.1:5555 (mobile)      • 127.0.0.1:5555                           •
      ohos-arm64     • Ohos OpenHarmony-5.0.0.25 (API 12)
    • Seven Wang's iPhone (mobile) • f5ddc9eca19d12a504ca77f3251c06aee370286c •
      ios            • iOS 15.2 19C56
    • iPhone 14 Pro Max (mobile)   • DB69DB93-982C-4244-8B83-BD57BA6A8B23     •
      ios            • com.apple.CoreSimulator.SimRuntime.iOS-16-4 (simulator)
    • macOS (desktop)              • macos                                    •
      darwin-arm64   • macOS 13.4.1 22F770820d darwin-arm64 (Rosetta)
    • Chrome (web)                 • chrome                                   •
      web-javascript • Google Chrome 126.0.6478.114

[✓] HTTP Host Availability
    • All required HTTP hosts are available

! Doctor found issues in 1 category.
```
## 四、创建支持鸿蒙平台的flutter项目（输出 All done! 就是创建成功了）
```shell
# 进入到要创建项目的文件夹
momo@momodeMac-mini ~ % cd /Users/momo/Desktop
# 创建 flutter项目，flutter create --platforms ohos <projectName>
momo@momodeMac-mini Desktop % ohosFlutter create --platforms ohos my_project
Creating project my_project...
Running "flutter pub get" in my_project...
Resolving dependencies in my_project... (1.0s)
+ async 2.10.0 (2.11.0 available)
+ boolean_selector 2.1.1
+ characters 1.2.1 (1.3.0 available)
+ clock 1.1.1
+ collection 1.17.0 (1.19.0 available)
+ cupertino_icons 1.0.6 (1.0.8 available)
+ fake_async 1.3.1
+ flutter 0.0.0 from sdk flutter
+ flutter_lints 2.0.3 (4.0.0 available)
+ flutter_test 0.0.0 from sdk flutter
+ js 0.6.5 (0.7.1 available)
+ lints 2.0.1 (4.0.0 available)
+ matcher 0.12.13 (0.12.16+1 available)
+ material_color_utilities 0.2.0 (0.12.0 available)
+ meta 1.8.0 (1.15.0 available)
+ path 1.8.2 (1.9.0 available)
+ sky_engine 0.0.99 from sdk flutter
+ source_span 1.9.1 (1.10.0 available)
+ stack_trace 1.11.0 (1.11.1 available)
+ stream_channel 2.1.1 (2.1.2 available)
+ string_scanner 1.2.0
+ term_glyph 1.2.1
+ test_api 0.4.16 (0.7.2 available)
+ vector_math 2.1.4
Changed 24 dependencies in my_project!
Wrote 43 files.

All done!
You can find general documentation for Flutter at: https://docs.flutter.dev/
Detailed API documentation is available at: https://api.flutter.dev/
If you prefer video documentation, consider: https://www.youtube.com/c/flutterdev

In order to run your application, type:

  $ cd my_project
  $ flutter run

Your application code is in my_project/lib/main.dart.

momo@momodeMac-mini Desktop % 

```
## 五、运行flutter项目
### 1、创建模拟器，官方提供的参考文档[https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/ide-run-emulator-0000001582636200#section147575117514](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/ide-run-emulator-0000001582636200#section147575117514)（鉴于这一块官方文档比较详细就赘诉了），这一步只要启动好模拟器就可以了。
### 2、使用模拟器运行flutter项目
```shell
# 进入到项目根目录
momo@momodeMac-mini Desktop % cd my_project
# 发现可用设备
momo@momodeMac-mini my_project % ohosFlutter devices
4 connected devices:

127.0.0.1:5555 (mobile)      • 127.0.0.1:5555                           • ohos-arm64
• Ohos OpenHarmony-5.0.0.25 (API 12)
iPhone 14 Pro Max (mobile)   • DB69DB93-982C-4244-8B83-BD57BA6A8B23     • ios
• com.apple.CoreSimulator.SimRuntime.iOS-16-4 (simulator)
macOS (desktop)              • macos                                    •
darwin-arm64   • macOS 13.4.1 22F770820d darwin-arm64 (Rosetta)
Chrome (web)                 • chrome                                   •
web-javascript • Google Chrome 126.0.6478.127
```
#### 上面可以看出我这有4个可用的设备，其中 127.0.0.1:5555 就是 鸿蒙的设备，下面开始运行到指定设备上
```shell
# 运行应用的指令： flutter run --release -d <device-id>
momo@momodeMac-mini my_project % ohosFlutter run --release -d 127.0.0.1:5555
Launching lib/main.dart on 127.0.0.1:5555 in release mode...
start hap build...
Compiling ohos_aot_bundle_release_ohos-arm64 for the Ohos...        10.8s
copy flutter assets to project start
copy directory from /Users/momo/Desktop/my_project/build/ohos/flutter_assets to
/Users/momo/Desktop/my_project/ohos/entry/src/main/resources/rawfile/flutter_assets
copy flutter assets to project end
copy flutter runtime to project start
copy from:
/Users/momo/Library/Huawei/flutter_flutter/bin/cache/artifacts/engine/ohos-arm64-rel
ease/flutter.har to /Users/momo/Desktop/my_project/ohos/har/flutter.har
copy flutter runtime to project end
ohpm INFO: MetaDataFetcher fetching meta info of package '@ohos/hypium' from
https://repo.harmonyos.com/ohpm/
ohpm INFO: fetch meta info of package '@ohos/hypium' success
https://repo.harmonyos.com/ohpm/@ohos/hypium
ohpm WARN: local dependency "@ohos/flutter_ohos" found in
"/Users/momo/Desktop/my_project/ohos/oh-package.json5" does not match the actual
name "flutter" of its oh-package.json5
install completed in 0s 912ms
ohpm install success.
ohpm WARN: local dependency "@ohos/flutter_ohos" found in
"/Users/momo/Desktop/my_project/ohos/oh-package.json5" does not match the actual
name "flutter" of its oh-package.json5
install completed in 0s 24ms
ohpm install success.
> hvigor Finished :entry:clean... after 3 ms 
> hvigor Finished :entry:default@PreBuild... after 67 ms 
> hvigor Finished :entry:default@GenerateMetadata... after 2 ms 
> hvigor Finished :entry:default@ConfigureCmake... after 1 ms 
> hvigor Finished :entry:default@MergeProfile... after 2 ms 
> hvigor Finished :entry:default@CreateBuildProfile... after 1 ms 
> hvigor Finished :entry:default@PreCheckSyscap... after 1 ms 
> hvigor Finished :entry:default@GeneratePkgContextInfo... after 1 ms 
> hvigor Finished :entry:default@ProcessIntegratedHsp... after 1 ms 
> hvigor Finished :entry:default@BuildNativeWithCmake... after 1 ms 
> hvigor Finished :entry:default@MakePackInfo... after 2 ms 
> hvigor Finished :entry:default@ProcessProfile... after 47 ms 
> hvigor Finished :entry:default@SyscapTransform... after 1 ms 
> hvigor Finished :entry:default@ProcessRouterMap... after 2 ms 
> hvigor Finished :entry:default@BuildNativeWithNinja... after 1 ms 
> hvigor Finished :entry:default@ProcessResource... after 2 ms 
> hvigor Finished :entry:default@GenerateLoaderJson... after 4 ms 
> hvigor Finished :entry:default@CompileResource... after 65 ms 
> hvigor Finished :entry:default@BuildJS... after 1 ms 
> hvigor Finished :entry:default@ProcessLibs... after 301 ms 
> hvigor Finished :entry:default@DoNativeStrip... after 9 ms 
> hvigor Finished :entry:default@CacheNativeLibs... after 120 ms 
> hvigor WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/common/Any.ets:16
:20
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/platform/Platform
ViewsController.ets:173:34
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/platform/Platform
ViewsController.ets:173:78
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/platform/Platform
ViewsController.ets:174:38
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/platform/Platform
ViewsController.ets:175:32
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/platform/Platform
ViewsController.ets:175:62
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN: For details about ArkTS syntax errors, see FAQs
WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/util/StringUtils.ets:35:
24
 'decode' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/util/ByteBuffer.ets:750:
20
 'decode' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/util/ByteBuffer.ets:786:
30
 'encodeInto' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/view/DynamicView/dynamic
View.ets:173:6
 'clip' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/view/DynamicView/dynamic
View.ets:178:6
 'clip' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/embedding/engine/systemc
hannels/PlatformChannel.ets:393:28
 To use this API, you need to apply for the permissions:
 ohos.permission.READ_PASTEBOARD

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/PlatformPlugin.et
s:126:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/PlatformPlugin.et
s:130:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/PlatformPlugin.et
s:134:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/PlatformPlugin.et
s:138:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/PlatformPlugin.et
s:142:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/PlatformPlugin.et
s:229:28
 To use this API, you need to apply for the permissions:
 ohos.permission.READ_PASTEBOARD

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/embedding/engine/rendere
r/FlutterRenderer.ets:73:47
 'createImageReceiver' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/embedding/engine/Flutter
Napi.ets:16:21
 Currently module for 'libflutter.so' is not verified. If you're importing napi, its
 verification will be enabled in later SDK version. Please make sure the
 corresponding .d.ts file is provided and the napis are correctly declared.

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/view/DynamicView/dynamic
View.ets:189:18
 The regular property 'child as DVModel' cannot be assigned to the @ObjectLink
 property 'model'.

> hvigor Finished :entry:default@CompileArkTS... after 5 s 393 ms 
> hvigor Finished :entry:default@GeneratePkgModuleJson... after 1 ms 
> hvigor Finished :entry:default@PackageHap... after 332 ms 
> hvigor WARN: Will skip sign 'hos_hap'. No signingConfigs profile is configured in
current project.
             If needed, configure the signingConfigs in
             /Users/momo/Desktop/my_project/ohos/build-profile.json5.


> hvigor Finished :entry:default@SignHap... after 1 ms 
> hvigor Finished :entry:assembleHap... after 1 ms 
> hvigor BUILD SUCCESSFUL in 6 s 727 ms 
success! when invoke: hvigorw assembleHap --no-daemon.
请通过DevEco Studio打开ohos工程后配置调试签名(File -> Project Structure -> Signing Configs
勾选Automatically generate signature)
Exception: Failed to get the hap file:
/Users/momo/Desktop/my_project/ohos/./entry/build/default/outputs/default/entry-defa
ult-signed.hap
#0      throwToolExit (package:flutter_tools/src/base/common.dart:10:3)
#1      OhosDevice._installApp
(package:flutter_tools/src/ohos/ohos_device.dart:136:7)
#2      OhosDevice.installApp
(package:flutter_tools/src/ohos/ohos_device.dart:401:15)
<asynchronous suspension>
#3      OhosDevice.startApp (package:flutter_tools/src/ohos/ohos_device.dart:324:10)
<asynchronous suspension>
#4      FlutterDevice.runCold
(package:flutter_tools/src/resident_runner.dart:526:33)
<asynchronous suspension>
#5      ColdRunner.run (package:flutter_tools/src/run_cold.dart:57:28)
<asynchronous suspension>
#6      RunCommand.runCommand (package:flutter_tools/src/commands/run.dart:715:27)
<asynchronous suspension>
#7      FlutterCommand.run.<anonymous closure>
(package:flutter_tools/src/runner/flutter_command.dart:1257:27)
<asynchronous suspension>
#8      AppContext.run.<anonymous closure>
(package:flutter_tools/src/base/context.dart:150:19)
<asynchronous suspension>
#9      CommandRunner.runCommand (package:args/command_runner.dart:209:13)
<asynchronous suspension>
#10     FlutterCommandRunner.runCommand.<anonymous closure>
(package:flutter_tools/src/runner/flutter_command_runner.dart:283:9)
<asynchronous suspension>
#11     AppContext.run.<anonymous closure>
(package:flutter_tools/src/base/context.dart:150:19)
<asynchronous suspension>
#12     FlutterCommandRunner.runCommand
(package:flutter_tools/src/runner/flutter_command_runner.dart:229:5)
<asynchronous suspension>
#13     run.<anonymous closure>.<anonymous closure>
(package:flutter_tools/runner.dart:64:9)
<asynchronous suspension>
#14     AppContext.run.<anonymous closure>
(package:flutter_tools/src/base/context.dart:150:19)
<asynchronous suspension>
#15     main (package:flutter_tools/executable.dart:91:3)
<asynchronous suspension>
```
#### 上面输出内容中有一段提示：“请通过DevEco Studio打开ohos工程后配置调试签名(File -> Project Structure -> Signing Configs勾选Automatically generate signature)”，按提示用DevEco Studio打开项目文件夹中的ohos工程配置好调试签名, 配置好后记得要点击 【Apply】按钮，然后再继续执行运行应用的指令
```shell
# 运行应用的指令： flutter run --release -d <device-id>
momo@momodeMac-mini my_project % ohosFlutter run --release -d 127.0.0.1:5555
Launching lib/main.dart on 127.0.0.1:5555 in release mode...
start hap build...
Compiling ohos_aot_bundle_release_ohos-arm64 for the Ohos...        461ms
copy flutter assets to project start
copy directory from /Users/momo/Desktop/my_project/build/ohos/flutter_assets to
/Users/momo/Desktop/my_project/ohos/entry/src/main/resources/rawfile/flutter_assets
copy flutter assets to project end
copy flutter runtime to project start
copy from:
/Users/momo/Library/Huawei/flutter_flutter/bin/cache/artifacts/engine/ohos-arm64-rel
ease/flutter.har to /Users/momo/Desktop/my_project/ohos/har/flutter.har
copy flutter runtime to project end
ohpm WARN: local dependency "@ohos/flutter_ohos" found in
"/Users/momo/Desktop/my_project/ohos/oh-package.json5" does not match the actual
name "flutter" of its oh-package.json5
install completed in 0s 25ms
ohpm install success.
ohpm WARN: local dependency "@ohos/flutter_ohos" found in
"/Users/momo/Desktop/my_project/ohos/oh-package.json5" does not match the actual
name "flutter" of its oh-package.json5
install completed in 0s 24ms
ohpm install success.
> hvigor Finished :entry:default@PreBuild... after 61 ms 
> hvigor Finished :entry:default@GenerateMetadata... after 3 ms 
> hvigor Finished :entry:default@ConfigureCmake... after 1 ms 
> hvigor Finished :entry:default@MergeProfile... after 2 ms 
> hvigor Finished :entry:default@CreateBuildProfile... after 1 ms 
> hvigor Finished :entry:default@PreCheckSyscap... after 1 ms 
> hvigor Finished :entry:default@GeneratePkgContextInfo... after 1 ms 
> hvigor Finished :entry:default@ProcessIntegratedHsp... after 1 ms 
> hvigor Finished :entry:default@BuildNativeWithCmake... after 1 ms 
> hvigor Finished :entry:default@MakePackInfo... after 2 ms 
> hvigor Finished :entry:default@ProcessProfile... after 45 ms 
> hvigor Finished :entry:default@SyscapTransform... after 15 ms 
> hvigor Finished :entry:default@ProcessRouterMap... after 2 ms 
> hvigor Finished :entry:default@BuildNativeWithNinja... after 3 ms 
> hvigor UP-TO-DATE :entry:default@ProcessResource...  
> hvigor Finished :entry:default@GenerateLoaderJson... after 4 ms 
> hvigor Finished :entry:default@CompileResource... after 50 ms 
> hvigor Finished :entry:default@BuildJS... after 1 ms 
> hvigor Finished :entry:default@ProcessLibs... after 324 ms 
> hvigor UP-TO-DATE :entry:default@DoNativeStrip...  
> hvigor UP-TO-DATE :entry:default@CacheNativeLibs...  
> hvigor WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/common/Any.ets:16
:20
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/platform/Platform
ViewsController.ets:173:34
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/platform/Platform
ViewsController.ets:173:78
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/platform/Platform
ViewsController.ets:174:38
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/platform/Platform
ViewsController.ets:175:32
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/platform/Platform
ViewsController.ets:175:62
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN: For details about ArkTS syntax errors, see FAQs
WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/util/StringUtils.ets:35:
24
 'decode' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/util/ByteBuffer.ets:750:
20
 'decode' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/util/ByteBuffer.ets:786:
30
 'encodeInto' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/view/DynamicView/dynamic
View.ets:173:6
 'clip' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/view/DynamicView/dynamic
View.ets:178:6
 'clip' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/embedding/engine/systemc
hannels/PlatformChannel.ets:393:28
 To use this API, you need to apply for the permissions:
 ohos.permission.READ_PASTEBOARD

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/PlatformPlugin.et
s:126:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/PlatformPlugin.et
s:130:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/PlatformPlugin.et
s:134:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/PlatformPlugin.et
s:138:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/PlatformPlugin.et
s:142:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/plugin/PlatformPlugin.et
s:229:28
 To use this API, you need to apply for the permissions:
 ohos.permission.READ_PASTEBOARD

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/embedding/engine/rendere
r/FlutterRenderer.ets:73:47
 'createImageReceiver' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Desktop/my_project/ohos/oh_modules/.ohpm/@ohos+flutter_ohos@rfodw3qgijuv
xybjw+dahknaebm=/oh_modules/@ohos/flutter_ohos/src/main/ets/embedding/engine/Flutter
Napi.ets:16:21
 Currently module for 'libflutter.so' is not verified. If you're importing napi, its
 verification will be enabled in later SDK version. Please make sure the
 corresponding .d.ts file is provided and the napis are correctly declared.


> hvigor Finished :entry:default@CompileArkTS... after 3 s 402 ms 
> hvigor Finished :entry:default@GeneratePkgModuleJson... after 1 ms 
> hvigor Finished :entry:default@PackageHap... after 254 ms 
> hvigor Finished :entry:default@SignHap... after 1 s 628 ms 
> hvigor Finished :entry:assembleHap... after 1 ms 
> hvigor BUILD SUCCESSFUL in 6 s 167 ms 
success! when invoke: hvigorw assembleHap --no-daemon.
installing hap. bundleName: com.example.my_project 
install hap finish. bundleName: com.example.my_project 

Flutter run key commands.
h List all available interactive commands.
c Clear the screen
q Quit (terminate the application on the device).
```
#### 上面输出 Flutter run key commands. 就说明已经在模拟器中跑起来了，同时也可以看到之前启动的模拟器已经打开我们的flutter项目了。