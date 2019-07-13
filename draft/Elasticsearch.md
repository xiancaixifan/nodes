# 1.什么是Elasticsearh es
> 分布式文档数据库
> 核心是索引,每个字段都可以被搜索读取
> 通常作为具有复杂搜索功能的核心

- 中文社区 https://ex.xiaoleilu.com/

# 1.1 应用场景
    - 搜索栏(百度,维基)
    - 新闻网站
    - 电商网站
    - github
    - 站内搜索


# 2. es的存储结构
    > JSON格式存储数据 

    > 关系型数据库: 数据库 => 表 => 行 => 列 
    > es : 索引(index) => 类型(type) => 文档(Document) => 字段(Fields) 

# 2.1. 测试用例
```
GET _search
{
  "query": {
    "match_all": {}
  }
}


# 创建索引 PUT
PUT /lyuweigh

# 查询索引
GET /lyuweigh

# 添加内容到索引, type = user id = 1 
PUT /lyuweigh/user/1
{
  "name":"lvheng123",
  "age":18,
  "sex":1
}
 
 # 查询具体的, id不存在 ,返回的内容中found字段为false
GET /lyuweigh/user/id


# 删除操作,可以从索引删除,也可以从类型删除
DELETE /lyuweigh/user/1
```

# 2.3 乐观锁进行版本控制
> version

- 乐观锁和悲观锁的区别
    - 悲观锁: 当数据即将发生冲突时,阻止一切不满足数据一致性的操作
    - 乐观锁: 但数据即将发生冲突时,进行一些操作,比如数据的版本控制,进行数据区分


# 端口号9002 和 9003 的区别

- 9300: ES节点之间的通讯使用 针对集群环境啊,互相通讯 走的是tcp协议
- 9200: ES节点和外部通讯使用的 正确使用es的方式,提供restful接口 走的是http协议,基于tcp通道
