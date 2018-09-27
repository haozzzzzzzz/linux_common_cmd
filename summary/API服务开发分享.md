# GO API服务开发分享



## 辅助工具

有点：统一标准、加速开发、降低学习成本、提高工作效率、便于维护。

包括开发工具、测试工具、部署工具



##配置管理

配置源：文件、其他服务提供配置。



## 版本管理

文档记录对原有的服务进行的更改以及更新顺序



##阶段管理



## 依赖包管理





## 程序错误处理

日志输出



## 监控点



## 服务划分

功能划分、服务演变。



## 开发流程

设置服务大体代码结构、组件->优先提供接口，接口提供假数据->实现接口，可暂时不加缓存->测试->优化sql查询->增加缓存->测试->增加监控->测试



## API结构管理

uri、path参数、query参数、post参数、请求头参数、API 返回码



## go routines应用

监控：

promethues监控

pprof工具



在go routine中使用xray，则不能直接用contenxt.BackgroundContext()。

xray context 要关闭，不然会segment的go routine会一直不关闭

BeginSegment的正确使用方式～



context的应用

