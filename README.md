# android-useful-commands
安卓实用命令汇总

## adb 命令

### wm
获取/修改设备分辨率

```
usage: wm [subcommand] [options]
       wm size [reset|WxH]
       wm density [reset|DENSITY]
       wm overscan [reset|LEFT,TOP,RIGHT,BOTTOM]

wm size: return or override display size.

wm density: override display density.

wm overscan: set overscan area for display.
```

### uiautomator
dump ui树

```
Usage: uiautomator <subcommand> [options]

Available subcommands:

……（省略）

dump: creates an XML dump of current UI hierarchy
    dump [--verbose][file]
      [--compressed]: dumps compressed layout information.
      [file]: the location where the dumped XML should be stored, default is
      /storage/emulated/legacy/window_dump.xml

```

### dumpsys
获取top Activity

* dumpsys activity | grep Focus
* dumpsys window windows | grep Focus
* 安卓10：dumpsys activity activities | grep Resumed

### am
命令行启动应用

```
usage: am [subcommand] [options]
usage: am start [-D] [-W] [-P <FILE>] [--start-profiler <FILE>]
               [--R COUNT] [-S] [--opengl-trace]
               [--user <USER_ID> | current] <INTENT>
       ……（省略）
       am broadcast [--user <USER_ID> | all | current] <INTENT>

```
* am start [-D] [-W] pkg/main_activity
* am broadcast 

### pm
查看应用安装路径

```
usage: pm list packages [-f] [-d] [-e] [-s] [-3] [-i] [-u] [--user USER_ID] [FILTER]
       pm path PACKAGE
```
* pm path pkg

### input
模拟输入事件

```
Usage: input [<source>] <command> [<arg>...]

The sources are: 
      trackball
      joystick
      touchnavigation
      mouse
      keyboard
      gamepad
      touchpad
      dpad
      stylus
      touchscreen

The commands and default sources are:
      text <string> (Default: touchscreen)
      keyevent [--longpress] <key code number or name> ... (Default: keyboard)
      tap <x> <y> (Default: touchscreen)
      swipe <x1> <y1> <x2> <y2> [duration(ms)] (Default: touchscreen)
      press (Default: trackball)
      roll <dx> <dy> (Default: trackball)
```
* 模拟按键事件：input keyevent keyCode
* 模拟触屏事件：input tap x y

### getevent
获取系统输入事件

```
Usage: getevent [-t] [-n] [-s switchmask] [-S] [-v [mask]] [-d] [-p] [-i] [-l] [-q] [-c count] [-r] [device]
    -t: show time stamps
    -n: don't print newlines
    -s: print switch states for given bits
    -S: print all switch states
    -v: verbosity mask (errs=1, dev=2, name=4, info=8, vers=16, pos. events=32, props=64)
    -d: show HID descriptor, if available
    -p: show possible events (errs, dev, name, pos. events)
    -i: show all device info and possible events
    -l: label event types and names in plain text
    -q: quiet (clear verbosity mask)
    -c: print given number of events then exit
    -r: print rate events are received
```
* 获取输入设备支持哪些事件：getevent -p
* 打印事件：getevent [-l] [device]

### sendevent
```
use: sendevent device type code value
```

### screencap
截屏

```
usage: screencap [-hp] [-d display-id] [FILENAME]
   -h: this message
   -p: save the file as a png.
   -d: specify the display id to capture, default 0.
If FILENAME ends with .png it will be saved as a png.
If FILENAME is not given, the results will be printed to stdout.
```
* 截屏并将图片存储到sdcard：screencap -p /sdcard/tmp.png
* 将截屏导出到电脑：adb pull /sdcard/tmp.png ./


### screenrecord
录屏
```doc
Usage: screenrecord [options] <filename>

Android screenrecord v1.2.  Records the device's display to a .mp4 file.

Options:
--size WIDTHxHEIGHT
    Set the video size, e.g. "1280x720".  Default is the device's main
    display resolution (if supported), 1280x720 if not.  For best results,
    use a size supported by the AVC encoder.
--bit-rate RATE
    Set the video bit rate, in bits per second.  Value may be specified as
    bits or megabits, e.g. '4000000' is equivalent to '4M'.  Default 4Mbps.
--bugreport
    Add additional information, such as a timestamp overlay, that is helpful
    in videos captured to illustrate bugs.
--time-limit TIME
    Set the maximum recording time, in seconds.  Default / maximum is 180.
--verbose
    Display interesting information on stdout.
--help
    Show this message.

Recording continues until Ctrl-C is hit or the time limit is reached.

```

### reboot
重启或关机
```doc
usage: reboot [-p] [rebootcommand]
```

* 重启：reboot
* 重启至 recovery 模式：reboot recovery
* 重启至 bootloader 模式：reboot bootloader
* 关机：reboot -p



## 非adb命令

### aapt
获取包名和主界面，添加文件到apk中/删除apk中的文件

```
Android Asset Packaging Tool

Usage:
 aapt l[ist] [-v] [-a] file.{zip,jar,apk}
   List contents of Zip-compatible archive.

 aapt d[ump] [--values] [--include-meta-data] WHAT file.{apk} [asset [asset ...]]
   strings          Print the contents of the resource table string pool in the APK.
   badging          Print the label and icon for the app declared in APK.
   permissions      Print the permissions from the APK.
   resources        Print the resource table from the APK.
   configurations   Print the configurations in the APK.
   xmltree          Print the compiled xmls in the given assets.
   xmlstrings       Print the strings of the given compiled xml assets.
   
 aapt r[emove] [-v] file.{zip,jar,apk} file1 [file2 ...]
   Delete specified files from Zip-compatible archive.

 aapt a[dd] [-v] file.{zip,jar,apk} file1 [file2 ...]
   Add specified files to Zip-compatible archive.
```

* 列出apk的所有文件：aapt list file.apk
* 获取包名：aapt d badging file.apk | grep package
* 获取主界面：aapt d badging file.apk | grep launch
* 添加文件到apk：aapt a file.apk file1 [file2 ...]
* 删除apk中的文件：aapt r file.apk file1 [file2 ...]
