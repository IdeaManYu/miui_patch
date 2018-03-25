## 状态栏（系统用户界面）
APK位置： `/system/priv-app/MiuiSystemUI/MiuiSystemUI.apk`

apktool命令： `apktool d -s *.apk`

### 移除下拉搜索框
代码位置： `res/values/bools.xml`
```
# 将代码 config_show_statusbar_search 的键值改为 false
```

### 应用SPEC状态栏
代码位置： `res/values/dimens.xml`
```
# 将代码段：
<dimen name="single_page_toggles_offset">8.0dip</dimen>
<dimen name="single_page_toggles_space">8.0dip</dimen>
# 修改为：
<dimen name="single_page_toggles_offset">0.0dip</dimen>
<dimen name="single_page_toggles_space">2.0dip</dimen>
```

### 移除状态栏数字电量显示的百分号
代码位置： `res/values/strings.xml`
```
# 将代码 status_bar_settings_battery_meter_format 的键值由 %d%% 改为 %d
```

### 增加农历显示
代码位置： `res/values-zh-rCN/strings.xml`
```
# 将代码 status_bar_clock_date_format 的键值由『 M月d日 E 』改为『 M月d日 E N月e t 』
```

### 移除任务管理上方的音乐播放控件（MIUI9 已取消）
代码位置： `com/android/systemui/taskmanager/RecentsPanelMusicController.smali`
```
.method public isMusicActive()Z
# return false
```
