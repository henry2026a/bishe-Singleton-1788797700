# 设计模式-单例 Singleton

> **发布时间**: 2025-05-28
> **标签**: 无

---

## 📞 联系方式
> 💬 **微信：sen232sen** | 🚀 **专业定制各类管理系统** | ⭐ **源码获取/技术支持**

---

场景：重量级的对象，不需要多个实例，如线程池、数据库连接池  
单一职责：一个类和方法只做一件事。  
开闭原则：对修改关闭，对扩展开发。  
里氏替换原则：子类可扩展新方法，但尽量不要重写父类已有方法（注意是尽量而非绝对不可，实际中重写父类方法还是很常见的），避免多态调用时出现程序错误。  
依赖倒置：依赖于抽象，而非具体实现，即面向接口编程（如方法参数，类属性使用接口声明，这样可接收任何子类）。  
接口隔离：一个接口只干一件事，降低功能耦合。  
最少知道/迪米特原则：降低类之间的依赖，聚合，组合等。

### 懒汉（延迟加载）

    public class Mgr03 {
        private static Mgr03 INSTANCE;
    
        private Mgr03(){
        }
    
        //在调用时才初始化,同步代码块来增加效率
        public synchronized static Mgr03 getInstance(){
            if (INSTANCE == null){
                INSTANCE = new Mgr03();
            }
            return INSTANCE;
        }
    
    }

因为有加锁，所以这个时候会出现性能的问题，于是可以引入双重检索的机制缩小同步代码块

    public class Mgr06 {
        private static Mgr06 INSTANCE;
    
        private Mgr06(){
        }
    
        //在调用时才初始化,同步代码块来增加效率
        public  static Mgr06 getInstance(){
            //可以解决已经初始化了还有线程进来的问题
            if (INSTANCE == null){
                //锁在里面，线程可能会同时进到这里
                synchronized(Mgr06.class){
                    //双重检索
                    if (INSTANCE == null){
                        INSTANCE = new Mgr06();
                    }
                }
            }
            return INSTANCE;
        }
    }

也可以用静态内部类来实现，利用JVM保证线程安全的问题，静态内部类持有对象，只会加载初始化一次

    public class Mgr07 {
    
        private Mgr07(){
    
        }
        
        public Mgr07 getInstance (){
            return Mgr07Holder.INSTANCE;
        }
        private static class Mgr07Holder{
            private static final Mgr07 INSTANCE = new Mgr07();
        }
    }

最好的办法就是使用枚举，可以解决反射的问题

    public enum Mgr08 {
        INSTANCE;
    
        public void m(){}
    }

### 饿汉（立即加载）

类加载到内存后，就实例化一个单例，jvm保证线程安全

    public class Mgr01 {
        private static final Mgr01 INSTANCE = new Mgr01();
        //单例最重要的一点，私有化构造函数，不让外面的人调用
        private Mgr01(){
    
        }
    
        public static Mgr01 getInstance(){
            return INSTANCE;
        }
    
    }

最简单也是最常用的方法。设计模式的核心是考虑设计，而不是考虑各种极端的场景

---

![sensen](images/sensen.png)
