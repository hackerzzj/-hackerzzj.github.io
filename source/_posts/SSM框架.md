# SSM框架-mybites框架

### 配置

#### 步骤：

1. 创建maven工程并且引入坐标

   ```xml
   <dependency>
     <groupId>org.mybatis</groupId>
     <artifactId>mybatis</artifactId>
     <version>x.x.x</version>
   </dependency>
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>5.1.6</version>
   </dependency>
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.12</version>
   </dependency>
   <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.11 </version>
   </dependency>
   ```

   

2. 创建实体类和dao的接口

3. 创建Mybatis的主配置文件（SqlMapConfig.xml）

   ```java
   @Select("select * from user")
   public List<User> getAll();
   ```

   

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
     PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
     "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
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
     <mappers>
       <mapper resource="org/mybatis/example/BlogMapper.xml"/><!--通过xml的方式-->
       <mapper class="org.mybatis.example.BlogDao"/><!--通过注解的方式，class为注解的类-->
     </mappers>
   </configuration>
   ```

   

4. 创建映射配置文件（IUserDao.xml）

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
     PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
     "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="org.mybatis.example.BlogMapper">
     <select id="selectBlog" resultType="Blog">
       select * from Blog where id = #{id}
     </select>
   </mapper>
   ```

   

#### 注意事项：

1. 创建映射配置文件时，文件的目录结构要和dao层的目录结构相同，方便mybatis的自动寻找。
2. 映射配置文件的mapper标签namespace属性取值dao接口的权限定类名
3. 映射配置文件的操作配置（select），id属性取值dao接口类的方法名

### 使用

```java
  // 配置文件
InputStream in =Resources.getResourceAsStream("SqlMapConfig.xml");
// 创建SqlSessionFactory 对象 并且得到session对象
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
SqlSession sqlSession = factory.openSession();
// 通过session对象获取到dao接口的动态代理对象
IUser userimpl = sqlSession.getMapper(IUser.class);
// 使用动态代理类得到数据
List<user> all = userimpl.getAll();
// 关闭资源
sqlSession.close();
in.close();
```

#### 读取配置文件

1. 使用类加载器读取，他只能读取类路径下的配置文件。
2. 使用ServletContext对象的getRealPath()方法读取配置文件。

#### mybatis使用代理dao的方式实现的增删改查方式

1. 创建代理对象（可以使用dom4j的技术来解析xml文件）

   连接数据库的信息（xml文件中）connection

   mapper映射配置信息

   通过mapper配置信息找到所对应的sql语句 和 封装的实体类限定名    prepareStatement

   

2. 使用代理对象调用增删改查方法