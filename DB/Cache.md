---
title: Cache
description: 
published: true
date: 2023-08-28T00:06:39.653Z
tags: db, cache
editor: markdown
dateCreated: 2023-05-17T07:03:15.435Z
---

# Cache
- [ ] [什麼是快取 (Cache)？快取 (Cache) 的機制為何？](https://www.explainthis.io/zh-hant/interview-guides/backend/cache-mechanism)
- [ ] [Top caching strategies](https://blog.bytebytego.com/p/top-caching-strategies?utm_source=profile&utm_medium=reader2)
- [ ] [Cache miss attack](https://blog.bytebytego.com/p/cache-miss-attack?utm_source=profile&utm_medium=reader2)
- [ ] [Where do we cache data?](https://blog.bytebytego.com/p/ep-38-where-do-we-cache-data?utm_source=profile&utm_medium=reader2)
- [ ] [Things to consider when using cache](https://blog.bytebytego.com/p/ep46-step-by-step-guide-on-system?utm_source=profile&utm_medium=reader2)
- [ ] [A Crash Course in Caching - Part 1](https://blog.bytebytego.com/p/a-crash-course-in-caching-part-1?utm_source=profile&utm_medium=reader2)
- [ ] [A Crash Course in Caching - Part 2](https://blog.bytebytego.com/p/a-crash-course-in-caching-part-2?utm_source=profile&utm_medium=reader2)
- [ ] [Cache Systems Every Developer Should Know](https://www.youtube.com/watch?time_continue=98&v=dGAgxozNWFE&embeds_referring_euri=https%3A%2F%2Fblog.bytebytego.com%2F&feature=emb_title&ab_channel=ByteByteGo)
- [ ] [Caching in Node JS](https://medium.com/@adarsh_d/chingcaching-in-node-js-5caa040d0ad8)
- [ ] [Caching Strategies for Node.js: Boosting Performance with Node Cache, Redis, and Memcached](https://singh-sandeep.medium.com/caching-strategies-for-node-js-boosting-performance-with-node-cache-redis-and-memcached-cc148a978a32)
- [ ] [6-Caching Strategies to Remember while designing Cache System](https://javascript.plainenglish.io/6-caching-strategies-to-remember-while-designing-cache-system-da058a3757cf)
- [ ] [Deep Dive In Caching](https://vishalrana9915.medium.com/deep-dive-in-caching-9780bc55ea7)
- [ ] [System Design Basics: 5 Common Caching Misunderstandings Explained](https://medium.com/geekculture/system-design-basics-5-common-caching-misunderstandings-explained-2f19b1c88373)
## 什麼是緩存

![cache.png](http://192.168.25.60:8000/files/file_storage/72d1ff10.png)

## 可以在什麼地方緩存資料

![Where do we cache data.png](http://192.168.25.60:8000/files/file_storage/4ccf9db2.png)

## backend caching strategies

![backend caching strategies.png](http://192.168.25.60:8000/files/file_storage/1bdfe62f.gif)

## Things to consider when using cache

![Things to consider when using cache.png](http://192.168.25.60:8000/files/file_storage/958bbea8.png)

𝐒𝐮𝐢𝐭𝐚𝐛𝐥𝐞 𝐒𝐜𝐞𝐧𝐚𝐫𝐢𝐨𝐬:
- In-memory solution
- Read heavy system
- Data is not frequently updated

𝐂𝐚𝐜𝐡𝐢𝐧𝐠 𝐓𝐞𝐜𝐡𝐧𝐢𝐪𝐮𝐞𝐬:
- Cache aside
- Write-through
- Read-through
- Write-around
- Write-back

𝐂𝐚𝐜𝐡𝐞 𝐄𝐯𝐢𝐜𝐭𝐢𝐨𝐧 𝐀𝐥𝐠𝐨𝐫𝐢𝐭𝐡𝐦𝐬:
- Least Recently Used (LRU)
- Least Frequently Used (LFU)
- First-in First-out (FIFO)
- Random Replacement (RR)

𝐊𝐞𝐲 𝐌𝐞𝐭𝐫𝐢𝐜𝐬:
- Cache Hit Ratio
- Latency
- Throughput
- Invalidation Rate
- Memory Usage
- CPU usage
- Network usage

𝐎𝐭𝐡𝐞𝐫 𝐢𝐬𝐬𝐮𝐞𝐬:
- Thunder herd on cold start
- Time-to-live (TTL)