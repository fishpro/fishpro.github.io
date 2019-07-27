---
layout: post
title: Spring Boot Mybatis 使用教程
categories: SpringBoot
description: Spring Boot Mybatis 使用教程
keywords: Mybatis,SpringBoot
---

Mybatis 在当下互联网开发环境，十分重要。本章主要讲述 Mybatis 如何使用。

从本系列开始，都需要用到 mysql 数据库 和其他一些参考的数据库。请准备相关环节。本章需要以下环境支撑：

- mysql 5.6+
- jdk1.8+
- spring boot 2.1.6
- idea 2018.1

[本项目源码下载](https://github.com/fishpro/spring-boot-study/tree/master/spring-boot-study-mybatis)


# 1 准备数据库
数据库教程系列都是使用相同的数据，如在 [Spring Boot JDBC 使用教程]()使用的一样

|字段|类型|主键|说明|
|--|--|--|---|
|id|int|是|自动编号|
|user_name|varchar(100)|否|用户名|
|password|varchar(255)|否|密码|
|last_login_time|date|否|最近登录时间|
|sex|tinyint|否|性别 0男 1女 2其他|
```sql

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for t_user
-- ----------------------------
DROP TABLE IF EXISTS `t_user`;
CREATE TABLE `t_user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_name` varchar(255) DEFAULT NULL,
  `password` varchar(255) DEFAULT NULL,
  `last_login_time` datetime DEFAULT NULL,
  `sex` tinyint(4) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=armscii8;

-- ----------------------------
-- Records of t_user
-- ----------------------------
BEGIN;
INSERT INTO `t_user` VALUES (1, 'json', '123', '2019-07-27 16:01:21', 1);
INSERT INTO `t_user` VALUES (2, 'jack jo', '123', '2019-07-24 16:01:37', 1);
INSERT INTO `t_user` VALUES (3, 'manistal', '123', '2019-07-24 16:01:37', 1);
INSERT INTO `t_user` VALUES (4, 'landengdeng', '123', '2019-07-24 16:01:37', 1);
INSERT INTO `t_user` VALUES (5, 'max', '123', '2019-07-24 16:01:37', 1);
COMMIT;

SET FOREIGN_KEY_CHECKS = 1;
```

# 2 新建 Spring Boot Maven 示例工程项目

1. File > New > Project，如下图选择 `Spring Initializr` 然后点击 【Next】下一步
2. 填写 `GroupId`（包名）、`Artifact`（项目名） 即可。点击 下一步
    groupId=com.fishpro   
    artifactId=mybatis
3. 选择依赖 `Spring Web Starter` 前面打钩，勾选SQL选项的 Mybatis Framework , MySQL Driver
4. 项目名设置为 `spring-boot-study-mybatis`.



# 3 依赖引入 Pom.xml 配置
在新建工程的时候，如果选择 `Mybatis` 依赖，则不需要单独配置，以下是本项目的全部依赖。

```xml
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.0</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

```

# 3 Mybatis 的工程配置 application.yml
无论是 `jdbc` 还是 `mybatis` 都需要对 数据源进行配置，而且配置项是一样的。除了数据源基础的配置，我们还需要配置 `mybatis` 的映射文件 *.xml 的路径，`mybatis` 的独立 `config.xml` 文件路径等。

- `mapper-locations` 配置实体与数据库表的映射文件xml位置，我们配置为 `/src/main/resource/mybatis/**maper.xml` 下
- `map-underscore-to-camel-case` 开启mybatis的驼峰命名转换
- `type-aliases-package` 指定 实体 `domain` 的类，这是 `com.fishpro` 下所有以 `domain` 结尾的实体类。

```yml
server:
  port: 8086
mybatis:
  configuration:
    map-underscore-to-camel-case: true
  mapper-locations: mybatis/**/*Mapper.xml
  type-aliases-package: com.fishpro.**.domain
```

针对 mybatis 的配置，我们也可以把所有 mybatis 配置配置到单独的 config 文件中。

# 5 编写示例代码
这里我们首次使用到 三层结构来演示本项目，即 controller-service-dao-mapper-mybatis

![mybatis三层结构示例](https://www.cnblogs.com/images/cnblogs_com/fishpro/1453719/o_mybatis1.png)

**本示例包括新增的页面**

1. src/main/java/com/fishpro/mybatis/controller/UserController.java 控制层 rest api
2. src/main/java/com/fishpro/mybatis/domain/UserDO.java 实体对象
3. src/main/java/com/fishpro/mybatis/dao/UserDao.java Dao数据库访问层
4. src/main/java/com/fishpro/mybatis/service/UserService.java 服务层包括了接口+接口实现
5. src/main/java/com/fishpro/mybatis/service/impl/UserServiceImpl.java 服务层包括了接口+接口实现
6. src/main/resources/mybatis/UserMapper.xml 映射 xml 文件

## 5.1 映射文件 UserMapper.xml
UserMapper.xml 是一个以 mybatis 语法来编写的 xml 映射文件。
```
+ mapper 节点是根节点 具有属性 namespace 表示 mapper 对应的 java 的 Dao 接口类
  |--select 查询节点
    |--where
    |--choose
        |--when
        |--otherwise
  |--insert 插入节点
  |--update 更新节点
  |--delete 删除节点
```
详细的代码如下
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fishpro.mybatis.dao.UserDao">
    <select id="get" resultType="com.fishpro.mybatis.domain.UserDO">
		select `id`,`user_name`,`password`,`last_login_time`,`sex` from t_user where id = #{value}
	</select>
    <select id="list" resultType="com.fishpro.mybatis.domain.UserDO">
        select `id`,`user_name`,`password`,`last_login_time`,`sex` from t_user
        <where>
            <if test="id != null   and id != '-1' " > and id = #{id} </if>
            <if test="userName != null  and userName != '' " > and user_name = #{userName} </if>
            <if test="password != null  and password != '' " > and password = #{password} </if>
            <if test="lastLoginTime != null  and lastLoginTime != '' " > and last_login_time = #{lastLoginTime} </if>
            <if test="sex != null   and sex != '-1' " > and sex = #{sex} </if>
        </where>
        <choose>
            <when test="sort != null and sort.trim() != ''">
                order by ${sort} ${order}
            </when>
            <otherwise>
                order by id desc
            </otherwise>
        </choose>
        <if test="offset != null and limit != null">
            limit #{offset}, #{limit}
        </if>
    </select>
    <select id="count" resultType="int">
        select count(*) from t_user
        <where>
            <if test="id != null   and id != '-1'  " > and id = #{id} </if>
            <if test="userName != null  and userName != ''  " > and user_name = #{userName} </if>
            <if test="password != null  and password != ''  " > and password = #{password} </if>
            <if test="lastLoginTime != null  and lastLoginTime != ''  " > and last_login_time = #{lastLoginTime} </if>
            <if test="sex != null   and sex != '-1'  " > and sex = #{sex} </if>
        </where>
    </select>
    <insert id="save" parameterType="com.fishpro.mybatis.domain.UserDO" useGeneratedKeys="true" keyProperty="id">
		insert into t_user
		(
			`user_name`,
			`password`,
			`last_login_time`,
			`sex`
		)
		values
		(
			#{userName},
			#{password},
			#{lastLoginTime},
			#{sex}
		)
	</insert>
    <update id="update" parameterType="com.fishpro.mybatis.domain.UserDO">
        update t_user
        <set>
            <if test="userName != null">`user_name` = #{userName}, </if>
            <if test="password != null">`password` = #{password}, </if>
            <if test="lastLoginTime != null">`last_login_time` = #{lastLoginTime}, </if>
            <if test="sex != null">`sex` = #{sex}</if>
        </set>
        where id = #{id}
    </update>
    <delete id="remove">
		delete from t_user where id = #{value}
	</delete>

    <delete id="batchRemove">
        delete from t_user where id in
        <foreach item="id" collection="array" open="(" separator="," close=")">
            #{id}
        </foreach>
    </delete>
</mapper>
```

## 5.2 实体对象 UserDO
与数据库表对应的（可自定义）对象实体类
```java

public class UserDO {
    private Integer id;
    private String userName;
    private String password;
    private Integer sex;
    private Date lastLoginTime;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public Integer getSex() {
        return sex;
    }

    public void setSex(Integer sex) {
        this.sex = sex;
    }

    public Date getLastLoginTime() {
        return lastLoginTime;
    }

    public void setLastLoginTime(Date lastLoginTime) {
        this.lastLoginTime = lastLoginTime;
    }
}
``` 
## 5.3 数据库访问层 UserDao  
Dao 是一个接口层，接口方法对应 mybatis/**Mapper.xml 中 Mapper 节点下的 id 值。例如上面的 `<select id="get" resultType="com.fishpro.mybatis.domain.UserDO">` 那么对应的接口方法 `UserDO get(Integer id) `

**Dao 必须使用 @Mappger注解，才能实现从Dao到Mapper.xml的映射**

```java

/**
 * 数据库访问层 用户Dao t_user
 * @author fishpro
 */
@Mapper
public interface UserDao {
    /**
     * 对应节点 select id="get" resultType="com.fishpro.mybatis.domain.UserDO"
     * */
    UserDO get(Integer id);
    /**
     * 对应节点 select id="list" resultType="com.fishpro.mybatis.domain.UserDO"
     * */
    List<UserDO> list(Map<String, Object> map);
    /**
     * 对应节点 select id="count" resultType="int"
     * */
    int count(Map<String, Object> map);
    /**
     * 对应节点 insert id="save" parameterType="com.fishpro.mybatis.domain.UserDO" useGeneratedKeys="true" keyProperty="id"
     * */
    int save(UserDO user);
    /**
     * 对应节点 update id="update" parameterType="com.fishpro.mybatis.domain.UserDO"
     * */
    int update(UserDO user);
    /**
     * 对应节点 delete id="remove"
     * */
    int remove(Integer id);
    /**
     * 对应节点 delete id="batchRemove"
     * */
    int batchRemove(Integer[] ids);
}

```

## 5.4 服务类 UseService

**注意 UserServiceImpl 一定要使用 @Service 注解，才能实现 UserService类自动注入功能**
在本示例中只是简单的增删改查功能
UserService 接口类
```java
public interface UserService {

    UserDO get(Integer id);

    List<UserDO> list(Map<String, Object> map);

    int count(Map<String, Object> map);

    int save(UserDO user);

    int update(UserDO user);

    int remove(Integer id);

    int batchRemove(Integer[] ids);
}
```
UserServiceImpl 实现类
```java

@Service
public class UserServiceImpl implements UserService {
	@Autowired
	private UserDao userDao;
	
	@Override
	public UserDO get(Integer id){
		return userDao.get(id);
	}
	
	@Override
	public List<UserDO> list(Map<String, Object> map){
		return userDao.list(map);
	}
	
	@Override
	public int count(Map<String, Object> map){
		return userDao.count(map);
	}
	
	@Override
	public int save(UserDO user){
		return userDao.save(user);
	}
	
	@Override
	public int update(UserDO user){
		return userDao.update(user);
	}
	
	@Override
	public int remove(Integer id){
		return userDao.remove(id);
	}
	
	@Override
	public int batchRemove(Integer[] ids){
		return userDao.batchRemove(ids);
	}
	
}

```

## 5.5 控制层实现 Rest Api 增删改查接口 UserController
本示例代码使用标准的 Restful 风格编写 具体代码如下：
```java

/**
 * RESTful API + Mybatis 风格示例 对资源 user 进行操作
 * 本示例没有使用数据库，也没有使用 service 类来辅助完成，所有操作在本类中完成
 * 请注意几天
 *    1.RESTful 风格使用 HttpStatus 状态返回 GET PUT PATCH DELETE 通常返回 201 Create ，DELETE 还有时候返回 204 No Content
 *    2.使用 RESTful 一定是要求具有幂等性，GET PUT PATCH DELETE 本身具有幂等性，但 POST 不具备，无论规则如何定义幂等性，需要根据业务来设计幂等性
 *    3.RESTful 不是神丹妙药，实际应根据实际情况来设计接口
 * */
@RestController
@RequestMapping("/api/user")
public class UserController {

    @Autowired
    private UserService userService;

    /**
     * 测试用 参数为 name
     * */
    @RequestMapping("/hello")
    public String hello(String name){
        return "Hello "+name;
    }

    /**
     * SELECT 查询操作，返回一个JSON数组
     * 具有幂等性
     * */
    @GetMapping("/users")
    @ResponseStatus(HttpStatus.OK)
    public Object getUsers(){

        return userService.list(new HashMap<>());
    }

    /**
     * SELECT 查询操作，返回一个新建的JSON对象
     * 具有幂等性
     * */
    @GetMapping("/users/{id}")
    @ResponseStatus(HttpStatus.OK)
    public Object getUser(@PathVariable("id") String id){

        if(null==id){
            return  null;
        }
        return userService.get(Integer.valueOf(id));
    }

    /**
     * 新增一个用户对象
     * 非幂等
     * 返回 201 HttpStatus.CREATED 对创建新资源的 POST 操作进行响应。应该带着指向新资源地址的 Location 头
     * */
    @PostMapping("/users")
    @ResponseStatus(HttpStatus.CREATED)
    public Object addUser(@RequestBody UserDO user){
        if(user.getId()==0){
            return null;
        }
        userService.save(user);
        return user;
    }

    /**
     * 编辑一个用户对象
     * 幂等性
     * */
    @PutMapping("/users/{id}")
    @ResponseStatus(HttpStatus.CREATED)
    public Object editUser(@PathVariable("id") String id, @RequestBody UserDO user){

        userService.update(user);
        return user;
    }

    /**
     * 删除一个用户对象
     * 幂等性
     * 返回 HttpStatus.NO_CONTENT 表示无返回内容
     * */
    @DeleteMapping("/users/{id}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    public void deleteUser(@PathVariable("id") String id){
        userService.remove(Integer.valueOf(id));
    }
}
```

# 6 运行示例

右键 `MybatisApplication` > `Run MybatisApplication` 在浏览器输入 `http://localhost:8086/api/user/users`

使用 PostMan 测试工具可测试 GET\POST\PUT\DELETE 动作的 Restful api

![mybatis 查询所有用户](https://www.cnblogs.com/images/cnblogs_com/fishpro/1453719/o_mybatis2.png)

# 7 问题思考
1. 如何自动生成代码
2. 如何动态配置数据源
3. 如何分表分库
4. xml的详细语法由哪些坑
5. xml 好还是 注解好
6. 如何解决事物处理
7. 扩库事物如何处理
   
下一章节我们演示下使用 注解来实现


[本项目源码下载](https://github.com/fishpro/spring-boot-study/tree/master/spring-boot-study-mybatis)