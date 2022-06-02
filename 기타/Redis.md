## What is in-memory?

```
In-memory databases are purpose-built databases that rely primarily on memory 
for data storage, in contrast to databases that store data on disk or SSDs.
In-memory data stores are designed to enable minimal response times by eliminating 
the need to access disks. Because all data is stored and managed exclusively 
in main memory, in-memory databases risk losing data upon a process or server failure. 
In-memory databases can persist data on
disks by storing each operation in a log or by taking snapshots.

In-memory databases are ideal for applications that require microsecond 
response times or have large spikes in traffic such as gaming leaderboards,
 session stores, and real-time analytics.

요약: 디스크에 데이터를 저장하는 것이 아니라 메모리에 저장해서 좀 더 빠르게 데이터를 가져올 수 있다.
```

## Redis

- 쓰이는 곳
  - Real-Tie biddign
  - Caching

```bash
# https://redis.io/
Redis is an open source (BSD licenㅎsed), in-memory data structure store, used as a database, cache, and message broker

Redis provides data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes, and streams.

 Redis has built-in replication, Lua scripting, LRU eviction, transactions, and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster
```

- 데이터베이스, 캐시 및 메시지 브로커로 사용되는 메모리 내 데이터 구조 저장소이다.
- 데이터 구조체를 제공한다. (위에 있는 내용들을)
- Redis는 복제, Lua스크립팅 ,LRU 제거
- 특징  
    - 다른 인메모리 디비들과의 다르게 다양한 자료구조를 지원한다.
    

- 파일설정- 1 (yml,build.gradle)

```
//build.gradle
implementation ("org.springframework.boot:spring-boot-starter-data-redis")


//yml 
spring:
  redis:
    host: localhost
    port: 6370
```

- 파일설정 - redisConfig파일
```
@Configuration
@EnableRedisRepositories
class RedisConfig {
    @Value("\${spring.redis.host}")
    private val redisHost: String? = null

    @Value("\${spring.redis.port}")
    private val redisPort = 0

    @Bean
    fun redisConnectionFactory(): RedisConnectionFactory? {
        println("redisPort = $redisPort")
        return LettuceConnectionFactory(redisHost!!, redisPort)
    }

    @Bean
    fun redisTemplate(): RedisTemplate<*, *>? {
        val redisTemplate = RedisTemplate<ByteArray, ByteArray>()
        redisTemplate.setConnectionFactory(redisConnectionFactory()!!)
        return redisTemplate
    }

}
```



