---
layout: post
title: "理解 JAVA 中的 ANNOTATIONS"
description: ""
category: java
tags: [java, annotation]
---
{% include JB/setup %}

**Annotations** 自 J2SE 5.0 它就已经出现，现在看来它早已成为 Java 中一个非常重要的部分。

Annotations 是「元数据」，即描述数据的数据。所以 Annotations 就是代码的元数据。也就是说，**Annotations 是用来描述代码的**。

这里使用「描述」这个字眼也是有原因的，因为 Annotations 仅仅是描述代码而已，并不会做任何的逻辑操作。要想让这些 Annotations 变得真正有意义，那么我们需要在某个地方来使用这些 Annotation。要想获得一个类的 Annotations 则需要用到「反射」。

例如我声明了一个 `@Todo` 的 Annotation，那么当我说一个函数 `@Todo` 时，只是说这个函数「Need Todo」，而事实上如果不使用反射获取相关的 Annotations，那和没使用 Annotations 是效果是一样的。

声明一个 Annotation：


    public @interface Todo {
        String value();
    }


使用 Annotation 标记一个函数


    public class OneClass {
    
        @Todo("Hehe")
        public void oneMethod() {
        
        }
    }


这个时候的 `@Todo` 只是描述了 `oneMethod` 需要 *Todo*，而没有任何别的作用了。我们想要让这个 Annotation 做些什么，则需要去写相关的代码，取出 Annotations 进行判断，然后才能去做之后的事情。


    public class TodoChecker {
        public static void check() {  
            Class oneClass = OneClass.class;
            for (Method method : oneClass.getMethods()) {
                Todo todoAnnotations = (Todo)method.getAnnotation(Todo.class);
                if (todoAnnotations != null) {
                    System.out.println("Value: " + todoAnnotation.value());
                }
            }
        }
    }


于是，这个时候，我们总算让 Annotations 有了「一点意义」。

##### 延伸阅读

* [HOW ANNOTATIONS WORK IN JAVA ](http://idlebrains.org/tutorials/java-tutorials/how-annotations-work-java/)