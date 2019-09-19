---
title: "Algorithm Intermediate"
layout: post
date: 2019-08-27 23:59
tag:
- algorithm
- notes
category: blog
author: jaekwang
description: 알고리즘 중급 정리
---

# 그리디 알고리즘

사실 그냥 문제 보고 풀이대로 코딩하면 되기에 최대,최소 찾는 최적화 문제에서<br>
**쉽다는 오해가 있지만 증명을 해야하기 때문에 어렵다->제일 마지막에 문제풀기**

DP vs Greedy<br>
DP : 어떤 상황에서 할 수 있는 선택을 **모두** 다 살펴보고 가장 좋은거(최적) 선택

Greedy : 할 수 있는 선택중 하나(좋다생각하는 뇌피)를 선택해 정답.<br>
-> 순간 가장 좋은것을 선택하는것이 항상 최적인 경우에 사용

**무조건 최대임을 만족한다는 증명 필요**

ex) 동전의 최소 개수를 구하는 문제에서
화폐가치가 1 10 50 처럼 배수이면 항상 이전 화폐가치에서 n개가 모이면 다음 화폐가치로 바꿀수 있기때문에 Greedy

1 4 7 경우는 DP로 해결한다.

ex) ATM
1. 문제를 읽고 예제를 통해 파악
2. 걸리는 시간이 작은애 부터 하는게 정답일꺼 같은 느낌들음.(Greedy)
3. 느낌이기 때문에 증명이 필요
4. 나의 느낌인 해결방법을 가정하고 증명
각자 걸리는 시간을 p로 정의
p1 <= p2 <= .. <= pn 순으로 줄스는것이 최적이다 가정.

s = np1 + (n-i+1)pi + .. +(n-j+1)pj + .. + pn

이때, i<j 에서 pi와 pj를 바꾸어 서는 반례를 들어옴.<br>
이때 s'은 항상 s보다 커야지 내 증명이 맞다.<br>
s' = np1 + (n-i+1)pj + .. +(n-j+1)pi + .. + pn

s-s' = (i-j)pj + (j-i)pi로 항상 만족을 증명

# 분할 정복(divide and conquer)

문제를 작은 부분 문제로 나눈어 푼다음 다시합쳐 정답을 구한다<br>
대표적 알고리즘 : 퀵소트, 머지소트, 이분탐색 등

**recusive한 부분과 중단지점을 잘 파악하자**<br>
-> 보통 패턴대로 행위 반복하는 문제<br>
-> 행위의 개수만큼 계속 트리가 생성되는 느낌<br>

![img](../assets/images/algorithm(IM)/2.png)


다이나믹 vs 분할정복<br>
다이나믹 경우 나눈 문제가 겹쳐서 겹치는 것을 memorization하고<br>
분할정복 경우 나눈 문제가 각각 **독립적** 이다.

## 이분탐색(Binary Search)

시간복잡도 : O(logN)
1. 탐색 대상을 오름차순으로 정리
2. left는 0  right는 N-1로 idx를 주어 mid를 통해 탐색
3. right<left 일경우 종료한다

~~~
int solve(int L, int R, int des)
{
	if (L > R) return 0;
	else
	{
		int M = (L + R) / 2;
		if (a[M] == des) return 1;
		else if (des < a[M]) solve(L, M - 1, des);
		else solve(M + 1, R, des);
	}
}

~~~

## 머지소트

시간복잡도 : O(NlogN)

![img](../assets/images/algorithm(IM)/1.png)

mergeSort
~~~
void mergeSort(int L, int R)
{
    int m = (L+R)/2;
    if(L<R)
    {
      mergeSort(L,m);
      mergeSort(m+1,R);
      merge(L,R);
    }
}
~~~

merge
~~~
void merge(int L, int R)
{
  int tempL1 =L, int tempR1=(L+R)/2;
  int tempL2 =tempR1+1, int tempR2=R;
  int k=L;
  while (tempL1 <= tempR1 && tempL2 <= tempR2)
  {
    if (A[tempL1] < A[tempL2]) C[k++] = A[tempL1++];
		else C[k++] = B[tempL2++];
  }
	//남아있는거 빼주기, 둘중 하나만 남음
	while(tempL1<= tempR1) C[k++] = A[tempL1++];
	while(tempL2<=tempR2) C[k++] = A[tempL2++];
}
~~~

## 하노이탑이 왜 분할 정복인가?

분할정복은 문제를 작은 **독립적인** 문제로 분할한뒤 문제를 해결한다.

하노이탑 문제를 해결하기 위해서는
1. N개의 핀을 3으로 위치시킨다.
2. 그러기 위해선 N번째 핀을 움직이기 위해 위에 모든핀 N-1개를 2로 위치 시켜야한다.
3. 그후 3번에 N번 핀을 위치시킨다.

이러한 행위를 반복적으로 실행된다.<br>
맨아래 부터 차례로 3으로 위치 시키기 위해
그위 핀들을 3이 아닌곳에 옮긴다

~~~
void solve(int n, int x, int y)
{
	if (n == 1) printf("%d %d\n", x, y);
	else
	{
		solve(n - 1, x, 6 - x - y); //아래 판을 움직이기 위해 위에 판을 다른곳으로 옮긴다
		printf("%d %d\n", x, y);
		solve(n - 1, 6 - x - y, y);
	}
}

~~~

## 트리순회

외우기 쉽게 명칭은 루트노트를 언제 방문하느냐.<br>
프리오더 : 루트 L(재귀) R(재귀) -> DFS방문<br>
인오더 : L 루트 R <br>
포스트오더 : L R 루트 -> 마지막 방문은 최상위 루트노드<br>

![img](../assets/images/algorithm(IM)/3.png)

위에서 알 수 있듯 어떠한 order방식으로 문제가 주어질때<br>
트리의 왼쪽 자식과 오른쪽 자식을 파악하기 위해서는 루트노드의 위치를
아는 것이 중요하다.<br>
포스트오더의 끝은 항상 루트노드이므로 이를 이용하여 루트 노드를 기준으로 인오더가 주어지면
왼쪽 자식과 오른쪽 자식수를 알 수 있다<br>
ex) BOJ 2263번 문제

## 재귀 느낌
- DFS 생각, stack
- 중단 조건까지 내려간뒤에 순차적(stack pop)으로 재귀이후 문장이 이뤄짐
- 중간과정 생각 하지말고 (헷갈려짐) 문제를 푸는 함수를 잘 정의하여 재귀 시킨다.(분할하는 행위)

**시간복잡도**
1. 공간을 어떻게 분할하는지? -> 계속 반으로 분할 할 경우 O(logN)
2. 끝까지 쪼갠후 시간복잡도 A
총 시간 복잡도 A*O(logN)

# 이분 탐색(Binary Search)으로 정답 찾기

이분탐색의 풀이개념으로 문제를 푼다. 어떤문제?
1. 정답의 범위가 주어지고
2. 문제가 특정 x의 값으로 될 수 있는지 아닌지? 에서
3. 최적화 문제를 구하는
전제조건: 정답의 범위가 정렬 되어 있어야한다.

![img](../assets/images/algorithm(IM)/4.png)

정답범위를 이분하여 찾아가므로 탐색공간이 줄어 시간복잡도 효율이 좋다.<br>
끝부분엔 항상 탐색이 이웃하고 한개의  element에서 끝난다

# 완전탐색으로 정답찾기

## 기초지식

### 1. 비트마스크

비트연산(&, |, ~, ^(xor, 두 피연산자가 다르면1 같으면0))을 이용해 부분집합을 나타내는 방법

- not연산 주의 : 자료형에 따라 값이 달라진다. 앞에 공백은 기본적으로 0으로 채워지므로

- shift연산 : 공백은 0으로 채워지고 범위를 넘어가면 사라진다.

A<<B : A*2^B<br>
A>>B : A / 2^B

이분탐색에서 (A+B) /2 를 (A+B) >> 1로<br>
if(N%1 == 1)를 if(N&1)로 변경가능

- 집합 표현
{1,3,4,5,9} 를 2^1 + 2^3 + 2^4 + 2^5 + 2^9 = 570으로 집합을 정수로 표현할 수 있다.
(중복된 원소의 표현은 불가)

0~N까지 정수가 element로 될 수 있는 집합에서<br>
이를 이진수로 표현 하여  부분집합에 존재하는수는 해당 자리에 1 존재하지않은수는 0으로 채운다.

1. 정수로 표현된 집합에서 어떠한 수가 존재하는지?<br>
비트 and 연산을 이용하면된다.<br>
ex) 2가 존재하는가? n &(1<<2) 존재하면 2^2 와같은 수가 return 없을 경우 0

2. 집합에 어느 수를 추가<br>
비트 or 연산을 이용하면된다.

3. 집합에서 어떠한 수 제거<br>
&~ 연산

4. i번수 존재여부 토글 : N^(1<<i)

물론 배열을 사용하는 것이 편리하지만 비트마스크는 부분집합을 배열의 idx로 사용시 유용하다(상태 다이나믹)

### 2. 순열

1~N 까지 이뤄진 수열(겹치는 수x)<br>
사전순 나열(순서가 존재한다)

- next_permutation 알고리즘

1. A[i-1] < A[i]를 만족하는 가장 큰 i를 찾는다
(i이후 내림차순 or 같음)

2. j>= i이면서 (i 오른쪽이며) A[i-1] < A[j]
인 가장 큰 j를 찾는다

3. A[i-1]과 A[j]를 swap

4. i부터 순열을 뒤집는다

-> 가능한 이유는 가장 앞선것은 오름차순이며
제일 마지막은 내림차순이기에

cf) 이전순열은 방식은 똑같지만 부등호만 반대로

이러한 함수는 algorithm 헤더 파일에 존재한다

bool next_permutation(시작주소, 끝주소)<br>
이며 false의 경우 다음순열이 없음을 의미하며 false return할 경우도 시작 순열로 돌아감.

보통 사용안하는 do-while을 여기서 사용한다.

팩토리얼 시간 복잡도로 보통 n이 10미만일때 사용하라.

## 완전탐색 이용 문제 해결

Brute-Force Search로 가능한 모든 경우의 수를 만들어보고 탐색하는 방법.<br>
-> 시간복잡도를 확인하여 할지말지 선택.

BFS, 재귀, 비트마스크, 순열, 백트래킹

### 1. 그냥 다 해보기 N중 for문

### 2. 큐사용하기 -> BFS

현재 state에서 갈 수 있는 모든 state를 방문한다.
1. 문제의 상태의 개수 (시간복잡도 내)
2. 그래프에서 최단경로를 찾는 문제(최소)
3. state - state 간선의 가중치가 모두 1이여야 함. 이때 최단경로 보장.

**state를 어떻게 표현하는지가 핵심**<br>
이차원 state를 저장하는 방법 : yx의 idx 정보를 정수 z로 표현하여 저장한다.

cf) idx를 0~n이라 두면 이차원에서는 y = idx / 너비, x = idx % 너비, 너비 개수대로 그룹이 묶이므로
높이 기준으로 묶을때는 반대<br>
and<br>
이렇게 구한 y,x를 통해 다시 idx구하려면 idx = 너비 * y + x 가된다

BFS에는 map을 이용하는것이 유용<br>
.count() 있는지 bool<br>
-> 이렇게 state가 연속적이 않을때 (1,2,3,4 이런게 아닌 12,24,28 같이) 유용함



# 자료구조2

단독으로 사용되는 문제보단 알고리즘 내에서 시간복잡도를 줄이기 위해 사용
stack을 제외한 유니온파인드, heap, BST는 트리를 이용

## stack

**핵심은 가장 나중에 저장된 것이 제일 빠르게 나온다**

- 스택에 어떠한 자료를 넣고 최근에 넣은것이 넣으려는것과 하나의 **쌍** 을 이루는 경우에 많이사용 (문자열 폭발)

- stack을 이용하여 어떠한 기준이 되는 정보를 저장함으로써 문제의 시간 복잡도를 줄여준다<br>
(문제의 특성에 따라 스택을 오름 or 내림으로 저장됨, 오아시스 재결합, 히스토그램에서 가장 큰 직사각형, 탐색 O(n^2) -> o(n))

## Union-Find

**집합으로 묶고 어떤 원소가 어떤 집합에 포함된지 볼때 사용한다** , 크루스칼 알고리즘에 사용

**집합의 연산에서 합집합 연산만 존재하며 빠른 알고리즘을 요구할때**

구현은 간단한 트리, 2가지 연산 존재

- Find : x가 어떤 집합에 포함되는지, 부모루트 return

- Union : x가 포함되어있는 집합에 y가 포함되어 있는 집합을 붙인다. (루트 노드끼리 합치지 않고 그저 x와 y의 부모노드 끼리 붙이면 나머지 애들이 잘려 나감)

parent[i]에 i의 부모를 저장한다.

1. 초기 parent배열에 자기자신을 저장

~~~
int Find(int x)
{
	// 루트노드를 의미
	if(parent[x] == x) return x;
	else return Find(parent[x]);
}
// 항상 각 노드 마다  루트노드를 리턴한다.
~~~

~~~
void Union(int x, int y)
{
	// 각 최상위 루트노드를 찾고 이음
	x = Find(x);
	y = Find(y);
	parent[y] =x;
}
~~~

![img](../assets/images/algorithm(IM)/5.png)

~~~
//경로 압축 Find
int Find(int x)
{
	if(parent[x] ==x) return x;
	else return parent[x] =  Find(parent[x])
}
~~~


## 힙


**자료에서 최대 최소를 구할때**, N개를 넣고 빼면 힙소트가된다.

검색, 삽입, 제거의 시간복잡도가 트리의 높이만큼인 logN이 된다.<br>
가장 간단하게 배열을 통해 이진트리를 구현한다.<br>
i의 왼쪽자식은 2*i 오른쪽 자식은 2*i +1  부모는 i/2<br>
Last_Idx로 마지막 노드의 위치를 파악해둔다.


Max-Heap 삽입 알고리즘
1. 마지막 위치에 새로운 수를 넣는다
2. parent와 비교해가며 parent에 더 큰 수가 올 수 있도록 한다.

~~~
void Push(int x)
{
	heap[++LastIdx] = x;
	for (int i = LastIdx; i > 1; i /= 2)
	{
		if (heap[i] >= heap[i / 2]) swap(heap[i], heap[i / 2]);
		else break;
	}
}
~~~

Max-Heap 제거 알고리즘
1. 마지막 노드를 최상위 노드로 올린다.
2. 아래로 내려가며 swap한다

~~~
int Pop() //아래로
{
	if (!LastIdx) return 0;
	int ans = heap[1];
	heap[1] = heap[LastIdx];
	heap[LastIdx--] = 0;
	for (int i = 1; i*2 <= LastIdx;)
	{
		if (heap[i] > heap[2 * i] && heap[i] > heap[2 * i + 1]) break;
		else if (heap[2 * i] > heap[2 * i + 1])
		{
			swap(heap[i], heap[2 * i]);
			i = 2 * i;
		}
		else
		{
			swap(heap[i], heap[2 * i + 1]);
			i = 2 * i +1;
		}
	}
	return ans;
}
~~~

**최소힙은 부등호만 반대로**

stl의 queue를 이용해 쉽게 구할수 있음.

최대힙 : priority_queue<int> q;<br>
최소힙 : priority_queue<int, vector<int>, greater<int>> q;<br>
or는 최대힙에서 넣을때 -넣고 뺄때 -를 붙여서 빼면 된다.

### 참고

- 완전 이진트리

leaf 노드를 제외한 모든 노드 자식이 2개
높이가 h인 트리의 노드 수는 2^h-1

- Complete Binary Tree

오른쪽에서 부터 몇개 사라진 형태<br>
힙의 자료구조

##  이진 검색 트리 (Binary Search Tree)

**무엇인가 검색,삭제,삽입이 O(logN)**

배열과 같이 무엇인가를 저장하는 자료형 이지만 시간복잡도를 줄이는 효과를 가진다.<br>
배열은 O(N) 하지만 idx를 이용해 접근할수있다는 장점

이진트리는 현재노드의 왼쪽 서브트리는 항상 작은값을 가지면 오른쪽 서브트리에는
항상 큰값을 가진다.<br>
-> O(logN)을 가지기 위해 균형있는 BST를 사용해야한다.
(STL의 set을 이용해 쉽게 구현)

set은 set헤더 파일에 존재하며<br>
set<자료형> s; //선언<br>
s.insert(변수); //삽입<br>
s.erase(s.find(변수)) //삭제<br>
s.count(변수) //return bool, 존재여부<br>


# 번외

## 문제풀이 방법

최적 문제 => DP vs Greedy
1. Greedy로 풀 수 있는지?
해당 문제의 특성상 어떤 해결 방법이 항상 최적을 만족하는지 증명가능한가?
2. DP로 풀 수 있는지?
이전 상황을 이용(최적값) 다음 상황에서 최적을 얻는다.(d 정의)

1. 문제를 읽고 예제를 파악
2. 문제의 해결 방법 끌어내기
3. 시간 복잡도 확인
4. 해당 문제풀이를 시간 복잡도 내로 할수있을지
5. 어떤 자료형을 사용할지
6. 의사코드 대로 예제 돌려보기

## 팁
- 숫자를 의미하는 문자열을 int로 바꾸는법<br>
n진법을 10진법으로 바꾸는 방식과 동일하게한다<br>
바꾼수 = 바꾼수 * n + 앞자리 (반복문)

- n의 배수인가?<br>
ex) 30의 배수인가?
1. 수를 %30 연산시 0이 되여야한다.
2. 1의 방식에서 경우가 너무많아 시간복잡도 넘을경우.<br>
30 = 2x3x5 이기 때문에<br>
특정 수가 2의배수이고 3의배수이고 5의 배수 여야 함.

2의배수 : 끝이 0 2 4 6 8 로 끝나면 된다.<br>
5의배수 : 끝이 0 5 로 끝나면 된다.<br>
3의배수 : 각 자리수의 합이 3으로 나눠 떨어진다

- 비둘기 집 원리<br>

NMK문제 해결 방법은 1-N 오름차순으로 만든후<br>
K개씩 그룹을 나눈후 각 그룹별로 reverse하여 내림차순으로 만든다.<br>
(각그룹내에서 감소수열 발생)<br>
각 그룹의 대표 아무거나가 증가수열 발생.<br>

그룹을 집의 개수라 생각<br>
이때 비둘기 집 원리에 따라 N<=M*k일때만 가능<br>
why n개집에 n+1마리 비둘기 넣을때 적어도 어느 상자에는 2마리 이상의 비둘기가 있다.<br>
비둘기집 원리에 의해 숫자가 (M * K + 1)개일 때는-> 집의 개수가 M+1개가 되므로 -> 최대 증가 부분 수열의 길이는 M + 1, 최대 감소 부분 수열의 길이는 K + 1이 되기 때문에 N은 M * K 이하

- 보통 3가지 경우가 있을때 이를 1,2,3으로 두면 세수의 합 - (두경우의합)으로 ...다음 하나 택가능

- 버블소트와 머지소트의 관계
분할된 배열 오른쪽에 수가 더 작으면 왼쪽의 배열 수만큼 cnt에 더해준다

- long long과 int의 연산

int간 연산에서 오버플로우 발생을 염두해 long long에 담을때 주의 해야한다(long long = int * int)<br>
우측식에서 기본적으로 int형에 임시로 담기기에 오버플로 발생<br> (long long) int * (long long) int 로 형변환 후 사용

- 반복문 중단<br>

break : 반복문을 빠져나간다<br>
continue : 조건문 확인하러간다.<br>
goto EXIT; - EXIT: 해당 레이블로 이동후 코드를 실행

- 2차원 idx를 1개의 정수로

idx를 0~n이라 두면 이차원에서는 <br>
y = idx / 너비<br>
x = idx % 너비  -> 너비 개수대로 그룹이 묶이므로<br>
높이 기준으로 묶을때는 반대<br>
이렇게 구한 y,x를 통해 다시 idx구하려면 idx = 너비 * y + x 가된다

- string 유용 메소드

to_string<br>
stoi<br>
find<br>
==  //비교<br>
보통은 char 배열을 이용하고 문자열 전체를 사용해야할 경우 string을 이용하는편.<br>
입력을 받기도 편하고(cin>>변수>>변수)<br>
cin과 cout은 매우 느려, count << << endl
과 count<< <<'\n'도 매우 큰차이

### 시간복잡도의 문제가 있을때

- 분할정복 같은 경우 재귀함수에서 모든 경우를 재귀시키지 말고 탐색할 필요 없는 부분은 재귀 안하게끔.

- n중 루프를 사용안하고 빼낼 순 없는지?