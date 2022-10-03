### idea的maven项目中xml文件无法自动补全或者标签写错了也不报错的做法
```in
在file>setting中找到Editor->File Types,
在找到xml的文件类型、选择添加*.xml即可解决。
```
![[Pasted image 20220523202817.png]]


###  spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver报错
```in
首先是mysql-connector-java这个包中，手动指定版本号。（没解决）

最后是加上mysql-connector-java.jar这个包，清除缓存重启才解决报红。(解决了)
```
![[Pasted image 20220523213117.png]]


### IDEA连接Mysql数据库之后，不会自动提示表信息
```in
我们需要进入File-Settings-Languages & Frameworks-SQL Dialects下添加相关联的项目。
```
![[Pasted image 20220525150458.png]]


### @Autowired注解爆红原理及解决方案
```in
SpringBoot中的service层经常需要将mapper注入进来，但是注入一个mapper接口时经常会爆红
```
![[Pasted image 20220525151815.png]]
![[Pasted image 20220525151836.png]]
```in
可以看到,爆红的原因是`@Autowired`是`Spring`的注解，提示找不到对他的bean，因为你没有显示的将userMapper注入到Spring容器中去管理。
```

```in
这种情况只需要在ArticleMapper.java接口上添加`@Repository`注解即可，此注解是Spring的注解，将当前类注册到Spring容器中实例化为一个bean，所以`@Autowired`就能找到此bean了。
```
![[Pasted image 20220525151952.png]]
![[Pasted image 20220525152000.png]]

```in
还有一种方式就是直接将`@Autowired`换成`@Resource`注解，此注解是JDK中的注解，不会向`@Autowired`那样去Spring容器中寻找bean。
```
![[Pasted image 20220525152059.png]]


### 关于Invalid bound statement (not found)出现原因和解决方法
```in
网上找了半小时，结果文件夹重新建了一遍ok了，无语了。
Invalid bound statement (not found)

其实出现这个问题实质就是mapper接口和mapper.xml文件没有映射起来。
常见的错误如下：
1.mapper.xml中的namespace和实际的mapper文件是否一致

2.mapper接口中的方法名和mapper.xml中的id标签是否一致

3.打开target看看对应的mapper.xml文件在不在

4.porm.xml的<build>中是否配置<resources>

5.特别的坑：实际上在resouces中所放的xxxMapper.xml文件的位置要和mapper接口的位置对应，resources目录下建文件夹要一个一个建,不要像建package一样com.xxx.xx，这样也会导致找不到mapper.xml文件。（类似下图结构）
```
![[Pasted image 20220525162309.png]]

### 后端（分布式id）雪花算法生成的id导致前端查询的问题
```in
若id是由雪花算法生产的19位数字，初始查询返回的json数据中id为19位,而jsNumber类型最多16位，超出的位数不保证精度。导致前端再次查询文章时的请求参数id出错。  
解决方法：可在对应Vo类的id字段上添加 @JsonSerialize(using = ToStringSerializer.class)，防止前端精度损失，把id转为String
```


### 通过缓存得到数据时，若数据中存在分布式产生的id字段时，会存在精度损失问题
```in
最好的解决办法是将id的类型改为String。
```