### ADB指令

ADB :android debug bridge

环境变量的配置：C:\kaifa\adt-bundle-windows-x86_64_20140101\sdk\platform-tools

***
#### 常用指令
    *adb devices :列出当前电脑所连接的android设备
    *adb push pc_path  phone_path :将电脑端文件放到手机端
    *adb pull phone_paht pc_path :将手机端文件拉到电脑端
    *adb install [-r] apkpath ; 安装一个电脑端的apk文件。-r：强制安装
    *adb uninstall packagename; 卸载一个应用
    *adb kill-server : 结束adb服务的链接
    *adb start-server ：开启adb服务的链接
    *netstat -oan 查看端口: 查看端口  
    *adb shell：进入当前设备linux环境下
    *adb shell + ls -l ：查看当前设备的目录结构
    *adb shell+ logcat :查看系统运行中的日志信息
    *adb shell+ df 查看系统盘符
    *adb shell+ pm list packages -f 输出已安装的应用
    *adb shell+ input keyevent 模拟按键输入
    *adb shell input touchscreen <x1><y1><x2><y2> 模拟滑动输入
    *adb shell am start -n 包名/包名+类名 启动一个activity
    *adb shell screenrecord /sdcard/demo.mp4 录制屏幕
    *adb reboot 重新启动

	注意： 如果当前电脑链接的是多台android设备，需要指定操作的是哪台设备，需要在adb后加 -s 设备序列号。
