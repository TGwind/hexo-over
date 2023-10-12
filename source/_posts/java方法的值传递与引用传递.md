---
title: Java方法的值传递与引用传递
---

```java
public class test {
    public static void main(String[] args) {
        student stu1 = new student();
        stu1.setName("aaa");
        System.out.println("调用方法前："+"stu1.name="+stu1.getName());
        change(stu1);
        System.out.println("调用方法后："+"stu1.name="+stu1.getName());
    }
    public static void change(student stu){
        stu.setName("bbb");
    }
}

```

输出结果：

<img src="http://myblog.over2022.top/image-20221021233344246.png" alt="image-20221021233344246" style="zoom:150%;" />

```java
public class test {
    public static void main(String[] args) {
        student stu1 = new student();
        stu1.setName("aaa");
        System.out.println("调用方法前："+"stu1.name="+stu1.getName());
        change(stu1);
        System.out.println("调用方法后："+"stu1.name="+stu1.getName());
    }
    public static void change(student stu){
        stu = new student();
        stu.setName("ccc");
    }
}
```

输出：

![image-20221021233522528](http://myblog.over2022.top/image-20221021233522528.png)

![image-20221022001159795](http://myblog.over2022.top/image-20221022001159795.png)
