## 主题
APK位置： `/system/app/ThemeManager/ThemeManager.apk`

apktool命令： `apktool d -r *.apk`

**注**： V9.5及以上版本已加入混淆，须根据代码特征修改

### 删除首页banners广告位
代码路径： `com/android/thememanager`
```
.method public getAdMarker()I
# return false

# 在上面方法对应的类中有一个布尔型函数，通过 ->getImgUrls()Ljava/util/List; 定位
# return false
# 函数原型： .method private static isSupported
```
代码路径： `com/android/thememanager/view`
```
# 在 res/values/public.xml 中找到 ad_banner_view 对应的 id
# 在当前路径搜索该id所在的方法，并在当前类搜索调用该方法的函数，return null
```
代码路径： `com/android/thememanager/v9`
```
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
