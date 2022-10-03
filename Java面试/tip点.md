接口中方法默认被public abstract修饰，抽象方法不可以有方法体

String.intern()是一个Native方法,它的作用是:如果字符中已经包含一个等于此String对象的字符串,则返回常量池中字符串的引用,否则,将新的字符串放入常量池,并返回新字符串的引用 。

数组复制效率： System.arraycopy>clone>Arrays.copyOf>for循环

Java中的byte，short，char进行计算时都会提升为int类型。因此一个int类型赋值给byte就不行。但是final修饰后的byte不会转换成int计算。

类初始化阶段先执行最顶层父类的静态初始化块，依次向下执行，最后执行当前类的静态初始化块；创建对象时，先调用顶层父类的构造方法，依次向下执行，最后调用本类的构造方法。
静态初始化块只在类加载时执行，且只会执行一次，同时静态初始化块只能给静态变量赋值，不能初始化普通的成员变量
初始化代码块在构建对象的时候就会执行，初始化块在构造器执行之前执行
执行顺序:  静态成员变量或静态代码块>main方法>非静态成员变量或非静态代码块>构造方法 （父类先，子类后）

java命名规范:
1. 只能由字母，数字，下划线，美元符号组成
2. 不能以数字开头
3. 区分同一个字母的大小写



类的加载包括：加载，验证，准备，解析，初始化。


当程序执行到try{}语句中的return方法时，它会干这么一件事，将要返回的结果存储到一个临时栈中，然后程序不会立即返回，而是去执行finally{}中的程序。
```in

public class Test{	
    public int add(int a,int b){	
         try {	
             return a+b;		
         } 
        catch (Exception e) {	
            System.out.println("catch语句块");	
         }	
         finally{	
             System.out.println("finally语句块");	
         }	
         return 0;	
    } 
     public static void main(String argv[]){ 
         Test test =new Test(); 
         System.out.println("和是："+test.add(9, 34)); 
     }
}

```


初始化为类变量赋予正确的初始值。初始化会在某个类被首次主动使用的时候触发，主动使用分为6类：
1.new 一个类的对象；
2.访问类或者接口的静态变量，或者进行赋值；
3.调用类的静态方法；
4.反射；
5.初始化某个子类，其父类也会初始化；
6.在jvm启动时被标明为启动类的类。

 
 # 创建对象5种方式
Java有5种方式来创建对象： 
1、使用 new 关键字（最常用）： ObjectName obj = new ObjectName(); 2、使用反射的Class类的newInstance()方法： ObjectName obj = ObjectName.class.newInstance(); 
3、使用反射的Constructor类的newInstance()方法： ObjectName obj = ObjectName.class.getConstructor.newInstance(); 
4、使用对象克隆clone()方法： ObjectName obj = obj.clone(); 
5、使用反序列化（ObjectInputStream）的readObject()方法： try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(FILE_NAME))) { ObjectName obj = ois.readObject(); }

 
  String通过+号来拼接字符串的时候，如果有字符串变量参与,实际上底层会转成通过StringBuilder的append( )方法来实现，最后StringBuilder 的 toString( )方法底层new了一个String对象，所以会在在堆内存中重新开辟了新空间，所以第二个为false