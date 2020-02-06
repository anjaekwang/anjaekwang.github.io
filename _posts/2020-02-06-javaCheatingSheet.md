---
title: "[Java] Java Cheat Sheet"
layout: post
date: 2020-02-06 15:13
tag:
- JAVA
- notes
category: blog
author: jaekwang
description: Java Cheating Sheet 정리
---

# Java Cheating Sheet

자바는 모두 객체로 이뤄졌다 생각.  
아래 메소드를 사용하기위해선 기본형자료형선언이 아닌 참조형으로!(Wrapper)  
**import java.util.원하는것;**

## Stack  
import.java.util.Stack;
stack<참조형> s = new Stack<>();

### 추가 및 삭제
~~~
s.push(element);                       // top에 원소추가
s.pop();                               // top원소삭제
~~~

### 조회
~~~
s.peek();                              // top의 원소조회
~~~

### 기타
~~~
s.search(n);                           // n이 stack에 몇번째에 위치하는지
s.empty();                             // stack이 비워있는지?
s.size();                              // stack size 리턴
~~~

---

## Queue

import.java.util.Queue;
import.java.util.LinkedList;
queue<참조형> q = new LinkedList<참조형>();

### 추가 및 삭제
~~~
q.offer(element);                       // 뒤에 원소추가
q.poll();                               // 앞에 원소삭제
~~~

### 조회
~~~
q.peek();                               // 앞 원소접근
~~~

### 기타
~~~
q.empty();                             // queue가 비워있는지?
q.size();                              // queue size 리턴
~~~
---

## ArrayList

C++ STL의 vector대신 사용
import java.util.ArrayList;
ArrayList<참조형> v = new ArrayList<참조형>();

### 추가 및 삭제
~~~
v.add(element);                        // 뒷부분에 원소추가
v.add(idx, element);                   // v[i]부분에 원소삽입
v.remove(idx);                         // v[i] 원소삭제
~~~

### 조회
~~~
v.get(idx);                            // i번째 원소접근
~~~

### 기타
~~~
v.isEmpty();                           // Vector 비워있는지?
v.size();                              // 벡터사이즈
~~~

---

## 문자열 처리

### 입력받기
~~~
import java.util.Scanner;
Scanner sc = new Scanner(System.in);
var a = newInt자료형();                // int,double등 사용한자료형에 맞는
var b = nextLine();                    // 한줄 string으로 받는다
                                       // 즉 개행문자까지 받는다
                                       // 앞에서 어떠한 nextInt()를 받고
                                       // nextLine()시 그냥 넘어간다
                                       // 입력버퍼에 개행문자가 남아있어서.
var c = next();                        // 화이트스페이스 기준으로 한단어 입력받음
~~~

### String 메소드 정리

String은 변하지 않는 문자열을 다룰때 사용.  
변하는 문자열을 사용시엔 StringBuffer를 사용한다.
~~~
s.length();                            // 문자열 길이
s.subString(idx);                      // 문자열 복사, idx부터 끝까지
s.subString(idx1, idx2);               // 복사, idx1부터 idx2까지
s.compareTo(s2);                       // s와 s2 문자열 비교
s.equals(s2)                           // s와 s2 문자열 같은지
s1 += s2;                              // 문자열 변경
s.indexOf(문자(열));                    // 해당 문자(열)을 시작하는 idx 반환
~~~

---

## HashMap

import java.util.HashMap;
map<key자료형, value자료형> m = new HashMap<>();

### 추가 및 삭제
~~~
m.put(key, value);                     // key,value쌍을 추가
m.remove(key);                         // key해당하는것 삭제
m.clear();
~~~

### 조회
~~~
m.get(key);                            // key에 해당하는 value 반환
                                       // 없을시 null 반환
~~~

### 기타
~~~
m.isEmpty();
m.size();
~~~

---

## Sorting

자바배열 또한 객체로   
int[] arr = new int[길이]; arr[1] = 1; 으로 사용하거나  
int[10] arr = {1,2,3, ..}; 로 사용.  
~~~
arr.length;                                         // 길이
Arrays.sort(arr);                                   // 배열정렬
Arrays.sort(arr, Collections.reverseOrder());       
Collections.sort(ArrayList);                        // ArrayList 정렬
Collections.sort(ArrayList, new Ascending자료형());

~~~

---

~~~
Integer.toString(숫자);                 // 숫자를 문자열로
Integer.parseInt(String);               // 문자열을 숫자로
~~~

## Wrapper Class

기본형 타입 변수를 객체로 사용해야하는 경우에 사용  
-> Callby reference와 같이 참조형으로 다뤄야 할때  
Integer age = new Integer(30);  
age.intValue(); 하면 다시 기본형으로..  
