需求：tableview显示全部笔记标签和最多两行的笔记内容，笔记标签间距相等且最长为屏幕宽度，一行内多个标签放不下自动换行。

难点：每个tableview cell的高度不一致，需要动态更新

解决方案：tableViewCell嵌套collectionViewCell，collectionViewCell显示标签，collectionViewCell下方显示笔记内容。

具体实现：先重写collectionViewCell的tagFlowLayOut的系统布局方法，调整cell的间距，获取最后一个cell的最大y坐标，就是collectionView的最大高度。用委托的方式将最大y坐标传给嵌套collectionView的tableviewcell，在委托方法里实现tableview的高度更新。

目的：积累经验，学习

自我介绍：

面试官晚上好，我叫李远卓，今年25岁，就读于四川大学计算机学院软件工程专业，将于明年毕业，研究方向是计算机视觉的图像超分辨率。目前在墨墨背单词实习，岗位是IOS开发实习生。主要的工作内容是完成实习生的项目考核，现在完成了项目阶段一和阶段二，包括用户注册和登录，笔记列表的创建更新和搜索。我投递的岗位是美团的IOS开发，因为我对移动端开发比较感兴趣，希望以后能从事这份职业。以上就是我的自我介绍，谢谢。

反问：

1. 是什么部门，负责什么业务。
2. 工作中主要用到的技术。
3. 如何平衡工作和学习。
4. 对我有什么建议。

5. 对我项目的建议。



美团到店餐饮一面

- 自我介绍 再优化一下

- 项目介绍 最好背一下
  - 最难的点 怎样解决 答了tableview高度适应和搜索
  - 最大的收获
  - 如何和后端，产品沟通
  - UI控件 挑几个写再背一下
    - button textfied label textview collectionview tableview searchController scrollview alertController segmentcontroll
  - tableview优化 答了高度缓存 还有呢

- 操作系统 答得不好 重点背
  - 进程和线程
  - 进程的通信
  - app启动线程
  - 用过缓冲吗
  - 虚拟内存
  - 分页分段 碎片
  - 死锁
  
- 计算机网络
  - 七层模型详细 再背背
  - TCP UDP 再深入一下
  - 路由器和交换机
  - https请求过程 对称加密与非对称加密
  - 三次握手详细
  - http报文头详细 再背背
  - GET POST 和其他方法
  - HTTP编码是由什么决定的
  
- 语言 主要是C++
  - 结构体和类的区别
  - “ ”头文件和<>头文件
  - 顶层const和底层const,  char* chonst p, char * p const, 
  - 堆栈内存
  - OC机制了解吗 runtime runloop 不展开讲了
  - 循环引用
  
- 数据结构
  - 数组和链表 增删改查复杂度 应用场景	
  - 栈和队列 应用场景 答了navigationBar和单元格复用（再深入一下）
  - 二叉树 前中后遍历 二叉树在计算机是怎么存储的？
  - 哈希表 增删改查复杂度 原理 冲突 再复习一下
  
- 算法
  - 二叉树中序遍历 递归和非递归 为什么非递归可以用栈？
  - 判断链表的入环口 快指针为什么是两步？
  
- 反问 反问前先给场景和理由 针对面试官的角色提出有针对的问题
  - 什么部门，负责什么业务 到店餐饮 提供SDK
  - 如何平衡工作和学习
  - 对我的建议