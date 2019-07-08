---
title: "Algorithm Basic"
layout: post
date: 2019-07-08 23:59
tag:
- algorithm
- notes
category: blog
author: jaekwang
description: 알고리즘 기초 정리.
---

# 입출력 방식

입출력 방식의 큰 구분은 버퍼 사용여부, 형식의 유무로 구분된다.

## 입출력, 버퍼를 사용하는 경우

scanf : 버퍼(문자배열)에 내용이 없을경우 OS가 사용자의 입력을 대기한다. 이때
키보드로 입력을주고 Enter(\n, 개행문자)를 치면 \n까지 입력버퍼에 저장된다.<br>
**이때 scanf format에 맞게 처리 된다**<br>
- 특정문자 : 해당 문자를 읽고 버려라. ex) scanf("%d,%d", &a, &b);
- 공백,개행문자: 화이트 스페이스가 아닌 문자가 나올떄까지 화이트 스페이스 읽어서 버려라
- **%c** : 뭐가 되든 입력버퍼로 부터 1byte 읽어라! -> 주의!!
- %d,%lld 등: 읽기 시작부터 화이트스페이스를 버린후 대상을 화이트스페이스가 **나오기 전** 까지 읽는다.

형식지정자의미 : 입력버퍼(문자열) 대상을 형식지정자에 맞게 읽는다. ex) 숫자로
cf) scanf의 반환형은 입력에 성공한 서식 문자수

**주의점** : %c는 무조건 1byte를 읽기때문에 개행문자가 버퍼에 남게되면 의도치 않게 원하는 입력이 안들어간다.

### 한줄 받기

**보통 본인 scanf("%s", 배열); 이나 for(입력크기만큼) scanf("%d", a+i) 식으로 받는다.**

1. scanf("%[^\n]\n", char배열);
 - %[abc] : a or b or c가 **아닌** 문자가 나오는 곳에서 입력 받는것 종료
 - %[^abc]: a or b or c 문자가 **나오는** 곳에서 입력 받는것 종료

2. fgets(배열,배열길이,stdin); **끝에 \n이 포함**
  입력실패시 EOF \0(NULL)를 반환
3. getline(cin, s); **끝에 \n포함  안되는거 주의!**
cf) Ctrl+Z를 통해 while(fgets())를 빠져나온다

## 문자열 함수(string.h 헤더)

- strlen(s) : 문자열 길이 반환(개행문자 이전까지)
- strcpy(des, src) : des에 src 문자열을 복사한다.
- strcmp(s1, s2) : 아스키 코드(사전적 순서), s1이 빠를수록 s1-s2<0
  같으면 == 0 느리면 >0
- strcat(s1, s2) : s1뒤에 s2를 붙인다. **(s1배열 크기가 충분해야함)**

# 자료구조

- 헤더 : queue, stack
- .push(a) : 데이터 추가
- .top()(stack) or .front() or .back()(queue) : 데이터 top or 앞,뒤 확인
   **확인만, pop안됨**
- .pop() : 데이터 pop
- .empty() : 비워져있는지 확인
- .size() : size 반환

## 스택

LIFO(Last In First Out) 구조<br>
ex) 괄호검사, 쇠막대기, 에디터, **DFS, 재귀함수**

## 큐

FIFO(First In First Out) 구조<br>
ex) 조세퍼스, **BFS**

## 덱

앞뒤로 넣을수 있는 자료구조 -> 따라서 push,pop이 앞뒤로 나뉘어짐<br>
-> 잘 안씀

## vector

1. vector<자료형> G; <br>
2. vector<자료형> G[100]; <br>

1은 일차원배열, 2는 이차원배열로 생각!

G.push_back(원소); <br>
G.size(); <br>
G.clear(); <br>
그래프에서, 위에 함수 자주 사용

---

참고해두기 <br>
G.pop_back(원소); <br>
G.empty(); <br>
G.resize(n,x); size를 n으로 변경하고 x로 초기화 <br>
v1 == v2 모두 같은원소?<br>
v1 > v2 문자열 비교 처럼

## pair

pair<자료형, 자료형> point; 선언<br>
make_pair(val1, val2); pair 반환<br>
point.first; 첫번째 val<br>
point.second; 두번째 val<br>

# DP(Dynamic programing)

**d[n]은 한번 구하면 고정되어 변하지 않는다**<br>
-> 때문에 이를 이용해 문제풀때 직전 단계만 생각! 아주 머나먼 전단계를
생각하면서 머리속 꼬이지 말기!<br>
**이전에 구해놓은 d를 이용하여 현재의 답을 알 수 있어야한다**<br>

1. 문제를 파악하고 수기로 값이 어떻게 변하는지 파악

![img](../assets/images/algorithm(basic)/1.PNG)

2. 1처럼 하나씩 값을 적어가며 현재 구하는 N문제가 N보다 작은 값의 답이 이용 되면 DP 문제임을 확인
3. 이제 문제의 조건에 따라 값을 1~N까지 구하는 main문 작성

ex) 1로 만들기 문제 -> 가능한 연산 /3, /2, -1 -> 이전의 답을 이용할 수 있음(현재 값보다 작은값으로 줄어들기에) -> **이때 /A와 -1중에 어떠한것이 최소의 결과를 도출할지 장담 못하기에 min(d[N/a], d[N-1])으로 최소값 이용**
-> DP문제 따로 오답노트 만들기.

# 수학

**(A+B)%C = ((A%C) + (B%C))** : DP풀때 출력을 %c한 값으로 하라는 경우가 있는데 이는 마지막 출력때 하는것이 아니라 각 D의 값을 구할때 마다 하는것이 좋다
-> 경우의수의 너무커 오버플로 나올 수 있기 때문에.

위 식은 +와 * 또한 가능하지만 /경우 다르다.<br>
-경우 : (A-B)%C = ((A%C)- (B%C) +C) %C (음수가 나올 수 있기에 +C한다)

## 최대공약수 (GCD, Greatest Common Divisor)

A와 B의 최대 공약수 G는 A B의 공통된 약수중 가장 큰 정수<br>
서로소 : 최대 공약수가 1인 수

1. 2부터 min(A,B)까지 모든 정수를 보며 두 수 모두 모두 나눠 떨어지는 것, 최대 -> O(n)
2. 유클리드 호제법 <br>
GCD(a,b) = GCD(b, a%b) 를 이용 arg2가 0이 될때 arg1의 값이 GCD이다

![img](../assets/images/algorithm(basic)/2.PNG)

-> a,b 대소 상관 없음.<br>
3개의수 abc경우 GCD(GCD(a,b),c)로 풀면됨, n개의수 마찬가지

## 최소공배수 (LCM, Least Common Multiple)

두수의 공통된 배수중 가장 작은 정수<br>
**LCM = (A*B)/GCD** -> LCD는 AorB보다 클수 있기 때문에 **자료형 범위 주의**

## 진법 변환

### 10진수 N을 B진법으로

N/B -> 몫1 나머지1<br>
몫1/B -> 몫2 나머지2<br>
.. -> 이를 몫>0 까지 진행하고 나머지를 역순으로 출력하면된다.<br>
(string에 저장하고 revers(s.begin(), s.end())이용)

![img](../assets/images/algorithm(basic)/3.PNG)

예외) -2진법 변환<br>
1. 나머지는 항상 양수가 되게 한다(%연산이 항상 +인것은 아님)
2. 음수 홀수의 경우 몫에 -1을 해주어 나눠준다

![img](../assets/images/algorithm(basic)/5.PNG)


### B진법을 10진수로 변환

앞에서 부터 순차적으로 변환한다.<br>
102(3)의 경우.<br>
1-> 1 * 3<br>
10 -> 이전결과(1*3) * 3 +0<br>
102 -> 이전결과(1*3^2 + 0 * 3) + 2<br>
즉 이전결과 * B + 끝수<br>

![img](../assets/images/algorithm(basic)/4.png)

### A진법을 B진법으로

A진법 -> 10진법 -> B진법<br>
예외) 2진수를 8진수로 -> 끝에서 부터 3개 묶어서 10진법으로 바꾸면됨(0~7)

## 소수

약수가 1과 자기 밖에 없는수로 즉 2~N-1사이의 어떠한 정수로도 나누어 떨어지면 안된다.

### 단순 소수 찾기 문제

단순 2~N-1 for문 경우 o(n)<br>
24라는 소수가아닌 수를 생각해보자 이는 2 3 4 6 8 12 항상 쌍으로 존재하기에
2부터 그 중간값인 root(24)까지 즉, 왼쪽만 살펴보면 된다<br>
o(루트(n))

![img](../assets/images/algorithm(basic)/6.png)

![img](../assets/images/algorithm(basic)/7.PNG)

### 1~N범위내 모든 소수 찾기

범위내 소수들을 이용해 문제를 풀때 사용한다.<br>
ex) 골든바흐의 추측

#### 에라토스테네스의 체

1. 2~N 모든 수를 써둔다
2. 아직 지워지지 않은수중 가장 작은 수 찾는다.
3. 그 수는 소수이다.
4. 그의 배수를 모두 지운다.

-> 이전 지워진 수 다음 소수의 제곱 이전 까진 다 지워져 있으므로
 현재 소수의 제곱부터 배수들을 지워가면 된다<br>
 -> 제곱에서 오버플로우 발생할수 있으므로 현재소수 * 2 부터 지운다

 ![img](../assets/images/algorithm(basic)/8.png)


## 소인수분해, 팩토리얼, 조합

### 소인수 분해

1이 될때까지 2~N-1까지 수 중 먼저 나머지 0 되면 그걸로 나눠주는것을 반복

### 팩토리얼

팩토리얼 직접구하면 너무 큰값이다. 10!정도 까지만 우리가 풀 수 있다.<br>
N!의 0의 갯수 -> 소인수 분해 -> 너무 크면 불가 -> 2와 5 소인수가 몇개 들은지만 알면됨 -> 또 2의 제곱수가 반드시 5의 제곱수 보다 많기때문에
5의 제곱수만 확인하면됨

**N!의 0의 갯수 = [N/5] + [N/5^2] + [N/5^3] + ..(pow(5,k)가 N보다 작을때 까지 k 증가)**<br>
-> 5^2같은경우 5가 두개므로 다시 뒤에서 더해준다.

### 조합

nCm = n!/(m!(n-m)!)<br>
조합의 0의 개수  -> 이거 또한 같은 방식이지만<br>
나누기가 들어가 2의 개수가 적을 수 있으므로 2의 개수도 세줘야한다.<br>

# 정렬

O(n)(버블, 순차, 삽입) vs O(nlogn) (힙, 퀵, 병합)<br>
stl내부 sort의 시간복잡도 o(nlogn)<br>

sort(start, end) -> **start주소부터 end-1주소 까지 정렬한다**<br>
sort(v.start(), v.end()) -> 기본 오름차순<br>
sort(v.start(), v.end(), greater<int>()) -> 내림차순<br>

## pair<int, int>
-> vector<pai<int,int>> 경우 first먼저 증가순으로 하고 같으면 second
증가순으로 정렬한다.

## 구조체 정렬
기준점이 필요하므로 cmp 함수를 따로 만들어야한다. <br>
문제의 정렬 순서에 따라 작성한다.<br>
**중요! a가 b보다 앞에오는게 맞으면 true를 리턴하는 방식으로 작성한다.**

~~~
bool cmp(const 구조체 이름 &a, const 구조체 이름 &b) //a,b는 매개
{
  if(a.x < b.x)
    return true;
  else if(a.x == b.x)
    a.y<b.y  // 같을 경우 다른 정렬기준을 작성
  else return false;
}
~~~

cf) 백준 10989 - 수정렬3 : 모든 수를 저장할수없을때 -> 반복되는것을 줄여주게 데이터를 저장한다

# 그래프

경로, cycle, 방향그래프, 무방향그래프, 차수, 정점, 간선

## 그래프 저장방법

1. 인접행렬

정점의 개수를 V개라 하면 V*V 크기의 이차원 배열을 만든다.<br>
A[i][j] =1 (i->j로의 간선 정보가 있을시)<br>
-> 잘 사용 안한다. 없는 간선도 메모리 차지 하기 많기때문에<br>
-> 쉬운 문제에 사용<br>
-> 가중치 저장시 1대신 weight를 저장

2. **인접리스트**

정점의 개수 만큼 Vector<int> G[V]; 를 만든다. <br>
G[i].push_back(j) (i->j 간선 정보 있을시)<br>
->**자주 사용** <br>
-> 가중치 저장시 pair or struct로 간선 정보 저장<br>
**주로 struct사용**<br>

~~~
struct Edge
{
  int next;
  int weight;
  Edgd(int next, int weight) : next(next), weight(weight){}
};
~~~

3. 간선 리스트

**stl을 사용하지 못할 경우**<br>
1. E라는 배열에 모든 간선 정보 저장, E[M][2], 간선 M개 2개 노드
2. E를 E[][0]을 기준으로 sort
3. cnt[N](정점개수)에 차수를 저장
4. cnt 누적합으로 변경
-> 이웃정보 : i의 이웃은 E배열에서 E[cnt[i-1]][1] 부터 E[cnt[i]-1][1]이다.

## 그래프 탐색

모든 노드를 방문한다. check로 방문여부 파악.

### DFS

stack이용(재귀 함수로 구현됨)<br>
갈 수 있는 만큼 최대한 많이가고 갈수 없을경우 이전노드에서 또 다른 노드
갈 수 있는지 확인<br>

1. 시작 노드 check, 방문
2. 시작 노드에서 갈수 있는 노드 파악
3. 갈수 있으면 해당 노드 방문

~~~
void DFS(int x)
{
  check[x] =1;
  printf("%d ", x);
  for(int i=0; i<v[x].size(); i++)
  {
    int next = v[x][i];
    if(!check[next]) DFS(next);
  }
}
~~~
시간복잡도 : o(v + E)

### BFS

queue 이용<br>
큐를 이용해 지금 위치에서 갈수 있는 모든 노드를 큐에 넣는다.<br> **큐에 넣읗떄 방문** 했다고 체크 해야함

1. 시작 노드 check, queue넣고, 방문
2. !q.empty() 동안 q.front에서 갈수 있는곳 모두 queue넣고 방문
3. queue에 모두 넣었으면 pop

~~~
void BFS(int x)
{
  queue<int> q;
  check[x] =1;
  q.push(x);

  while(!q.empty())
  {
      int cur = q.front();
      for(int i=0; i<v[cur].size(); i++)
      {
        int next = v[cur][i];
        if(!check[next])
        {
          check[next]=1;
          q.push(next);
        }
      }
      q.pop();
  }
}
~~~

**BFS는 최단거리 보장! -> 단계이용**

![img](../assets/images/algorithm(basic)/10.PNG)

~~~
void BFS(int x)
{
  queue<int> q;
  check[x] =1;
  q.push(x);

  while(!q.empty())
  {
    int qSize = q.size(); //기존 코드에서 크게 감싼다, level파악위해
    for(int j=0; j<qSize; i++)
    {
      int cur = q.front();
      for(int i=0; i<v[cur].size(); i++)
      {
        int next = v[cur][i];
        if(!check[next])
        {
          check[next]=1;
          q.push(next);
        }
      }
      q.pop();
    }
    level ++ // 여기까지  
  }
}
~~~

![img](../assets/images/algorithm(basic)/11.png)

## 문제

- 연결 요소의 개수

**문제에서 주어지는 모든 간선정보가 한 그래프가 생성된다는 생각 ㄴㄴ**<br>
요소개수를 파악하기위해
~~~
for(int i=1; i<=N; i++) // 모든 Vertex에 대해서
                        // 나는 보통 Vertex 1부터 시작
{
  if(!check[i]) // 연결된 한그래프(?)에 대해서만 완전 탐색 되므로
  {
    DFS(i);             
    cnt ++;
  }
}
~~~

- 이분 그래프

그래프의 정점의 집합을 둘로 분할하여, 각 집합에 속한 정점끼리는 서로 인접하지 않도록 분할 할수있는 그래프

모든 꼭짓점을 빨파로 칠하되 연결된 두 노드는 서로 다른 색으로 칠한다. (색이 k개면 k분 그래프), 사이클 갯수가 홀수이면 이분 그래프가 될 수 없다.

색칠하는 방법<br>

BFS : 이웃한 모든층 모두 같은색, 레벨 다르면 다른색<br>
DFS : 진행 하면서 색깔 번갈아 가며 칠함<br>

**주의** 1번 정점에 대해서만 하면 안됨! -> 그래프가 여러개 일 수 도 있어서<br>
이분 그래프 확인 : 한 노드에서 인접한 모든 노드 색이 달라야함<br>

~~~
void DFS(int x, int color)  //색은 1과 2로
{
	check[x] = color;         // check를 1대신 색으로 표현
	for (int i = 0; i < G[x].size(); i++)
	{
		int temp = G[x][i];
		if (!check[temp])
			DFS(temp, 3 - color);
	}
}
~~~

- 간선이 하나인 그래프에서 싸이클 갯수 확인

~~~
void DFS(int x)
{
	check[x]= 1;
	for (int i = 0; i < G[x].size(); i++)
	{
		int temp = G[x][i];
		if (check[temp] == 0)
			DFS(temp);
		// 이미 완전히 끝났다 판단되는거는 cnt 안하기 위해
		// 써클 찾는 문제 이므로 이미 방문해서 써클안되는 노드는 절때 써클 안됨(간선 한개씩 일시에)
		else if(!done[temp])		// 재귀 마치면 마칠 당시를
			                        // x라 하면 (x-1, x)쌍
									// 그다음은 (x-2, x-1) 쌍

		{
			for (int j = temp; j != x; j = G[j][0]) // 마지막 노드가 써클이룸
				cnt++;							    // 그 써클타고 계속 돌면 자기 자신이 나옴
													// 써클 멤버들을 모두 방문
			cnt++;									// 자기 자신을 세워준다
		}
		done[x] = 1; // 방문했던거 모두 이제 done 1.
					 // stack인해 방문한 노드 모두
		             // done 1이 채워진다
	}
	//!! 이거 재귀 순서 정리 잘하기!
}
~~~

- 단지문제 (2차원 map에 의해 정보 주어질때)

map yx에대해 그때마다 인접표현을 상하좌우 간것으로 표현하고 체크한다.<br>
dx = {0,0,-1,1} dy = {1-1-00}

~~~
int dy[4] = { -1,1,0,0 }; //상하좌우 순서대로
int dx[4] = { 0,0,-1,1 };

void DFS(int y, int x)
{
	check[y][x] = 1;
	cnt++;

	for (int i = 0; i < 4; i++) //yx에대한 상하좌우 인접을 의미
	{
		int ny = y + dy[i];
		int nx = x + dx[i];

		if (ny >= 0 && ny < N && nx >= 0 && nx < N) //map 범위 벗어나지 않게
		{
			if (!check[ny][nx]&& house[ny][nx])
			{
				DFS(ny, nx);
			}
		}
	}

}
~~~

# 트리

**싸이클이 없는 그래프**<br>
-> 항상 나타나는 특징으로 정점수 V개면 간선수 V-1개이다<br>
-> **경로가 무조건 한개이다.**

그럼 정점 V, 간선 V-1개면 트리인가? ㄴㄴ, 임의의 두 정점 사이 경로가 항상존재
(모두 연결됨)임이 추가 되어야한다.

## 트리의 표현

1. **그래프와 같이(난 무방향으로 품)** -> 주로 이렇게
2. 모든 노드의 부모를 저장(배열에) -> 부모 찾긴 좋으나 자식찾는건 힘듬
3. 이진트리 경우 배열 이용
int Tree[Vertex갯수][2]; ->행:Vertex 열: 0->왼쪽 1 ->오른쪽

## 트리 탐색

DFS, BFS 가능하며 트리경우 사용가능한 순회 방식이 있다.<br>
빈노드는 특정값으로 비었음을 확인

1. preorder

노드방문 - 왼쪽자식preorder - 오른쪽자식 preorder

~~~
void preorder(int Vertex)
{
	if (Vertex != -1) {
		printf("%c", (char)(Vertex + 'A'));
		preorder(Tree[Vertex][0]);
		preorder(Tree[Vertex][1]);
	}
}
~~~

2. inorder

왼쪽자식inorder - 노드방문 - 오른쪽자식 inorder<br>
BST에서 Delete 구현할때 사용한다.

~~~
void inorder(int Vertex)
{
	if (Vertex != -1) {
		inorder(Tree[Vertex][0]);
		printf("%c", (char)(Vertex + 'A'));
		inorder(Tree[Vertex][1]);
	}
}
~~~

3. postorder

왼쪽자식 postorder - 오른쪽자식 postorder - 노드방문<br>
자식 먼저 처리해야하는 경우 사용된다.
~~~
void postorder(int Vertex)
{
	if (Vertex != -1) {
		postorder(Tree[Vertex][0]);
		postorder(Tree[Vertex][1]);
		printf("%c", (char)(Vertex + 'A'));
	}
}
~~~

### 부모 찾기 문제

now->next<br>
now에서 갈수있는 노드 next는 자식이므로 트리입장에서 next의 부모가 now다.
**parent 배열 따로 두어,** parent[next] = now;

### 트리의 지름

트리에 존재하는 모든 경로중 가장 긴 길이.<br>
BFS두번 <br>
1. 임의의 정점에서(보통 루트) 모든 정점 까지 거리를 구한다.
2. 가장 먼 노드를 A라 할때 A에서 모든 정점 거리를 구한다
dis[next] = dis[cur] + Tree[cur][i].weight;


cf 헤더파일
vector -> vector<br>
stack -> stack<br>
queue -> queue<br>
문자열 -> String.h<br>
memset -> memory.h<br>
sort, min -> algorithm<br>
MAX_INT -> limits.h<br>
