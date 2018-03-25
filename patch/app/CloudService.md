## 小米云服务
APK位置： `/system/app/CloudService/CloudService.apk`

apktool命令： `apktool d -r *.apk`

### 移除小米云会员推广 banners
- 在 `com/miui/cloudservice/ui` 路径下搜索 `micloudMember` 找到类位置；
- 在该类中搜索代码 `micloudMember` 删除 `addPreference ` 对应的所有代码段，该方法有个布尔型函数的 if 跳转语句，使其直接跳转至 `return-void`；
- MIUI9 中小米云会员推广 banners 跟 `micloudMember` 处在同一方法，删除 `addPreference` 对应的代码段即可。

修改前：
```
.method private ky()V
    .locals 2

    .prologue
    .line 312
    iget-object v0, p0, Lcom/miui/cloudservice/ui/M;->gb:Lcom/miui/cloudservice/ui/z;

    const-string/jumbo v1, "micloudMember"

    invoke-virtual {v0, v1}, Lcom/miui/cloudservice/ui/z;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/PreferenceCategory;

    iput-object v0, p0, Lcom/miui/cloudservice/ui/M;->gc:Landroid/preference/PreferenceCategory;

    .line 314
    new-instance v0, Lcom/miui/cloudservice/ui/k;

    iget-object v1, p0, Lcom/miui/cloudservice/ui/M;->fV:Landroid/app/Activity;

    invoke-direct {v0, v1}, Lcom/miui/cloudservice/ui/k;-><init>(Landroid/content/Context;)V

    iput-object v0, p0, Lcom/miui/cloudservice/ui/M;->gd:Lcom/miui/cloudservice/ui/k;

    .line 315
    iget-object v0, p0, Lcom/miui/cloudservice/ui/M;->gd:Lcom/miui/cloudservice/ui/k;

    const/4 v1, 0x0

    invoke-virtual {v0, v1}, Lcom/miui/cloudservice/ui/k;->setOrder(I)V

    .line 316
    iget-object v0, p0, Lcom/miui/cloudservice/ui/M;->gc:Landroid/preference/PreferenceCategory;

    iget-object v1, p0, Lcom/miui/cloudservice/ui/M;->gd:Lcom/miui/cloudservice/ui/k;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceCategory;->addPreference(Landroid/preference/Preference;)Z

    .line 318
    invoke-direct {p0}, Lcom/miui/cloudservice/ui/M;->kA()Z

    move-result v0

    if-nez v0, :cond_0

    .line 319
    invoke-direct {p0}, Lcom/miui/cloudservice/ui/M;->kq()Lcom/miui/cloudservice/ui/Q;

    move-result-object v0

    iput-object v0, p0, Lcom/miui/cloudservice/ui/M;->gf:Lcom/miui/cloudservice/ui/Q;

    .line 320
    iget-object v0, p0, Lcom/miui/cloudservice/ui/M;->gf:Lcom/miui/cloudservice/ui/Q;

    if-eqz v0, :cond_0

    .line 321
    iget-object v0, p0, Lcom/miui/cloudservice/ui/M;->gc:Landroid/preference/PreferenceCategory;

    iget-object v1, p0, Lcom/miui/cloudservice/ui/M;->gf:Lcom/miui/cloudservice/ui/Q;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceCategory;->addPreference(Landroid/preference/Preference;)Z

    .line 311
    :cond_0
    return-void
.end method
```

修改后：
```
.method private ky()V
    .locals 2

    .prologue
    .line 312
    iget-object v0, p0, Lcom/miui/cloudservice/ui/M;->gb:Lcom/miui/cloudservice/ui/z;

    const-string/jumbo v1, "micloudMember"

    invoke-virtual {v0, v1}, Lcom/miui/cloudservice/ui/z;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/PreferenceCategory;

    iput-object v0, p0, Lcom/miui/cloudservice/ui/M;->gc:Landroid/preference/PreferenceCategory;

    .line 314
    new-instance v0, Lcom/miui/cloudservice/ui/k;

    iget-object v1, p0, Lcom/miui/cloudservice/ui/M;->fV:Landroid/app/Activity;

    invoke-direct {v0, v1}, Lcom/miui/cloudservice/ui/k;-><init>(Landroid/content/Context;)V

    iput-object v0, p0, Lcom/miui/cloudservice/ui/M;->gd:Lcom/miui/cloudservice/ui/k;

    .line 315
    iget-object v0, p0, Lcom/miui/cloudservice/ui/M;->gd:Lcom/miui/cloudservice/ui/k;

    const/4 v1, 0x0

    invoke-virtual {v0, v1}, Lcom/miui/cloudservice/ui/k;->setOrder(I)V

    .line 311
    return-void
.end method
```

### 移除小米云存储使用状态控件
```
# 在上面的类中搜索代码 cloudStorage 删除 addPreference 对应的代码段。
```

修改前：
```
.method private kz()V
    .locals 2

    .prologue
    .line 303
    iget-object v0, p0, Lcom/miui/cloudservice/ui/M;->gb:Lcom/miui/cloudservice/ui/z;

    const-string/jumbo v1, "cloudStorage"

    invoke-virtual {v0, v1}, Lcom/miui/cloudservice/ui/z;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/PreferenceCategory;

    iput-object v0, p0, Lcom/miui/cloudservice/ui/M;->fY:Landroid/preference/PreferenceCategory;

    .line 304
    new-instance v0, Lcom/miui/cloudservice/ui/P;

    iget-object v1, p0, Lcom/miui/cloudservice/ui/M;->fV:Landroid/app/Activity;

    invoke-direct {v0, v1}, Lcom/miui/cloudservice/ui/P;-><init>(Landroid/content/Context;)V

    iput-object v0, p0, Lcom/miui/cloudservice/ui/M;->gh:Lcom/miui/cloudservice/ui/P;

    .line 305
    new-instance v0, Lcom/miui/cloudservice/ui/U;

    iget-object v1, p0, Lcom/miui/cloudservice/ui/M;->fV:Landroid/app/Activity;

    invoke-direct {v0, v1}, Lcom/miui/cloudservice/ui/U;-><init>(Landroid/content/Context;)V

    iput-object v0, p0, Lcom/miui/cloudservice/ui/M;->ge:Lcom/miui/cloudservice/ui/U;

    .line 307
    iget-object v0, p0, Lcom/miui/cloudservice/ui/M;->fY:Landroid/preference/PreferenceCategory;

    iget-object v1, p0, Lcom/miui/cloudservice/ui/M;->ge:Lcom/miui/cloudservice/ui/U;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceCategory;->addPreference(Landroid/preference/Preference;)Z

    .line 308
    iget-object v0, p0, Lcom/miui/cloudservice/ui/M;->fY:Landroid/preference/PreferenceCategory;

    iget-object v1, p0, Lcom/miui/cloudservice/ui/M;->gh:Lcom/miui/cloudservice/ui/P;

    invoke-virtual {v0, v1}, Landroid/preference/PreferenceCategory;->addPreference(Landroid/preference/Preference;)Z

    .line 302
    return-void
.end method
```

修改后：
```
.method private kz()V
    .locals 2

    .prologue
    .line 303
    iget-object v0, p0, Lcom/miui/cloudservice/ui/M;->gb:Lcom/miui/cloudservice/ui/z;

    const-string/jumbo v1, "cloudStorage"

    invoke-virtual {v0, v1}, Lcom/miui/cloudservice/ui/z;->findPreference(Ljava/lang/CharSequence;)Landroid/preference/Preference;

    move-result-object v0

    check-cast v0, Landroid/preference/PreferenceCategory;

    iput-object v0, p0, Lcom/miui/cloudservice/ui/M;->fY:Landroid/preference/PreferenceCategory;

    .line 304
    new-instance v0, Lcom/miui/cloudservice/ui/P;

    iget-object v1, p0, Lcom/miui/cloudservice/ui/M;->fV:Landroid/app/Activity;

    invoke-direct {v0, v1}, Lcom/miui/cloudservice/ui/P;-><init>(Landroid/content/Context;)V

    iput-object v0, p0, Lcom/miui/cloudservice/ui/M;->gh:Lcom/miui/cloudservice/ui/P;

    .line 305
    new-instance v0, Lcom/miui/cloudservice/ui/U;

    iget-object v1, p0, Lcom/miui/cloudservice/ui/M;->fV:Landroid/app/Activity;

    invoke-direct {v0, v1}, Lcom/miui/cloudservice/ui/U;-><init>(Landroid/content/Context;)V

    iput-object v0, p0, Lcom/miui/cloudservice/ui/M;->ge:Lcom/miui/cloudservice/ui/U;

    .line 302
    return-void
.end method
```

### 屏蔽会员推广 banners 法2：反编译res修改layout布局
代码位置： `res/values/dimens.xml`
```
# 将 micloud_banner_height 的键值修改为 0.0dip
```
