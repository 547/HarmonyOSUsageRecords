# mac m1 鸿蒙和Flutter混编
#### [参考文档1](https://gitee.com/openharmony-sig/flutter_samples/blob/master/ohos/docs/03_environment/%E9%B8%BF%E8%92%99%E7%89%88Flutter%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA%E6%8C%87%E5%AF%BC.md#3%E4%B8%8B%E8%BD%BD%E9%B8%BF%E8%92%99%E7%89%88flutter)
#### [参考文档2](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/ide-creating-har-api9-0000001518082393)

## 原理
### flutter和原生混编是将一个flutter模块/插件（Module）集成到原生项目里面，通过平台通道调用flutter模块中的功能。

### 通过[文档](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/ide-creating-har-api9-0000001518082393),可以看出鸿蒙是支持多模块的。主要使用方式是导入HAR(Harmony Archive， 即静态共享包）、HSP（Harmony Shared Package，即动态共享包）。

### 同理，flutter模块想要集成到鸿蒙项目中就要编译成HAR或HSP。

#### 注意：编译前先要让已有的flutter模块支持鸿蒙平台

```shell
// flutter模块 添加鸿蒙平台支持
ohosFlutter config --enable-ohos
```

### 一、编译成共享包
#### 1. 编译成HAR
```shell
// 编译成har示例命令
ohosFlutter build har --local-engine='/Users/momo/Library/Huawei/src/out/ohos_release_arm64' --release
```
#### 编译成功后大概会输出以下内容， 同时flutter模块中，也出现了har文件夹
```shell
> hvigor Finished :flutter_module:default@PackageHar... after 1 s 577 ms 
> hvigor Finished :flutter_module:assembleHar... after 1 ms 
> hvigor BUILD SUCCESSFUL in 11 s 104 ms 
success! when invoke: /Users/momo/Documents/GitHub/flutter-pin-module/.ohos/hvigorw clean --mode module -p module=flutter_module@default -p
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

#### 2. 编译成HSP（尝试后编译HSP不成功就先放下，暂时记录一下）
```shell
// 编译成hsp示例命令
ohosFlutter build hsp --local-engine='/Users/momo/Library/Huawei/src/out/ohos_release_arm64' --release
```
#### 不知道是不是flutter模块不能编译成HSP，执行以上命令后会输出以下内容。

```shell
Oops; flutter has exited unexpectedly: "UnimplementedError".
A crash report has been written to /Users/momo/Documents/GitHub/flutter-pin-module/flutter_02.log.
This crash may already be reported. Check GitHub for similar crashes.
https://github.com/flutter/flutter/issues?q=is%3Aissue+UnimplementedError

To report your crash to the Flutter team, first read the guide to filing a bug.
https://flutter.dev/docs/resources/bug-reports

Create a new GitHub issue by pasting this link into your browser and completing the issue template. Thank you!
https://github.com/flutter/flutter/issues/new?title=%5Btool_crash%5D+UnimplementedError%3A+UnimplementedError&body=%23%23+Command%0A%60%60%60%0Aflutter+build+hsp+--local-engine%3D%2FUsers%2Fmomo%2FLibrary%2FHuawei%2Fsrc%2Fout%2Fohos_release_arm64+--release%0A%60%60%60%0A%0A%23%23+Steps+to+Reproduce%0A1.+...%0A2.+...%0A3.+...%0A%0A%23%23+Logs%0AUnimplementedError%3A+UnimplementedError%0A%60%60%60%0A%230++++++OhosHvigorBuilder.buildHsp+%28package%3Aflutter_tools%2Fsrc%2Fohos%2Fhvigor.dart%3A849%3A5%29%0A%231++++++BuildHspCommand.runCommand+%28package%3Aflutter_tools%2Fsrc%2Fcommands%2Fbuild_hsp.dart%3A69%3A24%29%0A%3Casynchronous+suspension%3E%0A%232++++++FlutterCommand.run.%3Canonymous+closure%3E+%28package%3Aflutter_tools%2Fsrc%2Frunner%2Fflutter_command.dart%3A1257%3A27%29%0A%3Casynchronous+suspension%3E%0A%233++++++AppContext.run.%3Canonymous+closure%3E+%28package%3Aflutter_tools%2Fsrc%2Fbase%2Fcontext.dart%3A150%3A19%29%0A%3Casynchronous+suspension%3E%0A%234++++++CommandRunner.runCommand+%28package%3Aargs%2Fcommand_runner.dart%3A209%3A13%29%0A%3Casynchronous+suspension%3E%0A%235++++++FlutterCommandRunner.runCommand.%3Canonymous+closure%3E+%28package%3Aflutter_tools%2Fsrc%2Frunner%2Fflutter_command_runner.dart%3A283%3A9%29%0A%3Casynchronous+suspension%3E%0A%236++++++AppContext.run.%3Canonymous+closure%3E+%28package%3Aflutter_tools%2Fsrc%2Fbase%2Fcontext.dart%3A150%3A19%29%0A%3Casynchronous+suspension%3E%0A%237++++++FlutterCommandRunner.runCommand+%28package%3Aflutter_tools%2Fsrc%2Frunner%2Fflutter_command_runner.dart%3A229%3A5%29%0A%3Casynchronous+suspension%3E%0A%238++++++run.%3Canonymous+closure%3E.%3Canonymous+closure%3E+%28package%3Aflutter_tools%2Frunner.dart%3A64%3A9%29%0A%3Casynchronous+suspension%3E%0A%239++++++AppContext.run.%3Canonymous+closure%3E+%28package%3Aflutter_tools%2Fsrc%2Fbase%2Fcontext.dart%3A150%3A19%29%0A%3Casynchronous+suspension%3E%0A%2310+++++main+%28package%3Aflutter_tools%2Fexecutable.dart%3A91%3A3%29%0A%3Casynchronous+suspension%3E%0A%60%60%60%0A%60%60%60%0A%1B%5B33m%5B%21%5D%1B%5B39m+Flutter+%28Channel+master%2C+3.7.12-ohos%2C+on+macOS+13.4.1+22F770820d+darwin-arm64%2C+locale+zh-Hans-CN%29%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Flutter+version+3.7.12-ohos+on+channel+master+at+%2FUsers%2Fmomo%2FLibrary%2FHuawei%2Fflutter_flutter%0A++++%1B%5B33m%21%1B%5B39m+Warning%3A+%60flutter%60+on+your+path+resolves+to+%2FUsers%2Fmomo%2Fdevelopment%2Fflutter%2Fbin%2Fflutter%2C+which+is+not+inside+your+current+Flutter+SDK%0A++++++checkout+at+%2FUsers%2Fmomo%2FLibrary%2FHuawei%2Fflutter_flutter.+Consider+adding+%2FUsers%2Fmomo%2FLibrary%2FHuawei%2Fflutter_flutter%2Fbin+to+the+front%0A++++++of+your+path.%0A++++%1B%5B33m%21%1B%5B39m+Warning%3A+%60dart%60+on+your+path+resolves+to+%2FUsers%2Fmomo%2Fdevelopment%2Fflutter%2Fbin%2Fdart%2C+which+is+not+inside+your+current+Flutter+SDK%0A++++++checkout+at+%2FUsers%2Fmomo%2FLibrary%2FHuawei%2Fflutter_flutter.+Consider+adding+%2FUsers%2Fmomo%2FLibrary%2FHuawei%2Fflutter_flutter%2Fbin+to+the+front%0A++++++of+your+path.%0A++++%1B%5B33m%21%1B%5B39m+Upstream+repository+https%3A%2F%2Fgitee.com%2Fopenharmony-sig%2Fflutter_flutter.git+is+not+a+standard+remote.%0A++++++Set+environment+variable+%22FLUTTER_GIT_URL%22+to+https%3A%2F%2Fgitee.com%2Fopenharmony-sig%2Fflutter_flutter.git+to+dismiss+this+error.%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Framework+revision+baa9b985d4+%285+weeks+ago%29%2C+2024-05-17+06%3A19%3A35+%2B0000%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Engine+revision+1a65d409c7%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Dart+version+2.19.6%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+DevTools+version+2.20.1%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Pub+download+mirror+https%3A%2F%2Fpub.flutter-io.cn%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Flutter+download+mirror+https%3A%2F%2Fstorage.flutter-io.cn%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+If+those+were+intentional%2C+you+can+disregard+the+above+warnings%3B+however+it+is+recommended+to+use+%22git%22+directly+to+perform+update%0A++++++checks+and+upgrades.%0A%0A%1B%5B32m%5B%E2%9C%93%5D%1B%5B39m+HarmonyOS+toolchain+-+develop+for+HarmonyOS+devices%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+OpenHarmony+Sdk+location%3A+%2FUsers%2Fmomo%2FLibrary%2FHuawei%2FSdk%2C+available+api+versions+has+%5BHarmonyOS-NEXT-DP2%5D%0A%0A%1B%5B32m%5B%E2%9C%93%5D%1B%5B39m+Android+toolchain+-+develop+for+Android+devices+%28Android+SDK+version+33.0.0%29%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Android+SDK+at+%2FUsers%2Fmomo%2FLibrary%2FAndroid%2Fsdk%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Platform+android-33%2C+build-tools+33.0.0%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Java+binary+at%3A+%2FApplications%2FAndroid+Studio.app%2FContents%2Fjre%2FContents%2FHome%2Fbin%2Fjava%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Java+version+OpenJDK+Runtime+Environment+%28build+17.0.10%2B0-17.0.10b1087.21-11572160%29%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+All+Android+licenses+accepted.%0A%0A%1B%5B32m%5B%E2%9C%93%5D%1B%5B39m+Xcode+-+develop+for+iOS+and+macOS+%28Xcode+14.3.1%29%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Xcode+at+%2FApplications%2FXcode.app%2FContents%2FDeveloper%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Build+14E300c%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+CocoaPods+version+1.15.2%0A%0A%1B%5B32m%5B%E2%9C%93%5D%1B%5B39m+Chrome+-+develop+for+the+web%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Chrome+at+%2FApplications%2FGoogle+Chrome.app%2FContents%2FMacOS%2FGoogle+Chrome%0A%0A%1B%5B32m%5B%E2%9C%93%5D%1B%5B39m+Android+Studio+%28version+2023.3%29%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Android+Studio+at+%2FApplications%2FAndroid+Studio.app%2FContents%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Flutter+plugin+can+be+installed+from%3A%0A++++++%F0%9F%94%A8+https%3A%2F%2Fplugins.jetbrains.com%2Fplugin%2F9212-flutter%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Dart+plugin+can+be+installed+from%3A%0A++++++%F0%9F%94%A8+https%3A%2F%2Fplugins.jetbrains.com%2Fplugin%2F6351-dart%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Java+version+OpenJDK+Runtime+Environment+%28build+17.0.10%2B0-17.0.10b1087.21-11572160%29%0A%0A%1B%5B32m%5B%E2%9C%93%5D%1B%5B39m+VS+Code+%28version+1.90.0%29%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+VS+Code+at+%2FApplications%2FVisual+Studio+Code.app%2FContents%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Flutter+extension+version+3.90.0%0A%0A%1B%5B32m%5B%E2%9C%93%5D%1B%5B39m+Proxy+Configuration%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+HTTP_PROXY+is+set%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+NO_PROXY+is+localhost%2C127.0.0.1%2Clocaladdress%2C.localdomain.com%2C%3A%3A1%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+NO_PROXY+contains+localhost%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+NO_PROXY+contains+127.0.0.1%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+NO_PROXY+contains+%3A%3A1%0A%0A%1B%5B32m%5B%E2%9C%93%5D%1B%5B39m+Connected+device+%284+available%29%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Seven+Wang%27s+iPhone+%28mobile%29+%E2%80%A2+f5ddc9eca19d12a504ca77f3251c06aee370286c+%E2%80%A2+ios++++++++++++%E2%80%A2+iOS+15.2+19C56%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+iPhone+14+Pro+Max+%28mobile%29+++%E2%80%A2+DB69DB93-982C-4244-8B83-BD57BA6A8B23+++++%E2%80%A2+ios++++++++++++%E2%80%A2%0A++++++com.apple.CoreSimulator.SimRuntime.iOS-16-4+%28simulator%29%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+macOS+%28desktop%29++++++++++++++%E2%80%A2+macos++++++++++++++++++++++++++++++++++++%E2%80%A2+darwin-arm64+++%E2%80%A2+macOS+13.4.1+22F770820d+darwin-arm64%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+Chrome+%28web%29+++++++++++++++++%E2%80%A2+chrome+++++++++++++++++++++++++++++++++++%E2%80%A2+web-javascript+%E2%80%A2+Google+Chrome+126.0.6478.62%0A%0A%1B%5B32m%5B%E2%9C%93%5D%1B%5B39m+HTTP+Host+Availability%0A++++%1B%5B32m%E2%80%A2%1B%5B39m+All+required+HTTP+hosts+are+available%0A%0A%1B%5B33m%21%1B%5B39m+Doctor+found+issues+in+1+category.%0A%0A%60%60%60%0A%0A%23%23+Flutter+Application+Metadata%0A%2A%2AType%2A%2A%3A+module%0A%2A%2AVersion%2A%2A%3A+1.0.0%2B1%0A%2A%2AMaterial%2A%2A%3A+true%0A%2A%2AAndroid+X%2A%2A%3A+true%0A%2A%2AModule%2A%2A%3A+true%0A%2A%2APlugin%2A%2A%3A+false%0A%2A%2AAndroid+package%2A%2A%3A+com.gree.salessystem.flutter.flutter_pin_module%0A%2A%2AiOS+bundle+identifier%2A%2A%3A+com.gree.salessystem.flutter.flutterPinModule%0A%2A%2ACreation+channel%2A%2A%3A+stable%0A%2A%2ACreation+framework+version%2A%2A%3A+ee4e09cce01d6f2d7f4baebd247fde02e5008851%0A%23%23%23+Plugins%0Aaudioplayers-4.1.0%0Aaudioplayers_android-3.0.2%0Aaudioplayers_darwin-4.1.0%0Aaudioplayers_linux-2.1.0%0Aaudioplayers_web-3.1.0%0Aaudioplayers_windows-2.0.2%0Acamera-0.10.5%2B5%0Acamera_android-0.10.8%2B13%0Acamera_avfoundation-0.9.13%2B7%0Acamera_web-0.3.2%2B2%0Adevice_info-2.0.3%0Afile_selector_linux-0.9.2%2B1%0Afile_selector_macos-0.9.3%2B3%0Afile_selector_windows-0.9.3%2B1%0Afk_user_agent-2.1.0%0Aflutter_boost-d63b3d1e0aeee72957fa0ada94ea935bf51d92b2%0Aflutter_image_compress-1.1.3%0Aflutter_inappwebview-5.5.0%0Aflutter_keyboard_visibility-5.4.1%0Aflutter_keyboard_visibility_linux-1.0.0%0Aflutter_keyboard_visibility_macos-1.0.0%0Aflutter_keyboard_visibility_web-2.0.0%0Aflutter_keyboard_visibility_windows-1.0.0%0Aflutter_plugin_android_lifecycle-2.0.17%0Aflutter_qr_reader-1.0.5%0Aflutter_user_channel-1.0.3%0Afluttertoast-8.2.2%0Agoogle_mlkit_barcode_scanning-0.5.0%0Agoogle_mlkit_commons-0.2.0%0Aimage_gallery_saver-2.0.3%0Aimage_picker-1.0.4%0Aimage_picker_android-0.8.8%2B2%0Aimage_picker_for_web-3.0.1%0Aimage_picker_ios-0.8.8%2B4%0Aimage_picker_linux-0.2.1%2B1%0Aimage_picker_macos-0.2.1%2B1%0Aimage_picker_windows-0.2.1%2B1%0Aota_update-4.0.1%0Apackage_info-2.0.2%0Apath_provider-2.1.1%0Apath_provider_android-2.2.1%0Apath_provider_foundation-2.3.1%0Apath_provider_linux-2.2.1%0Apath_provider_windows-2.2.1%0Apermission_handler-10.2.0%0Apermission_handler_android-10.3.6%0Apermission_handler_apple-9.1.4%0Apermission_handler_windows-0.1.3%0Aphoto_manager-2.2.1%0Aqr_code_scanner-1.0.1%0Ashare_plus-4.5.3%0Ashare_plus_linux-3.0.1%0Ashare_plus_macos-3.0.1%0Ashare_plus_web-3.1.0%0Ashare_plus_windows-3.0.1%0Ashared_preferences-2.0.15%0Ashared_preferences_android-2.2.1%0Ashared_preferences_ios-2.1.1%0Ashared_preferences_linux-2.3.2%0Ashared_preferences_macos-2.0.5%0Ashared_preferences_web-2.2.1%0Ashared_preferences_windows-2.3.2%0Asqflite-2.2.8%2B4%0Aurl_launcher-6.1.6%0Aurl_launcher_android-6.2.0%0Aurl_launcher_ios-6.2.0%0Aurl_launcher_linux-3.1.0%0Aurl_launcher_macos-3.1.0%0Aurl_launcher_web-2.0.19%0Aurl_launcher_windows-3.1.0%0Avideo_player-2.7.2%0Avideo_player_android-2.4.10%0Avideo_player_avfoundation-2.4.11%0Avideo_player_web-2.0.17%0Awebview_flutter-3.0.4%0Awebview_flutter_android-2.10.4%0Awebview_flutter_wkwebview-2.9.5%0A%0A&labels=tool%2Csevere%3A+crash
```
### 二、在鸿蒙项目中导入HAR
#### 上面编译HAR成功后的输出内容中有简单提示，如何导入到项目中，也可以查看[官方文档](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides/ide-har-import-0000001547293682)


