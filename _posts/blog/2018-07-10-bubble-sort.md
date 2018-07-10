---
layout: post
title: Bubble sort
excerpt: "가장 간단한 정렬 알고리즘 - 버블 정렬"
date: 2018-07-10
modified: 2018-07-10
author: sclee
categories: blog
tags: [R]
image:
  url: http://i.imgur.com/p0TzIOM.gif
comments: true
share: true
---


##   Bubble sort

### 개요

가장 단순한 정렬 알고리즘 중에 하나인 버블정렬입니다. `Bubble sort`는 인접한 두 수를 비교하여 큰 수를 뒤로 보내는 정렬 알고리즘입니다.


### 알고리즘 분석

시간 복잡도는 $O(n^2)$을 가집니다. 

 - 비교 횟수
N개의 숫자가 주어졌을 때 버블 정렬은 한 번의 순회에서는 N-1번의 비교 연산을 진행합니다. 그리고 다음 순회 때는 비교 연산이 한 개씩 줄어듭니다. 마지막에 있는 숫자는 정렬이 완료 되었기 때문입니다. 그래서 총 비교 연산은 아래와 같습니다.

$$(n-1) + (n-2 ) + ... + 2 + 1 = (n-1) *n/2 = \frac{n(n-1)}{2}$$

- 최악의 경우에는 비교를 할 때마다 교환이 이루어집니다. 그리고 최선의 경우에는 정렬이 이미 되어 있는 경우로써 자리 교환이 한 번ㄷ 일어나지 않습니다.


### 코드

```
for(int i=0;i<N;++i){
	for(int j=0;j<N-(1+i));++j){
		if(arr[j]>arr[j+1]){
			int tmp = arr[j];
			arr[j] = arr[j+1];
			arr[j+1] = tmp;
		}
	}
}
```
