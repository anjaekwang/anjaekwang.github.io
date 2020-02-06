---
title: "[C] C++ STL Cheating Sheet"
layout: post
date: 2020-02-06 10:13
tag:
- C
- notes
category: blog
author: jaekwang
description: C++ STL Cheating Sheet 정리
---

# C++ STL Cheating Sheet

## Stack  

#include<stack>  
stack<자료형> s;  
**무조건 s다룰때 Last in First Out, 다른거 못봄**

### 추가 및 삭제
~~~
s.push(element);                      // top에 원소추가
s.pop();                              // top 원소삭제
~~~

### 조회
~~~
s.top();                               // top의 원소접근
~~~

### 기타
~~~
s.empty();                             // stack이 비워있는지?
s.size();                              // stack size 리턴
~~~

**쌍을 찾는 문제에** 자주사용, 최근의 것이 먼저 찾아진는 쌍  
ex) 괄호찾기, 히스토그램에서 가장 큰 직사각형  
-> 아직 쌍이 안찾아진것을 stack에 push하고, 쌍이 찾아지면(top) pop한다.  
(보통 idx를 넣고, 연산), 시간복잡도 줄인다.  
+DFS, simulation이 디버깅 편한

---

## Queue

#include<queue>  
queue<자료형> q;  

### 추가 및 삭제
~~~
q.push(element);                      // 뒤에 원소추가
q.pop();                              // 앞에 원소삭제
~~~

### 조회
~~~
q.front();                            // 앞 원소접근
~~~

### 기타
~~~
q.empty();                             // queue가 비워있는지?
q.size();                              // queue size 리턴
~~~

**BFS에 사용**, 단계를 찾는 문제.

---

## Vector

#include <vector>  
vector<자료형> v;  

### 반복자(iterator)
~~~
v.begin();                             // 시작 반복자 반환
v.end();                               // 끝 반복자 반환
~~~

### 추가 및 삭제
~~~
v.push_back(element);                  // 뒷부분에 원소추가
v.insert(v.begin() + idx, element);    // v[i]부분에 원소삽입
v.erase(v.begin() + idx);              // v[i] 원소삭제
~~~

### 조회
~~~
v[i];                                  // i번째 원소접근
v.front();                             // 앞 원소접근
v.back();                              // 끝 원소접근
~~~

### 기타
~~~
v.empty();                             // Vector 비워있는지?
v.size();                              // 벡터사이즈
~~~
**가변적인 자료형 사용시 자주이용**

---

## 문자열 처리

나는 문자를 다룰땐 **아스키코드** 를 이용하고  
문자열을 다룰땐 **String** 객체를 이용한다.  

### 한줄받기(\n)

입력자체는 입력 버퍼로 부터 문자열을 읽는다.

1. scanf("%[^\n]\n", char배열이름)  
 -> %[abc], a or b or c가 아닌 문제에서 읽어오는거 중단  
 -> %[^abc] a or b or c 문자에서 읽어오는거 중단

2. fgets(배열이름, 배열크기, stdin)  
 -> 끝에 \n 포함해서 저장

3. getline(cin, 배열이름)  
 -> \n 전까지 저장

### char배열 문자열(string.h)와 string객체비교

||char 배열|string 객체|
|:---:|:---:|:---:|
|문자열 길이|strlen(arr)|s.length()|
|문자열 복사|strcpy(des, src)|s.substr(시작위치idx, 글자수)|
|비교|strcmp(arr1, arr2)|부등호|
|문자열 붙이기|strcat(arr1, arr2)| arr1 += arr2|

### 유용 메소드
~~~
s.find("abc");                          // 문자or문자열 시작idx반환
s.insert(idx, "문자(열)");               // idx위치에 문자(열)삽입
s.erase(idx, n);                        // idx위치부터 n개문자 삭제
~~~

state 저장시 string객체도 잘이용중  
1. info[][]에 state 전체(예를들어 지도)를 저장 -> 시간이나 메모리 측면에서 절때불가  
2. 지도전체를 저장하고 yx위치에 따른 방문여부  
    visited[y][x] = true  
    백트래킹 재귀 밑단 visited[y][x] = false
    -> 단계가 더 복잡해지면 3
3. string으로 지도저장  
 -> 숫자125이상 될시 불가능

---

## Map

#include<map>
map<key자료형, value자료형> m;

### 반복자(iterator)
~~~
m.begin();                             // 시작 반복자 반환
m.end();                               // 끝 반복자 반환
~~~

### 추가 및 삭제

cf) pair<int, char> p = make_pair(1, 'a');
    p.first, p.second로 접근
~~~
m.insert(make_pair(key, value));       // key,value쌍을 추가
m[key] = value;                        // key,value쌍을 추가
m.erase(map.find(key));                // 삭제
m.clear();
~~~

### 관련유용 메소드
~~~
m.find(key);                           // key에 해당하는 반복자 반환
m.count(key);                          // key에 해당하는 원소수 반환
                                       // 존재여부 조건문에 자주사용
~~~

### 기타.
~~~
m.empty();
m.size();
~~~

---

# 그외 잘 사용하는 API
~~~
reverse(s.begin(), s.end())            
~~~

## Sort
#include<algorithm>
~~~
sort(arr, arr+sizeof(arr))
sort(v.begin(), v.end())
sort(v.begin(), v.end(), greater())     // 내림차
                                        // v의 element가 pair시 first를 기준으로
~~~

## 구조체 정렬
~~~
struct temp{
  int a;
  int b;
  temp(int a, int b):a(a), b(b)()       // 생성자
}
~~~
의 sorting을 위해선 기준을 정해줘야한다.  
~~~
//c가 d의 앞에오는게 맞으면 true를 리턴하는 방식으로
bool cmp(const temp &c, const temp &d){
  if(c.a < d.a) return true;            // 작은게 먼저 오게 정렬
  else if(c.a == d.a) return c.b<d.b    // 다른기준
  else return false;
}

sort(구조체배열이름, 구조체배열이름 + 배열개수, cmp);
~~~

memset(arr, 초기화갑스 sizeof(arr));

## Brute force에서 nCr 모든경우 나타내기
Q) a = {'A', 'B', 'C', 'D', 'E'} 에서 3명으로 묶는 모든경우는?
~~~
int d[5] = {0, 0, 1, 1, 1};             //flag라 생각
do{
  ...
  if(d[i] == 1) 이면
    v.push_back(a[i])                   // 와 같이 v에 조합넣음
}while(next_permutation(d, d+5));
~~~

## Set

이진트리이며 탐색,삭제등 O(logN)이 걸려 다름 자료형보다 빠름.  
중복된 원소 불가능
~~~
set<자료형> A;
A.insert(element);
A.erase(A.find(element));
A.count(element);
~~~

## 힙
최대힙 : priority_queue q;  
최소힙 : priority_queue<int, vector, greater> q;

## 그래프
vector<int> G[n];   n은 노드수  
G[i].push_back(a); a는 인접된 노드
