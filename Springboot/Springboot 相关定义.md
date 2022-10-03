### vo对象与数据库实体对象区别
因为数据库中的数据和展示数据并不一致，因此使用vo对象。vo对象是用来包装数据库的实体对象类，和前端进行交互的。

### mybatisplus一些使用
mybatisplus 无法进行多表查询

### do对象含义
dos包，do对象也是数据库的对象，但不需要持久化的对象

### 数据库中日期使用毫秒值存储，如何转换成年，月等
数据库中的时间类型是bigint，因此我们需要转换,原理就是把13位的时间格式/1000等于时间戳，使用FROM_UNIXTIME把时间戳转换成具体的日期
year(FROM_UNIXTIME(create_date/1000))

### JWT基础内容
登录使用JWT技术
jwt可以生成一个加密token
 
 
### 数据库中自增长id的设定 
保存用户的id，会自动生成递增  
默认生成的id是分布式id ，采用了雪花算法  
在mybaits-plus框架中，id默认类型为IdType.ASSIGN_ID(自增长类型)  
当用户多了之后，就需要进行分表操作，那么id就需要使用分布式id了

### 本地线程ThreadLocal保存用户信息

#### 为什么要用完后删除ThreadLocal中的信息
**实线代表强引用,虚线代表弱引用**
![[Pasted image 20220531222154.png]]
每一个Thread维护一个ThreadLocalMap, key为使用**弱引用**的ThreadLocal实例，value为线程变量的副本。

**强引用**，使用最普遍的引用，一个对象具有强引用，不会被垃圾回收器回收。当内存空间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不回收这种对象。

**如果想取消强引用和某个对象之间的关联，可以显式地将引用赋值为null，这样可以使JVM在合适的时间就会回收该对象。**

**弱引用**，JVM进行垃圾回收时，无论内存是否充足，都会回收被弱引用关联的对象。在java中，用java.lang.ref.WeakReference类来表示。

### 线程的操作
** 在查看文章的时候，同时更新阅读数量，要求更新阅读数量失败时不影响我们查看文章，这个时候可以使用线程来操作 **
在你修改浏览记录前，数据库的浏览数据可能会被修改，如果你不加这个where判断一下数据库现有的值和你查询到的值相等，那么就会产生一种情况，本来你查询到的是100浏览记录进行+1操作，但是在你执行修改之前，数据库的浏览记录一下增加了10个人，接下来你执行修改操作，判断条件是满足id就行就会把这值修改成101，那么这这10个人就飞走了~我也不知道对不对

### 实体类中属性的定义类型，关联到mybatis数据库中的表查询
int属于java的基本数据类型，而Integer是int的包装类。
int默认值为0，而Integer对象的默认值为NULL，mybatis执行更新操作时，若参数不为NULL，就会参与更新。
```java
int viewCounts = article.getViewCounts();  
Article articleUpdate = new Article();  
articleUpdate.setViewCounts(viewCounts+1);  
LambdaUpdateWrapper<Article> updateWrapper=new LambdaUpdateWrapper<>();  
updateWrapper.eq(Article::getId,article.getId());  
//在多线程操作下，为了线程的安全，多设置一个比较值  
updateWrapper.eq(Article::getViewCounts,viewCounts);  
//sql:update article set view_count = 100 where view_count=99 and id=11  
articleMapper.update(articleUpdate,updateWrapper);
```
在从下图中，我们只需要更新viewCounts(阅读量),红框中。但是评论数和比重却也参与了更新，并且它们的值都为0，黄框中。
![[Pasted image 20220606202935.png]]
 当我们将评论数和比重的数据类型更改为Integer时，即可解决该Bug。
 
### Spring中BeanUtils的copyProperties属性方法问题
将有相同属性的两个类赋值，a赋值给b
BeanUtils.copyProperties(a,b);
** 这里有需要注意的点是，当a和b对象中有相同属性名，但是属性一个是基本数据类型，而另一个是封装类时，会报错。**


### AOP日志


###  Mybatis中的#号与$符号的区别
```in
1、#{变量名}可以进行预编译、类型匹配等操作，

2、#{变量名}会转化为jdbc的类型。

3、${变量名}不进行数据类型匹配，直接替换。 

4、#方式能够很大程度防止sql注入。

5、$方式无法方式sql注入。

6、$方式一般用于传入数据库对象，例如传入表名。

7、尽量多用#方式，少用$方式。

8、#会自动加双引号，$不会加双引号

 这两个符号在mybatis中最直接的区别就是：#相当于对数据 加上 单引号，$相当于直接显示数据（只讨论字符串类型的）。
 1、#对传入的参数视为字符串，也就是它会预编译，select * from user where name = #{name}，比如我传一个aaa，那么传过来就是 select * from user where name = 'aaa'；

2、$将不会将传入的值进行预编译，select * from user where name=${name}，比如我传一个aaa，那么传过来就是 select * from user where name = aaa；

3、#的优势就在于它能很大程度的防止sql注入，而$则不行。比如：用户进行一个登录操作，后台sql验证式样的：select * from user where username=#{name} and password = #{pwd}，如果前台传来的用户名是“wang”，密码是 “1 or 1=1”，用#的方式就不会出现sql注入，而如果换成$方式，sql语句就变成了 select * from user where username=wang and password = 1 or 1=1。这样的话就形成了sql注入。

4、MyBatis排序时使用order by 动态参数时需要注意，用$而不是#
 
```


### 后端分布式id的变量类型建议为String
```in
1.前后端可能会出现精度差，js Number有精度丢失的问题，超过整数超过53位之后，后面几位是随机变的。前端拿到的ID从一开始就是错的。
2.不利于平行扩展。想一下这样一种场景，如果日注册用户数很大，那么为了高可用高并发，必须做ID的分段，这时候int32就明显不够用了。
```