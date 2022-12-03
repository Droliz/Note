## 使用

原本保存用户登录信息采用的cookie中，但是直接保存在cookie中会暴露用户信息，所以采用将`uid`（自定义）保存在cookie中，然后在后端用`session`保存键值对，通过`uid`找到对应的用户信息，但是当用户量过大，会导致内存泄漏，所有这里采用`redis`数据库解决，此数据库采用键值对形式存放，速度快

```js
const redis = require('redis') // 引入 redis  v4.5
  
const redisClient = redis.createClient() // 创建客户端
  
// 监听错误信息
redisClient.on('err', err => {
    console.error('redis client error: ', err)
})

  
// 连接
const con = redisClient.connect(6379, '127.0.0.1')
  
// 查询
con.then(() => {
    redisClient.get('mykey').then(res => {
        console.log(res)
        redisClient.quit()   // 退出，如果不退出，进程一直卡住
    })
})
```

## 封装

配置

```js
let REDIS_CONF

REDIS_CONF = {
	port: 6379,
	host: '127.0.0.1'
}
```

操作

```js

```