# 终端管理app鸿蒙和Flutter混编目前遇到的问题
## flutter模块使用的第三方库
### 1、目前鸿蒙有计划，但还没支持的
#### qr_code_scanner 
#### flutter_qr_reader
#### fk_user_agent
#### photo_manager
#### keyboard_actions
#### 关于二维码识别的鸿蒙目前支持[recognition_qrcode](https://gitee.com/openharmony-sig/fluttertpc_recognition_qrcode) ,后续可以看看能不能替换
### 2、鸿蒙 Flutter三方库适配计划 未提到的 （大部份是纯dart的，应该没什么问题，少部份和原生有关，等后续测试发现问题）
#### get
#### flutter_screenutil
#### jh_debug
#### ana_page_loop
#### ota_update
#### retrofit
#### logger
#### get_it
#### json_annotation
#### injectable
#### flutter_smart_dialog
#### badges
#### wechat_assets_picker
#### left_scroll_actions
#### flutter_slidable
#### flutter_switch
#### chucker_flutter（抓包的，会导致在鸿蒙中dio请求成功后不走回调）
#### lpinyin
#### flutter_image_compress
#### flutter_boost
#### bubble_box
#### flutter_datetime_picker
#### google_mlkit_commons
#### google_mlkit_barcode_scanning
#### dotted_decoration
#### multiple_stream_builder
#### grouped_list
#### extended_text
### 3、鸿蒙已经支持的
#### [shared_preferences](https://gitee.com/openharmony-sig/flutter_packages)
#### [package_info_plus](https://gitee.com/openharmony-sig/flutter_plus_plugins) （目前用的是package_info）
#### [url_launcher](https://gitee.com/openharmony-sig/flutter_packages)
#### [permission_handler](https://gitee.com/openharmony-sig/flutter_permission_handler)
#### [fluttertoast](https://gitee.com/openharmony-sig/flutter_fluttertoast)
#### [path_provider](https://gitee.com/openharmony-sig/flutter_packages)
#### [flutter_keyboard_visibility](https://gitee.com/openharmony-sig/flutter_keyboard_visibility)
#### [image_picker](https://gitee.com/openharmony-sig/flutter_packages)
#### [share_plus](https://gitee.com/openharmony-sig/flutter_plus_plugins)
#### [webview_flutter](https://gitee.com/openharmony-sig/flutter_packages)
#### [camera](https://gitee.com/openharmony-sig/flutter_packages)
#### [image_gallery_saver](https://gitee.com/openharmony-sig/flutter_image_gallery_saver)
#### [audioplayers](https://gitee.com/openharmony-sig/flutter_audioplayers)
#### [device_info_plus](https://gitee.com/openharmony-sig/flutter_plus_plugins) （目前用的是device_info）
#### [flutter_inappwebview](https://gitee.com/openharmony-sig/flutter_inappwebview)
 
 
## 鸿蒙项目中模块管理
#### 背景：目前是将flutter module编译好har包，拷贝到鸿蒙项目中，然后添加依赖。
#### 问题：有没有类似iOS的cocoapods这样的管理工具？或者在鸿蒙项目中配置好flutter module 路径，就可以导入到项目中使用的方法。