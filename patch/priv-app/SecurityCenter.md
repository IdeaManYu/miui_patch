## 安全中心
APK位置： `/system/priv-app/SecurityCenter/SecurityCenter.apk`

apktool命令： `apktool d -r *.apk`

对 `IS_INTERNATIONAL_BUILD:Z` 统一处理的方法：将 `winter` 文件夹拷贝到 `smali/com` 目录下，将 `Lmiui/os/Build;->IS_INTERNATIONAL_BUILD:Z` 修改为 `Lcom/winter/mysu;->TRUE:Z`


### 移除安全扫描、游戏加速、应用锁、流量助手中的广告

反编译 res，在 `values/public.xml` 分别找到 `display_antivirus_Ads` 、`display_gamebooster_Ads` 、`display_gamebooster_xunyou` 、`overlay_config_datausage_purchase_enabled` （均为 bool 型）对应的 `id`，再在 `smali` 中查找其对应的布尔型函数，return false
```
# 同时在路径 com/miui/networkassistant 中搜索以下方法，return false
.method public isNATrafficPurchaseAvailable()Z
.method public isPurchaseTipsEnable()Z
.method public static isDataUsagePurchaseEnabled
```

### 移除安全月报
代码路径： `com/miui/monthreport`
```
# 搜索 Lmiui/os/Build;->IS_INTERNATIONAL_BUILD:Z 将其改成 Lcom/winter/mysu;->TRUE:Z

# 同时也将 smali 中其它匹配 monthreport 的 Lmiui/os/Build;->IS_INTERNATIONAL_BUILD:Z 改成 Lcom/winter/mysu;->TRUE:Z
# 一般在 com/miui/push 目录
```

### 移除网络助手的『流量购买』标签
代码位置： `com/miui/networkassistant/ui/NetworkAssistantActivity.smali`
```
.method private checkTrafficPurchaseAvaliable
# 或者 .method private checkTrafficPurchaseEnable
# 搜索 Lcom/miui/networkassistant/utils/DeviceUtil;->IS_INTERNATIONAL_BUILD:Z 将其改成 Lcom/winter/mysu;->TRUE:Z
```

### 移除网络诊断的一键WLAN测速功能
代码位置： `com/miui/networkassistant/ui/activity/NetworkDiagnosticsActivity.smali`
```
.method public onCreateOptionsMenu
# 搜索 Lmiui/os/Build;->IS_INTERNATIONAL_BUILD:Z 将其改成 Lcom/winter/mysu;->TRUE:Z
```

### 恢复应用权限监控&USB安装管理设置入口
代码位置： `com/miui/permcenter/MainAcitivty.smali`
```
.method protected onCreate
# 找到该方法中调用的布尔型函数()Z，在 com/miui/permcenter 路径查找对应方法，return ture
# 搜索代码 IS_STABLE_VERSION:Z 移除开发版自带的ROOT权限管理（对其 return true）
```

### 移除安全体检（立即优化）完成页的资讯推荐
代码路径： `com/miui/securityscan`
```
# 在该路径中查找：key_sc_setting_news_recommend ，此代码会在两个方法出现，对布尔值函数return false
# 等同于：反编译res，在 values/public.xml 找到 preference_key_information_setting_close 对应的id，再在 smali 中查找其对应的布尔值函数，return false
```

### 移除安全中心首页底部的推荐广告（会导致安全中心主界面云控方案下发失效，不建议执行此修改）
代码位置：com/miui/securityscan/MainActivity.smali
```
# 搜索 Lmiui/os/Build;->IS_INTERNATIONAL_BUILD:Z 将其改成 Lcom/winter/mysu;->TRUE:Z
```

修改后：
```
.method private Gq()Z
    .locals 2

    sget-boolean v0, Lcom/winter/mysu;->TRUE:Z

    if-nez v0, :cond_0

    const-string/jumbo v0, "zh_CN" # 关键词："zh_CN"

    invoke-static {}, Ljava/util/Locale;->getDefault()Ljava/util/Locale;

    move-result-object v1

    invoke-virtual {v1}, Ljava/util/Locale;->toString()Ljava/lang/String;

    move-result-object v1

    invoke-virtual {v0, v1}, Ljava/lang/String;->equals(Ljava/lang/Object;)Z

    move-result v0

    :goto_0
    return v0

    :cond_0
    const/4 v0, 0x0

    goto :goto_0
.end method
```

### 移除安全体检不必要的扫描检测项
代码路径： `com/miui/securityscan/model/manualitem` 、`com/miui/securityscan/model/system`
```
.method public scan()V
# return null
```

### 移除安全体检的“系统更新自动下载检测”
代码位置： `com/miui/securityscan/model/system/AutoDownloadModel.smali`
```
.method public getDesc()Ljava/lang/String;
# 修改为：
.method public getDesc()Ljava/lang/String;
    .locals 1

    const/4 v0, 0x0

    return-object v0
.end method
# 注：须同时对 .method public scan()V 方法 return null
```

### 移除安全中心设置的信息流设置项

代码位置： `com/miui/securityscan/ui/settings/SettingsActivity.smali`
```
.method protected onCreate
# 在方法结束处增加以下代码，其中 0x7f0704f4为 preference_category_information_setting 对应的 id（在 res/values/public.xml 中检索）
invoke-virtual {p0}, Lcom/miui/securityscan/ui/settings/SettingsActivity;->getPreferenceScreen()Landroid/preference/PreferenceScreen;

move-result-object v1

const v0, 0x7f0704f4

invoke-virtual {p0, v0}, Lcom/miui/securityscan/ui/settings/SettingsActivity;->getString(I)Ljava/lang/String;

move-result-object v0

invoke-virtual {p0, v0}, Lcom/miui/securityscan/ui/settings/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

move-result-object v0

check-cast v0, Landroid/preference/PreferenceCategory;

invoke-virtual {v1, v0}, Landroid/preference/PreferenceScreen;->removePreference(Landroid/preference/Preference;)Z
```

### 移除安全扫描的系统更新检测
代码路径： `com/miui/antivirus`
```
# 搜索代码 key_check_item_update，此代码会在两个方法出现，对布尔值函数 return false
# 并在 com/miui/antivirus/activity/SettingsActivity.smali 中查找该方法，在其下方创建removePreference 方法移除该设置项。
```

修改前：
```
.method protected onCreate(Landroid/os/Bundle;)V
    .locals 4

    const/4 v3, 0x0

    const/4 v2, 0x1

    invoke-super {p0, p1}, Lcom/miui/common/c/b;->onCreate(Landroid/os/Bundle;)V

    invoke-virtual {p0}, Lcom/miui/antivirus/activity/SettingsActivity;->getIntent()Landroid/content/Intent;

    move-result-object v0

    if-eqz v0, :cond_0

    const-string/jumbo v1, "extra_settings_title"

    invoke-virtual {v0, v1}, Landroid/content/Intent;->getStringExtra(Ljava/lang/String;)Ljava/lang/String;

    move-result-object v0

    invoke-static {v0}, Landroid/text/TextUtils;->isEmpty(Ljava/lang/CharSequence;)Z

    move-result v1

    if-nez v1, :cond_0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->setTitle(Ljava/lang/CharSequence;)V

    :cond_0
    const v0, 0x7f050029

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->addPreferencesFromResource(I)V

    invoke-static {p0}, Lcom/miui/antivirus/a;->getInstance(Landroid/content/Context;)Lcom/miui/antivirus/a;

    move-result-object v0

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mf:Lcom/miui/antivirus/a;

    invoke-static {p0}, Lcom/miui/antivirus/l;->getInstance(Landroid/content/Context;)Lcom/miui/antivirus/l;

    move-result-object v0

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mk:Lcom/miui/antivirus/l;

    invoke-static {p0}, Lcom/miui/guardprovider/a;->getInstance(Landroid/content/Context;)Lcom/miui/guardprovider/a;

    move-result-object v0

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Ml:Lcom/miui/guardprovider/a;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Ml:Lcom/miui/guardprovider/a;

    invoke-virtual {v0, v3}, Lcom/miui/guardprovider/a;->bhM(Lcom/miui/guardprovider/b;)V

    const v0, 0x7f07054f

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/PreferenceCategory;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Me:Landroid/preference/PreferenceCategory;

    const v0, 0x7f070557

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Lmiui/preference/ValuePreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mi:Lmiui/preference/ValuePreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mi:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, p0}, Lmiui/preference/ValuePreference;->setOnPreferenceClickListener(Landroid/preference/Preference$OnPreferenceClickListener;)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mi:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, v2}, Lmiui/preference/ValuePreference;->setShowRightArrow(Z)V

    const v0, 0x7f07055a

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v1

    invoke-virtual {p0, v1}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mj:Landroid/preference/CheckBoxPreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mj:Landroid/preference/CheckBoxPreference;

    invoke-static {v1, v2}, Lcom/miui/common/persistence/a;->atc(Ljava/lang/String;Z)Z

    move-result v1

    invoke-virtual {v0, v1}, Landroid/preference/CheckBoxPreference;->setChecked(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mj:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

    const v0, 0x7f070551

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mm:Landroid/preference/CheckBoxPreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mm:Landroid/preference/CheckBoxPreference;

    invoke-static {p0}, Lcom/miui/support/provider/f;->aZS(Landroid/content/Context;)Z

    move-result v1

    invoke-virtual {v0, v1}, Landroid/preference/CheckBoxPreference;->setChecked(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mm:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

    const v0, 0x7f070550

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mg:Landroid/preference/CheckBoxPreference;

    const v0, 0x7f070556

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Lmiui/preference/ValuePreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mn:Lmiui/preference/ValuePreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mn:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, p0}, Lmiui/preference/ValuePreference;->setOnPreferenceClickListener(Landroid/preference/Preference$OnPreferenceClickListener;)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mn:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, v2}, Lmiui/preference/ValuePreference;->setShowRightArrow(Z)V

    const v0, 0x7f070560

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/PreferenceCategory;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mo:Landroid/preference/PreferenceCategory;

    const v0, 0x7f070561

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mp:Landroid/preference/CheckBoxPreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mp:Landroid/preference/CheckBoxPreference;

    invoke-static {}, Lcom/miui/antivirus/i;->Xt()Z

    move-result v1

    invoke-virtual {v0, v1}, Landroid/preference/CheckBoxPreference;->setChecked(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mp:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

    const v0, 0x7f070562

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mr:Landroid/preference/CheckBoxPreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mr:Landroid/preference/CheckBoxPreference;

    invoke-static {}, Lcom/miui/antivirus/i;->Xz()Z

    move-result v1

    invoke-virtual {v0, v1}, Landroid/preference/CheckBoxPreference;->setChecked(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mr:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

    const v0, 0x7f070563

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/PreferenceCategory;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mh:Landroid/preference/PreferenceCategory;

    const v0, 0x7f070564

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mw:Landroid/preference/CheckBoxPreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mw:Landroid/preference/CheckBoxPreference;

    invoke-static {}, Lcom/miui/antivirus/i;->XA()Z

    move-result v1

    invoke-virtual {v0, v1}, Landroid/preference/CheckBoxPreference;->setChecked(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mw:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

    const v0, 0x7f070565

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mq:Landroid/preference/CheckBoxPreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mq:Landroid/preference/CheckBoxPreference;

    invoke-static {}, Lcom/miui/antivirus/i;->XB()Z

    move-result v1

    invoke-virtual {v0, v1}, Landroid/preference/CheckBoxPreference;->setChecked(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mq:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

    const v0, 0x7f070566

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mt:Landroid/preference/CheckBoxPreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mt:Landroid/preference/CheckBoxPreference;

    invoke-static {}, Lcom/miui/antivirus/i;->XC()Z

    move-result v1

    invoke-virtual {v0, v1}, Landroid/preference/CheckBoxPreference;->setChecked(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mt:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

    const v0, 0x7f07054e

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Lmiui/preference/ValuePreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mv:Lmiui/preference/ValuePreference;

    const v0, 0x7f070567

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Lmiui/preference/ValuePreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Ms:Lmiui/preference/ValuePreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mv:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, v2}, Lmiui/preference/ValuePreference;->setShowRightArrow(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Ms:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, v2}, Lmiui/preference/ValuePreference;->setShowRightArrow(Z)V

    invoke-direct {p0}, Lcom/miui/antivirus/activity/SettingsActivity;->TV()Ljava/util/List;

    move-result-object v0

    invoke-direct {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->TY(Ljava/util/List;)V

    invoke-static {}, Lcom/miui/securitycenter/c;->aEL()I

    move-result v0

    const/4 v1, 0x5

    if-ge v0, v1, :cond_3

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mh:Landroid/preference/PreferenceCategory;

    iget-object v1, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mv:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceCategory;->removePreference(Landroid/preference/Preference;)Z

    :goto_0
    sget-boolean v0, Lmiui/os/Build;->IS_INTERNATIONAL_BUILD:Z

    if-eqz v0, :cond_1

    invoke-virtual {p0}, Lcom/miui/antivirus/activity/SettingsActivity;->getPreferenceScreen()Landroid/preference/PreferenceScreen;

    move-result-object v0

    iget-object v1, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mo:Landroid/preference/PreferenceCategory;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceScreen;->removePreference(Landroid/preference/Preference;)Z

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mh:Landroid/preference/PreferenceCategory;

    iget-object v1, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mw:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceCategory;->removePreference(Landroid/preference/Preference;)Z

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mh:Landroid/preference/PreferenceCategory;

    iget-object v1, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Ms:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceCategory;->removePreference(Landroid/preference/Preference;)Z

    :cond_1
    sget-boolean v0, Lmiui/os/Build;->IS_ALPHA_BUILD:Z

    if-eqz v0, :cond_2

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mh:Landroid/preference/PreferenceCategory;

    iget-object v1, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mq:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceCategory;->removePreference(Landroid/preference/Preference;)Z

    :cond_2
    return-void

    :cond_3
    invoke-virtual {p0}, Lcom/miui/antivirus/activity/SettingsActivity;->getLoaderManager()Landroid/app/LoaderManager;

    move-result-object v0

    const/16 v1, 0x64

    invoke-virtual {v0, v1, v3, p0}, Landroid/app/LoaderManager;->initLoader(ILandroid/os/Bundle;Landroid/app/LoaderManager$LoaderCallbacks;)Landroid/content/Loader;

    goto :goto_0
.end method
```

修改后：
```
.method protected onCreate(Landroid/os/Bundle;)V
    .locals 4

    const/4 v3, 0x0

    const/4 v2, 0x1

    invoke-super {p0, p1}, Lcom/miui/common/c/b;->onCreate(Landroid/os/Bundle;)V

    invoke-virtual {p0}, Lcom/miui/antivirus/activity/SettingsActivity;->getIntent()Landroid/content/Intent;

    move-result-object v0

    if-eqz v0, :cond_0

    const-string/jumbo v1, "extra_settings_title"

    invoke-virtual {v0, v1}, Landroid/content/Intent;->getStringExtra(Ljava/lang/String;)Ljava/lang/String;

    move-result-object v0

    invoke-static {v0}, Landroid/text/TextUtils;->isEmpty(Ljava/lang/CharSequence;)Z

    move-result v1

    if-nez v1, :cond_0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->setTitle(Ljava/lang/CharSequence;)V

    :cond_0
    const v0, 0x7f050029

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->addPreferencesFromResource(I)V

    invoke-static {p0}, Lcom/miui/antivirus/a;->getInstance(Landroid/content/Context;)Lcom/miui/antivirus/a;

    move-result-object v0

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mf:Lcom/miui/antivirus/a;

    invoke-static {p0}, Lcom/miui/antivirus/l;->getInstance(Landroid/content/Context;)Lcom/miui/antivirus/l;

    move-result-object v0

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mk:Lcom/miui/antivirus/l;

    invoke-static {p0}, Lcom/miui/guardprovider/a;->getInstance(Landroid/content/Context;)Lcom/miui/guardprovider/a;

    move-result-object v0

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Ml:Lcom/miui/guardprovider/a;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Ml:Lcom/miui/guardprovider/a;

    invoke-virtual {v0, v3}, Lcom/miui/guardprovider/a;->bhM(Lcom/miui/guardprovider/b;)V

    const v0, 0x7f07054f

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/PreferenceCategory;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Me:Landroid/preference/PreferenceCategory;

    const v0, 0x7f070557

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Lmiui/preference/ValuePreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mi:Lmiui/preference/ValuePreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mi:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, p0}, Lmiui/preference/ValuePreference;->setOnPreferenceClickListener(Landroid/preference/Preference$OnPreferenceClickListener;)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mi:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, v2}, Lmiui/preference/ValuePreference;->setShowRightArrow(Z)V

    const v0, 0x7f07055a

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v1

    invoke-virtual {p0, v1}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mj:Landroid/preference/CheckBoxPreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mj:Landroid/preference/CheckBoxPreference;

    invoke-static {v1, v2}, Lcom/miui/common/persistence/a;->atc(Ljava/lang/String;Z)Z

    move-result v1

    invoke-virtual {v0, v1}, Landroid/preference/CheckBoxPreference;->setChecked(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mj:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

    const v0, 0x7f070551

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mm:Landroid/preference/CheckBoxPreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mm:Landroid/preference/CheckBoxPreference;

    invoke-static {p0}, Lcom/miui/support/provider/f;->aZS(Landroid/content/Context;)Z

    move-result v1

    invoke-virtual {v0, v1}, Landroid/preference/CheckBoxPreference;->setChecked(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mm:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

    const v0, 0x7f070550

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mg:Landroid/preference/CheckBoxPreference;

    const v0, 0x7f070556

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Lmiui/preference/ValuePreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mn:Lmiui/preference/ValuePreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mn:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, p0}, Lmiui/preference/ValuePreference;->setOnPreferenceClickListener(Landroid/preference/Preference$OnPreferenceClickListener;)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mn:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, v2}, Lmiui/preference/ValuePreference;->setShowRightArrow(Z)V

    const v0, 0x7f070560

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/PreferenceCategory;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mo:Landroid/preference/PreferenceCategory;

    const v0, 0x7f070561

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mp:Landroid/preference/CheckBoxPreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mp:Landroid/preference/CheckBoxPreference;

    invoke-static {}, Lcom/miui/antivirus/i;->Xt()Z

    move-result v1

    invoke-virtual {v0, v1}, Landroid/preference/CheckBoxPreference;->setChecked(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mp:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

    const v0, 0x7f070562

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mr:Landroid/preference/CheckBoxPreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mr:Landroid/preference/CheckBoxPreference;

    invoke-static {}, Lcom/miui/antivirus/i;->Xz()Z

    move-result v1

    invoke-virtual {v0, v1}, Landroid/preference/CheckBoxPreference;->setChecked(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mr:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

    const v0, 0x7f070563

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/PreferenceCategory;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mh:Landroid/preference/PreferenceCategory;

    const v0, 0x7f070564

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mw:Landroid/preference/CheckBoxPreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mw:Landroid/preference/CheckBoxPreference;

    invoke-static {}, Lcom/miui/antivirus/i;->XA()Z

    move-result v1

    invoke-virtual {v0, v1}, Landroid/preference/CheckBoxPreference;->setChecked(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mw:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

    const v0, 0x7f070565

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mq:Landroid/preference/CheckBoxPreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mq:Landroid/preference/CheckBoxPreference;

    invoke-static {}, Lcom/miui/antivirus/i;->XB()Z

    move-result v1

    invoke-virtual {v0, v1}, Landroid/preference/CheckBoxPreference;->setChecked(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mq:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

    const v0, 0x7f070566

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/CheckBoxPreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mt:Landroid/preference/CheckBoxPreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mt:Landroid/preference/CheckBoxPreference;

    # XC()Z 是 key_check_item_update 对应的布尔型函数
    invoke-static {}, Lcom/miui/antivirus/i;->XC()Z

    move-result v1

    invoke-virtual {v0, v1}, Landroid/preference/CheckBoxPreference;->setChecked(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mt:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, p0}, Landroid/preference/CheckBoxPreference;->setOnPreferenceChangeListener(Landroid/preference/Preference$OnPreferenceChangeListener;)V

    # 增加代码开始
    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mh:Landroid/preference/PreferenceCategory;

    iget-object v1, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mt:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceCategory;->removePreference(Landroid/preference/Preference;)Z
    # 增加代码结束

    const v0, 0x7f07054e

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Lmiui/preference/ValuePreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mv:Lmiui/preference/ValuePreference;

    const v0, 0x7f070567

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->getString(I)Ljava/lang/String;

    move-result-object v0

    invoke-virtual {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Lmiui/preference/ValuePreference;

    iput-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Ms:Lmiui/preference/ValuePreference;

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mv:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, v2}, Lmiui/preference/ValuePreference;->setShowRightArrow(Z)V

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Ms:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, v2}, Lmiui/preference/ValuePreference;->setShowRightArrow(Z)V

    invoke-direct {p0}, Lcom/miui/antivirus/activity/SettingsActivity;->TV()Ljava/util/List;

    move-result-object v0

    invoke-direct {p0, v0}, Lcom/miui/antivirus/activity/SettingsActivity;->TY(Ljava/util/List;)V

    invoke-static {}, Lcom/miui/securitycenter/c;->aEL()I

    move-result v0

    const/4 v1, 0x5

    if-ge v0, v1, :cond_3

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mh:Landroid/preference/PreferenceCategory;

    iget-object v1, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mv:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceCategory;->removePreference(Landroid/preference/Preference;)Z

    :goto_0
    sget-boolean v0, Lmiui/os/Build;->IS_INTERNATIONAL_BUILD:Z

    if-eqz v0, :cond_1

    invoke-virtual {p0}, Lcom/miui/antivirus/activity/SettingsActivity;->getPreferenceScreen()Landroid/preference/PreferenceScreen;

    move-result-object v0

    iget-object v1, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mo:Landroid/preference/PreferenceCategory;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceScreen;->removePreference(Landroid/preference/Preference;)Z

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mh:Landroid/preference/PreferenceCategory;

    iget-object v1, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mw:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceCategory;->removePreference(Landroid/preference/Preference;)Z

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mh:Landroid/preference/PreferenceCategory;

    iget-object v1, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Ms:Lmiui/preference/ValuePreference;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceCategory;->removePreference(Landroid/preference/Preference;)Z

    :cond_1
    sget-boolean v0, Lmiui/os/Build;->IS_ALPHA_BUILD:Z

    if-eqz v0, :cond_2

    iget-object v0, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mh:Landroid/preference/PreferenceCategory;

    iget-object v1, p0, Lcom/miui/antivirus/activity/SettingsActivity;->Mq:Landroid/preference/CheckBoxPreference;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceCategory;->removePreference(Landroid/preference/Preference;)Z

    :cond_2
    return-void

    :cond_3
    invoke-virtual {p0}, Lcom/miui/antivirus/activity/SettingsActivity;->getLoaderManager()Landroid/app/LoaderManager;

    move-result-object v0

    const/16 v1, 0x64

    invoke-virtual {v0, v1, v3, p0}, Landroid/app/LoaderManager;->initLoader(ILandroid/os/Bundle;Landroid/app/LoaderManager$LoaderCallbacks;)Landroid/content/Loader;

    goto :goto_0
.end method
```

### 移除应用管理的资源推荐，关闭应用升级提醒
代码路径/位置： `com/miui/appmanager/model` 、`com/miui/appmanager/AppManagerMainActivity.smali`
```
# 搜索 Lmiui/os/Build;->IS_INTERNATIONAL_BUILD:Z 将其改成 Lcom/winter/mysu;->TRUE:Z
# ps：在 AppManagerMainActivity.smali 中有个存在关键代码 com.google.gms 的方法跳过不用修改
```
代码路径： `com/miui/appmanager`
```
# 搜索
am_update_app_notify 
am_ads_enable
# 对应的布尔值函数，return false
```
