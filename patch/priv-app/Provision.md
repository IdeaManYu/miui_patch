## 开机引导
APK位置： `/system/priv-app/Provision/Provision.apk`

apktool命令： `apktool d *.apk`

### 移除不必要的设置引导杂项
代码位置： `com/android/provision/fragment/OtherSettingsFragment.smali`
```
# 移除锁屏画布（仅 MIUI8 适用）
.method private isSupportMifg()Z
# return false

.method public onCreate
# 关键代码：
button_upload_debug_log_key
button_auto_update_key
# 创建 removePreference 方法移除不必要的条目，例如：
iput-object v0, p0, Lcom/android/provision/fragment/OtherSettingsFragment;->mAutoUpdatePreference:Landroid/preference/CheckBoxPreference;

# 增加代码开始
invoke-virtual {p0}, Lcom/android/provision/fragment/OtherSettingsFragment;->getPreferenceScreen()Landroid/preference/PreferenceScreen;

move-result-object v0

iget-object v1, p0, Lcom/android/provision/fragment/OtherSettingsFragment;->mAutoUpdatePreference:Landroid/preference/CheckBoxPreference;

invoke-virtual {v0, v1}, Landroid/preference/PreferenceScreen;->removePreference(Landroid/preference/Preference;)Z
# 增加代码结束
# 注意这里申请了2个本地寄存器变量，函数起始位置的 .locals 1 应改为 .locals 2
```

### 默认关闭相关设置
代码位置： `res/xml/other_settings.xml`
```
# 修改以下4项：
button_user_experience_key
button_mifg_online_content
button_upload_debug_log_key
button_auto_update_key
# 的键值为false
```
