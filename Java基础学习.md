## 对象

### 封装



### 继承（extends）

#### 定义

1. 继承是类和类之间的一种关系。（还存在依赖，组合，聚合等关系）
2. 继承关系的两个类，一个为子类（派生类），一个为父类（基类）。子类继承父类，使用关键字extends来表示
3. 子类和父类间，从意义上来说具有 “子类 is 父类”。比如啄木鸟是鸟。

**注意**： Java 中只有单继承，没有多继承，即一个子类只能继承一个父类。

#### super和this

super注意点：

1. super 调用父类的构造方法，必须在构造方法的第一个
2. super 必须只能出现在子类的方法或者构造方法中
3.  super和this不能在构造方法同时调用

super vs this 的不同点：

- 代表对象不同：

	this:本身调用者这个对象

	 super:代表父类对象

- 前提：

​	this：没有继承也可以使用

​	super：只能在继承条件下使用

- 构造方法

​	this（）：本类的构造方法

​	super（）：父类的构造方法

#### 方法的重载

***定义*** ： 一个类中定义多个同名的方法，它们的参数列表不同。

**区分** ：

1. 参数类型
2. 参数个数
3. 参数顺序

***注意*** ：仅有返回类型不同时不足以区分两个重载的方法的。

#### 方法的重写（覆盖）

***定义***：子类把继承来的方法重新定义，实现不同功能。

1. 需要有继承关系，子类才能重写父类的方法
2. 方法头与父类一样（方法名，返回类型，参数表），方法体不同，即实现功能不一样；
3. 修饰符：范围可以扩大  Public>Protected>Default>Private
4. 抛出的异常：范围，可以被缩小，但不能扩大       ClassNotFountException（小） ------> Exception（大）

 ### 多态

**定义** ：同一方法根据发送对象的不同而采取不同的行为方法。

**注意点** ：

1. 多态是方法的多态，属性没有多态

2. 父类和子类，有联系才能转换 （向上转型——子类转为父类；向下转型——父类转为子类） （***注意***： 类型转换异常！ ClassCastException）

3. 存在条件： 

	- 继承关系 
	- 方法需要重写 ，个别方法不能重写（如：static ， final ，private方法等不能重写） 
	- 父类引用指向子类对象 （ Father a=new Son（），a为父类引用对象，=右边为实例化后的子类对象 ）

	```java
	//      对象实际类型是确定的
	        new Person();
	        new Student();
	//      Student 子类，能调用的方法都是 自己的或者是继承父类的
	        Student s1=new Student();
	//      Person 父类，可以指向子类，但是不能调用子类中属性和独有的方法（重写方法等）
	        Person  s2=new Student(); 		
	//      Object 类是所有类的源头，即祖宗类
	        Object s3=new Student();
	
	
	//       对象能执行哪些方法，主要看左边对象类型，和右边关系不大，（即执行看左边对象类型，编译看右边对象类型
	        s1.run();
	        s2.run();
	//      将父类强制转换成子类，调用子类特有的方法
	        ((Student) s2).study();
	```
	
	

### static关键字

#### static方法

​	**定义** ：static修饰的方法称为静态方法，不依赖于任何对象就可以进行访问，因此对于静态方法不存在this。（静态方法中不能访问类的非静态成员变量和非静态成员方法，因为非静态成员方法/变量都是必须依赖具体的对象才能够被调用。）


非静态方法可以调用静态方法，反之静态方法不能调用非静态方法。

```java
	void run()
    {
        eat();
    }
    static  void eat()
    {
        run(); //无法调用
    }
```
#### static变量

​	**静态变量与非静态变量区别** ：

1. - 静态变量被所有的对象所共享 ;
	-  非静态变量是实例化的对象所拥有的。
2. - 静态变量内存中只有一个副本【存放在方法区】，它当且仅当在类初次加载时会被初始化
	- 在创建对象的时候被初始化，存在多个副本，各个对象拥有的副本互不影响。
3. - 静态变量也叫类变量，不用实例化，使用类名.属性名来使用。
	- 非静态变量也叫实例变量，只有实例化后才可以使用。

```java

public class Test
{
    public static void main(String[] args)
    {
//        静态变量调用
        Egg.count=1;
//        非静态变量调用
        Egg a=new Egg();
        a.old=14;
        a.name="樱花";
    }
}
class Egg
{
    static int count;
    int old;
    String name;
}
```

#### static代码块 
利用静态代码块的特性(**只会在类加载的时候执行一次**)，来进行初始化操作都放在static代码块中进行，来达到优化程序性能的目的。

```java
 private Date birthDate;
    private static Date startDate,endDate;
    static{
        startDate = Date.valueOf("1946");
        endDate = Date.valueOf("1964");
    }
//	判断是否在1946-1964年出生
	boolean isBornBoomer() {
        return birthDate.compareTo(startDate)>=0 && birthDate.compareTo(endDate) < 0;
    }
```

初始化的执行顺序（静态代码块>匿名代码块>构造方法）

```java
class Milk
{
//  2 匿名代码块，每次实例化该类都会执行一次
    {
        System.out.println("匿名代码块");
    }
//  1  静态代码块只执行1次，
    static
    {
        System.out.println("静态代码块");
    }
//  3  
    public Milk(){
        System.out.println("构造方法");
    }
}
```

#### 静态内部类

#### 静态导包



### 抽象类（abstract）

**定义** 

使用关键字 **abstract** 修饰



**抽象方法声明**

```java
//[修饰符] abstract 方法返回值类型 方法名(参数);
public abstract void eat();
```

**注意**：

1. 抽象方法声明只需给出方法头，不需要方法体。
2. 构造方法不能声明为抽象方法。

**抽象类声明** 

```java
//[修饰符] abstract class  
public abstract class Milk
{
//	类体
}
```

**注意** ：

1. 抽象方法只能在抽象类中，但抽象方法中也可以不包含抽象方法。
2. 抽象类不能**实例化**，即抽象类中没有声明抽象方法。
3. 抽象类的子类，只有**重写完父类的所有抽象方法**后，才可以创建子类。





## Java输入输出系统

### File类

​	**定义** ：通过创建一个File类的对象，可以得到文件或者目录的描述信息，包括文件名称，所在目录，可读性等，还可以生成新的目录，改名文件名，删除文件等。

​	***注意***：File类只能操作文件（新建，删除，重命名等），不能操作文件内容（读取信息，向文件写入信息），它仅仅描述文件本身的属性。

​	**File的主要方法**：

 1. 构造方法

	* public File(String pathname) 通过将给定路径名字符串转换为抽象路径名来创建一个新 File 实例。
	* public File(String parent,String child) 根据指定的父路径和文件路径创建一个新File对象实例
	* public File(File parent,String child) 根据指定的父路径对象和文件路径创建一个新的File对象实例

	```java
	//public File(String pathname) 通过将给定路径名字符串转换为抽象路径名来创建一个新 File 实例。
	File f1=new File("G:\\Demo\\test.txt");
	
	//public File(String parent,String child) 根据指定的父路径和文件路径创建一个新File对象实例
	File f2=new File("G:\\Demo","test.txt");
	
	//public File(File parent,String child) 根据指定的父路径对象和文件路径创建一个新的File对象实例
	File f3=new File("G:\\Demo");
	File f4=new File(f3,"test.txt");
	```

2. 创建和删除功能
	* boolean createNewFile()   指定路径不存在该文件时创建文件，返回true 否则false
	*	boolean mkdir（） 当指定的单击文件夹不存在时创建文件夹并返回true 否则false
	*	boolean mkdirs（） 当指定的多级文件夹在某一级文件夹不存在时，创建多级文件夹并返回true 否则false
	*	boolean delete（） 删除文件或者删除单级文件夹
	* ***注意***：删除文件夹时，这个文件夹下面不能有其他的文件和文件夹
	
	```java
	        File f=new File("G:\\Demo\\test.txt");
	//        创建test.txt文件
	        f.createNewFile();
	
	        File f1=new File("G:\\Demo\\q1");
	//        只能创建单层目录
	        f1.mkdir();
	
	        File f2=new File("G:\\Demo\\q1\\q2\\q3");
	//        创建多层目录
	        f2.mkdirs();
	
	//        删除文件或者文件夹
	        f.delete();
	```

3. 判断操作
  * boolean exists() 判断指定路径的文件或文件夹是否为空
  * boolean isAbsolute() 判断当前路径是否是绝对路径
  * boolean isDirectory() 判断对象是否是目录
  * boolean isFile() 判断对象是否是文件
  * boolean isHidden() 判断当前路径是否是一隐藏文件
  * boolean canWrite() 判断文件是否可写
  * boolean canRead() 判断文件是否可读

  ```java
          File f=new File("G:\\Demo\\test.txt");
  //      判断test.txt是否存在于指定目录
          if(f.exists()) System.out.println("存在");
  //      判断当前路径是否是绝对路径
          if (f.isAbsolute()) System.out.println("是绝对路径");
  //        判断对象是否是目录
          if(f.isDirectory()) System.out.println("是目录");
  //        判断对象是否是文件
          if(f.isFile()) System.out.println("是文件");
  //       判断对象是否是隐藏文件
          if(f.isHidden()) System.out.println("是隐藏文件");
  //        判断文件是否可读
          if(f.canRead()) System.out.println("文件可读");
  //        判断文件是否可写
          if(f.canWrite()) System.out.println("文件可写");
  ```

4. 获取和修改操作
	*  File getAbsoluteFile()      获取文件的绝对路径，返回File对象
	*  String getAbsolutePath()       获取文件的绝对路径，返回路径的字符串
	*  File getParentFile()        获取当前路径的父级路径，返回该父级路径File对象
	*  String getParent()        获取当前路径的父级路径，以字符串形式返回该父级路径
	*  String getName()       获取文件或文件夹的名称
	*  String getPath()        获取File对象中封装的路径
	*  long lastModified()         以毫秒值返回最后修改时间
	*  long length()         返回文件的字节数
	*  boolean renameTo(File dest)        将当前File对象所指向的路径修改为指定File所指向的路径
	*  String[] list()       以字符串的形式返回当前路径下所有的文件和文件夹的名称
	*  File[] listFile      以File对象的形式返回当前路径下的所有文件和文件夹名称
	
	```java
	        File f=new File("G:\\Demo\\test.txt");
	//        得到文件绝对路径,返回File对象
	        System.out.println(f.getAbsoluteFile());
	//        得到文件绝对路径，返回路径的字符串形式
	        System.out.println(f.getAbsolutePath());
	//         获取当前路径的父级路径，返回该父级路径File对象
	        System.out.println(f.getParentFile());
	//        获取当前路径的父级路径，以字符串形式返回该父级路径
	        System.out.println(f.getParent());
	//        获取文件或文件夹的名称
	        System.out.println(f.getName());
	//        获取File对象中封装的路径
	        System.out.println(f.getPath());
	//        以毫秒值返回最后修改时间
	        System.out.println(f.lastModified());
	//        返回文件的字节数
	        System.out.println(f.length());
	
	//      以字符串的形式返回当前路径下所有的文件和文件夹的名称
			String[] files1 = f.list();
	    	for (int i=0;i<files1.length;i++)
	        {
	        	System.out.println(files1[i]);
	   		}
	
	//		以File对象的形式返回当前路径下的所有文件和文件夹名称
		    File[] files2 = f.listFiles();
	        for (File file3 : files2) 
	        {
	            System.out.println(file3.getName());
	        }
	```
	
	

