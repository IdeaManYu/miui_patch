## 联系人
APK位置： `/system/priv-app/Contacts/Contacts.apk`

apktool命令： `apktool d -r *.apk`

### 移除拨号菜单的充话费、充流量、电话加油包按钮
代码位置： `com/android/contacts/dialer/DialerMenuDialog.smali`
```
代码位置： com/android/contacts/dialer/DialerMenuDialog.smali 定位代码： 在 res/values/public.xml 中分别找到 dialer_menu_recharge 、dialer_menu_mobiledata 、dialer_menu_livetalk 、dialer_menu_misim 、dialer_menu_ip 对应的id

.method private createMenuItems()V
# 利用 sget-boolean v0, Lcom/winter/mysu;->FALSE:Z 进行跳转，不创建相关菜单

# 移除小红点提示？
.method public static isShowDialerMenuRedDot
.method private static isShowMenuEntryRedDot
# return false
```

### 移除生活黄页的部分广告
代码位置： `com/android/contacts/yellowpage/adapter/YellowPageAdapter.smali`
```
.method public statAdView()V
# return null
```
代码位置： `com/android/contacts/yellowpage/ui/NavigationFragment.smali`
```
.method public static getIsAdOn()Z
# return false
```

### 移除标记为诈骗电话时出现的小米保险推广（MIUI9 已取消）
代码位置： `com/android/contacts/util/YellowPageProxy.smali`
```
.method public static showFraudInsurance
# return false
```

### 移除生活黄页的电话加油包组件 （新版 MIUI9 已取消）
代码位置： `com/android/contacts/yellowpage/util/LivetalkUtils.smali`
```
.method public static isSupportLivetalk
.method public static isLivetalkEnabled
.method public static isLivetalkSwitchOn
# return false
```

### 移除生活黄页TAB （仅适用 MIUI8）
代码位置： `com/android/contacts/util/YellowPageProxy.smali`
```
.method public static isYellowPageTabAvailable()Z
# 关联方法：.method private static isMainlandBuild()Z
# return false
```
