YML 语言

​	key: value格式（value前必须有空格，可以有多个，不能用tab键替代）

​	大小写敏感

​	字符串默认不需要使用引号，单引号和双引号的区别是单引号不支持转移字符

​	注释方式：#

YML支持的三种数据类型

​	字面量：直接量，单个不能被拆分的值（数字、字符串、布尔）

​	对象：键值对形式存在

```yaml
#对象的写法
friend:
	name: lisi
	sex: 女
#或者
friend: {name: lisi,sex: 女}
```

​	数组：字面量/对象的组合

```yaml
#数组的写法
pets:
	- 小狗
	- 小猫
	- 小猪
# 或者
pets: [小狗，小猫，小猪]
```

Value注入：${字面量}、${配置文件中取值}、#{spEL表达式}

```xml
<bean id="Person">
	<property name="lastName" value="字面量/${key}从环境变量、配置文件中获取值/         #{SpEL}">	</property>
</bean>
```

