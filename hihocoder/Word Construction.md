# 题目链接
 Word Construction[http://hihocoder.com/problemset/problem/1334]

# 思路
最大独立集，Bron–Kerbosch算法。

# 代码
	#define MAXN 105
	int edge[MAXN][MAXN];
	string words[MAXN];
	int path[MAXN],record[MAXN],dp[MAXN];
	int ans,N;
	bool share_letters(string a, string b)
	{
		set<char> chs;
		for (auto ch : a)
			chs.insert(ch);
		for (auto ch : b)
			if (chs.find(ch) != chs.end())
				return true;
		return false;
	}
	bool dfs(int u, int deep)
	{
		for (int i = u+1; i < N; i++)
		{
			if (dp[i] + deep <= ans) return false;
			if (edge[u][i])
			{
				int j;
				for (j = 0; j < deep; j++)
					if (!edge[i][path[j]]) break;
				if (j == deep)
				{
					path[j] = i;
					if (dfs(i, deep + 1)) return true;
				}
			}
		}
		if (deep > ans)
		{
			transform(path, path + deep, record, [](int a) {return a; });
			ans = deep;
			return true;
		}
		return false;
	}
	void maxclique()
	{
		for (int i = N-1; i >=0; i--)
		{
			path[0] = i;
			dfs(i, 1);
			dp[i] = ans;
		}
	}
	int main() {
	#ifdef DEBUG
		ifstream cin("in.txt");
		ofstream cout("out.txt");
		//freopen("in.txt", "r", stdin);
		//freopen("out.txt", "w", stdout);
	#endif
		
		cin >> N;
		memset(edge, 0, sizeof(edge));
		for (size_t i = 0; i < N; i++)
			cin >> words[i];
		for (size_t i = 0; i < N; i++)
			for (size_t j = i+1; j < N; j++)
				if (share_letters(words[i], words[j]))
				{
					edge[i][j] = 1;
					edge[j][i] = 1;
				}
		for (size_t i = 0; i < N; i++)
			for (size_t j = 0; j < N; j++)
				edge[i][j] = !edge[i][j];
		maxclique();
		cout << ans << endl;
		return 0;
	}