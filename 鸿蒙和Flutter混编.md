# mac m1 鸿蒙和Flutter混编
#### [参考文档1](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/ide-creating-har-api9-0000001518082393)、[参考文档2](https://gitee.com/openharmony-sig/flutter_samples/blob/master/ohos/docs/04_development/%E5%BC%80%E5%8F%91module.md)
## 写在前面
### 1、本文的内容是基于已经配置好了鸿蒙版flutter的情况的，如还未配置请参考[mac m1 鸿蒙版flutter配置](https://github.com/547/HarmonyOSUsageRecords/blob/main/%E9%B8%BF%E8%92%99%E7%89%88Flutter%E9%85%8D%E7%BD%AE.md) 先行配置。
### 2、本文使用的命令 ohosFlutter 是我为鸿蒙版flutter设置的别名，因为我还安装了官方的flutter，如只安装了鸿蒙版flutter，可以直接使用flutter。
### 3、本文要混编的flutter模块是一个已有的旧模块，该模块之前支持的平台是iOS、Android，是和iOS、Android混编的。
### 4、当前使用的是HarmonyOS NEXT Developer Beta1（5.0.3.403）、commandline-tools-mac-arm64-5.0.3.403 。

## 原理
### flutter和原生混编是将一个flutter模块/插件（Module）集成到原生项目里面，通过平台通道调用flutter模块中的功能。
### 通过[鸿蒙官方文档](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/ide-creating-har-api9-0000001518082393),可以看出鸿蒙是支持多模块的。主要使用方式是导入HAR(Harmony Archive， 即静态共享包）、HSP（Harmony Shared Package，即动态共享包）。
### 同理，flutter模块想要集成到鸿蒙项目中就要编译成HAR或HSP。
## 一、编译共享包
### 1、先让已有的flutter模块支持鸿蒙平台
```shell
# 进入到flutter模块根目录
momo@momodeMac-mini ~ % cd /Users/momo/Documents/GitHub/flutter-pin-module
# flutter模块 添加鸿蒙平台支持
momo@momodeMac-mini flutter-pin-module % ohosFlutter config --enable-ohos
Setting "enable-ohos" value to "true".

You may need to restart any open editors for them to read new settings.
```
### 2、 编译成HAR（输出 Consuming the Module，就是编译成功了）
```shell
# 编译har指令
momo@momodeMac-mini flutter-pin-module % ohosFlutter build har --release
Asset manifest contains a null or empty uri.
Compiling ohos_aot_bundle_release_ohos-arm64 for the Ohos...        68.5s
copy flutter assets to project start
copy directory from
/Users/momo/Documents/GitHub/flutter-pin-module/build/ohos/flutter_assets to
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/./flutter_module/src/main/
resources/rawfile/flutter_assets
copy flutter assets to project end
copy flutter runtime to project start
copy from:
/Users/momo/Library/Huawei/flutter_flutter/bin/cache/artifacts/engine/ohos-arm64
-release/flutter.har to
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/har/flutter.har
copy flutter runtime to project end
ohpm INFO: MetaDataFetcher fetching meta info of package '@ohos/hypium' from
https://repo.harmonyos.com/ohpm/
ohpm INFO: fetch meta info of package '@ohos/hypium' success
https://repo.harmonyos.com/ohpm/@ohos/hypium
ohpm WARN: local dependency "@ohos/flutter_ohos" found in
"/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/entry/oh-package.json5"
does not match the actual name "flutter" of its oh-package.json5
ohpm WARN: local dependency "@ohos/flutter_ohos" found in
"/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/flutter_module/oh-package
.json5" does not match the actual name "flutter" of its oh-package.json5
install completed in 5s 872ms
ohpm install success.
> hvigor Finished :flutter_module:clean... after 1 ms 
> hvigor Finished :flutter_module:default@PreBuild... after 74 ms 
> hvigor Finished :flutter_module:default@ProcessOHPackageJson... after 1 ms 
> hvigor Finished :flutter_module:default@CreateHarBuildProfile... after 1 ms 
> hvigor Finished :flutter_module:default@ConfigureCmake... after 1 ms 
> hvigor Finished :flutter_module:default@MergeProfile... after 2 ms 
> hvigor Finished :flutter_module:default@GeneratePkgContextInfo... after 1 ms 
> hvigor Finished :flutter_module:default@ProcessIntegratedHsp... after 1 ms 
> hvigor Finished :flutter_module:default@BuildNativeWithCmake... after 1 ms 
> hvigor Finished :flutter_module:default@ProcessProfile... after 78 ms 
> hvigor Finished :flutter_module:default@ProcessRouterMap... after 2 ms 
> hvigor Finished :flutter_module:default@BuildNativeWithNinja... after 1 ms 
> hvigor Finished :flutter_module:default@ProcessResource... after 1 ms 
> hvigor Finished :flutter_module:default@GenerateLoaderJson... after 4 ms 
> hvigor Finished :flutter_module:default@CompileResource... after 236 ms 
> hvigor Finished :flutter_module:default@ProcessObfuscationFiles... after 1 ms 
> hvigor Finished :flutter_module:default@ProcessLibs... after 318 ms 
> hvigor Finished :flutter_module:default@DoNativeStrip... after 10 ms 
> hvigor Finished :flutter_module:default@CacheNativeLibs... after 141 ms 
> hvigor WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/plugin/common/Any.ets:16:20
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/plugin/platform/PlatformViewsController.ets:173:34
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/plugin/platform/PlatformViewsController.ets:173:78
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/plugin/platform/PlatformViewsController.ets:174:38
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/plugin/platform/PlatformViewsController.ets:175:32
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/plugin/platform/PlatformViewsController.ets:175:62
 Usage of 'ESObject' type is restricted (arkts-limited-esobj)

WARN: ArkTS:WARN: For details about ArkTS syntax errors, see FAQs
WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/util/StringUtils.ets:35:24
 'decode' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/util/ByteBuffer.ets:750:20
 'decode' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/util/ByteBuffer.ets:786:30
 'encodeInto' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/view/DynamicView/dynamicView.ets:173:6
 'clip' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/view/DynamicView/dynamicView.ets:178:6
 'clip' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/embedding/engine/systemchannels/PlatformChannel.ets:393:28
 To use this API, you need to apply for the permissions:
 ohos.permission.READ_PASTEBOARD

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/plugin/PlatformPlugin.ets:126:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/plugin/PlatformPlugin.ets:130:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/plugin/PlatformPlugin.ets:134:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/plugin/PlatformPlugin.ets:138:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/plugin/PlatformPlugin.ets:142:18
 To use this API, you need to apply for the permissions: ohos.permission.VIBRATE

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/plugin/PlatformPlugin.ets:229:28
 To use this API, you need to apply for the permissions:
 ohos.permission.READ_PASTEBOARD

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/embedding/engine/renderer/FlutterRenderer.ets:73:47
 'createImageReceiver' has been deprecated.

WARN: ArkTS:WARN File:
/Users/momo/Documents/GitHub/flutter-pin-module/.ohos/oh_modules/.ohpm/@ohos+flu
tter_ohos@2l++k9h7lifa7u2xhjgubdt2wl0=/oh_modules/@ohos/flutter_ohos/src/main/et
s/embedding/engine/FlutterNapi.ets:16:21
 Currently module for 'libflutter.so' is not verified. If you're importing napi,
 its verification will be enabled in later SDK version. Please make sure the
 corresponding .d.ts file is provided and the napis are correctly declared.


> hvigor Finished :flutter_module:default@HarCompileArkTS... after 4 s 404 ms 
> hvigor WARN: Local dependencies detected during har packing of module
flutter_module. Declaring local dependencies in har module might cause failing
during install har package.
	 Detail: Check package.json/oh-package.json5 of current library module and
	 replace the local dependencies.
	 at
	 /Users/momo/Documents/GitHub/flutter-pin-module/.ohos/flutter_module/oh-packag
	 e.json5


> hvigor Finished :flutter_module:default@PackageHar... after 1 s 133 ms 
> hvigor WARN: Will skip sign 'har'. No signingConfigs profile is configured in
current project.
             If needed, configure the signingConfigs in
             /Users/momo/Documents/GitHub/flutter-pin-module/.ohos/build-profile
             .json5.


> hvigor Finished :flutter_module:default@PackageSignHar... after 1 ms 
> hvigor Finished :flutter_module:assembleHar... after 1 ms 
> hvigor BUILD SUCCESSFUL in 6 s 831 ms 
success! when invoke: hvigorw --mode module -p module=flutter_module@default -p
product=default assembleHar --no-daemon.

Consuming the Module
    1. Open <host project>/oh-package.json5
    2. Add flutter_module to the dependencies list:

      "dependencies": {
        "@ohos/flutter_module": "file:path/to/har/flutter_module.har"
      }

    3. Override flutter and plugins dependencies:

      "overrides" {
        "@ohos/flutter_ohos": "file:path/to/har/flutter.har",
        "plugin_xxx":'file:path/to/har/plugin_xxx.har',
      }
```
#### 执行完上面的指令后，可以看到，flutter模块文件夹中多出了.ohos文件夹，.ohos文件夹里面还有har文件夹，就说明已经编译成功了。上面最后输出的内容有教我们怎么使用编译出来的har。
## 二、在鸿蒙项目中导入HAR
### 1、进入到要鸿蒙项目根目录找到har文件夹，如果没有就自己手动创建一下， 然后把上面编译生成的har文件夹中的所有文件都复制到进去。
### 2、进入到要鸿蒙项目根目录找到oh-package.json5文件，添加依赖
```shell
{
  "modelVersion": "5.0.0",
  "description": "Please describe the basic information.",
  "dependencies": {
  // 这是我们自己的flutter模块, 对应的是flutter_module.har在鸿蒙项目中的路径
    "@ohos/flutter_module": "har/flutter_module.har",
  },
  "overrides": {
  // 有冲突的依赖需要放到这里消除冲突
  // 这是根据官方flutter打包的模块，因为@ohos/flutter_module也依赖了@ohos/flutter_ohos，所以是有冲突的要放在这里
    "@ohos/flutter_ohos": "har/flutter.har",
  },
  "devDependencies": {
    "@ohos/hypium": "1.0.18",
    "@ohos/hamock": "1.0.0"
  }
}
```
#### 添加完依赖后保存，然后同步一下点击文件上方出现的【Sync Now】按钮， 同步成功后oh_modules/@ohos文件夹中就会出现flutter_module文件夹。
### 3、使用flutter模块中功能
#### 例1:我在项目中创建了一个flutter_intermediate模块，我要在这个模块中使用导入的flutter_module.har和flutter.har，就要在该模块的oh-package.json5文件中也添加依赖，大致如下：
```shell
{
  "name": "flutter_intermediate",
  "version": "1.0.0",
  "description": "Please describe the basic information.",
  "main": "",
  "author": "",
  "license": "",
  "dependencies": {
    "@ohos/flutter_module": "file:../har/flutter_module.har",
    "@ohos/flutter_ohos": "file:../har/flutter.har"
  }
}
```
#### 添加完后记得也要同步一下


