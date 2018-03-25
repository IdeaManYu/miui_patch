## 传送门
APK位置： `/system/priv-app/ContentExtension/ContentExtension.apk`

apktool命令： `apktool d -r *.apk`

### 删除搜索点击事件，如果系统搜索程序替换成『搜索Lite』的话
代码位置： `com/miui/contentextension/text/TextSegmentScreen.smali`

修改前：
```
.method public onClick(Landroid/view/View;)V
    .locals 5
    .param p1, "v"    # Landroid/view/View;

    .prologue
    .line 69
    invoke-virtual {p1}, Landroid/view/View;->getId()I

    move-result v2

    packed-switch v2, :pswitch_data_0

    .line 68
    :goto_0
    return-void

    .line 71
    :pswitch_0
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mSegmentContainer:Lcom/miui/contentextension/text/TextSegmentContainer;

    invoke-virtual {v2}, Lcom/miui/contentextension/text/TextSegmentContainer;->getRequestString()Ljava/lang/String;

    move-result-object v0

    .line 72
    .local v0, "queryString":Ljava/lang/String;
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mContext:Landroid/content/Context;

    const/4 v3, 0x0

    invoke-static {v2, v3, v0}, Lcom/miui/contentextension/utils/Utilities;->searchPlainText(Landroid/content/Context;Landroid/content/ComponentName;Ljava/lang/String;)V

    .line 73
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mContext:Landroid/content/Context;

    iget-object v3, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mSegmentContainer:Lcom/miui/contentextension/text/TextSegmentContainer;

    invoke-virtual {v3}, Lcom/miui/contentextension/text/TextSegmentContainer;->getSeletedCount()I

    move-result v3

    invoke-static {v2, v3}, Lcom/miui/contentextension/utils/MiStatUtils;->recordWordChooseSearchClick(Landroid/content/Context;I)V

    .line 74
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mService:Lcom/miui/contentextension/services/TextContentExtensionService;

    invoke-virtual {v2}, Lcom/miui/contentextension/services/TextContentExtensionService;->cancelTask()V

    goto :goto_0

    .line 77
    .end local v0    # "queryString":Ljava/lang/String;
    :pswitch_1
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mSegmentContainer:Lcom/miui/contentextension/text/TextSegmentContainer;

    invoke-virtual {v2}, Lcom/miui/contentextension/text/TextSegmentContainer;->getRequestString()Ljava/lang/String;

    move-result-object v1

    .line 78
    .local v1, "segmentString":Ljava/lang/String;
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mContext:Landroid/content/Context;

    invoke-static {v2, v1}, Lcom/miui/contentextension/utils/Utilities;->copyToClipboard(Landroid/content/Context;Ljava/lang/String;)V

    .line 79
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mContext:Landroid/content/Context;

    iget-object v3, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mSegmentContainer:Lcom/miui/contentextension/text/TextSegmentContainer;

    invoke-virtual {v3}, Lcom/miui/contentextension/text/TextSegmentContainer;->getSeletedCount()I

    move-result v3

    invoke-static {v2, v3}, Lcom/miui/contentextension/utils/MiStatUtils;->recordWordChooseCopyClick(Landroid/content/Context;I)V

    .line 80
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mService:Lcom/miui/contentextension/services/TextContentExtensionService;

    invoke-virtual {v2}, Lcom/miui/contentextension/services/TextContentExtensionService;->cancelTask()V

    goto :goto_0

    .line 83
    .end local v1    # "segmentString":Ljava/lang/String;
    :pswitch_2
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mSegmentSelectOperation:Landroid/widget/TextView;

    invoke-virtual {v2}, Landroid/widget/TextView;->getText()Ljava/lang/CharSequence;

    move-result-object v2

    iget-object v3, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mContext:Landroid/content/Context;

    invoke-virtual {v3}, Landroid/content/Context;->getResources()Landroid/content/res/Resources;

    move-result-object v3

    sget v4, Lmiui/R$string;->select_all:I

    invoke-virtual {v3, v4}, Landroid/content/res/Resources;->getString(I)Ljava/lang/String;

    move-result-object v3

    invoke-virtual {v2, v3}, Ljava/lang/Object;->equals(Ljava/lang/Object;)Z

    move-result v2

    if-eqz v2, :cond_0

    .line 84
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mSegmentContainer:Lcom/miui/contentextension/text/TextSegmentContainer;

    invoke-virtual {v2}, Lcom/miui/contentextension/text/TextSegmentContainer;->selectAll()V

    .line 85
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mSegmentContainer:Lcom/miui/contentextension/text/TextSegmentContainer;

    invoke-virtual {v2}, Lcom/miui/contentextension/text/TextSegmentContainer;->getChildCount()I

    move-result v2

    invoke-virtual {p0, v2}, Lcom/miui/contentextension/text/TextSegmentScreen;->onSelectedCountChanged(I)V

    .line 86
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mContext:Landroid/content/Context;

    invoke-static {v2}, Lcom/miui/contentextension/utils/MiStatUtils;->recordWordChooseAll(Landroid/content/Context;)V

    goto :goto_0

    .line 88
    :cond_0
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mSegmentContainer:Lcom/miui/contentextension/text/TextSegmentContainer;

    invoke-virtual {v2}, Lcom/miui/contentextension/text/TextSegmentContainer;->unselectAll()V

    .line 89
    const/4 v2, 0x0

    invoke-virtual {p0, v2}, Lcom/miui/contentextension/text/TextSegmentScreen;->onSelectedCountChanged(I)V

    goto :goto_0

    .line 69
    nop

    :pswitch_data_0
    .packed-switch 0x7f0b002e
        :pswitch_0
        :pswitch_1
        :pswitch_2
    .end packed-switch
.end method
```

修改后：
```
.method public onClick(Landroid/view/View;)V
    .locals 5
    .param p1, "v"    # Landroid/view/View;

    .prologue
    .line 69
    invoke-virtual {p1}, Landroid/view/View;->getId()I

    move-result v2

    packed-switch v2, :pswitch_data_0

    .line 68
    :goto_0
    return-void

    .line 71
    :pswitch_0
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mSegmentContainer:Lcom/miui/contentextension/text/TextSegmentContainer;

    invoke-virtual {v2}, Lcom/miui/contentextension/text/TextSegmentContainer;->getRequestString()Ljava/lang/String;

    move-result-object v1

    .line 78
    .local v1, "segmentString":Ljava/lang/String;
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mContext:Landroid/content/Context;

    invoke-static {v2, v1}, Lcom/miui/contentextension/utils/Utilities;->copyToClipboard(Landroid/content/Context;Ljava/lang/String;)V

    .line 79
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mService:Lcom/miui/contentextension/services/TextContentExtensionService;

    invoke-virtual {v2}, Lcom/miui/contentextension/services/TextContentExtensionService;->cancelTask()V

    goto :goto_0

    .line 83
    .end local v1    # "segmentString":Ljava/lang/String;
    :pswitch_1
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mSegmentSelectOperation:Landroid/widget/TextView;

    invoke-virtual {v2}, Landroid/widget/TextView;->getText()Ljava/lang/CharSequence;

    move-result-object v2

    iget-object v3, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mContext:Landroid/content/Context;

    invoke-virtual {v3}, Landroid/content/Context;->getResources()Landroid/content/res/Resources;

    move-result-object v3

    sget v4, Lmiui/R$string;->select_all:I

    invoke-virtual {v3, v4}, Landroid/content/res/Resources;->getString(I)Ljava/lang/String;

    move-result-object v3

    invoke-virtual {v2, v3}, Ljava/lang/Object;->equals(Ljava/lang/Object;)Z

    move-result v2

    if-eqz v2, :cond_0

    .line 84
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mSegmentContainer:Lcom/miui/contentextension/text/TextSegmentContainer;

    invoke-virtual {v2}, Lcom/miui/contentextension/text/TextSegmentContainer;->selectAll()V

    .line 85
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mSegmentContainer:Lcom/miui/contentextension/text/TextSegmentContainer;

    invoke-virtual {v2}, Lcom/miui/contentextension/text/TextSegmentContainer;->getChildCount()I

    move-result v2

    invoke-virtual {p0, v2}, Lcom/miui/contentextension/text/TextSegmentScreen;->onSelectedCountChanged(I)V

    goto :goto_0

    .line 88
    :cond_0
    iget-object v2, p0, Lcom/miui/contentextension/text/TextSegmentScreen;->mSegmentContainer:Lcom/miui/contentextension/text/TextSegmentContainer;

    invoke-virtual {v2}, Lcom/miui/contentextension/text/TextSegmentContainer;->unselectAll()V

    .line 89
    const/4 v2, 0x0

    invoke-virtual {p0, v2}, Lcom/miui/contentextension/text/TextSegmentScreen;->onSelectedCountChanged(I)V

    goto :goto_0

    .line 69
    :pswitch_data_0
    # 注意此处 id 值的变更，0x7f0b002f 为 segment_copy 的 id 值
    # 在 res/values/public.xml 中查看
    .packed-switch 0x7f0b002f
        :pswitch_0
        :pswitch_1
    .end packed-switch
.end method
```
