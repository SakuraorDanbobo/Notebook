### 基本代码 ###

```java
//设置窗体大小固定
this.setResizable(false)
//自动填充剩余空间
this.pack(); 
//字体类
//三个参数分别表示： 字体，样式（粗体，斜体等），字号
new Font("宋体", Font.PLAIN, 16)
//按钮文字边框线取消
bt1.setFocusPainted(false);
//按钮外边框线取消
bt1.setBorderPainted(false);
//文本框设置符号替换（用于输入密码时*的形式
tf.setEchoChar('*');
//画笔
@Override
public void paint(Graphics g)
{
     super.paint(g);
     g.setColor(Color.black);
     g.drawLine(0,0,150,150);
}
```



### 三种基本布局 ###

流式布局（FlowLayout）**默认**

边界布局（BorderLayout）

网格布局（GridLayout）

简易布局

实现效果：

![image-20211220234033426](G:\学习笔记\pic\image-20211220234033426.png)

代码如下：

```java
package Demo001;

import javax.swing.*;
import java.awt.*;

public class demo01
{
    public static void main(String[] args)
    {
        new MyJFrame();
    }
}
class MyJFrame extends JFrame
{
    public MyJFrame()
    {
	
        JButton bt1=new JButton("1");
        JButton bt2=new JButton("2");
        JButton bt3=new JButton("3");
        JButton bt4=new JButton("4");
        JButton bt5=new JButton("5");
        JButton bt6=new JButton("6");
        JButton bt7=new JButton("7");
        JButton bt8=new JButton("8");
        JButton bt9=new JButton("9");
        JButton bt10=new JButton("10");


        JPanel jp1=new JPanel(new GridLayout(2,1));
        jp1.add(bt2);
        jp1.add(bt3);
        JPanel jp2=new JPanel(new BorderLayout());
        jp2.add(bt1,BorderLayout.WEST);
        jp2.add(jp1,BorderLayout.CENTER);
        jp2.add(bt4,BorderLayout.EAST);


        JPanel jp4=new JPanel(new GridLayout(2,2));
        jp4.add(bt7);
        jp4.add(bt8);
        jp4.add(bt9);
        jp4.add(bt10);
        JPanel jp3=new JPanel(new BorderLayout());
        jp3.add(bt5,BorderLayout.WEST);
        jp3.add(jp4,BorderLayout.CENTER);
        jp3.add(bt6,BorderLayout.EAST);

        this.setLayout(new GridLayout(2,1));
        this.add(jp2);
        this.add(jp3);


        //设置标题
        setTitle("Demo1");
        //设置窗体大小
        setSize(800,800);
        //居中
        setLocationRelativeTo(null);
        //设置窗体大小不可变
        setResizable(false);
        //设置退出默认关闭
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        //设置可见
        setVisible(true);
    }
}
```



### 为什么添加一个JPanel后就把窗口全覆盖了？ ###

```java
/*
如果不主动设置layout(布局管理器），默认是BorderLayout，frame.add(contentPane);
这条语句默认情况下将contentPane放在了BorderLayout.CENTER位置，且frame上没有其它的组件，
因此contentPane覆盖了frame的全部区域。
*/
//取消默认布局即可
this.setLayout(null);
```



### 事件监听 ###

```java
//1.鼠标点击事件监听
MouseListener
//鼠标的移动和拖放
MouseMotionListener
//2.执行命令事件监听
ActionListener
//3.键盘监听
KeyListener

//4.窗口监听
WindowListener
```



### 多个按钮共享一个事件监听 ###

```java
package Demo001;

import javax.swing.*;
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;

public class demo01
{
    public static void main(String[] args)
    {
        new MyJFrame();
    }
}
class MyJFrame extends JFrame
{
    public MyJFrame()
    {
        JButton bt1=new JButton("打印a");
        JButton bt2=new JButton("打印b");
        //新建一个MouseListener类去使用 
        bt1.addMouseListener(new MyMouse());
        bt2.addMouseListener(new MyMouse());
        this.setLayout(new GridLayout(2,1));
        this.add(bt1);
        this.add(bt2);

        //设置标题
        setTitle("Demo1");
        //设置窗体大小
        setSize(800,800);
        //居中
        setLocationRelativeTo(null);
        //设置窗体大小不可变
        setResizable(false);
        //设置退出默认关闭
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        //设置可见
        setVisible(true);
    }
}
class MyMouse extends MouseAdapter
{
    @Override
    public void mouseClicked(MouseEvent e)
    {
        //获取当前点击事件的对象
        JButton bt= (JButton) e.getSource();
        if(bt.getText().equals("打印a")) System.out.println("aaaaaaaaa");
        else System.out.println("bbbbbbbbbbbbbb");
    }
}
```



### 简易计算器面向对象监听类实现 ###

```java
package Demo001;

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class demo01
{
    public static void main(String[] args)
    {
        new MyJFrame();
    }
}
class MyJFrame extends JFrame
{
    TextField  num1,num2,num3;
    public MyJFrame()
    {
        //3个文本框
        num1=new TextField(10);//字符数
        num2=new TextField(10);
        num3=new TextField(20);

        //1个按钮
        JButton bt=new JButton("=");
        bt.addActionListener(new MyAction());
        //1个标签
        JLabel label=new JLabel("+");

        setLayout(new FlowLayout());
        add(num1);
        add(label);
        add(num2);
        add(bt);
        add(num3);

        //设置标题
        setTitle("Demo1");
        //设置窗体大小
        setSize(800,800);
        //居中
        setLocationRelativeTo(null);
        //设置窗体大小不可变
        setResizable(false);
        //设置退出默认关闭
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        //设置可见
        setVisible(true);
    }
    //内部类实现
    class MyAction implements ActionListener
    {
        @Override
        public void actionPerformed(ActionEvent e) {

            int x1=Integer.parseInt(num1.getText());
            int x2=Integer.parseInt(num2.getText());
            num3.setText((x1+x2)+"");
        }
    }
}
```



### 简单画图实现 ###

```java
package Demo001;

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.*;
import java.util.List;

public class demo01
{
    public static void main(String[] args)
    {
        new MyJFrame();
    }
}
class MyJFrame extends JFrame
{
    //获得点的所有位置
    private List<Point> pointarr=new ArrayList<Point>();
    public MyJFrame()
    {
        //给窗体加上鼠标监听器
        this.addMouseMotionListener(new Mymouse());

        //设置标题
        setTitle("Demo1");
        //设置窗体大小
        setSize(800,800);
        //居中
        setLocationRelativeTo(null);
        //设置窗体大小不可变
        setResizable(false);
        //设置退出默认关闭
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        //设置可见
        setVisible(true);
    }
    //画笔
    @Override
    public void paint(Graphics g)
    {
        super.paint(g);
        g.setColor(Color.RED);
        Iterator<Point> it=pointarr.listIterator();
        //遍历点，绘制线条
        while(it.hasNext())
        {
            Point p=it.next();
            g.fillOval(p.x,p.y,10,10);
        }
    }
    private class Mymouse extends MouseMotionAdapter
    {
        @Override
        public void mouseDragged(MouseEvent e) {
            super.mouseDragged(e);
            //每次鼠标移动，便将点记录下来
            pointarr.add(new Point(e.getX(),e.getY()));
            //每次都需要重绘一遍
            repaint();
        }
    }

}

```

### 上下左右简单实现 ###

```java
package Demo001;

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.*;
import java.util.List;

public class demo01
{
    public static void main(String[] args)
    {
        new MyJFrame();
    }
}
class MyJFrame extends JFrame
{
    //获得点的所有位置
    private List<Point> pointarr=new ArrayList<Point>();
    public MyJFrame()
    {
        this.addKeyListener(new KeyAdapter() {
            @Override
            public void keyPressed(KeyEvent e) {
                super.keyPressed(e);
                //根据按下的键位不同，产生不同的keycode
                int keycode=e.getKeyCode();
                //对内置的KeyEvent中键位值进行比较即可
                if(keycode==KeyEvent.VK_UP)
                {
                    System.out.println("你按下了上键");
                }
                else if(keycode==KeyEvent.VK_DOWN)
                {
                    System.out.println("你按下了下键");
                }
                else if(keycode==KeyEvent.VK_LEFT)
                {
                    System.out.println("你按下了左键");
                }
                else if(keycode==KeyEvent.VK_RIGHT)
                {
                    System.out.println("你按下了右键");
                }
            }
        });

        //设置标题
        setTitle("Demo1");
        //设置窗体大小
        setSize(800,800);
        //居中
        setLocationRelativeTo(null);
        //设置窗体大小不可变
        setResizable(false);
        //设置退出默认关闭
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        //设置可见
        setVisible(true);
    }
}
```

