# [缓存的正确使用方式](https://www.cnblogs.com/baishuchao/p/9206521.html)

## 阅读目录(Content)
1. 引子
2. 先更新数据库，再更新缓存
3. 先删缓存，再更新数据库
4. 先更新数据库，再删缓存

---

### 引子

首先，缓存由于其适应高并发和高性能的特性，已经在项目中被广泛使用。在读取缓存方面，大家没啥疑问，都是按照下图的流程来进行业务操作。

![aaa](About%20Cache%20Update.1.png)