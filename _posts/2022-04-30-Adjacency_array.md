---
title: 인접 배열로 그래프 나타내기
author: Yongjun042
date: 2022-04-30 22:48:00 +0900
categories: [Blog, Develope]
tags: [Algorithm, Graph, C++]

---

## 인접 배열(Adjacency Array)이란?

인접 배열은 인접 리스트에서 간선에 대한 정보를 한곳에 모두 몰아넣은 형태이다.

메모리의 지역성이 있다는 특성이 있다.

정점에 관한 정보 V배열과 간선에 관한 정보 E배열 2개로 이루어져 있다.

V배열에는 인덱스(정점 번호)와 해당 정점과 이어진 간선의 첫 번호가 저장되어 있다.

E배열에는 각 간선의 정보가 저장되어 있다. 간선이 가르키는 정점과 간선이 출발한 정점과 이어진 다음 간선에 대한 정보가 저장되어 있다.

1번과 이어진 간선을 알고 싶으면 E[V[1]] 를 참고하고 해당 배열에 들어있는 다음 간선 정보를 참고해서 순회를 하면 된다.

![adjacency list](/assets/img/postimg/adjacencyarray/1.png "각 정점 번호마다 연결된 다른 정점의 번호 (간선)를 화살표로 연결한 리스트가 있다.")
다음과 같은  인접 리스트에서

![trasnforming](/assets/img/postimg/adjacencyarray/2.png "이전 사진에서 연결된 간선의 리스트를 한곳에 모은다.")
도착하는 정점의 정보(간선)을 모두 한 곳에 몰아넣는다.

![Adjacency Array](/assets/img/postimg/adjacencyarray/3.png "리스트를 배열로 바꾸고 리스트의 연결 정보를 대응하는 배열 번호로 바꾸어 저장한다.")
그리고 해당 정보를 다음과 같이 배열의 형태로 변형을 하면 된다.

### C++ 구현

간선의 개수 n을 입력으로 받고 간선의 정보(연결된 두 정점 a b)를 입력으로 받는다.

정점의 개수가 얼마나 될지 몰라 정점의 정보는 map으로 저장을 하였다.

``` C++
#include <iostream>
#include <vector>
#include <unordered_map>
#include <utility>
#include <queue>
#include <stack>
using namespace std;

//v[n]{start,end} n번 정점에 연결된 간선(e)의 시작 지점(start)과 마지막 지점(end)
unordered_map<int, pair<int, int>> v;
//e[m] {next, pointTo} 정점 n에 연결된 간선 m. m이 가르키는 정점는 pointTo. n에 연결된 다른 간선 next.
//마지막 간선의 경우 다음 간선이 -1
vector<pair<int, int> > e;
unordered_map<int, bool> visit;

void insert_aa(int a, int b)
{
	//새로운 간선이 들어갈 위치 e[pos]
	int pos = e.size();
	//기존에 존재하는 정점를 연결할 경우
	if (v.count(a))
	{
		//해당정점 간선 리스트의 마지막을 업데이트
		e[v[a].second].first = pos;
		v[a].second = pos;
	}
	else
	{
		//정점 생성
		v[a].first = pos;
		v[a].second = pos;
	}
	//간선 생성
	e.push_back({ -1,b });
}

void dfs_aa(int cur)
{
	visit[cur] = true;
	cout << cur << ' ';
	//정점에 연결된 간선 번호
	int it = v[cur].first;
	//현재 정점에 연결된 간선 번호 순회
	while (it != -1)
	{
		//it 간선이 가르키는 정점
		int next = e[it].second;
		//방문하지 않았으면 방문
		if (!visit[next])
		{
			dfs_aa(next);
		}
		//다음 간선
		it = e[it].first;
	}
}

void bfs_aa(int sp)
{
	queue<int> q;
	q.push(sp);
	visit[sp] = true;
	while (!q.empty())
	{
		cout << q.front() << ' ';
		//정점에 연결된 간선 번호
		int it = v[q.front()].first;
		q.pop();
		while (it != -1)
		{
			//it 간선이 가르키는 정점
			int next = e[it].second;
			if (!visit[next])
			{
				q.push(next);
				visit[next] = true;
			}
			//다음 간선
			it = e[it].first;
		}
	}
}

int main()
{
	int n;
	cin >> n;
	for (int i = 0; i < n; i++)
	{
		//a,b를 연결하는 무방향 간선 생성
		int a, b;
		cin >> a >> b;
		insert_aa(a, b);
		insert_aa(b, a);
	}

	bfs_aa(0);
	visit.clear();
	cout << endl;
	dfs_aa(0);

	return 0;
}	
/*
* 입력 예시
5
0 1
0 2
3 2
2 4
5 0
*/

```
