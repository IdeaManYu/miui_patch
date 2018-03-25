## 负一屏（信息助手）
APK位置： `/system/priv-app/PersonalAssistant/PersonalAssistant.apk`

apktool命令： `apktool d -r *.apk`

### 移除运动计步关联的小米VIP，使其点击无效
代码位置： `com/miui/home/launcher/assistant/ui/view/SportCardView.smali`
```
.method public onClick(Landroid/view/View;)V
# return null
```

### 移除快递物流信息详情页的菜鸟裹裹APP推广
代码位置： `com/miui/personalassistant/express/fragment/ExpressProgressFragment.smali`

修改前：
```
.method private showCainiaoBanner()V
    .locals 3

    .prologue
    const/4 v2, 0x0

    .line 232
    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mResult:Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment$ExpressProgressResult;

    iget-boolean v0, v0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment$ExpressProgressResult;->mHasItem:Z

    if-eqz v0, :cond_0

    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mActivity:Lcom/miui/personalassistant/express/activity/ExpressBaseActivity;

    invoke-static {v0}, Lcom/miui/personalassistant/express/util/CainiaoUtil;->getAuthorizedNumber(Landroid/content/Context;)Ljava/lang/String;

    move-result-object v0

    invoke-static {v0}, Landroid/text/TextUtils;->isEmpty(Ljava/lang/CharSequence;)Z

    move-result v0

    if-nez v0, :cond_0

    .line 233
    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mGoodsDetail:Landroid/widget/LinearLayout;

    invoke-virtual {v0, v2}, Landroid/widget/LinearLayout;->setVisibility(I)V

    .line 234
    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mExpressSummary:Landroid/widget/RelativeLayout;

    new-instance v1, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment$1;

    invoke-direct {v1, p0}, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment$1;-><init>(Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;)V

    # 点击事件，须删除相关代码段
    invoke-virtual {v0, v1}, Landroid/widget/RelativeLayout;->setOnClickListener(Landroid/view/View$OnClickListener;)V

    .line 242
    :cond_0
    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mBannerDivider:Landroid/widget/ImageView;

    if-eqz v0, :cond_1

    .line 243
    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mBannerDivider:Landroid/widget/ImageView;

    invoke-virtual {v0, v2}, Landroid/widget/ImageView;->setVisibility(I)V

    .line 245
    :cond_1
    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mCainiaoBanner:Landroid/widget/RelativeLayout;

    if-eqz v0, :cond_2

    .line 246
    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mCainiaoBanner:Landroid/widget/RelativeLayout;

    invoke-virtual {v0, v2}, Landroid/widget/RelativeLayout;->setVisibility(I)V

    .line 247
    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mCainiaoBanner:Landroid/widget/RelativeLayout;

    new-instance v1, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment$2;

    invoke-direct {v1, p0}, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment$2;-><init>(Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;)V

    # 点击事件，须删除相关代码段
    invoke-virtual {v0, v1}, Landroid/widget/RelativeLayout;->setOnClickListener(Landroid/view/View$OnClickListener;)V

    .line 254
    :cond_2
    return-void
.end method
```

修改后：
```
.method private showCainiaoBanner()V
    .locals 3

    .prologue
    const/16 v2, 0x8 # 传参给 setVisibility 方法，使布局不可见且不占用空间

    .line 232
    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mResult:Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment$ExpressProgressResult;

    iget-boolean v0, v0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment$ExpressProgressResult;->mHasItem:Z

    if-eqz v0, :cond_0

    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mActivity:Lcom/miui/personalassistant/express/activity/ExpressBaseActivity;

    invoke-static {v0}, Lcom/miui/personalassistant/express/util/CainiaoUtil;->getAuthorizedNumber(Landroid/content/Context;)Ljava/lang/String;

    move-result-object v0

    invoke-static {v0}, Landroid/text/TextUtils;->isEmpty(Ljava/lang/CharSequence;)Z

    move-result v0

    if-nez v0, :cond_0

    .line 233
    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mGoodsDetail:Landroid/widget/LinearLayout;

    invoke-virtual {v0, v2}, Landroid/widget/LinearLayout;->setVisibility(I)V

    .line 242
    :cond_0
    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mBannerDivider:Landroid/widget/ImageView;

    if-eqz v0, :cond_1

    .line 243
    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mBannerDivider:Landroid/widget/ImageView;

    invoke-virtual {v0, v2}, Landroid/widget/ImageView;->setVisibility(I)V

    .line 245
    :cond_1
    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mCainiaoBanner:Landroid/widget/RelativeLayout;

    if-eqz v0, :cond_2

    .line 246
    iget-object v0, p0, Lcom/miui/personalassistant/express/fragment/ExpressProgressFragment;->mCainiaoBanner:Landroid/widget/RelativeLayout;

    invoke-virtual {v0, v2}, Landroid/widget/RelativeLayout;->setVisibility(I)V

    .line 254
    :cond_2
    return-void
.end method
```

### 移除垃圾的保护机制（貌似是签名）
代码路径： `com/baidu/platform/comapi`
```
# 搜索 context must be an Application Context 所在的方法，return null
```

修改前：
```
.method public static a(Ljava/lang/String;Landroid/content/Context;)V
    .locals 3

    sget-boolean v0, Lcom/baidu/platform/comapi/b;->a:Z

    if-nez v0, :cond_4

    if-nez p1, :cond_0

    new-instance v0, Ljava/lang/IllegalArgumentException;

    const-string/jumbo v1, "context can not be null"

    invoke-direct {v0, v1}, Ljava/lang/IllegalArgumentException;-><init>(Ljava/lang/String;)V

    throw v0

    :cond_0
    instance-of v0, p1, Landroid/app/Application;

    if-eqz v0, :cond_5

    invoke-static {}, Lcom/baidu/platform/comapi/NativeLoader;->getInstance()Lcom/baidu/platform/comapi/NativeLoader;

    invoke-static {p1}, Lcom/baidu/platform/comapi/NativeLoader;->setContext(Landroid/content/Context;)V

    invoke-static {}, Lcom/baidu/platform/comapi/a;->a()Lcom/baidu/platform/comapi/a;

    move-result-object v0

    invoke-virtual {v0, p1}, Lcom/baidu/platform/comapi/a;->a(Landroid/content/Context;)V

    invoke-static {}, Lcom/baidu/platform/comapi/a;->a()Lcom/baidu/platform/comapi/a;

    move-result-object v0

    invoke-virtual {v0}, Lcom/baidu/platform/comapi/a;->c()Z

    check-cast p1, Landroid/app/Application;

    invoke-static {p1}, Lcom/baidu/mapapi/JNIInitializer;->setContext(Landroid/app/Application;)V

    if-eqz p0, :cond_3

    const-string/jumbo v0, ""

    invoke-virtual {p0, v0}, Ljava/lang/String;->equals(Ljava/lang/Object;)Z

    move-result v0

    if-nez v0, :cond_3

    :try_start_0
    new-instance v0, Ljava/io/File;

    new-instance v1, Ljava/lang/StringBuilder;

    invoke-direct {v1}, Ljava/lang/StringBuilder;-><init>()V

    invoke-virtual {v1, p0}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;

    move-result-object v1

    const-string/jumbo v2, "/test.0"

    invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;

    move-result-object v1

    invoke-virtual {v1}, Ljava/lang/StringBuilder;->toString()Ljava/lang/String;

    move-result-object v1

    invoke-direct {v0, v1}, Ljava/io/File;-><init>(Ljava/lang/String;)V

    invoke-virtual {v0}, Ljava/io/File;->exists()Z

    move-result v1

    if-eqz v1, :cond_1

    invoke-virtual {v0}, Ljava/io/File;->delete()Z

    :cond_1
    invoke-virtual {v0}, Ljava/io/File;->createNewFile()Z

    invoke-virtual {v0}, Ljava/io/File;->exists()Z

    move-result v1

    if-eqz v1, :cond_2

    invoke-virtual {v0}, Ljava/io/File;->delete()Z
    :try_end_0
    .catch Ljava/lang/Exception; {:try_start_0 .. :try_end_0} :catch_0

    :cond_2
    invoke-static {p0}, Lcom/baidu/mapapi/common/EnvironmentUtilities;->setSDCardPath(Ljava/lang/String;)V

    :cond_3
    const/4 v0, 0x1

    sput-boolean v0, Lcom/baidu/platform/comapi/b;->a:Z

    :cond_4
    return-void

    :cond_5
    new-instance v0, Ljava/lang/RuntimeException;

    const-string/jumbo v1, "context must be an Application Context"

    invoke-direct {v0, v1}, Ljava/lang/RuntimeException;-><init>(Ljava/lang/String;)V

    throw v0

    :catch_0
    move-exception v0

    invoke-virtual {v0}, Ljava/lang/Exception;->printStackTrace()V

    new-instance v0, Ljava/lang/IllegalArgumentException;

    const-string/jumbo v1, "provided sdcard path can not used."

    invoke-direct {v0, v1}, Ljava/lang/IllegalArgumentException;-><init>(Ljava/lang/String;)V

    throw v0
.end method
```

修改后：
```
.method public static a(Ljava/lang/String;Landroid/content/Context;)V
    .locals 0

    return-void
.end method
```
