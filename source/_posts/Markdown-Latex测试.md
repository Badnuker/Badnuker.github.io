---
title: Markdown&Latex测试
date: 2023-09-20 23:40:20
tags: 测试
mathjax: true
feature: true
---

$\LaTeX$

$$
\LaTeX
$$

~~~cpp
#include<bits/stdc++.h>
using namespace std;
const int maxn = 5e5 + 5;

int tot, ccnt;
int deg[maxn];
unordered_map<string, int> col;
vector<int> p[maxn];

struct DSU {
	int fa[maxn];
	DSU() {
		for (int i = 1; i < maxn; i++) {
			fa[i] = i;
		}
	}
	int getfa(int x) {
		if (fa[x] == x) {
			return x;
		} else {
			return fa[x] = getfa(fa[x]);
		}
	}

	bool merge(int x, int y) {
		int fx = getfa(x), fy = getfa(y);
		if (fx == fy) {
			return 0;
		}
		fa[fx] = fy;
		return 1;
	}
} dsu;

int main() {
	ios::sync_with_stdio(0);
	cin.tie(0);
	string s, t;
	while (cin >> s >> t) {
		if (!col[s]) {
			col[s] = ++tot;
		}
		if (!col[t]) {
			col[t] = ++tot;
		}
		if (dsu.merge(col[s], col[t])) {
			ccnt++;
		}
		p[col[s]].push_back(col[t]);
		p[col[t]].push_back(col[s]);
		deg[col[s]]++, deg[col[t]]++;
	}
	if (ccnt < tot - 1) {
		cout << "Impossible\n";
		return 0;
	}
	int cnt = 0;
	for (int i = 1; i <= tot; i++) {
		if ((deg[i] & 1) && ++cnt > 2) {
			cout << "Impossible\n";
			return 0;
		}
	}
	cout << "Possible\n";
	return 0;
}
~~~