## 下载管理
APK位置： `/system/priv-app/DownloadProviderUi/DownloadProviderUi.apk`

apktool命令： `apktool d -r *.apk`

### 移除下载热榜
代码位置： `com/android/providers/downloads/ui/config/AppConfig.smali`
```
.method public static isShowRank()Z
# return false
```

### 移除资源推荐
代码位置： `com/android/providers/downloads/ui/utils/PreferenceUtil.smali`
```
.method public isShowInfoFlow()Z
.method public isShowRecommend()Z
.method public isShowVideoEnter()Z
# return false
```

### 移除信息流设置项
代码位置： `com/android/providers/downloads/ui/fragment/DownloadSettingFragment.smali`
```
.method private initInfoFlowPref()V
# 搜索代码 isCmTestBuilder()Z 并对其 return true，调用 removePreference 方法
```

修改后：
```
.method private initInfoFlowPref()V
    .locals 2

    .prologue
    .line 266
    const-string/jumbo v0, "pref_info_flow"

    invoke-virtual {p0, v0}, Lcom/android/providers/downloads/ui/fragment/DownloadSettingFragment;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    iput-object v0, p0, Lcom/android/providers/downloads/ui/fragment/DownloadSettingFragment;->mInfoFlowPref:Landroid/preference/Preference;

    .line 273
    invoke-virtual {p0}, Lcom/android/providers/downloads/ui/fragment/DownloadSettingFragment;->getPreferenceScreen()Landroid/preference/PreferenceScreen;

    move-result-object v0

    iget-object v1, p0, Lcom/android/providers/downloads/ui/fragment/DownloadSettingFragment;->mInfoFlowPref:Landroid/preference/Preference;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceScreen;->removePreference(Landroid/preference/Preference;)Z

    .line 265
    return-void
.end method
```

### 移除WLAN测速入口
代码位置： `com/android/providers/downloads/ui/DownloadList.smali`
```
.method public onOptionsItemSelected
.method public onPrepareOptionsMenu
# 反编译 res，在 values/public.xml 找到 speed_entrance 对应的id，并在这两个方法中删除与之有关的代码段
```
