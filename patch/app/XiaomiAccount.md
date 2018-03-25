## 小米帐号
APK位置： `/system/app/XiaomiAccount/XiaomiAccount.apk`

apktool命令： `apktool d -s *.apk`

### 删除米币中心、小米钱包、小米卡包入口
代码位置： `res/xml/account_settings_preference.xml`
```
# 删除以下代码：
<com.xiaomi.account.ui.AccountValuePreference android:title="@string/account_mibi_center" android:key="pref_mibi_center" />
<com.xiaomi.account.ui.AccountValuePreference android:title="@string/account_mipay_info" android:key="pref_mipay_info" />

<com.xiaomi.account.ui.AccountValuePreference android:title="@string/vip_account_settings" android:key="pref_vip_settings" />
```
