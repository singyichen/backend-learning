---
title: PostgreSQL
description: 
published: true
date: 2023-08-09T02:11:21.117Z
tags: postgresql, db
editor: markdown
dateCreated: 2022-10-26T00:34:10.467Z
---

# PostgreSQL
- [ ] [PostgreSQL](https://www.postgresql.org/)
- [ ] [PostgreSQL 正體中文使用手冊](https://docs.postgresql.tw/)
- [ ] [PostgreSQL official documentation](https://www.postgresql.org/docs/current/)
- [ ] [PostgreSQL Tutorial](https://www.postgresqltutorial.com/)
- [ ] [Day 8：資料管理伺服器 (6) - 使用 PostgreSQL 資料庫儲存資料](https://ithelp.ithome.com.tw/articles/10234856)
- [ ] [PostgreSQL 安裝、備份、還原](https://jonny-huang.github.io/projects/05_postgresql_00/)
- [ ] [Prisma PostgreSQL](https://www.prisma.io/docs/concepts/database-connectors/postgresql)
- [ ] [PostgreSQL Trigger](https://ithelp.ithome.com.tw/articles/10227402)
- [ ] [PostgreSQL如何为主键创建自增序列(Sequences)](https://blog.csdn.net/timo1160139211/article/details/78191470?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-78191470-blog-99950883.pc_relevant_multi_platform_whitelistv3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-78191470-blog-99950883.pc_relevant_multi_platform_whitelistv3&utm_relevant_index=2)
- [ ] [Postgresql对不足位数的查询结果进行前后补0](https://blog.csdn.net/cao_mr/article/details/106617958)
- [ ] [Why is PostgreSQL voted as the most loved database by Stackoverflow 2022 Developer Survey?](https://blog.bytebytego.com/p/ep30-why-is-postgresql-the-most-loved?utm_source=profile&utm_medium=reader2)
- [ ] [Citus 12: Schema-based sharding for PostgreSQL](https://www.citusdata.com/blog/2023/07/18/citus-12-schema-based-sharding-for-postgres/)
- [ ] [關於Trigger的使用與設計 - Rubin](https://www.youtube.com/watch?v=KAdJwrOA1Ek&ab_channel=%E8%B3%87%E6%96%99%E5%BA%AB%E9%BB%91%E6%89%8B&loop=0)

#  為什麼 PostgreSQL 被評為最受歡迎的數據庫

![Why is PostgreSQL voted as the most loved database by developers.png](http://192.168.25.60:8000/files/file_storage/77497cd2.png)

## OLTP (Online Transaction Processing)
We can use PostgreSQL for CRUD (Create-Read-Update-Delete) operations.

## OLAP (Online Analytical Processing)
We can use PostgreSQL for analytical processing. PostgreSQL is based on HTAP (Hybrid transactional/analytical processing) architecture, so it can handle both OLTP and OLAP well.

## FDW (Foreign Data Wrapper)
A FDW is an extension available in PostgreSQL that allows us to access a table or schema in one database from another.

## Streaming
PipelineDB is a PostgreSQL extension for high-performance time-series aggregation, designed to power real-time reporting and analytics applications.

## Geospatial
PostGIS is a spatial database extender for PostgreSQL object-relational database. It adds support for geographic objects, allowing location queries to be run in SQL.

## Time Series
Timescale extends PostgreSQL for time series and analytics. For example, developers can combine relentless streams of financial and tick data with other business data to build new apps and uncover unique insights.

## Distributed Tables
CitusData scales Postgres by distributing data & queries. 




