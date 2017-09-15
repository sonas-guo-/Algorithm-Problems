# 题目链接
小Hi与花盆[http://hihocoder.com/problemset/problem/1572]

# 思路
每次放新花盆只与相邻的两个花盆有关，所以用个set保存已经放的花盆，每次插入的时候找到相邻的，比较下之间的差即可。

# 代码
	set<int> S;
	int main() {
	#ifdef DEBUG
		/*ifstream cin("in.txt");
		ofstream cout("out.txt");*/
		freopen("in.txt", "r", stdin);
		freopen("out.txt", "w", stdout);
	#endif
		int n, k, a;
		cin >> n >> k;
		S.insert(0);
		S.insert(n + 1);
		for (size_t i = 0; i < n; i++)
		{
			cin >> a;
			auto iter = S.insert(a).first;
			auto pre = iter, next = iter;
			advance(pre, -1);
			advance(next, 1);
			if (((*iter) - (*pre) == k+1) || ((*next) - (*iter) == k+1))
			{
				cout << i+1 << endl;
				return 0;
			}
		}
		cout << -1 << endl;
		return 0;
	}