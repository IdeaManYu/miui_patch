## MIUI软件包安装程序
APK位置： `/system/priv-app/MiuiPackageInstaller/MiuiPackageInstaller.apk`

安卓L及以下版本： `/system/priv-app/PackageInstaller/PackageInstaller.apk`

apktool命令： `apktool d *.apk`

### 移除广告
代码位置： `com/android/packageinstaller/config/CommonConfig.smali`
```
.method public isAdsEnable()Z
# return false
```

### 移除『资源推荐』设置项
代码位置： `com/android/packageinstaller/SettingsActivity.smali`
```
.method protected onCreate
# 关键代码：pref_key_open_ads
# 删除该方法中所有存在 mOpenAds 的代码段
```
代码位置： `res/xml/settings.xml`
```
# 删除 <CheckBoxPreference android:title="@string/pref_ads" android:key="pref_key_open_ads" />
```
