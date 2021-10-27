

https://www.cnblogs.com/jhxxb/p/11564843.html







https://www.jianshu.com/p/5c62b71fd882



`instrument`的底层实现依赖于`JVMTI`，也就是`JVM Tool Interface`，它是JVM暴露出来的一些供用户扩展的接口集合，JVMTI是基于事件驱动的，JVM每执行到一定的逻辑就会调用一些事件的回调接口（如果有的话），这些接口可以供开发者去扩展自己的逻辑。



作者：Java高级架构狮
链接：https://www.jianshu.com/p/5c62b71fd882
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。