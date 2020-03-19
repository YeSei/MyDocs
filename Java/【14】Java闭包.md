# 【11】Java闭包
总结：
- Java闭包是一个可调用的对象，它记录了一些信息，这些信息来自于创建它的作用域。由函数和引用环境组合成的实体对象。
- 当父类和要实现的接口具有相同的方法名。需要采用闭包来解决，使用内部类实现接口。还可以用内部类实现多继承。静态内部类实现安全单例模式。
- 闭包会导致资源不会被回收。

## 11.1 自由变量
> 在学习闭包概念之前，需要先了解自由变量的概念。
> 自由变量：未在当前作用域中声明的变量。（即在其他作用域中声明的变量）

```
public class Demo1｛
private int x = 0;
void f1(){
    int res = 1 + x;
    System.out.println(x);
    }
｝
```
> 以上代码中的`res`使用的`x`未在`f1()`中定义，`x`即为自由变量。

## 11.2 Java内部类

### 11.2.1 内部类的定义

> Java内部类是在类的内部再定义一个类。
> 这个内部类通过`this`引用外部类的变量。并且内部类需要依附于外部类而存在。通过外部类对象的`.new`来创建内部类对象。

```
public class Outer {
	private int y = 5;
    
	private class Inner {
		private int x = 10;
		public int add() {
			return x + y;
		}
        
        public Outer getOuter(){
            return Outer.this;
        }
	}
}
```
> 此处，内部类`Inner`中`add`方法持有了内部类作用域的自由变量`x`，形成闭包。

### 11.2.2 内部类的作用
> 内部类最大的作用是每个内部类都能继承一个父类，实现类的多重继承。

```
public class TestClass extends AbstractFather {
    @Override
    public String sayHello() {
        return fatherName;
    }

    class TestInnerClass extends AbstractMother {
        @Override
        public String sayHello() {
            return motherName;
        }
    }
}
```
> `AbstractFathor`和`AbstractMother`都含有`sayHello`方法。

### 11.2.3 内部类的分类
1. 局部内部类：定义在方法内部，或类似if...代码块中。
```
public class Parcel5 {
public Destionation destionation(String str){
    class PDestionation implements Destionation{
        private String label;
        private PDestionation(String whereTo){
            label = whereTo;
        }
        public String readLabel(){
            return label;
        }
    }
    return new PDestionation(str);
}
```

2. 静态内部类：使用了static修饰的内部类即为静态内部类，和普通的成员内部类最大的不同是，静态内部类没有了指向外围类的引用。因此，它的创建不需要依赖于外围类，但也不能够使用任何外围类的非static成员变量和方法。
```
public class Singleton {  
    private static class SingletonHolder {  
        private static final Singleton INSTANCE = new Singleton();  
    }  
    private Singleton (){}  
    public static final Singleton getInstance() {  
        return SingletonHolder.INSTANCE; 
    }  
}
```
> **静态类最大的作用是创建线程安全的单例模式。**

3. 匿名内部类：匿名内部类就是没有被命名的内部类，当我们需要快速创建多个Thread的时候，经常会使用到它：
```
new Thread(new Runnable() {
        @Override
        public void run() {
            System.out.println("hello");
        }
    }).start();
```
> 匿名内部类没有访问修饰符，也没有构造方法。
> 匿名内部类依附于接口而存在，如果实现的接口不存在，那么他就无法创建。
> 如果匿名内部类要访问所在方法的自由变量，那么这个自由变量要被final修饰。

## 11.3 Java闭包
> 闭包由函数+引用环境构成。真正的闭包在函数引用的变量是在栈中时，会将该变量移到堆内存中，赋值引用即可。因为Java是值传递，不支持引用传递，
> 所以Java闭包算是半个闭包。Java对于引用的自由变量的处理方式是使用将该值拷贝一份到函数中的方式。
