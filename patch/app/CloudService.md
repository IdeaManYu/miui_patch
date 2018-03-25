## 小米云服务
APK位置： `/system/app/CloudService/CloudService.apk`

apktool命令： `apktool d -r *.apk`

### 移除小米云会员推广 banners
代码位置： `com/miui/cloudservice/ui/MiCloudCategoryControler.smali`
```
.method private initBannerCategory()V
# 关键代码： "micloudBanner"
# 删除 addPreference 代码段，如：
iget-object v0, p0, Lcom/miui/cloudservice/ui/MiCloudCategoryControler;->mBannerCategory:Landroid/preference/PreferenceCategory;

iget-object v1, p0, Lcom/miui/cloudservice/ui/MiCloudCategoryControler;->mPreferenceBanner:Lcom/miui/cloudservice/ui/PreferenceBanner;

invoke-virtual {v0, v1}, Landroid/preference/PreferenceCategory;->addPreference(Landroid/preference/Preference;)Z
```
