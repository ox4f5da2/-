## CDN

### CDN是什么

CDN(Content Delivery Network)是指内容分发网络。CDN是构建在现有网络基础之上的智能虚拟网络，依靠部署在各地的**边缘服务器**，通过中心平台的负载均衡，内容分发，调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。

> 边缘服务器：接近用户的服务器
>
> CDN服务器简单来说就是帮服务器近距离给用户分发网页内容的



### 分发内容

网页内容分为：**静态内容**和**动态内容**

- 静态内容：网页上长期不需要改变的内容，比如某些图标和文字等
- 动态内容：有一部分是经常需要改变的，固定不了的，比如点赞数等

> 即使是静态内容，也不是一直保存在CDN里面的，源服务器发送文件给CDN的时候就可以利用HTTP头部的**cache-control**，这个头部可以设置文件的缓存形式，这样CDN就知道哪些需要保存，哪些不能以及哪些需要保存多久



### 分发流程

静态内容服务：

- CDN没有网站的源内容的，因此源服务器就会把静态内容**提前备份给CDN**（也叫push），这样在世界各地用户需要访问网页的时候就近的CDN服务器就会吧静态内容提供给用户
- 如果源服务器**没有把静态内容提前提前备份给CDN**， 那么当用户访问的时候，CDN就得去问源服务器索取相应的静态内容（也叫pull），源服务器可以让CDN备份，有了备份后的操作就如同第一个所述

动态内容服务：

- CDN服务器可以提供**时间服务**，万一源服务器网络不稳定，时间久没办法同步

> CDN的布局无形中给服务器增加一道墙



### 获取CDN资源服务器的IP

- 假如没有CDN，访问资源时会使用DNS进行解析获取资源服务器的IP地址进行数据交互。**传统模式下DNS的调度过程**可以参考[DNS域名解析过程](./DNS域名解析过程)。
- CDN是构建在承载网上的一个Cache应用层，也就是CDN作为用户和网站服务器之间的Cache来参与整个过程。其中一种常见的调度方案就是**DNS调度**，前半部分和传统模式类似，重要的区别在于专用DNS调度服务器的出现。

#### 专用CDN调度过程：

1. **网站去CDN服务商进行域名加速**，比如为源站[http://abc.com](https://link.zhihu.com/?target=http%3A//abc.com)到阿里云进行域名加速，配置完成后阿里云会自动关联生成加速域名的别名如[http://abc.com.aliyuncdn.net](https://link.zhihu.com/?target=http%3A//abc.com.aliyuncdn.net)，这个别名也称为CNAME。由于加速域名已经进行了CDN的**CNAME**配置，在**权威DNS服务器的解析下得到的并不是IP地址，而是CNAME**。
2. **权威DNS服务器的请求转发**，当用户访问[http://abc.com](https://link.zhihu.com/?target=http%3A//abc.com)时，传统的权威DNS服务器对[http://abc.com](https://link.zhihu.com/?target=http%3A//abc.com)进行解析时得到的是[http://abc.com.aliyuncdn.net](https://link.zhihu.com/?target=http%3A//abc.com.aliyuncdn.net)这个配置的CNAME，从而通过CNAME顺利将请求转到CDN服务商专用的DNS服务器，由该服务器返回CDN的资源节点。

![]( https://pic4.zhimg.com/80/v2-cc4ae0254317ae8fe6d07cd2f08a535f_1440w.jpg)

> - CNAME记录，也叫别名记录，比如[http://www.xx.com](https://link.zhihu.com/?target=http%3A//www.xx.com)的别名是[http://www.yy.com](https://link.zhihu.com/?target=http%3A//www.yy.com)，CNAME记录是一种指向关系，把[http://www.yy.com](https://link.zhihu.com/?target=http%3A//www.yy.com)指向了[http://www.xx.com](https://link.zhihu.com/?target=http%3A//www.xx.com)，一个域名可以有多个别名，存在多对一的关系。
> - A记录，即Address记录，我们可以把它理解为一种域名和IP地址的映射关系。
>
> ![](https://pic4.zhimg.com/80/v2-e3c900b743584145fd0b7c3ab8a9e96b_1440w.jpg)



### 安全性和可靠性

防止DDOS攻击：

布局多台CDN服务器在各个地方，然后监控这些CDN服务器的负载情况，如果某台服务器超载了就会把用户那边的请求转移到没有超载的CDN服务器那边，为的就是**平均分配网络流量（也叫负载均衡）**。

采用何种方式转移流量：

采用任播方式 ，用了这种方式以后，服务器对外都**拥有同一个IP**，如果这个IP收到用户请求，那么请求就会由距离用户的最近的服务器来响应，因此，利用任播的技术把流量转移到另外没有超载的服务器上就可以缓解了。