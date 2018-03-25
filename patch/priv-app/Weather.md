## 天气
APK位置： `/system/priv-app/Weather/Weather.apk`

apktool命令： `apktool d -r *.apk`

### 移除信息流设置项（MIUI9 新版已取消）
代码位置： `com/miui/weather2/ActivitySet.smali`
```
.method protected onCreate
# 在res/values/public.xml中找到 information_manager 对应的id并在当前方法搜索，删除所在的代码段，同时删除以下代码段：
iget-object v2, p0, Lcom/miui/weather2/ActivitySet;->mFooterInformationView:Landroid/view/View;

invoke-virtual {v1, v2}, Landroid/widget/ListView;->addFooterView(Landroid/view/View;)V
```

### 移除主界面的广告
代码路径： `com/miui/weather2/tools`
```
# 搜索代码 Lmiui/os/Build;->IS_TABLET:Z 找到函数名包含 (Landroid/content/Context;)Z 的方法
# return false （改成 Lcom/winter/mysu;->TRUE:Z ）
```

### 移除15天趋势预报的广告
代码位置： `com/miui/weather2/structures/DailyForecastAdData.smali`
```
.method public isAdInfosExistence()Z
.method public isAdTitleExistence()Z
.method public isLandingPageUrlExistence()Z
.method public isUseSystemBrowserExistence()Z
# return false

# 貌似新版以上方法已变更，如下：
.method public isAdInfosValid()Z
.method public isAdParamValid()Z
.method public isAdTitleValid()Z
.method public isLandingPageUrlValid()Z
.method public isUseSystemBrowserValid()Z
# return false
```

### 保留语音播报功能
代码位置： `com/miui/weather2/ActivityWeatherMain.smali`
```
# 搜索函数名包含 (Landroid/widget/ListView;Lcom/miui/weather2/structures/CityData;Z)V 的方法
# 在该方法中找到诸如以下代码：
.line 884
:cond_1
invoke-static {p0}, Lcom/miui/weather2/tools/bi;->Q(Landroid/content/Context;)Z

move-result v2

if-eqz v2, :cond_2

iget-object v2, p0, Lcom/miui/weather2/ActivityWeatherMain;->G:Lcom/miui/weather2/tools/ax;

invoke-virtual {v2}, Lcom/miui/weather2/tools/ax;->a()Ljava/lang/Boolean;

move-result-object v2

invoke-virtual {v2}, Ljava/lang/Boolean;->booleanValue()Z

move-result v2

if-eqz v2, :cond_2

.line 885
const v2, 0x7f090098    # 0x7f090098 为 menu_speeker 对应的 id （在res/values/public.xml中查看）

invoke-virtual {p0, v2}, Lcom/miui/weather2/ActivityWeatherMain;->getString(I)Ljava/lang/String;

move-result-object v2

invoke-virtual {v1, v2}, Ljava/util/ArrayList;->add(Ljava/lang/Object;)Z

# 将其中例如以下的代码：
invoke-static {p0}, Lcom/miui/weather2/tools/bi;->Q(Landroid/content/Context;)Z

move-result v2
# 修改为 sget-boolean v2, Lcom/winter/mysu;->TRUE:Z
```
