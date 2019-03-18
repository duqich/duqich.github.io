---
layout: post
title:  "Stack和Queue实现"
date:   2019-03-14 14:21:49
categories: algorithm
tags: 算法 
---

## 1.目的
- Stack和Queue是最基础的数据结构，也是非常常用的数据结构，因为java没有比较简单的实现，所以需要自己构造；

## 2.Stack的API
- **维护成员变量**
```java
    private Node head;
    private int N;
```
- **需要一个普通节点类**

```java
public class Node<T> {
    T data;
    Node next;
    public Node(T data){
        this.data = data;
    }
    public Node(){}

}
```

|描述|方法|
|-|-|
|入栈|void push(T t)|
|出栈|T pop()|
|判断栈是否为空|boolean isEmpty()|
|返回栈的元素个数|int size()|
|符合出栈规则的迭代器|Iterator<T> iterator()|

## 3.Stack实现代码

```java
package struct;

import java.util.Iterator;

public class Stack<T> implements Iterable<T>{
    private Node head;
    private int N;

    public Stack() {
        head = new Node();
    }

    public void push(T t){
        Node tmp = head.next;
        head.next = new Node(t);
        head.next.next = tmp;
        N++;
    }
    public T pop(){
        if (isEmpty()) return null;
        Node tmp = head.next;
        head.next = tmp.next;
        N--;
        return (T)tmp.data;
    }

    public boolean isEmpty(){
        return head.next==null;
    }
    public int size(){
        return N;
    }

    public Iterator<T> iterator() {
        return new StackIterator();
    }
    private class StackIterator implements Iterator{
        Node cur;
        public StackIterator() {
            cur = head;
        }

        public boolean hasNext() {
            return cur.next!=null;
        }

        public T next() {
            Node tmp = cur.next;
            cur = cur.next;
            return (T)tmp.data;
        }
        public void remove() { }
    }


}
```

## 4.Queue
- Queue和Stack都是差不多的api和实现办法

```java
package struct;

import java.util.Iterator;

public class Queue<T> implements Iterable<T>{
    private Node head;
    private int N;

    public Queue() {
        head = new Node();
    }

    public void enqueue(T t){
        Node cur = head;
        while (cur.next!=null){
            cur = cur.next;
        }
        cur.next = new Node(t);
        N++;
    }
    public T dequeue(){
        if (isEmpty()) return null;
        Node cur = head.next;
        head.next = cur.next;
        N--;
        return (T)cur.data;
    }

    public boolean isEmpty(){
        return head.next==null;
    }
    public int size(){
        return N;
    }

    public Iterator<T> iterator() {
        return new QueueIterator();
    }

    private class QueueIterator<T> implements Iterator{
        private Node cur;
        public QueueIterator() {
            cur = head;
        }

        public boolean hasNext() {
            return cur.next!=null;
        }

        public T next() {
            Node tmp = cur.next;
            cur = cur.next;
            return (T)tmp.data;
        }

        public void remove() { }
    }
}

```



