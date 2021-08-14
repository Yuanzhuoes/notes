# App生命周期

app从点击图标到运行：

- 未运行：应用程序未启动或者被终止
- 前台：WillEnterForeground
  - 待激活：应用程序在前台运行，但是不能接收事件。程序状态切换时，经常存在该状态。WillResignAcive
  - 激活：应用程序在前台运行，可以接收事件。DidBecomeActive
- 后台：应用程序在后台运行，正在执行代码. DidEnterBackground
- 挂起：应用程序在后台运行，不能执行代码
- 终止：内存不足，操作系统可能会把挂起的程序移除。WillTerminate

# Controller生命周期

- alloc() 创建对象，分配空间
- init() 初始化对象，初始化数据
- loadView() 完成一些关键View的初始化和加载
- viewDidLoad() 视图载入完成，可以自定义数据或创建其他控件
- viewWillAppear() 视图即将出现在屏幕
- viewWillLayoutSubview() 将要对子视图进行布局
- viewDidLayoutSubview() 完成对子视图的布局
- viewDidAppear() 视图已经在屏幕渲染完成

- viewWillDisappear() 视图即将从屏幕中被移除
- viewDidDisappear() 视图已经重屏幕中被移除
- dealloc 销毁视图，对init和viewDidLoad中创建的对象进行释放
- didReceiveMemoryWarning