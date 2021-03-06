# Android基础题-window

## 目录

1. Window的作用
2. Window的创建，更新和删除
3. Activity的Window创建过程

## Window的介绍

Window用于显示View和接收各种事件，Window有三种类型：应用Window(每个Activity对应一个Window)、子Window(不能单独存在，附属于特定Window)、系统window(Toast和状态栏)
Window分层级，应用Window在1-99、子Window在1000-1999、系统Window在2000-2999.WindowManager提供了增删改View三个功能。

Window是抽象类，实现类是PhoneWindow, 我们通过WindowManager来创建Window，而Window的具体实现是在WindowManagerService当中。
Android中所有的视图都是通过Window来实现的，无论是Activity，Dialog还是Toast，他们的视图都是附加在window上，由window来负责管理。

## Window的创建，更新和删除

有以下角色参与：

Window：抽象类，实现类是PhoneWindow
WindowManager： 通过WindowManager来操作Window的增删改
WindowManagerImpl: WindowManager的实现类
WindowManagerGlobal：WindowManagerImpl将真正的操作委托给WindowManagerGlobal来做。
ViewRootImpl：每一个Window对应着一个View和ViewRootImpl，Window通过ViewRootImpl来和View建立联系
WindowSession： ViewRootImpl通过WindowSession和WindowManagerService进行IPC通信
WindowManagerService：Window的具体实现是在WindowManagerService当中

**Window的添加**

1. Window的添加是通过WindowManager的addView的方法来实现的。WindowManager是一个接口，实现类是WindowManagerImpl，而WindowManagerImpl将具体逻辑委托给WindowManagerGlobal来执行。
2. WindowManagerGlobal会检查布局参数合法性并调整子Window的布局参数
3. 创建ViewRootImpl，并将View添加到列表中。
4. 通过ViewRootImpl来更新界面，通过WindowSession和WindowManagerService进行IPC通信，实现Window的添加

## Activity的Window创建过程

Activity的attach方法中，会创建Activity所属的Window对象并为其设置回调接口。具体过程是：

1. 如果没有DecorView，创建它
2. 将Acitivity的视图添加到DecorView的mContentParent中
3. 回调Activity的onContentChanged方法通知Activity视图已经发生改变



