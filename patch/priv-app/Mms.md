## 短信
APK位置： `/system/priv-app/Mms/Mms.apk`

apktool命令： `apktool d -r *.apk`

### 移除广告
代码位置： `com/android/mms/util/SmartMessageUtils.smali`
```
.method public static disableAD()Z
# return true

# 移除底部菜单
.method public static allowMenuMode
# return false
```
**注**： V9.5及以上版本 `allowMenuMode` 方法在 `/system/app/SmsExtra/SmsExtra.apk`中，貌似因为资源保护的原因，反编译请使用 `APKDB` 进行仅反编译内部Dex文件（smali）操作，重新编译时使用 `7za.exe` 将 `classes.dex` 更新到apk，命令如下：
```
7za u -tzip SmsExtra.apk classes.dex
```
代码位置： `com/miui/smsextra/ui/BottomMenu.smali`
```
.method public static allowMenuMode
# return false
```
或者使用 `baksmali/smali`

反编译内部Dex命令：
```
java -jar baksmali.jar d SmsExtra.apk -o SmsExtra
```
编译Dex命令：
```
java -jar smali.jar a -o classes.dex SmsExtra
```
打包进APK：
```
7za u -tzip SmsExtra.apk classes.dex
```

