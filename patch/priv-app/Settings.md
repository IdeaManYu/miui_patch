## 设置
APK位置： `/system/priv-app/Settings/Settings.apk`

apktool命令： `apktool d *.apk`

### 优化恢复默认应用时浏览器显示包名 com.android.browser 的问题
代码位置： `com/android/settings/applications/PreferredListSettings$1.smali`
```
.method public onClick
# 搜索代码：com.android.browser
# 删除该行代码，同时删除下面invoke-virtual调用语句中包含该代码的变量，如v4
# 将 setDefaultBrowserPackageNameAsUser(Ljava/lang/String;I)Z 修改为 getDefaultBrowserPackageNameAsUser(I)Ljava/lang/String;
# 注：此处可能是 setDefaultBrowserPackageName(Ljava/lang/String;I)Z ，须修改为 getDefaultBrowserPackageName(I)Ljava/lang/String;
```

### 移除『我的设备』，恢复为『关于手机』
代码路径： `com/android/settings/device`
```
# 搜索代码 IS_GLOBAL_BUILD:Z ，该语句会在多个方法出现，选择例如下面的布尔值类型函数，并对该语句 return true (Lcom/winter/mysu;->TRUE:Z)
.method public static Ib(Landroid/content/Context;)Z
    .locals 1

    .prologue
    .line 231
    sget-boolean v0, Lmiui/os/Build;->IS_GLOBAL_BUILD:Z

    const/4 v0, 0x1

    if-eqz v0, :cond_0

    .line 232
    const/4 v0, 0x0

    return v0

    .line 234
    :cond_0
    invoke-static {}, Lcom/android/settings/device/a;->In()Z

    move-result v0

    return v0
.end method
```

### 移除『关于手机』的系统更新菜单
代码位置： `com/android/settings/device/MiuiDeviceInfoSettings.smali`
```
# MIUI8
.method public onCreateOptionsMenu
# return null

# MIUI9
.method public onResume()V
# 搜索代码 Lcom/android/settings/widget/CustomValuePreference 删除相关代码段，如：
.line 487
iget-object v0, p0, Lcom/android/settings/device/MiuiDeviceInfoSettings;->OO:Lcom/android/settings/widget/CustomValuePreference;

invoke-virtual {v0, v4}, Lcom/android/settings/widget/CustomValuePreference;->setShowRightArrow(Z)V

.line 488
iget-object v0, p0, Lcom/android/settings/device/MiuiDeviceInfoSettings;->OO:Lcom/android/settings/widget/CustomValuePreference;

iget-boolean v1, p0, Lcom/android/settings/device/MiuiDeviceInfoSettings;->OI:Z

invoke-virtual {v0, v1}, Lcom/android/settings/widget/CustomValuePreference;->setEnabled(Z)V

# 代码位置：res/xml/device_info_settings.xml
# 将以下代码：
<com.android.settings.widget.CustomValuePreference android:title="@string/miui_updater" android:key="miui_update">
    <intent android:targetPackage="com.android.settings" android:targetClass="com.android.settings.TranslationContributors" />
</com.android.settings.widget.CustomValuePreference>
<PreferenceCategory />
<miui.preference.ValuePreference android:persistent="false" android:title="@string/device_name" android:key="device_name" />
# 修改为：
<miui.preference.ValuePreference android:persistent="false" android:title="@string/device_name" android:key="device_name" />
<PreferenceCategory />
```

### 移除音效设置中的均衡器设置项
代码位置： `com/android/settings/HeadsetSettings.smali`
```
# 搜索代码 equalizer 找到其 Preference 的头部，然后在当前类中搜索该头部，将最后一处的removePreference 代码复制到其方法结束 return-void 的前面
```

### 恢复『更多应用』原来的展示样式
关键代码： `com.miui.appmanager.AppManagerMainActivity`

代码位置： `com/android/settings/applications/ApplicationsContainer.smali`
```
.method public onActivityCreated
# 搜索 Lmiui/os/Build;->IS_TABLET:Z 将其改成 Lcom/winter/mysu;->TRUE:Z
```
代码位置： `com/android/settings/applications/ManageApplicationsActivity.smali`
```
.method protected onCreate
# 搜索 Lmiui/os/Build;->IS_TABLET:Z 将其改成 Lcom/winter/mysu;->TRUE:Z
```
代码位置： `com/android/settings/search/tree/ApplicationsSettingsTree.smali`
```
.method public getIntent
# 搜索 Lmiui/os/Build;->IS_TABLET:Z 将其改成 Lcom/winter/mysu;->TRUE:Z
```
代码位置： `com/android/settings/MiuiSettings.smali`
```
.method public onBuildStartFragmentIntent
# 将以下代码：
const-string/jumbo v1, "com.miui.securitycenter"

const-string/jumbo v2, "com.miui.appmanager.AppManagerMainActivity"

invoke-virtual {v0, v1, v2}, Landroid/content/Intent;->setClassName(Ljava/lang/String;Ljava/lang/String;)Landroid/content/Intent;

# 修改为：
const-class v1, Lcom/android/settings/applications/ManageApplicationsActivity;

invoke-virtual {v0, p0, v1}, Landroid/content/Intent;->setClass(Landroid/content/Context;Ljava/lang/Class;)Landroid/content/Intent;
```

### 移除系统与安全中的广告设置项

代码位置： `res/xml/security_settings_debug_log.xml`
```
# 删除代码段：
<CheckBoxPreference android:persistent="false" android:title="@string/user_experience_program_title" android:key="user_experience_program" android:summary="@string/user_experience_program_summary" android:defaultValue="true" />

<Preference android:title="@string/ad_service" android:key="ad_control_settings">
	<intent android:targetPackage="com.android.settings" android:targetClass="com.android.settings.ad.AdServiceSettings" />
</Preference>

# 将代码段：
<CheckBoxPreference android:persistent="false" android:title="@string/upload_debug_log_title" android:key="upload_debug_log" android:summary="@string/upload_debug_log_summary" android:defaultValue="true" />
//的键值改为 false
```

### 关于手机显示贡献者信息
代码位置： `res/values/bools.xml`
```
# 将代码 has_translation_contributors 的键值改为 true

# 修改贡献者信息
# 代码位置：
res/raw/translation_contributors
res/raw-bo-rCN/translation_contributors
res/raw-ug-rCN/translation_contributors
# 同时修改这3个文件中的文本即可
```
