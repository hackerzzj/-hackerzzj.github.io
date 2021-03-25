java

java构造赋值，先父类后子类，先默认后构造的顺序。

父类与子类：

​		 子父类拥有相同的变量，规则：自己有用自己，自己无用父类。

 		父类实现先于子类实现，显式调用父类构造器或者隐藏调用父类无参构造器

​	Serializable接口，相当于一个标志，告诉jvm可以将该类的对象进行序列化和反序列化。（可以使用transient修饰那些不需要序列化的属性），最好自己写一个serialVersionUID(static final long)，不会序列化static关键字修饰的属性。如果父类没有实现Serializable接口，那么必须有无参构造函数。

​	Externalizable接口，需要重写writeExternal和readExternal方法,效率比Serializable高，可以决定哪些属性需要序列化（即使transient修饰的），但是面对大量对象，或者重复对象，则效率低。

​	package打包后的类可以不在同一个文件夹中，但是所在文件夹的名字必须与包名一致。

​	import关键字，使用*号时，只会导入该包内的类，并不会导入包。

​	

抽象类：

​	防止创建没有对象性质的对象（例如：动物），只应该有方法声明，不应该有方法主题。

​	抽象类也是具有构造函数的，所有子类实现都必须先执行抽象类的构造函数。

​	抽象类中可以存在0到多个抽象方法，也可存在0到多个非抽象方法

接口：

​	接口是一种特殊的抽象类，也是引用数据类型（数组，类，接口）,包含抽象方法( public abstract)，（静态方法和默认方法(default不可省)）jdk8以后，成员属性都是常量（public static final）

​	默认方法，子类不需要重写。

​	实现接口是为了实现某个功能，是为了进行拓展。

类中代码会在属性初始化之后，无参构造函数之前

static

​	static 关键字修饰的 可以继承 但是不可以重写

final	

​	final 修饰成员变量必须赋值（直接赋值，代码块赋值，构造方法赋值），修饰局部变量可以声明和赋值分开。

​	final修饰的形参也是不能改变的

​	final 修饰引用类型，可以更改引用对象。

​    static final 修饰的就是一个常量  （直接赋值，静态代码块赋值）

​	final 修饰的方法不允许被重写

​	final 修饰的类不能被继承

多态

​	成员属性看左边，成员方法看右边（方法里面用右边变量）

Hashcode编码

​	用的Object.hash(Object... values)   底层 Arrays.hashCode(values)

实现clone方法

​	实现Cloneable接口,才能重写Object类的clone方法

javaBean规范

​	必须要有包

​	属性私有化

​	get、set方法

​	实现接口序列化  Serializable

​	public修饰类

​	增加无参构造函数

String类

​	底层char数组，String对象永远不变

​	字符串池的存在，所以字符串字面量是共享的

​	底层是char数组，所以可以进行char数组和字符串的互转

​    intern方法: （手动优化）如果字符串池中有则返回字符串池中得，否则向池中添加再返回

​	match方法：正则表达式

StringBuilder（异步，不安全）

​	append: 添加

​	insert: 插入位置的前面

​	delete: 删除包前不包后

正则表达式的作用

​	判断句子是否符合你的规则   match

​	按照你的规则进行文本切割   split

​	在文本中替换你想要得格式    replaceAll

​	

| .    | 匹配任意一个字符，除了\n      |
| ---- | ----------------------------- |
| []   | 匹配[]中列举的类型            |
| \d   | 匹配数字，即0-9               |
| \D   | 匹配非数字                    |
| \s   | 匹配空白，tab 空格            |
| \S   | 匹配非空白                    |
| \w   | 匹配单词字符，即a-z,A-Z,_,0-9 |
| \W   | 匹配非单词字符                |

正则表达式的斜杠成双原则  \\ \        因为编辑器认为\后的字母应该被转义，但是又找不到应该转义成的字符，我们需要的是一个完整的\d，所以斜杠成双。

| *     | 匹配前一个字符0或无数次，即可有可无   |
| ----- | ------------------------------------- |
| +     | 匹配前一个字符1或无数次，即至少有一次 |
| ？    | 匹配前一个字符1或0次，即不会多于一次  |
| {m}   | 匹配前一个字符m次                     |
| {m,}  | 匹配前一个字符至少m次                 |
| {m,n} | 匹配前一个字符m到n次                  |

#### 时间类型

long类型 仅仅是返回值类型 System.currentTimeMillis(),  1970年到现在的毫秒数

Data类型  日期 桥梁  由于千年虫事件 基本上不使用  被用来进行long类型和Calendar类型的数据转换以及时间的展示

Calendar类型  日历 日期的运算 属于抽象类，使用它的子类（GregorianGalendar）进行实例化，或者使用Calendar中的getInstance()方法  子类gregorianCalendar

从字符串转换为时间类型  DateFormat 抽象类，子类simpleDateFormat

格式化 date--》string

解析 string---》date

#### 集合类型

​	Collection 接口

add，remove，size，contains

addAll,  合并不会去重

removeAll, 去重交集

retainAll  得到交集

根据类型的equals和hashcode进行判断

###   List接口 ===默认为集合  （有序、重复）

List允许出现重复元素，线性方式的有序存储，通过索引来访问集合中的指定元素

##### *如果list是Integer泛型的话，如果使用remove方法，这里必须要理解的是：如果不需要自动装箱就能调用方法的话，就不会进行自动装箱。

ArrayList在操作指定下标的任意元素上面，速度比较快。(查找上和指定下标操作的操作上速度比较快)。

LinkedList底层是双向链表，所以在操作首尾元素的操作上速度比较快。

vector自从ArrayList出现以后，就不再使用了

#### 迭代器

相当于把所有的集合元素按照他们指定的顺序排放在一个类似于线性表的集合中。

使用集合对象调用iterator方法，获取iterator对象，如果有泛型，iterator也可以使用相应的泛型。

### 泛型

为了将运行时异常转换为编译时异常（深层次）

泛型不存在继承关系

### set接口

set接口中的元素无序，并且存入的元素不重复出现。

HashSet

​	无序且不重复、重写hashcode和equals方法

LinkedHashSet

​		有序

TreeSet(实现了set和sortset)

​	 具有自然排序功能的类

​	可以自定义排序

​    不重复，不能放null

​	因为TreeSet这个类需要吧里面的元素进行统一类型来比较，所以会配套使用泛型

​	TreeSet对自定义数据类型进行排序的话，需要两个前提

​				实现comparable接口

​				有具体的排序方法

###  比较器

在java中有两个常用的接口，分别是Comparator接口和Comparable接口

Comparator接口

​	临时  重写 compare方法 两个参数

​	经常在Arrays.sort以及Collections.sort里面来使用

​	不能在TreeSet中使用

​	使用内名内部类

Comparable接口

​	持久 重写 compareTo方法  一个参数

```
// 都返回0的话，就会输出一个元素
// 返回正数时，代表该属性值的升序-->本身属性-参数属性
// 返回负数时，代表该属性值的降序-->参数属性-本身属性
```

### Map接口

 等同于set的定义，建唯一不重复。键值对

​    键 无序、无下标、不允许重复

​	值 无序、无下标、允许重复

HashMap 不能保证顺序，键采用自定义类型时，就要重写hashCode和equals方法

LinkedHashMap  多了排序的功能

TreeSet 自然排序且键不重复，可以采用自定义比较。

HashMap与Hashtable的关系    ArrayList和vector的关系

### Throwable

​	编写错误不属于异常（需要try catch的才是编译型异常）

​	printStackTrace() 打印异常的详细信息。包含了异常的类型，异常的原因，异常出现的位置

​	getMessage() 获取异常的原因

​	toString() 获取异常的类型和异常描述信息

异常处理方式

​	try  catch  finally   不加catch的话会结束程序的执行

​	throw 后面跟异常对象  并且结束当前方法的执行 

​	throw 后面如果接运行时环境，一旦改代码执行，那么程序会停止，如果后面是检查时异常，就会要求你处理异常，程序不会停止。

​	如果throw后面跟的是一个编译时异常，jvm就会要求你必须处理此异常

​	throws 放在方法结构尾部，用来告诉方法的调用者，这个方法有异常。

​    Exception 和 继承Exception类的子类 属于编译期异常（除了RuntimeException 和 它的子类）

​	RuntimeException和继承 了RuntimeException的子类

​    自定义异常就是继承Exception或RuntimeException

