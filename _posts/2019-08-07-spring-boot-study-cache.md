这是一篇总结文，针对前面三篇文章的总结
1. [Spring Boot 缓存应用 Ehcache 入门教程](https://www.cnblogs.com/fishpro/p/spring-boot-study-ehcache.html)
2. [Spring Boot 2.x 缓存应用 Redis注解与非注解方式入门教程](https://www.cnblogs.com/fishpro/p/spring-boot-study-redis.html)
3. [Spring Boot 缓存应用 Memcached 入门教程](https://www.cnblogs.com/fishpro/p/spring-boot-study-memcached.html)
   
其中第1、2两篇采用了注解方式，第3篇采用了非注解方式。因为 Spring Boot 对 Memcached 并不是很友好，要想 Memcached 也支持注解方式，对缓存的支持也并不在原生支持的缓存列表中。



Ehcache、Redis、Memcached 都是为了解决缓存问题而存在，都可以在自己擅长的场景做自己擅长的事情。我们章我们主要解决几个问题

1. 缓存工具接入 Spring Boot 的流程是什么
2. 他们各自在什么场景下使用
3. 


