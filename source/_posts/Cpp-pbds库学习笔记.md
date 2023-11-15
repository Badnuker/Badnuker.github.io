---
title: C++ pb_ds库学习笔记
date: 2023-11-15 22:36:45
tags: 杂项
---

## 前言

pb_ds 库是 gnu 内置的一个功能强大的库，封装了许多常用但代码量大的数据结构，例如 红黑树、Splay、可并堆...

需要新引入的头文件和命名空间：

~~~cpp
#include<bits/extc++.h>
using namespace __gnu_cxx;
using namespace __gnu_pbds;
~~~

（注意：Clang++中没有此扩展库，需要使用g++编译器，并且不能是TDM-GCC）

## 堆

### 声明

~~~cpp
template<class _Tv, class Cmp_Fn = std::less<__gnu_pbds::priority_queue<_Tv, Cmp_Fn, Tag, _Alloc>::value_type>, class Tag = __gnu_pbds::pairing_heap_tag, class _Alloc = std::allocator<char>> class __gnu_pbds::priority_queue<_Tv, Cmp_Fn, Tag, _Alloc>
~~~

解析：

- `_Tv`：堆中元素的类型
- `Cmp_Fn`：比较函数，默认为`less`，与`std::priority_queue`相同，为大根堆
- `Tag`：默认即可
- `_Alloc`：别管

### 定义

定义一个存整数的小根堆：

~~~cpp
__gnu_pbds::priority_queue<int,greater<int>> q;
~~~

（为与STL区分开，此处应指定命名空间`__gnu_pbds::`）

### 用法

区别于STL的实用用法：

`push`：pb_ds中的`push`函数具有返回值，返回被`push`元素的迭代器

`modify`：传入`(迭代器，新值)`，将迭代器所指向的元素变成新的值

`join`：用法形如`a.join(b)`，将`b`堆合并到`a`堆（需要两个堆定义相同），并清空`b`堆

### 例题

**[洛谷P3377 【模板】左偏树/可并堆](https://www.luogu.com.cn/problem/P3377)**

通过代码（O2：212ms）：

~~~cpp
#include<bits/stdc++.h>
#include<bits/extc++.h>
using namespace std;
using namespace __gnu_cxx;
using namespace __gnu_pbds;
const int maxn = 1e5 + 5;

int n, m;
int d[maxn];
int fa[maxn];
__gnu_pbds::priority_queue<pair<int, int>, greater<pair<int, int>>> q[maxn];

int getfa(int x) {
	if (fa[x] == x) {
		return x;
	} else {
		return fa[x] = getfa(fa[x]);
	}
}

int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	cin >> n >> m;
	for (int i = 1, x; i <= n; i++) {
		cin >> x;
		q[i].push({x, i});
		fa[i] = i;
	}
	for (int i = 1, opt, x, y; i <= m; i++) {
		cin >> opt;
		if (opt == 1) {
			cin >> x >> y;
			if (d[x] || d[y]) {
				continue;
			}
			int fx = getfa(x), fy = getfa(y);
			if (fx == fy) {
				continue;
			}
			fa[fx] = fy;
			q[fy].join(q[fx]);
		} else {
			cin >> x;
			if (d[x]) {
				cout << -1 << '\n';
				continue;
			}
			int ff = getfa(x);
			cout << q[ff].top().first << '\n';
			d[q[ff].top().second] = 1;
			q[ff].pop();
		}
	}
	return 0;
}
~~~

# 未完待续...