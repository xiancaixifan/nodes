# Spring  Data JPA

# 一. 新特性

## 1.1 JPA `1.11`

- 优化了于Hibernate5.2的兼容性
- 支持更多的查询方法(Query By Examply)
- 分页查询执行优化
- Support for the `exists` projection in repository query derivation.

# 二. 引入依赖,配置数据源

```xml
<dependencies>
  <dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-jpa</artifactId>
  </dependency>
<dependencies>
```

- 自带JDBC

```java
   @Autowired
    private DataSource dataSource;


    server:
      port: 80

    spring:
      jpa:
        show-sql: true
        database: mysql
      datasource:
        username: root
        password: nbadmin
        driver-class-name: com.mysql.jdbc.Driver
        url: jdbc:mysql://127.0.0.1:3306/doit?characterEncoding=UTF-8&useSSL=false
```



# 三.SpringDataJpa的操作

- ` 抽象数据仓库`的核心是`Repository`接口,他将实体类和实体类的ID类型作为参数进行管理;

- 此接口主要用于捕获ApplicationContext中的符合要求的对象类型,标记当前接口作用,并且可扩展;
- CrudRepository接口为抽象数据仓库提供复杂的CRUD功能

## 2.1 CrudRepository 接口

```java
@NoRepositoryBean
public interface CrudRepository<T, ID> extends Repository<T, ID> {

	//保存给定的实体类对象
	<S extends T> S save(S entity);

	//保存给定的多有实体类对象
	<S extends T> Iterable<S> saveAll(Iterable<S> entities);

	//返回由指定id标识的实体对象
	Optional<T> findById(ID id);
	
    //表示指定id的对象是否存在
	boolean existsById(ID id);

	//返回当前表示实体对象的全部集合
	Iterable<T> findAll();

	//查询id集合返回的实体类型
	Iterable<T> findAllById(Iterable<ID> ids);

	//返回全部
	long count();

	//删除指定id的实体
    void deleteById(ID id);

	//删除指定符合要求的数据
	void delete(T entity);
    
	//删除全部符合要求的数据
	void deleteAll(Iterable<? extends T> entities);

	//删除全部
	void deleteAll();
}

```



## 2.2 分页接口实例

```java
public interface PagingAndSortingRepository<T, ID extends Serializable>
  extends CrudRepository<T, ID> {

  Iterable<T> findAll(Sort sort);

  Page<T> findAll(Pageable pageable);
}
```

1. 方法展示

```java

//分页 页码从0开始,这里表示访问第二页,20条数据
PagingAndSortingRepository<User, Long> repository = // … get access to a bean
Page<User> users = repository.findAll(PageRequest.of(1, 20));
```



## 2.3 CurdRespository接口扩展

```java
interface UserRepository extends CrudRepository<User, Long> {

  long countByLastname(String lastname);
}
```





