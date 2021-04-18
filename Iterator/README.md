# 迭代器

​		大家都知道自从java5之后提供了十分方便的foreach遍历循环方法，今天尝试如何对自定义的类使用foreach写法，方便遍历：

​		java定义了两个和迭代器有关的接口iterable和iterator，那么这两个是什么关系，网上更多的资料是关于iterator的相关说明，但没有多少和iterable相关的中文资料。

​		既然网上语焉不详，就直接开始动手测试。

##  1. 首先是定义一个类：

代码1

```java
private class ITest implements Iterable<Integer> {
    
}
```
代码2

```java
private class ITest implements Iterator<Integer> {
    
}
```

测试代码：

```java
@Test
public void test(){
	for(Integer i: new ITest){}
}
```

​		(先忽略测试代码中要求实现方法的错误提示)对比代码1和代码2，可以发先，在使用代码1实现时，测试代码并不会报错并提示错误，而使用代码2的实现方式时，测试代码产生了编译错误提示(代码错误提示)。

​		说明要实现foreach写法，需要实现的是Iterable接口。



## 2. Iterable的方法

​		可以看到Iterable中有3个方法：

```java
public interface Iterable<T> {
    Iterator<Integer> iterator();
    void forEach(Consumer<? super T> action);
    Spliterator<Integer> spliterator();
}
```

​		本来以为需要其中仅需要实现iterator方法即可，另外两个方法具有默认实现。

​		在测试中发现，

​		iterator方法，针对foreach写法，

​		而forEach方法，针对new ITest().forEach(x -> {...}) Lambda表达式