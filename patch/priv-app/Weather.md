## 天气
APK位置： `/system/priv-app/Weather/Weather.apk`

apktool命令： `apktool d -r *.apk`

### 移除信息流设置项
代码位置： `com/miui/weather2/ActivitySet.smali`
```
.method protected onCreate
# 在res/values/public.xml中找到 information_manager 对应的id并在当前方法搜索，删除所在的代码段，同时删除以下代码段：
iget-object v2, p0, Lcom/miui/weather2/ActivitySet;->mFooterInformationView:Landroid/view/View;

invoke-virtual {v1, v2}, Landroid/widget/ListView;->addFooterView(Landroid/view/View;)V
```

### 移除主界面的广告
代码位置： `com/miui/weather2/tools/ToolUtils.smali`
```
.method public static canRequestCommercial
.method public static canRequestCommercialInfo
# return false
```

### 移除15天趋势预报的广告
代码位置： `com/miui/weather2/structures/DailyForecastAdData.smali`
```
.method public isAdInfosExistence()Z
.method public isAdTitleExistence()Z
.method public isLandingPageUrlExistence()Z
.method public isUseSystemBrowserExistence()Z
# return false
```

### 保留语音播报功能
代码位置： `com/miui/weather2/ActivityWeatherMain.smali`
```
.method private setMenuListViewAdapter
# 将代码段：
invoke-static {p0}, Lcom/miui/weather2/tools/ToolUtils;->canRequestCommercialInfo(Landroid/content/Context;)Z

move-result v0
# 修改为 sget-boolean v0, Lcom/winter/mysu;->TRUE:Z
```
