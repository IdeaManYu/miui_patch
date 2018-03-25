## 主题
APK位置： `/system/app/ThemeManager/ThemeManager.apk`

apktool命令： `apktool d -r *.apk`

### 删除首页banners广告位
代码路径： `com/android/thememanager`
```
.method public getAdMarker()I
# return false

.method private static isSupported
# return false
```
代码位置： `com/android/thememanager/view/AdBannerView.smali`
```
.method protected init()V
# return null
```
代码位置： `com/android/thememanager/v9/adapter/V9ElementAdapter.smali`
```
.method public onCreateViewHolder
# 在 res/values/public.xml 中找到 element_ad_banner 、element_icon_group 对应的 id
# 在当前路径搜索该id所在的地方，删除相关代码段，例如：
# 删除前：
:pswitch_3
new-instance v0, Lcom/android/thememanager/v9/d/q;

iget-object v2, p0, Lcom/android/thememanager/v9/a/d;->at:Lcom/android/thememanager/activity/m;

const v3, 0x7f030016

invoke-virtual {v1, v3, p1, v6}, Landroid/view/LayoutInflater;->inflate(ILandroid/view/ViewGroup;Z)Landroid/view/View;

move-result-object v3

invoke-direct {v0, v2, v3}, Lcom/android/thememanager/v9/d/q;-><init>(Landroid/app/Fragment;Landroid/view/View;)V

goto :goto_0
# 删除后：
:pswitch_3
goto :goto_0
```
代码位置： `com/android/thememanager/jni/DrmAgent.smali`
```
# 移除DRM保护
.method static constructor <clinit>()V
# 关键代码： "jni_resource_drm"
# return null
```
