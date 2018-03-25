## 日历
APK位置： `/system/priv-app/Calendar/Calendar.apk`

apktool命令： `apktool d -r *.apk`

### 禁用日历本地化功能
代码位置： `com/miui/calendar/util/LocalizationUtils.smali`
```
.method public static isLocalFeatureEnabled
# return false

# 在该类增加以下方法，目的是在某些地方启用日历本地化功能
.method public static setTrue(Landroid/content/Context;)Z
    .locals 1
	
	const/4 v0, 0x1

    return v0
.end method
```

### 保留短信事件导入日历入口
代码位置： `com/android/calendar/settings/FeatureSettingActivity.smali`
```
.method protected onCreate
# 将代码 isLocalFeatureEnabled 修改为 setTrue

# 在上面的 onCreate 方法结束处增加以下语句，移除功能设置的『显示节日』卡片设置项
iget-object v1, p0, Lcom/android/calendar/settings/FeatureSettingActivity;->mHolidayCheckBox:Landroid/preference/CheckBoxPreference;

invoke-virtual {v0, v1}, Landroid/preference/PreferenceScreen;->removePreference(Landroid/preference/Preference;)Z
```

### 移除节日卡片
代码位置： `com/miui/calendar/card/single/local/HolidaySingleCard.smali`
```
.method public needDisplay()Z
# return false
```

### 保留短信事件导入日历功能
代码位置： `com/miui/calendar/sms/SmsReceiver.smali`
```
.method public onReceive
# 将代码 isLocalFeatureEnabled 修改为 setTrue
```
