# 题目链接
Full Binary Tree Picture[http://hihocoder.com/problemset/problem/1342]

# 思路
记录下每层树的结点的坐标，然后遍历每一层找满足条件的点，加起来即可。

# 代码
	vector<vector<int>> tree;
	int query(vector<vector<int>> tree, int x, int y)
	{
		int ans = 0;
		for (size_t i = 0; i<tree.size(); i++)
		{
			if ((-tree[i][0]) <= x)
			{
				int p = upper_bound(tree[i].begin(), tree[i].end(), y) - tree[i].begin();
				ans += p;
			}
			else
				break;
		}
		return ans;
	}
	int main() {
	#ifdef DEBUG
		ifstream cin("in.txt");
		ofstream cout("out.txt");
		//freopen("in.txt", "r", stdin);
		//freopen("out.txt", "w", stdout);
	#endif
		std::ios::sync_with_stdio(false);
		int N, M,edge;
		cin >> N>>M;
		tree.assign(N,vector<int>());
		tree[0].emplace_back(0);
		if (N == 1) 
			edge = 1;
		else
			edge = 3 * pow(2, N - 2);
		for (size_t i = 1; i < N; i++)
		{
			if (edge != 3)
				edge /= 2;
			else
				edge = 2;
			for (size_t j = 0; j < tree[i-1].size(); j++)
			{
				tree[i].emplace_back(tree[i - 1][j] - edge);
				tree[i].emplace_back(tree[i - 1][j] + edge);
			}
		}
		int a, b, c, d;
		for (size_t i = 0; i < M; i++)
		{
			cin >> a >> b >> c >> d;
			cout << query(tree, c, d) - query(tree, a - 1, d) - query(tree, c, b - 1) + query(tree, a - 1, b - 1) << endl;
		}
		return 0;
	}
