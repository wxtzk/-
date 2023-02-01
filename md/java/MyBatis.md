[TOC]
# MyBatis简介
[MyBatis官方文档](https://mybatis.org/mybatis-3/zh/)

## Mybatis的特点
```
MyBatis 支持定制化 SQL、存储过程以及高级映射，可以在实体类和 SQL 语句之间建立映射关系，是一种半自动化的 ORM 实现。其封装性低于 Hibernate，但性能优秀、小巧、简单易学、应用广泛。
```
> ORM（Object Relational Mapping，对象关系映射）是一种数据持久化技术，它在对象模型和关系型数据库之间建立起对应关系，并且提供了一种机制，通过 JavaBean 对象去操作数据库表中的数据。
### 优点
* MyBatisMyBatis 是免费且开源的
* 与 JDBC 相比，减少了 50% 以上的代码量
* MyBatis 是最简单的持久化框架，小巧并且简单易学
* MyBatis 相当灵活，不会对应用程序或者数据库的现有设计强加任何影响，SQL 写在 XML 中，和程序逻辑代码分离，降低耦合度，便于同一管理和优化，提高了代码的可重用性
* 提供 XML 标签，支持编写动态 SQL 语句
* 提供映射标签，支持对象与数据库的 ORM 字段关系映射
### 缺点
* 编写 SQL 语句工作量较大，对开发人员编写 SQL 语句的功底有一定要求
* SQL 语句依赖于数据库，导致数据库移植性差，不能随意更换数据库
### 使用场景
```
MyBatis 专注于 SQL 本身，是一个足够灵活的 DAO 层解决方案。适用于性能要求高，且需求变化较多的项目，如互联网项目。
```
## 从JDBC到MyBatis
### 优化一： 连接获取和释放
* 通过数据库连接池来解决资源浪费问题
* 通过连接池可以反复使用已建立的连接，减少开启和关闭连接的时间
### 优化二：SQL统一存取
> JDBC

* 可读性很差，不利于维护以及做性能调优
* 改动Java代码需要重新编译、打包部署
* 不利于取出SQL在数据库客户端执行
> MyBatis

* 将这些SQL语句统一集中放到配置文件或者数据库里面
* 然后通过SQL语句的key值去获取对应的SQL语句
### 优化三：传入参数映射和动态SQL
* key-value的Map，解析的时候根据变量名的具体值来判断
* 动态SQL
### 优化四：结果映射和结果缓存
* 半自动ORM
* 根据ORM，能将具体的值赋值到对应对象数据结构上
* 进而考虑对SQL执行结果的缓存来提升性能
### 优化五：解决重复SQL语句问题
* SQL抽象
* SQL引用
## Mybatis下载
[`官方网站`](http://mybatis.org)
[`github地址`](https://github.com/mybatis/mybatis-3/releases)
> 使用Maven引入
```XML
<dependencies>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.5</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.49</version>
    </dependency>
</dependencies>
```
# MyBatis原理
> MyBatis有三个基本要素
* 核心接口和类
* MyBatis配置文件(mybatis-config.xml)
* SQL映射文件（mapper.xml)
## 核心接口和类
![接口和类之间的关系](C:/Users/lenovo/Desktop/courses/Java/MyBatis/工作原理.png)
### SqlSessionFactoryBuilder
> 重载方法其实只有如下三种
* build(InputStream inputStream, String environment, Properties properties)
* build(Reader reader, String environment, Properties properties)
* build(Configuration config)
> SqlSessionFactoryBuilder的生命周期和作用域
```
SqlSessionFactoryBuilder 的最大特点就是用过即丢。创建 SqlSessionFactory 对象之后，这个类就不存在了，因此 SqlSessionFactoryBuilder 的最佳范围就是存在于方法体内，也就是局部变量。
```
### SqlSessionFactory(最核心)
`SqlSessionFactory 是工厂接口而不是现实类，他的任务就是创建 SqlSession`
```java
//SqlSession 提供的 openSession() 方法来获取 SqlSession 实例
public interface SqlSessionFactory {
    SqlSession openSession();
    SqlSession openSession(boolean autoCommit);
    SqlSession openSession(Connection connection);
    SqlSession openSession(TransactionIsolationLevel level);
    SqlSession openSession(ExecutorType execType);
    SqlSession openSession(ExecutorType execType, boolean autoCommit);
    SqlSession openSession(ExecutorType execType, TransactionIsolationLevel level);
    SqlSession openSession(ExecutorType execType, Connection connection);
    Configuration getConfiguration();
}
```
`SqlSessionFactory的生命周期和作用域`
```
SqlSessionFactory 对象一旦创建，就会在整个应用程序过程中始终存在。没有理由去销毁或再创建它，并且在应用程序运行中也不建议多次创建 SqlSessionFactory。因此 SqlSessionFactory 的最佳作用域是 Application，即随着应用程序的生命周期一直存在。这种“存在于整个应用运行期间，并且只存在一个对象实例”的模式就是所谓的单例模式（指在运行期间有且仅有一个实例）
```
### SqlSession
`SqlSession 是用于执行持久化操作的对象，类似于 JDBC 中的 Connection`
```java
oid clearCache();
Configuration getConfiguration();
void rollback(boolean force);
void commit(boolean force);
int delete(String statement, Object parameter);
...
```
`SqlSession 的用途主要有两种`
1. 获取映射器。让映射器通过命名空间和方法名称找到对应的 SQL，并发送给数据库，执行后返回结果
2. 直接通过“命名空间（namespace）+SQL id”的方式执行 SQL，不需要获取映射器
`SqlSession生命周期和作用域`
```
SqlSession 对应一次数据库会话。由于数据库会话不是永久的，因此 SqlSession 的生命周期也不是永久的，每次访问数据库时都需要创建 SqlSession 对象。
```
## MyBatis配置文件

```XML
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration><!-- 配置 -->
    <properties /><!-- 属性 -->
    <settings /><!-- 设置 -->
    <typeAliases /><!-- 类型命名 -->
    <typeHandlers /><!-- 类型处理器 -->
    <objectFactory /><!-- 对象工厂 -->
    <plugins /><!-- 插件 -->
    <environments><!-- 配置环境 -->
        <environment><!-- 环境变量 -->
            <transactionManager /><!-- 事务管理器 -->
            <dataSource /><!-- 数据源 -->
        </environment>
    </environments>
    <databaseIdProvider /><!-- 数据库厂商标识 -->
    <mappers /><!-- 映射器 -->
</configuration>
```
### properties
> 指定文件

```XML
<properties resource="mybatisDemo/resources/database.properties"/>
```

> 直接配置

```XML
<properties>
    <property name="username" value="root"/>
    <property name="password" value="root"/>
</properties>
```
> 使用方式

```XML
<environments default="development">
    <environment id="development">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <property name="driver" value="${driver}"/>
            <property name="url" value="${url}"/>
            <property name="username" value="${username}"/>
            <property name="password" value="${password}"/>
        </dataSource>
    </environment>
</environments>
```
### settings
`settings 标签用于配置 MyBatis 的运行时行为，它能深刻的影响 MyBatis 的底层运行，一般不需要大量配置，大部分情况下使用其默认值即可`

> 示例
```XML
<settings>
    <setting name="cacheEnabled" value="true"/>
    <setting name="lazyLoadingEnabled" value="true"/>
    <setting name="multipleResultSetsEnabled" value="true"/>
    <setting name="useColumnLabel" value="true"/>
    <setting name="useGeneratedKeys" value="false"/>
    <setting name="autoMappingBehavior" value="PARTIAL"/>
    <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
    <setting name="defaultExecutorType" value="SIMPLE"/>
    <setting name="defaultStatementTimeout" value="25"/>
    <setting name="defaultFetchSize" value="100"/>
    <setting name="safeRowBoundsEnabled" value="false"/>
    <setting name="mapUnderscoreToCamelCase" value="false"/>
    <setting name="localCacheScope" value="SESSION"/>
    <setting name="jdbcTypeForNull" value="OTHER"/>
    <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
</settings>
```
### typeAliases
`为了不在任何地方都指定类的全限定名，我们可以使用 typeAliases 标签定义一个别名`

> 示例
```XML
<typeAliases>
    <typeAlias alias = "Student" type = "net.bianchengbang.po.Student"/>
</typeAliases>
```
```XML
<!-- 对同一个包下的多个类定义别名 -->
<typeAliases>
    <package name="net.biancheng.po"/>
</typeAliases>
```
### typeHandlers
```
在 typeHandler 中，分为 jdbcType 和 javaType，其中 jdbcType 用于定义数据库类型，而 javaType 用于定义 Java 类型，typeHandler 的作用就是承担 jdbcType 和 javaType 之间的相互转换
```
### environments
* 用来配置多套Mybatis的运行环境（示例如上）
* environment 是 environments 的子标签，用来配置 MyBatis 的一套运行环境
* environment 标签提供了两个子标签，即 transactionManager 和 dataSource
### transactionManager
> 支持两种事务管理器`JDBC` 和 `MANAGED`

* JDBC:应用程序服务器负责事务管理操作
* MANAGED:mybatis自身不会去实现事务管理，而是让程序的容器来实现
### dataSource
`配置连接数据库属性`

> dataSource 中的 type 属性用于指定数据源类型，有以下 3 种类型

1. UNPOOLED:UNPOOLED 没有数据库连接池，效率低下
2. POOLED:对于 POOLED 数据源类型，MyBatis 将维护一个数据库连接池
3. JNDI:对于 JNDI 的数据源类型，MyBatis 将从 JNDI 数据源中获取连接

### mappers
`用于指定 MyBatis SQL 映射文件的路径`

> 示例
```XML
<mappers>
    <mapper resource="net/biancheng/mapper/Student.xml"/>
</mappers>
```

## MyBatis SQL映射器
`映射器由Java接口和XML文件（或注解）组成`

> 主要作用

* 定义参数类型
* 配置缓存
* 提供SQL和动态SQL
* 定义查询结果和ORM

>映射器的两种实现方式

* XML方式（复杂项目）
* 注解方式（简单项目）
## MyBatis执行SQL的两种方式
1. 通过SqlSession对象直接发送 SQL
2. 通 SqlSession对象获 Mappe 接口，通过 Mappe 接口发送 SQL
### SqlSession发送SQL
`这是MyBatis前身iBatis所留下的方式`
> 示例
```Java
Website website = (Website)sqlSession.selectOne("net.biancheng.mapper.WebsiteMapper.getWebsite",1);
//String对象由一个命名空间加SQL id组合而成，用来准确定位了一条SQL
//Object对象为需要传递的参数，也就是查询条件
//而SqlSession发送SQL,需要一个SQL id去匹配SQL，比较晦涩难懂
```
### Mapper接口发送 SQL
> 示例
```Java
WebsiteMapper websiteMapper = sqlSession.getMapper(WebsiteMapper.class);
Website website = websiteMapper.getWebsite(1);
//使用Mapper接口编程可以消除SqlSession带来的功能性代码，提高可读性
//使用Mapper接口，是完全面向对象的语言，更能体现业务的逻辑
```
## 缓存机制（一般针对查询功能）
### MyBatis系统中默认定义了两级缓存（一级缓存、二级缓存）
* 默认情况下，只有一级缓存（SqlSession缓存、本地缓存）开启
* 二级缓存（namespace缓存、全局缓存）需要手动开启和配置，可以被所有 SqlSession 共享
* **一级缓存缓存的是 SQL 语句，二级缓存缓存的是结果对象**
* MyBatis定义了缓存接口Cache，可以通过实现Cache接口来自定义二级缓存
### 一级缓存
**在`参数`和` SQL` 完全一样时，使用同一个 SqlSession 对象调用同一个 mapper 的方法，执行一次 SQL**
> 缓存失效的四种情况

1. 不同的SqlSession对应不同的一级缓存
2. 同一个SqlSession但是参数不同
3. 同一个SqlSession两次查询期间执行了任何一次增删改操作
4. 同一个SqlSession两次查询期间手动清空了缓存
### 二级缓存
**二级缓存在 SqlSession 关闭或提交之后才会生效**
> 使用步骤
1. 全局配置文件中(mybatis-config.xml)开启二级缓存
```XML
<settings>
    <setting name="cacheEnabled" value="true" />
</settings>
```
2. mapper文件中配置二级缓存
```XML
<mapper namescape="net.biancheng.WebsiteMapper">
    <!-- cache配置 -->
    <cache
        eviction="FIFO"<!-- 缓存回收策略 -->
        flushInterval="60000" <!-- 刷新间隔时间 -->
        size="512"<!-- 引用数目，正整数，代表缓存最多可以存储多少个对象 -->
        readOnly="true" <!-- 只读 --> />
    ...
</mapper>
```
![缓存机制](C:/Users/lenovo/Desktop/courses/Java/MyBatis/缓存机制.png)
## 插件开发

# MyBatis使用
## 功能架构
![功能架构](C:/Users/lenovo/Desktop/courses/Java/MyBatis/功能架构.png)
## select
> 示例
```XML
<!-- SQL -->
<select id="selectWebsiteByMap" resultType="net.biancheng.po.Website" parameterType="map">
    SELECT id,NAME,url FROM website
    WHERE name LIKE CONCAT ('%',#{name},'%')
    AND url LIKE CONCAT ('%',#{url},'%')
</select>
```
### 传递多个参数
1. 使用Map传递参数
2. 使用@Param注解传递参数
3. 使用JavaBean传递参数

#### Map递参
```Java
//接口代码
public List<Website> selectWebsiteByMap(Map<String, String> params);
//测试代码
Map<String,String> paramsMap = new HashMap<String,String>();
paramsMap.put("name","编程");
paramsMap.put("url","biancheng");
websiteMapper.selectWebsiteByMap(paramsMap);
```
#### @Param递参
```Java
public List<Website> selectWebsiteByAn(@Param("name") String name, @Param("url") String url);
```
#### JavaBean递参
```Java
public List<Website> selectWebsiteByAn(Website website);
```
#### 三种传参方式的区别
1. 使用 Map 传参会导致可读性降低，在实际应用中我们应该果断废弃该方式
2. 使用 @Param 传参数受到参数个数的影响，当 参数个数小于5 时较为合适
3. 当参数个数大于 5 个时，建议使用 JavaBean 方式
## insert
> 示例
```XML
<insert id="addWebsite" parameterType="string">
    insert into website(name)
    values(#{name})
</insert>
```
### 主键（自动递增）回填
> 有时可能需要将这个刚刚生成的自增主键回填到请求对象中，供其他业务使用
```XML
<!-- 添加一条信息，成功后将主键值返回填给id(po的属性) -->
<insert id="addWebsite" parameterType="net.biancheng.po.Website" keyProperty="id" useGeneratedKeys="true">
    insert into Website (name,url) values(#{name},#{url})
</insert>
```
### 自定义主键
> 若数据库不支持自增主键或自增主键未开启，可以在mapper文件实现自增自定义主键
```XML
<insert id="insertWebsite" parameterType="net.biancheng.po.Website">
    <!-- 先使用selectKey标签定义主键，然后再定义SQL语句 -->
    <selectKey keyProperty="id" resultType="Integer" order="BEFORE">
        select if(max(id) is null,1,max(id)+1) as newId from Website
    </selectKey>
    insert into Website (id,name,url) values(#{id},#{name},#{url})
</insert>
```
## update
> 示例
```XML
<!--update 标签-->
<update id="updateWebsite" parameterType="string">
    update website set name = #{name}
</update>
```
## delete
> 示例
```XML
<delete id="deleteWebsite" parameterType="string">
    delete from website where name = #{name}
</delete>
```
## resultMap元素
`resultMap中最复杂的元素,主要用于解决实体类属性名与数据库表中字段名不一致的情况`

> 现有的 MyBatis 版本只支持 resultMap 查询，不支持更新或者保存，更不必说级联的更新、删除和修改
### resultMap元素的构成
```XML
<resultMap id="" type="">
    <constructor><!-- 类在实例化时用来注入结果到构造方法,当一个类没有无参数构造方法时使用-->
        <idArg/><!-- ID参数，结果为ID -->
        <arg/><!-- 注入到构造方法的一个普通结果 --> 
    </constructor>
    <id/><!-- 用于表示哪个列是主键 -->
    <result/><!-- 注入到字段或JavaBean属性的普通结果 -->
    <association property=""/><!-- 用于一对一关联 -->
    <collection property=""/><!-- 用于一对多、多对多关联 -->
    <discriminator javaType=""><!-- 使用结果值来决定使用哪个结果映射 -->
        <case value=""/><!-- 基于某些值的结果映射 -->
    </discriminator>
</resultMap>
```
### 结果集的两种存储方式
* 使用 Map 存储
* 使用 POJO 存储

>Map存储示例
```Java
public List<Map<String,Object>> selectAllWebsite();
```
```XML
<select id="selectAllWebsite" resultType="map">
    select * from website
</select>
```
>POJO存储示例
```Java
package net.biancheng.po;
import java.util.Date;
@Data
@NoArgsConstructor
public class Website {
    private int id;
    private String uname;
    private String url;
    private int age;
    private String country;
    private Date createtime;
}
```
```XML
<resultMap type="net.biancheng.po.Website" id="myResult">
    <id property="id" column="id" />
    <result property="uname" column="name" />
</resultMap>
<select id="selectAllWebsite" resultMap="myResult">
    select id,name,url from website
</select>
```
## MyBatis注解(三种类型)
1. SQL 语句映射
2. 结果集映射
3. 关系映射
### SQL 语句映射
1. @Insert：实现新增、插入功能
2. @Delete：实现删除功能
3. @Update：实现更新功能
4. @Select：实现查询功能
5. @SelectKey：插入后，获取id的值
6. @Param：映射多个参数
> 示例
```Java
@Insert("insert into user(id,name) values(#{id},#{name})")
public int insert(User user);

@Delete("delete from  user  where id =#{id}")
void deleteById(Integer id);

@Update("update user set name= #{name},sex = #{sex},age =#{age} where id = #{id}")
void updateUserById(User user);

@Select("Select * from user")
@Results({
    @Result(id = true, column = "id", property = "id"),
    @Result(column = "name", property = "name"),
    @Result(column = "sex", property = "sex"),
    @Result(column = "age", property = "age")
})
List<User> queryAllUser();

@Insert("insert into user(id,name) values(#{id},#{name})")
@SelectKey(statement = "select last_insert_id()", keyProperty = "id", keyColumn = "id", resultType = int,before = false)
public int insert(User user);

int saveUser(@Param(value="user") User user,@Param("name") String name,@Param("age") Int age);
```
### 结果集映射
1. @Result
2. @Results
3. @ResultMap
> 示例
```Java
@Select({"select id, name, class_id from student"})
@Results(id="studentMap", value={
    @Result(column="id", property="id", jdbcType=JdbcType.INTEGER, id=true),
    @Result(column="name", property="name", jdbcType=JdbcType.VARCHAR),
    @Result(column="class_id ", property="classId", jdbcType=JdbcType.INTEGER)
})
List<Student> selectAll();

@Select({"select id, name, class_id from student where id = #{id}"})
@ResultMap(value="studentMap")
Student selectById(Integer id);
```
### 关系映射
1. @one：用于一对一关系映射
2. @many：用于一对多关系映射
> 示例
```Java
@Select("select * from student") 
@Results({ 
    @Result(id=true,property="id",column="id"), 
    @Result(property="name",column="name"), 
    @Result(property="age",column="age"), 
    @Result(property="address",column="address_id",one=@One(select="net.biancheng.mapper.AddressMapper.getAddress")) 
}) 
public List<Student> getAllStudents();  

@Select("select * from t_class where id=#{id}") 
@Results({ 
    @Result(id=true,column="id",property="id"), 
    @Result(column="class_name",property="className"), 
    @Result(property="students", column="id", many=@Many(select="net.biancheng.mapper.StudentMapper.getStudentsByClassId")) 
    }) 
public Class getClass(int id); 
```
## MyBatis关联（级联）查询
### 一对一
####  <association>标签
* property：指定映射到实体类的对象属性
* column：指定表中对应的字段（即查询返回的列名）
* javaType：指定映射到实体对象属性的类型
* select：指定引入嵌套查询的子 SQL 语句，该属性用于关联映射中的嵌套查询

`两种查询方式：分步查询 单步查询 `

> 示例
#### 分步查询
```XML
<!--StudentMapper-->
<mapper namespace="net.biancheng.mapper.StudentMapper">
    <!-- 一对一根据id查询学生信息：级联查询的第一种方法（嵌套查询，执行两个SQL语句） -->
    <resultMap type="net.biancheng.po.Student" id="cardAndStu1">
        <id property="id" column="id" />
        <result property="name" column="name" />
        <result property="sex" column="sex" />
        <!-- 一对一级联查询 -->
        <association property="studentCard" column="cardId"
            javaType="net.biancheng.po.StudentCard"
            select="net.biancheng.mapper.StudentCardMapper.selectStuCardById" />
    </resultMap>
    <select id="selectStuById1" parameterType="Integer"
        resultMap="cardAndStu1">
        select * from student where id=#{id}
    </select>
</mapper>

<!--StudentCardMapper-->	
<mapper namespace="net.biancheng.mapper.StudentCardMapper">
    <select id="selectStuCardById"
        resultType="net.biancheng.po.StudentCard">
        SELECT * FROM studentCard WHERE id = #{id}
    </select>
</mapper>
```
#### 单步查询
```XML
<resultMap type="net.biancheng.po.Student" id="cardAndStu2">
    <id property="id" column="id" />
    <result property="name" column="name" />
    <result property="sex" column="sex" />
    <!-- 一对一级联查询 -->
    <association property="studentCard"
        javaType="net.biancheng.po.StudentCard">
        <id property="id" column="id" />
        <result property="studentId" column="studentId" />
    </association>
</resultMap>
<select id="selectStuById2" parameterType="Integer"
    resultMap="cardAndStu2">
    SELECT s.*,sc.studentId FROM student s,studentCard sc
    WHERE
    s.cardId = sc.id AND s.id=#{id}
</select>
```
> 测试代码
```Java
public class Test {
    public static void main(String[] args) throws IOException {
        InputStream config = Resources.getResourceAsStream("mybatis-config.xml");
        SqlSessionFactory ssf = new SqlSessionFactoryBuilder().build(config);
        SqlSession ss = ssf.openSession();
        //分步查询
        Student stu = ss.getMapper(StudentMapper.class).selectStuById1(2);
        //单步查询
        Student stu = ss.getMapper(StudentMapper.class).selectStuById2(2);
        System.out.println(stu);
    }
}
```
### 一对多
#### <collection>标签
* property：指定映射到实体类的对象属性
* column：指定表中对应的字段（即查询返回的列名
* javaType：指定映射到实体对象属性的类型
* ofType：javaType实体对象中元素的类型
* select：指定引入嵌套查询的子 SQL 语句，该属性用于关联映射中的嵌套查询

`两种查询方式：分步查询 单步查询 `

> 分步查询
```XML
<!-- UserMapper -->
<!-- 一对多 根据id查询用户及其关联的订单信息：级联查询的第一种方法（分步查询） -->
<resultMap type="net.biancheng.po.User" id="userAndOrder1">
    <id property="id" column="id" />
    <result property="name" column="name" />
    <result property="pwd" column="pwd" />
    <!-- 一对多级联查询，ofType表示集合中的元素类型，将id传递给selectOrderById -->
    <collection property="orderList"
        ofType="net.biancheng.po.Order" column="id"
        select="net.biancheng.mapper.OrderMapper.selectOrderById" />
</resultMap>
<select id="selectUserOrderById1" parameterType="Integer"
    resultMap="userAndOrder1">
    select * from user where id=#{id}
</select>

<!-- OrderMapper -->
<!-- 根据id查询订单信息 -->
<select id="selectOrderById" resultType="net.biancheng.po.Order"
    parameterType="Integer">
    SELECT * FROM `order` where userId=#{id}
</select>
```
> 单步查询
```XML
<!-- 一对多 根据id查询用户及其关联的订单信息：级联查询的第二种方法（单步查询） -->
<resultMap type="net.biancheng.po.User" id="userAndOrder2">
    <id property="id" column="id" />
    <result property="name" column="name" />
    <result property="pwd" column="pwd" />
    <!-- 一对多级联查询，ofType表示集合中的元素类型 -->
    <collection property="orderList"
        ofType="net.biancheng.po.Order">
        <id property="oId" column="oId" />
        <result property="ordernum" column="ordernum" />
    </collection>
</resultMap>
<select id="selectUserOrderById2" parameterType="Integer"
    resultMap="userAndOrder2">
    SELECT u.*,o.id as oId,o.ordernum FROM `user` u,`order` o
    WHERE
    u.id=o.`userId` AND u.id=#{id}
</select>
```
> 测试代码
```Java
public class Test {
    public static void main(String[] args) throws IOException {
        InputStream config = Resources.getResourceAsStream("mybatis-config.xml");
        SqlSessionFactory ssf = new SqlSessionFactoryBuilder().build(config);
        SqlSession ss = ssf.openSession();
        //分步查询
        User us = ss.getMapper(UserMapper.class).selectUserOrderById1(1);
        //单步查询
        User us = ss.getMapper(UserMapper.class).selectUserOrderById2(1);
        System.out.println(us);
    }
}
```
### 多对多
`转化为两个一对多`

## 动态SQL
### if
当判断条件为 `true` 时，才会执行所包含的 SQL 语句

> 示例

```XML
<select id="selectAllWebsite" resultMap="myResult">
    select id,name,url from website where 1=1
    <if test="name != null">
        AND name like #{name}
    </if>
    <if test="url!= null">
        AND url like #{url}
    </if>
</select>
```
### choose、when和otherwise
多分支判断， choose-when-otherwise类似于 Java 中的` switch-case-default`

> 示例
```XML
<select id="selectWebsite"
        parameterType="net.biancheng.po.Website"
        resultType="net.biancheng.po.Website">
        SELECT id,name,url,age,country
        FROM website WHERE 1=1
        <choose>
            <when test="name != null and name !=''">
                AND name LIKE CONCAT('%',#{name},'%')
            </when>
            <when test="url != null and url !=''">
                AND url LIKE CONCAT('%',#{url},'%')
            </when>
            <otherwise>
                AND age is not null
            </otherwise>
        </choose>
    </select>
```
### where
代替SQL里面的where关键字，它会将 where 后的第一个 SQL 条件语句的 AND 或者 OR 关键词去掉，常与if标签配合使用

> 示例
```XML
<select id="selectWebsite" resultType="net.biancheng.po.Website">
    select id,name,url from website
    <where>
        <if test="name != null">
            AND name like #{name}
        </if>
        <if test="url!= null">
            AND url like #{url}
        </if>
    </where>
</select>
```
### set
代替SQL里面的set关键字，自动剔除set语句中末尾多余的逗号，常与if标签配合使用

> 示例
```XML
 <update id="updateWebsite"
        parameterType="net.biancheng.po.Website">
        UPDATE website
        <set>
            <if test="name!=null">name=#{name},</if>
            <if test="url!=null">url=#{url},</if><!-- 该逗号会被剔除 -->
        </set>
        WHERE id=#{id}
    </update>
```
### foreach
对于一些 SQL 语句中含有` in` 条件，需要迭代条件`集合`来生成的情况，可以使用 foreach 来实现 SQL 条件的迭代

> 语法
```XML
<foreach item="item" index="index" collection="list|array|map key" open="(" separator="," close=")">
    参数值
</foreach>
```
foreach 标签主要有以下属性:
* item：表示集合中每一个元素进行迭代时的别名
* index：索引、下标
* open：表示该语句以什么开始
* separator：表示以什么符号作为分隔符
* close：表示该语句以什么结束

> 示例
```XML
<select id="selectWebsite"
    parameterType="net.biancheng.po.Website"
    resultType="net.biancheng.po.Website">
    SELECT id,name,url,age,country
    FROM website WHERE age in
    <foreach item="age" index="index" collection="list" open="("
        separator="," close=")">
        #{age}
    </foreach>
</select>
```
### bind
bind 标签可以自定义一个上下文变量来表示一个` OGNL 表达式`

> 示例
```XML
<select id="selectWebsite" resultType="net.biancheng.po.Website">
    <bind name="pattern_name" value="'%'+name+'%'" />
    <bind name="pattern_url" value="'%'+url+'%'" />
    SELECT id,name,url,age,country
    FROM website
    WHERE name like #{pattern_name}
    AND url like #{pattern_url}
</select>
```
### trim
trim标签可以通过添加和忽略前缀、后缀更加灵活的实现动态SQL

> 语法
```XML
<trim prefix="前缀" suffix="后缀" prefixOverrides="忽略前缀字符" suffixOverrides="忽略后缀字符">
    SQL语句
</trim>
```
> 示例
```XML
<select id="selectWebsite" resultType="net.biancheng.po.Website">
    SELECT id,name,url,age,country
    FROM website
    <trim prefix="where" prefixOverrides="and">
        <if test="name != null and name !=''">
            AND name LIKE CONCAT ('%',#{name},'%')
        </if>
        <if test="url!= null">
            AND url like concat ('%',#{url},'%')
        </if>
    </trim>
</select>
```
###  `<![CDATA[]]>`的使用
1. 在XML文档的解析过程中，首先查找元素的起始符，即字符"<"和字符"&"。字符"<"表示为新元素的开始，字符"&"表示为字符实体的开始。CDATA的作用是保护这些特殊字符（例如，小于号<等）不被解析
> 提示：严格地讲，在 XML 中仅有字符 "<"和"&" 是非法的。省略号、引号和大于号是合法的，但是把它们替换为实体引用是个好的习惯，从而避免歧义。
2. 使用<![CDATA[]]>来包含不被XML解析器解析的内容。但要注意的是：不允许嵌套使用；不能再包含"]]>"
3. 在使用MyBatis过程中，有时我们的SQL是写在XML 映射文件中，如果写的SQL中有一些特殊的字符的话，在解析XML文件的时候会被当做XML自身元素，但我们不希望如此操作，所以我们要使用<![CDATA[ ]]>来解决

```xml
<if test="begntime != null">
	and DATE_FORMAT(BEGNTIME, '%Y-%m-%d') <![CDATA[>=]]> DATE_FORMAT(#{begntime}, '%Y-%m-%d')
</if>
<if test="endtime != null">
	and DATE_FORMAT(ENDTIME, '%Y-%m-%d') <![CDATA[<=]]> DATE_FORMAT(#{endtime}, '%Y-%m-%d')
</if>
```
4. 转义字符
    如果不想使用<![CDATA[]]>，那么请使用转义字符，效果一样的：
| 转义字符 | 表示符号 |  含义  |
| :----: | :--: | :----: |
|` &lt;`   | <    | 小于   |
|` &gt; `  | >    | 大于   |
| `&amp;`  | &    | 和号   |
|` &apos;` | '    | 单引号 |
| `&quot;` | " | 双引号  |
5. 、<![CDATA[]]>和XML转义字符的区别

|          | 转义字符 |   <![CDATA[]]>   |
| :------: | :------: | :--------------: |
| 适用场景 | 均可适用 | 不能场景适用所有 |
|  可读性  |   更好   |        差        |
| 解析速度 |    慢    |        快        |

# 扩展
## 插件
## 批量操作
## 存储过程
## typeHandler处理枚举
# MyBatisPlus