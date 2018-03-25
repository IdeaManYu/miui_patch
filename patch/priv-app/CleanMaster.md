## 垃圾清理
APK位置： `/system/priv-app/CleanMaster/CleanMaster.apk`

apktool命令： `apktool d -r *.apk`

### 移除广告
代码位置： `com/miui/optimizecenter/result/DataModel.smali`
```
.method public static post
# 对诸如以下的代码执行修改：
.line 400
.local v2, "controlled":Z
const v6, 0x7f08002a    # preference_key_information_setting_wlan 对应的 id（在 res/values/public.xml 中查看，下同）

invoke-virtual {v0, v6}, Lcom/miui/optimizecenter/Application;->getString(I)Ljava/lang/String;

move-result-object v6

const/4 v7, 0x0

invoke-interface {v5, v6, v7}, Landroid/content/SharedPreferences;->getBoolean(Ljava/lang/String;Z)Z

move-result v4    # 修改为 sget-boolean v4, Lcom/winter/mysu;->TRUE:Z

.line 401
.local v4, "isOpen":Z
const v6, 0x7f08002b    # preference_key_information_setting_close 对应的 id

invoke-virtual {v0, v6}, Lcom/miui/optimizecenter/Application;->getString(I)Ljava/lang/String;

move-result-object v6

const/4 v7, 0x1

invoke-interface {v5, v6, v7}, Landroid/content/SharedPreferences;->getBoolean(Ljava/lang/String;Z)Z

move-result v1    # 修改为 sget-boolean v1, Lcom/winter/mysu;->FALSE:Z

# 实际上是操作设置中的关闭资讯推荐选项，这样处理可在移除广告的同时保留『微信专清』功能
# "controlled":Z --> TRUE，"isOpen":Z --> FALSE
```

### 移除信息流设置项
代码位置： `com/miui/optimizecenter/settings/SettingsActivity.smali`
```
.method public onCreate
# 在方法结束（return-void）前增加以下代码：
invoke-virtual {v4, v2}, Landroid/preference/PreferenceCategory;->removePreference(Landroid/preference/Preference;)Z

# 其中 v4, v2 由以下代码产生，v4 对应 Landroid/preference/PreferenceScreen , v2 对应 Landroid/preference/PreferenceCategory
.line 133
invoke-virtual {p0}, Lcom/miui/optimizecenter/settings/SettingsActivity;->getPreferenceScreen()Landroid/preference/PreferenceScreen;

move-result-object v4

.line 134
.local v4, "preferenceScreen":Landroid/preference/PreferenceScreen;
const v6, 0x7f080029    # 对应 preference_category_information_setting 的 id

invoke-virtual {p0, v6}, Lcom/miui/optimizecenter/settings/SettingsActivity;->getString(I)Ljava/lang/String;

move-result-object v6

invoke-virtual {p0, v6}, Lcom/miui/optimizecenter/settings/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

move-result-object v2

check-cast v2, Landroid/preference/PreferenceCategory;

.line 135
.local v2, "newsCategory":Landroid/preference/PreferenceCategory;
```
