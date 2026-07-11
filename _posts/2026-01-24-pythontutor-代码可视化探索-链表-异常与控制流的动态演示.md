---
layout: post
title: "pythontutor - 代码可视化探索：链表、异常与控制流的动态演示"
date: 2026-01-24
tags: ["后端", "每天一个知识点", "Python", "树莓派", "Arduino", "开源"]
---

# PythonTutor 是什么

PythonTutor 是一个用于展示代码执行过程的可视化学习工具。它关注的不是程序最终输出的结果，而是代码在运行过程中每一步是如何执行的、变量和对象在内存中如何变化。

***

## PythonTutor 的核心价值

PythonTutor 会逐行执行代码，并实时展示：

*   当前正在执行的代码行
*   变量在不同阶段的取值变化
*   对象在内存中的分配情况
*   引用关系、指针指向和调用栈结构

这种方式可以帮助开发者真正理解程序的执行逻辑，而不仅仅是“跑通代码”。

***

## 可视化变量和内存结构

在很多语言中，变量本质上是对对象的引用。仅通过日志或打印输出，很难直观理解多个变量是否指向同一个对象。

PythonTutor 会将这些关系以图形形式展示出来，尤其适合理解以下概念：

*   引用与对象的关系
*   链表和指针结构
*   对象共享与副作用
*   栈与堆内存的变化

***

## Java 链表示例

下面是一个完整、可直接运行的 Java 单向链表示例，适合放入 PythonTutor 的 Java 模式中观察指针和对象的变化。

![链表虚拟化.gif](/assets/images/2026/01/链表虚拟化.gif)

```java
import java.util.Objects;

public class LinkedListDemo {

    static class Node {
        int value;
        Node next;

        Node(int value) {
            this.value = value;
        }
    }

    public static void main(String[] args) {
        Node head = buildList();
        printList(head);

        head = delete(head, 2);
        printList(head);

        System.out.println("是否包含 3: " + contains(head, 3));
        System.out.println("是否包含 5: " + contains(head, 5));

        head = reverse(head);
        printList(head);
    }

    static Node buildList() {
        Node n1 = new Node(1);
        Node n2 = new Node(2);
        Node n3 = new Node(3);
        Node n4 = new Node(4);

        n1.next = n2;
        n2.next = n3;
        n3.next = n4;

        return n1;
    }

    static Node delete(Node head, int target) {
        if (head == null) {
            return null;
        }
        if (head.value == target) {
            return head.next;
        }

        Node current = head;
        while (current.next != null) {
            if (current.next.value == target) {
                current.next = current.next.next;
                break;
            }
            current = current.next;
        }
        return head;
    }

    static boolean contains(Node head, int target) {
        Node current = head;
        while (current != null) {
            if (current.value == target) {
                return true;
            }
            current = current.next;
        }
        return false;
    }

    static Node reverse(Node head) {
        Node prev = null;
        Node current = head;

        while (current != null) {
            Node nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }
        return prev;
    }

    static void printList(Node head) {
        Node current = head;
        while (current != null) {
            System.out.print(current.value + " -> ");
            current = current.next;
        }
        System.out.println("null");
    }
}
```

复制下面的内容在浏览器地址访问

    https://pythontutor.com/render.html#code=import%20java.util.Objects%3B%0A%0Apublic%20class%20LinkedListDemo%20%7B%0A%0A%20%20%20%20//%20%E5%8D%95%E9%93%BE%E8%A1%A8%E8%8A%82%E7%82%B9%0A%20%20%20%20static%20class%20Node%20%7B%0A%20%20%20%20%20%20%20%20int%20value%3B%0A%20%20%20%20%20%20%20%20Node%20next%3B%0A%0A%20%20%20%20%20%20%20%20Node%28int%20value%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20this.value%20%3D%20value%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%0A%20%20%20%20private%20Node%20head%3B%0A%0A%20%20%20%20//%20%E5%B0%BE%E6%8F%92%E6%B3%95%E6%B7%BB%E5%8A%A0%E8%8A%82%E7%82%B9%0A%20%20%20%20public%20void%20add%28int%20value%29%20%7B%0A%20%20%20%20%20%20%20%20Node%20newNode%20%3D%20new%20Node%28value%29%3B%0A%0A%20%20%20%20%20%20%20%20if%20%28head%20%3D%3D%20null%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20head%20%3D%20newNode%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20return%3B%0A%20%20%20%20%20%20%20%20%7D%0A%0A%20%20%20%20%20%20%20%20Node%20cur%20%3D%20head%3B%0A%20%20%20%20%20%20%20%20while%20%28cur.next%20!%3D%20null%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20cur%20%3D%20cur.next%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20cur.next%20%3D%20newNode%3B%0A%20%20%20%20%7D%0A%0A%20%20%20%20//%20%E5%88%A0%E9%99%A4%E6%8C%87%E5%AE%9A%E5%80%BC%E7%9A%84%E8%8A%82%E7%82%B9%0A%20%20%20%20public%20void%20remove%28int%20value%29%20%7B%0A%20%20%20%20%20%20%20%20if%20%28head%20%3D%3D%20null%29%20return%3B%0A%0A%20%20%20%20%20%20%20%20if%20%28head.value%20%3D%3D%20value%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20head%20%3D%20head.next%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20return%3B%0A%20%20%20%20%20%20%20%20%7D%0A%0A%20%20%20%20%20%20%20%20Node%20cur%20%3D%20head%3B%0A%20%20%20%20%20%20%20%20while%20%28cur.next%20!%3D%20null%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20if%20%28cur.next.value%20%3D%3D%20value%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20cur.next%20%3D%20cur.next.next%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20return%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20cur%20%3D%20cur.next%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%0A%20%20%20%20//%20%E5%88%A4%E6%96%AD%E6%98%AF%E5%90%A6%E5%8C%85%E5%90%AB%E6%9F%90%E4%B8%AA%E5%80%BC%0A%20%20%20%20public%20boolean%20contains%28int%20value%29%20%7B%0A%20%20%20%20%20%20%20%20Node%20cur%20%3D%20head%3B%0A%20%20%20%20%20%20%20%20while%20%28cur%20!%3D%20null%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20if%20%28Objects.equals%28cur.value,%20value%29%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20return%20true%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20cur%20%3D%20cur.next%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20return%20false%3B%0A%20%20%20%20%7D%0A%0A%20%20%20%20//%20%E6%89%93%E5%8D%B0%E9%93%BE%E8%A1%A8%0A%20%20%20%20public%20void%20print%28%29%20%7B%0A%20%20%20%20%20%20%20%20Node%20cur%20%3D%20head%3B%0A%20%20%20%20%20%20%20%20while%20%28cur%20!%3D%20null%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20System.out.print%28cur.value%20%2B%20%22%20-%3E%20%22%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20cur%20%3D%20cur.next%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20System.out.println%28%22null%22%29%3B%0A%20%20%20%20%7D%0A%0A%20%20%20%20//%20%E5%8F%8D%E8%BD%AC%E9%93%BE%E8%A1%A8%0A%20%20%20%20public%20void%20reverse%28%29%20%7B%0A%20%20%20%20%20%20%20%20Node%20prev%20%3D%20null%3B%0A%20%20%20%20%20%20%20%20Node%20cur%20%3D%20head%3B%0A%0A%20%20%20%20%20%20%20%20while%20%28cur%20!%3D%20null%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20Node%20nextTemp%20%3D%20cur.next%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20cur.next%20%3D%20prev%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20prev%20%3D%20cur%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20cur%20%3D%20nextTemp%3B%0A%20%20%20%20%20%20%20%20%7D%0A%0A%20%20%20%20%20%20%20%20head%20%3D%20prev%3B%0A%20%20%20%20%7D%0A%0A%20%20%20%20//%20%E6%B5%8B%E8%AF%95%E5%85%A5%E5%8F%A3%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20LinkedListDemo%20list%20%3D%20new%20LinkedListDemo%28%29%3B%0A%0A%20%20%20%20%20%20%20%20list.add%281%29%3B%0A%20%20%20%20%20%20%20%20list.add%282%29%3B%0A%20%20%20%20%20%20%20%20list.add%283%29%3B%0A%20%20%20%20%20%20%20%20list.add%284%29%3B%0A%0A%20%20%20%20%20%20%20%20System.out.println%28%22%E5%8E%9F%E5%A7%8B%E9%93%BE%E8%A1%A8%3A%22%29%3B%0A%20%20%20%20%20%20%20%20list.print%28%29%3B%0A%0A%20%20%20%20%20%20%20%20System.out.println%28%22%E5%88%A0%E9%99%A4%202%3A%22%29%3B%0A%20%20%20%20%20%20%20%20list.remove%282%29%3B%0A%20%20%20%20%20%20%20%20list.print%28%29%3B%0A%0A%20%20%20%20%20%20%20%20System.out.println%28%22%E6%98%AF%E5%90%A6%E5%8C%85%E5%90%AB%203%3A%20%22%20%2B%20list.contains%283%29%29%3B%0A%20%20%20%20%20%20%20%20System.out.println%28%22%E6%98%AF%E5%90%A6%E5%8C%85%E5%90%AB%205%3A%20%22%20%2B%20list.contains%285%29%29%3B%0A%0A%20%20%20%20%20%20%20%20System.out.println%28%22%E5%8F%8D%E8%BD%AC%E9%93%BE%E8%A1%A8%3A%22%29%3B%0A%20%20%20%20%20%20%20%20list.reverse%28%29%3B%0A%20%20%20%20%20%20%20%20list.print%28%29%3B%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false

## Java 中的 try catch finally 示例

这个示例适合用来观察异常发生时控制流的变化，以及 finally 代码块一定会被执行的特性。

    public class TryCatchDemo {

        // 模拟一个需要手动释放的资源
        static class FakeResource {
            void open() {
                System.out.println("资源已打开");
            }

            void use() {
                System.out.println("正在使用资源");
                // 人为制造异常
                int x = 10 / 0;
            }

            void close() {
                System.out.println("资源已释放");
            }
        }

        public static void main(String[] args) {
            FakeResource resource = new FakeResource();

            try {
                resource.open();
                resource.use();
                System.out.println("这行代码不会被执行");
            } catch (ArithmeticException e) {
                System.out.println("捕获到异常: " + e.getMessage());
            } finally {
                // 无论是否发生异常，都会执行
                resource.close();
                System.out.println("finally 块执行完成");
            }
        }
    }

在 PythonTutor 中可以清楚看到：

*   异常发生后直接跳转到 catch
*   try 中剩余代码不再执行
*   finally 始终会被执行

复制下面的内容在浏览器地址访问

    https://pythontutor.com/render.html#code=public%20class%20TryCatchDemo%20%7B%0A%0A%20%20%20%20//%20%E6%A8%A1%E6%8B%9F%E4%B8%80%E4%B8%AA%E9%9C%80%E8%A6%81%E6%89%8B%E5%8A%A8%E9%87%8A%E6%94%BE%E7%9A%84%E8%B5%84%E6%BA%90%0A%20%20%20%20static%20class%20FakeResource%20%7B%0A%20%20%20%20%20%20%20%20void%20open%28%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28%22%E8%B5%84%E6%BA%90%E5%B7%B2%E6%89%93%E5%BC%80%22%29%3B%0A%20%20%20%20%20%20%20%20%7D%0A%0A%20%20%20%20%20%20%20%20void%20use%28%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28%22%E6%AD%A3%E5%9C%A8%E4%BD%BF%E7%94%A8%E8%B5%84%E6%BA%90%22%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20//%20%E4%BA%BA%E4%B8%BA%E5%88%B6%E9%80%A0%E5%BC%82%E5%B8%B8%0A%20%20%20%20%20%20%20%20%20%20%20%20int%20x%20%3D%2010%20/%200%3B%0A%20%20%20%20%20%20%20%20%7D%0A%0A%20%20%20%20%20%20%20%20void%20close%28%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28%22%E8%B5%84%E6%BA%90%E5%B7%B2%E9%87%8A%E6%94%BE%22%29%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20FakeResource%20resource%20%3D%20new%20FakeResource%28%29%3B%0A%0A%20%20%20%20%20%20%20%20try%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20resource.open%28%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20resource.use%28%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28%22%E8%BF%99%E8%A1%8C%E4%BB%A3%E7%A0%81%E4%B8%8D%E4%BC%9A%E8%A2%AB%E6%89%A7%E8%A1%8C%22%29%3B%0A%20%20%20%20%20%20%20%20%7D%20catch%20%28ArithmeticException%20e%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28%22%E6%8D%95%E8%8E%B7%E5%88%B0%E5%BC%82%E5%B8%B8%3A%20%22%20%2B%20e.getMessage%28%29%29%3B%0A%20%20%20%20%20%20%20%20%7D%20finally%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20//%20%E6%97%A0%E8%AE%BA%E6%98%AF%E5%90%A6%E5%8F%91%E7%94%9F%E5%BC%82%E5%B8%B8%EF%BC%8C%E9%83%BD%E4%BC%9A%E6%89%A7%E8%A1%8C%0A%20%20%20%20%20%20%20%20%20%20%20%20resource.close%28%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28%22finally%20%E5%9D%97%E6%89%A7%E8%A1%8C%E5%AE%8C%E6%88%90%22%29%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false

***

## Java 中的 switch case （命中穿透）示例

下面这个示例将 break 删除了，适合观察分支匹配和 break 对流程的影响。

        public class SwitchDemo {

            public static void main(String[] args) {
                int day = 1;

                System.out.println("day = " + day);
                System.out.println("开始执行 switch");

                switch (day) {
                    case 1:
                        System.out.println("命中 case 1：星期一");
                        // break;
                    case 2:
                        System.out.println("命中 case 2：星期二");
                        // break;
                    case 3:
                        System.out.println("命中 case 3：星期三");
                        // break;
                    default:
                        System.out.println("命中 default：未知日期");
                        // break;
                }

                System.out.println("switch 执行结束");
            }
        }

通过可视化可以看到：

*   switch 表达式只计算一次
*   第一个匹配的 case 就会被执行
*   缺少break 会导致命中穿透。

复制下面的内容在浏览器地址访问

    https://pythontutor.com/render.html#code=public%20class%20SwitchDemo%20%7B%0A%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20int%20day%20%3D%201%3B%0A%0A%20%20%20%20%20%20%20%20System.out.println%28%22day%20%3D%20%22%20%2B%20day%29%3B%0A%20%20%20%20%20%20%20%20System.out.println%28%22%E5%BC%80%E5%A7%8B%E6%89%A7%E8%A1%8C%20switch%22%29%3B%0A%0A%20%20%20%20%20%20%20%20switch%20%28day%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20case%201%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28%22%E5%91%BD%E4%B8%AD%20case%201%EF%BC%9A%E6%98%9F%E6%9C%9F%E4%B8%80%22%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20//%20break%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20case%202%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28%22%E5%91%BD%E4%B8%AD%20case%202%EF%BC%9A%E6%98%9F%E6%9C%9F%E4%BA%8C%22%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20//%20break%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20case%203%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28%22%E5%91%BD%E4%B8%AD%20case%203%EF%BC%9A%E6%98%9F%E6%9C%9F%E4%B8%89%22%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20//%20break%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20default%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28%22%E5%91%BD%E4%B8%AD%20default%EF%BC%9A%E6%9C%AA%E7%9F%A5%E6%97%A5%E6%9C%9F%22%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20//%20break%3B%0A%20%20%20%20%20%20%20%20%7D%0A%0A%20%20%20%20%20%20%20%20System.out.println%28%22switch%20%E6%89%A7%E8%A1%8C%E7%BB%93%E6%9D%9F%22%29%3B%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false

***

## while 循环与 continue、break 示例

这个示例非常适合在 PythonTutor 中观察循环变量的变化，以及 continue 和 break 对流程的影响。

        public class WhileDemo {

            public static void main(String[] args) {
                int i = 0;

                while (i < 5) {
                    i++;

                    if (i == 2) {
                        continue;
                    }

                    if (i == 4) {
                        break;
                    }

                    System.out.println("当前 i = " + i);
                }
            }
        }

在可视化过程中可以看到：

*   continue 会跳过本轮循环剩余代码
*   break 会直接跳出整个循环
*   i 的变化过程清晰可见

<!---->

    https://pythontutor.com/render.html#code=public%20class%20WhileDemo%20%7B%0A%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20int%20i%20%3D%200%3B%0A%0A%20%20%20%20%20%20%20%20while%20%28i%20%3C%205%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20i%2B%2B%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20if%20%28i%20%3D%3D%202%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20continue%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20if%20%28i%20%3D%3D%204%29%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20break%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20System.out.println%28%22%E5%BD%93%E5%89%8D%20i%20%3D%20%22%20%2B%20i%29%3B%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false

***

## Python 中的变量引用示例

下面是一个可以直接运行的 Python 示例，用于理解变量和对象之间的关系。

![python 虚拟化.gif](/assets/images/2026/01/python%20虚拟化.gif)

        a = [1, 2, 3]
        b = a

        print("a:", a)
        print("b:", b)

        b.append(4)

        print("修改 b 之后")
        print("a:", a)
        print("b:", b)

在 PythonTutor 中可以直观看到：

*   a 和 b 指向同一个列表对象
*   对 b 的修改会同步影响 a
*   内存中只存在一个列表实例

复制下面的内容在浏览器地址访问

    https://pythontutor.com/render.html#code=a%20%3D%20%5B1,%202,%203%5D%0Ab%20%3D%20a%0A%0Aprint%28%22a%3A%22,%20a%29%0Aprint%28%22b%3A%22,%20b%29%0A%0Ab.append%284%29%0A%0Aprint%28%22%E4%BF%AE%E6%94%B9%20b%20%E4%B9%8B%E5%90%8E%22%29%0Aprint%28%22a%3A%22,%20a%29%0Aprint%28%22b%3A%22,%20b%29&cumulative=false&curInstr=0&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=311&rawInputLstJSON=%5B%5D&textReferences=false

***

## 适合使用 PythonTutor 的场景

PythonTutor 特别适合用于：

*   学习基础语法和控制流程
*   理解异常处理逻辑
*   掌握循环与分支的执行细节
*   学习数据结构和引用模型

它并不是调试大型项目的工具，而是帮助建立正确执行认知的学习辅助工具。

***

## 总结

PythonTutor 通过可视化的方式，把隐藏在运行时内部的执行过程完整呈现出来。无论是链表、异常处理、分支判断还是循环控制，都可以通过图形化的方式被清晰理解，是学习和教学中非常有价值的工具。

## 扩展

IDEA 插件社区里有一个插件`Java Visualizer`可以在 IDEA 里调试时使用类似的效果，单只能下一步，不能往回走，非常好用！

![Java Visualizer.png](/assets/images/2026/01/Java%20Visualizer.png)
