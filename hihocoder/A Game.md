# 题目链接
A Game[http://hihocoder.com/problemset/problem/1338]

# 思路
动态规划。令f[i][j]为从i到j小ho先选的最大分数，那么它是从上一步小hi选转移过来的，小hi选的时候一定会选再上一步小ho可选的最小的那个，那么这个问题就是一个简单的minmax问题。

# 代码
	int f[MAXN][MAXN];
	int num[MAXN];
	int N, M;
	int dfs(int i, int j)
	{
		if (i > j) return 0;
		if (f[i][j] != 0) return f[i][j];
		if (i == j) f[i][j]=num[i];
		else  if (i + 1 == j) f[i][j]=max(num[i], num[j]);
		else
			f[i][j] = max(num[i]+min(dfs(i + 2, j), dfs(i + 1, j - 1)),num[j]+min(dfs(i, j - 2), dfs(i + 1, j - 1)));
		return f[i][j];
	}
	int main() {
	#ifdef DEBUG
		ifstream cin("in.txt");
		ofstream cout("out.txt");
		//freopen("in.txt", "r", stdin);
		//freopen("out.txt", "w", stdout);
	#endif
		memset(f, 0, sizeof(f));
		std::ios::sync_with_stdio(false);
		cin >> N;
		for (int i = 1; i <= N; i++)
			cin >> num[i];
		cout << dfs(1, N)<<endl;
		return 0;
	}