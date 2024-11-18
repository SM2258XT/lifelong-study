---
title: Redisson基础入门
updated: 2022-10-27T19:02:36
created: 2022-10-27T15:07:26
---

**使用Redisson工具库入门**
Spring-Data-Redis只支持Jedis、Lectture；而redission需要使用redisson-spring-data（好像并不属于Spring范畴）

\<dependency\>
\<groupId\>org.redisson\</groupId\>
\<artifactId\>redisson\</artifactId\>
\<version\>3.11.2\</version\>
\</dependency\>

@Bean
public RedissonClient redissonClient(){
Config config = new Config();
// 可以用"rediss://"来启用SSL连接
config.useSingleServer().setAddress("redis://wrt.senao.ltd:6379");
return Redisson.create(config);
}

public void saleWithLock() {
RLock lock = redissonClient.getLock("salWithLock");
try {
lock.lock();
Stock stock = stockMapper.selectById(1);
stock.setCount(stock.getCount() - 1);
log.info("当前库存： " + stock.getCount());
stockMapper.updateById(stock);
} finally {
lock.unlock();
}
}

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
